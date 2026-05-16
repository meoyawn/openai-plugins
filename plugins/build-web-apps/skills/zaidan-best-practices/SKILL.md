---
name: zaidan-best-practices
description: Use when working with Zaidan, @zaidan registry components, SolidJS shadcn-style UI, components.json using the kobalte style, Tailwind CSS v4 Zaidan themes, Kobalte/Corvu primitives, lucide-solid icons, or porting React shadcn/ui code to SolidJS/Zaidan.
---

# Zaidan Best Practices

Zaidan is a shadcn-style registry for SolidJS. It keeps the shadcn copy-into-your-app workflow, but the generated code is SolidJS and uses Kobalte/Corvu primitives, Tailwind CSS v4, and `lucide-solid`.

> **IMPORTANT:** Run shadcn CLI commands with the project's package runner: `bunx shadcn@latest`, `pnpm dlx shadcn@latest`, `yarn dlx shadcn@latest`, or `npx shadcn@latest`, based on the lockfile or `packageManager`. Examples below use `bunx`; substitute the project runner.

## Core Rule

Start from the normal shadcn composition rules, then apply the Solid/Zaidan adapter:

- React `className` -> Solid `class`; React `htmlFor` -> Solid `for`.
- React hooks -> Solid primitives: `createSignal`, `createMemo`, `createEffect`, `onMount`, `onCleanup`.
- React control flow -> Solid JSX: `<Show>`, `<For>`, `<Switch>/<Match>`.
- Radix/Base `asChild` or `render` -> Kobalte/Corvu `as={...}`.
- `lucide-react` -> `lucide-solid`.
- Recharts assumptions -> Zaidan Chart, which wraps Apache ECharts.

## Critical Rules

These rules are always enforced. Read the linked file before writing or fixing related code.

### Solid Adapter -> [solid-adapter.md](./rules/solid-adapter.md)

- Use Solid syntax and types: `class`, `for`, `ComponentProps`, `JSX.Element`, `splitProps`, `mergeProps`.
- No React imports, hooks, `forwardRef`, `className`, `htmlFor`, `key`, `asChild`, or `render`.
- Kobalte components use `as={Button}` / `as="a"` polymorphism.
- Corvu components use `DynamicProps`; read Corvu docs for `as` and context hooks.
- Event handlers read from `event.currentTarget`, not `event.target`.

### Registry & CLI -> [registry.md](./rules/registry.md)

- Configure `components.json` with `style: "kobalte"` and `@zaidan`.
- Use `shadcn add @zaidan/<name>` and preview with `--dry-run`, `--diff`, or `--view`.
- Read Zaidan component docs or registry JSON before composing a component.
- If editing the Zaidan repo itself, keep registry entries sorted and validate dependencies.

### Styling & Tailwind -> [styling.md](./rules/styling.md)

- Use `class` for layout, not component restyling.
- Prefer built-in variants and Zaidan style/theme registry items.
- Use semantic tokens: `bg-background`, `text-muted-foreground`, `text-destructive`.
- Use `gap-*`, not `space-x-*` / `space-y-*`.
- Use `size-*` when width and height are equal.
- No manual `dark:` color overrides or raw status colors.

### Forms & Inputs -> [forms.md](./rules/forms.md)

- Forms use `FieldGroup` + `Field`.
- Labels use `FieldLabel for="..."`.
- `InputGroup` uses `InputGroupInput`, `InputGroupTextarea`, `InputGroupAddon`, and `InputGroupButton`.
- Option sets use `ToggleGroup`; independent booleans use `Switch` or `Checkbox`.
- Validation uses `data-invalid` on `Field` and `aria-invalid` on the control.

### Component Composition -> [composition.md](./rules/composition.md)

- Use Zaidan components before custom markup.
- Dialog, Sheet, Drawer, and AlertDialog need titles.
- Use full `Card` composition.
- Use `Alert`, `Empty`, `Skeleton`, `Badge`, `Separator`, and `solid-sonner`.
- `TabsTrigger` belongs inside `TabsList`; `Avatar` needs `AvatarFallback`.

### Icons -> [icons.md](./rules/icons.md)

- Import icons from `lucide-solid`.
- Icons in buttons/badges use `data-icon="inline-start"` or `data-icon="inline-end"`.
- Do not add manual size classes to icons inside Zaidan components unless the user explicitly asks.

### Charts -> [charts.md](./rules/charts.md)

- Use Zaidan `ChartContainer` and ECharts options, not Recharts components.
- Use `ChartConfig`, `chartColors`, and CSS variables such as `var(--chart-1)`.

## Component Selection

| Need          | Use                                                                                                              |
| ------------- | ---------------------------------------------------------------------------------------------------------------- |
| Button/action | `Button` with variants and sizes                                                                                 |
| Forms         | `Field`, `Input`, `Textarea`, `Select`, `NativeSelect`, `Checkbox`, `RadioGroup`, `Switch`, `Slider`, `InputOTP` |
| Option sets   | `ToggleGroup` + `ToggleGroupItem`                                                                                |
| Data display  | `Table`, `Card`, `Badge`, `Avatar`, `Item`                                                                       |
| Navigation    | `Sidebar`, `NavigationMenu`, `Breadcrumb`, `Tabs`, `Pagination`                                                  |
| Overlays      | `Dialog`, `Sheet`, `Drawer`, `AlertDialog`, `Popover`, `HoverCard`, `Tooltip`                                    |
| Feedback      | `Toaster` from Zaidan sonner, `Alert`, `Progress`, `Skeleton`, `Spinner`                                         |
| Commands      | `Command` inside `Dialog`                                                                                        |
| Charts        | `ChartContainer` with ECharts options                                                                            |
| Layout        | `Card`, `Separator`, `Resizable`, `ScrollArea`, `Accordion`, `Collapsible`                                       |
| Empty states  | `Empty`                                                                                                          |
| Menus         | `DropdownMenu`, `ContextMenu`, `Menubar`                                                                         |

## Workflow

1. Inspect `components.json`, lockfile, aliases, existing UI files, and Tailwind CSS file.
2. Confirm Zaidan is configured: `style: "kobalte"` and `registries["@zaidan"]`.
3. Search/read before coding: Zaidan docs (`https://zaidan.carere.dev/ui/<component>`), registry JSON (`https://zaidan.carere.dev/r/kobalte/<name>.json`), and installed source.
4. Add missing components with `bunx shadcn@latest add @zaidan/<name>`. Use `--dry-run` or `--diff` before overwriting.
5. Read generated files after install. Fix React remnants, wrong aliases, wrong icon imports, and composition rule violations.
6. For primitive-specific behavior, read Kobalte docs at `https://kobalte.dev/docs/core/components/<component>` or Corvu docs at `https://corvu.dev/docs/primitives/<component>`.
7. Verify with the project's formatter/typecheck/test commands when available.

## Quick Patterns

```tsx
import { createSignal, For, Show } from "solid-js"
import { Search } from "lucide-solid"

import { Button } from "@/components/ui/button"
import { Badge } from "@/components/ui/badge"
import { Field, FieldGroup, FieldLabel } from "@/components/ui/field"
import { Input } from "@/components/ui/input"
import { Spinner } from "@/components/ui/spinner"

const [query, setQuery] = createSignal("")
const [saving, setSaving] = createSignal(false)

<FieldGroup>
  <Field>
    <FieldLabel for="query">Search</FieldLabel>
    <Input id="query" value={query()} onInput={(event) => setQuery(event.currentTarget.value)} />
  </Field>
</FieldGroup>

<Button disabled={saving()}>
  <Show when={saving()} fallback={<Search data-icon="inline-start" />}>
    <Spinner data-icon="inline-start" />
  </Show>
  Search
</Button>

<For each={items()}>{(item) => <Badge>{item.label}</Badge>}</For>
```

```tsx
<Dialog>
  <DialogTrigger as={Button} variant="outline">
    Edit profile
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Edit profile</DialogTitle>
      <DialogDescription>Update account details.</DialogDescription>
    </DialogHeader>
  </DialogContent>
</Dialog>
```

## Detailed References

- [rules/solid-adapter.md](./rules/solid-adapter.md) — React-to-Solid mappings, Kobalte/Corvu polymorphism
- [rules/registry.md](./rules/registry.md) — `components.json`, CLI, registry authoring
- [rules/forms.md](./rules/forms.md) — Field, InputGroup, validation, option controls
- [rules/composition.md](./rules/composition.md) — groups, overlays, cards, empty/loading/status components
- [rules/icons.md](./rules/icons.md) — `lucide-solid`, `data-icon`, icon sizing
- [rules/styling.md](./rules/styling.md) — tokens, variants, `class`, spacing, dark mode
- [rules/charts.md](./rules/charts.md) — Zaidan ECharts patterns
