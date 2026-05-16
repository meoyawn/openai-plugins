# Styling & Tailwind

Zaidan components rely on Tailwind v4 semantic tokens and `.z-*` style classes supplied by Zaidan style registry items.

## Semantic colors

Use tokens, not raw color utilities.

```tsx
// Incorrect.
<div class="bg-blue-500 text-white">
  <p class="text-gray-600">Secondary text</p>
</div>

// Correct.
<div class="bg-primary text-primary-foreground">
  <p class="text-muted-foreground">Secondary text</p>
</div>
```

For status indicators, prefer component variants and semantic tokens:

```tsx
<Badge variant="secondary">+20.1%</Badge>
<Badge>Active</Badge>
<span class="text-destructive">-3.2%</span>
```

If a missing success/warning token is needed, add a custom CSS variable or use a Badge variant instead of raw `text-green-*` / `bg-yellow-*`.

## Variants first

```tsx
// Incorrect.
<Button class="border border-input bg-transparent hover:bg-accent">Save</Button>

// Correct.
<Button variant="outline">Save</Button>
```

Use built-in `variant`, `size`, `orientation`, `spacing`, and component-specific props before custom classes.

## `class` for layout

Use `class` for layout and positioning around a component, not for replacing the component's colors or typography.

```tsx
<Card class="mx-auto max-w-md">
  <CardContent>Dashboard</CardContent>
</Card>
```

Good uses: `grid`, `flex`, `gap-*`, `mx-auto`, `max-w-*`, `min-h-*`, `w-full`.

Bad uses: overriding component backgrounds, borders, text size, font weight, hover colors, or focus rings when a prop or theme token exists.

## Spacing

Use `gap-*`, not `space-x-*` or `space-y-*`.

```tsx
<div class="flex flex-col gap-4">
  <Input />
  <Input />
  <Button>Submit</Button>
</div>
```

## Size shorthand

Use `size-*` when width and height are equal:

```tsx
<Avatar class="size-10">
  <AvatarFallback>JD</AvatarFallback>
</Avatar>
```

## Truncation

Use `truncate`, not the expanded trio.

```tsx
<span class="truncate">{label()}</span>
```

## Dark mode

Do not manually pair light/dark color utilities. Zaidan themes and CSS variables handle light/dark:

```tsx
// Incorrect.
<div class="bg-white text-slate-950 dark:bg-slate-950 dark:text-white" />

// Correct.
<div class="bg-background text-foreground" />
```

## Conditional classes

Use `cn()` from the configured utility alias.

```tsx
<div class={cn("flex items-center gap-2", active() && "bg-primary text-primary-foreground")} />
```

## Overlays

Do not add manual `z-50` / `z-[999]` to app code around `Dialog`, `Sheet`, `Drawer`, `AlertDialog`, `DropdownMenu`, `Popover`, `Tooltip`, or `HoverCard`. Zaidan overlay components own stacking.
