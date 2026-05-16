# Component Composition

## Use Zaidan components first

Before writing custom markup, check whether Zaidan has a component:

| Instead of | Use |
| --- | --- |
| Custom callout div | `Alert`, `AlertTitle`, `AlertDescription` |
| Custom empty state | `Empty` |
| `<hr>` or border div | `Separator` |
| Custom pulse placeholders | `Skeleton` |
| Styled status span | `Badge` |
| Custom menu | `DropdownMenu`, `ContextMenu`, `Menubar` |
| Custom modal | `Dialog`, `AlertDialog`, `Sheet`, `Drawer` |

## Groups

Keep items inside their group/list constructs and follow the component docs.

```tsx
<DropdownMenuContent>
  <DropdownMenuGroup>
    <DropdownMenuItem>Profile</DropdownMenuItem>
    <DropdownMenuItem>Settings</DropdownMenuItem>
  </DropdownMenuGroup>
</DropdownMenuContent>
```

For Zaidan `Select`, use Kobalte's `options`, `itemComponent`, and `sectionComponent` patterns:

```tsx
<Select
  options={items}
  optionValue="value"
  optionTextValue="label"
  placeholder="Select a fruit"
  itemComponent={(props) => (
    <SelectItem item={props.item}>{props.item.rawValue.label}</SelectItem>
  )}
>
  <SelectTrigger>
    <SelectValue<(typeof items)[number]>>{(state) => state.selectedOption().label}</SelectValue>
  </SelectTrigger>
  <SelectContent />
</Select>
```

## Overlays need titles

Dialog, Sheet, Drawer, and AlertDialog need an accessible title. Use `class="sr-only"` if hidden.

```tsx
<DialogContent>
  <DialogHeader>
    <DialogTitle>Edit profile</DialogTitle>
    <DialogDescription>Update profile details.</DialogDescription>
  </DialogHeader>
</DialogContent>
```

Use Kobalte `as`, not wrappers:

```tsx
<DialogTrigger as={Button} variant="outline">
  Edit profile
</DialogTrigger>
```

## Card structure

Use full Card composition.

```tsx
<Card>
  <CardHeader>
    <CardTitle>Team members</CardTitle>
    <CardDescription>Manage access.</CardDescription>
  </CardHeader>
  <CardContent>{/* content */}</CardContent>
  <CardFooter>
    <Button>Invite</Button>
  </CardFooter>
</Card>
```

Do not dump every element into `CardContent`.

## Loading buttons

`Button` has no `isPending` or `isLoading` prop. Compose `Spinner` with `disabled`.

```tsx
<Button disabled={saving()}>
  <Spinner data-icon="inline-start" />
  Saving...
</Button>
```

## Tabs

`TabsTrigger` must be inside `TabsList`.

```tsx
<Tabs defaultValue="account">
  <TabsList>
    <TabsTrigger value="account">Account</TabsTrigger>
    <TabsTrigger value="password">Password</TabsTrigger>
  </TabsList>
  <TabsContent value="account">...</TabsContent>
</Tabs>
```

## Avatar

Always include `AvatarFallback`.

```tsx
<Avatar>
  <AvatarImage src="/avatar.png" alt="Jane Doe" />
  <AvatarFallback>JD</AvatarFallback>
</Avatar>
```

## Empty state

```tsx
<Empty>
  <EmptyHeader>
    <EmptyMedia variant="icon">
      <Folder />
    </EmptyMedia>
    <EmptyTitle>No projects</EmptyTitle>
    <EmptyDescription>Create a project to get started.</EmptyDescription>
  </EmptyHeader>
  <EmptyContent>
    <Button>Create project</Button>
  </EmptyContent>
</Empty>
```

## Toasts

Use `solid-sonner` plus the Zaidan `Toaster` component.

```tsx
import { toast } from "solid-sonner"

toast.success("Changes saved.")
toast.error("Something went wrong.")
```

Render one `Toaster` near the app root.
