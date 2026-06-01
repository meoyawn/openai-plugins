# Solid Adapter

Use this before applying normal shadcn/ui examples. Zaidan is SolidJS, not React.

## React to Solid mapping

| React / shadcn | Solid / Zaidan |
| --- | --- |
| `className` | `class` |
| `htmlFor` | `for` |
| `className?: string` | `class?: string` |
| `React.ComponentProps<"div">` | `ComponentProps<"div">` from `solid-js` |
| `React.ReactNode` | `JSX.Element` |
| `{ className, ...props }` | `const [local, others] = splitProps(props, ["class"])` |
| Default destructuring | `mergeProps({ ...defaults }, props)` |
| `useState` | `createSignal` |
| `useMemo` | `createMemo` |
| `useEffect` | `createEffect`, `on`, `onMount`, `onCleanup` |
| `{condition && <El />}` | `<Show when={condition}><El /></Show>` |
| `items.map(...)` in JSX | `<For each={items}>{(item) => ...}</For>` |
| `asChild`, Base `render` | Kobalte/Corvu `as={Component}` or `as="a"` |
| `lucide-react` | `lucide-solid` |
| `event.target.value` | `event.currentTarget.value` |

## Imports

```tsx
// Incorrect.
import * as React from "react"
import { SearchIcon } from "lucide-react"

// Correct.
import type { ComponentProps, JSX, ValidComponent } from "solid-js"
import { createMemo, createSignal, For, mergeProps, Show, splitProps } from "solid-js"
import { Search } from "lucide-solid"
```

## Props

```tsx
type PanelProps = ComponentProps<"section"> & {
  class?: string
}

const Panel = (props: PanelProps) => {
  const [local, others] = splitProps(props, ["class"])

  return <section class={cn("flex flex-col gap-4", local.class)} {...others} />
}
```

With defaults:

```tsx
const BadgeList = (rawProps: BadgeListProps) => {
  const props = mergeProps({ limit: 5 }, rawProps)
  const [local, others] = splitProps(props, ["items", "limit", "class"])

  return (
    <div class={cn("flex flex-wrap gap-2", local.class)} {...others}>
      <For each={local.items.slice(0, local.limit)}>{(item) => <Badge>{item}</Badge>}</For>
    </div>
  )
}
```

## Kobalte polymorphism

Zaidan components wrap Kobalte primitives. Use `as`, not `asChild` or `render`.

```tsx
<DialogTrigger as={Button} variant="outline">
  Open
</DialogTrigger>

<TooltipTrigger as="span" class="inline-block">
  Hover me
</TooltipTrigger>
```

When authoring wrappers around Kobalte primitives:

```tsx
import type { PolymorphicProps } from "@kobalte/core/polymorphic"
import * as TooltipPrimitive from "@kobalte/core/tooltip"
import type { ComponentProps, ValidComponent } from "solid-js"
import { splitProps } from "solid-js"

type TooltipContentProps<T extends ValidComponent = "div"> = PolymorphicProps<
  T,
  TooltipPrimitive.TooltipContentProps<T>
> &
  Pick<ComponentProps<T>, "class">

const TooltipContent = <T extends ValidComponent = "div">(props: TooltipContentProps<T>) => {
  const [local, others] = splitProps(props as TooltipContentProps, ["class"])
  return <TooltipPrimitive.Content class={cn("z-tooltip-content", local.class)} {...others} />
}
```

## Corvu polymorphism

Corvu primitives use `DynamicProps`. Read Corvu docs before wrapping, especially for `as`, context hooks, and data attributes.

```tsx
import { type DynamicProps, Trigger, type TriggerProps } from "@corvu/drawer"
import type { ValidComponent } from "solid-js"

type DrawerTriggerProps<T extends ValidComponent = "button"> = DynamicProps<T, TriggerProps>

const DrawerTrigger = <T extends ValidComponent = "button">(props: DrawerTriggerProps<T>) => {
  return <Trigger data-slot="drawer-trigger" {...(props as TriggerProps)} />
}
```

## Control flow and events

```tsx
const [open, setOpen] = createSignal(false)
const [items, setItems] = createSignal<string[]>([])

<Show when={open()} fallback={<Button onClick={() => setOpen(true)}>Open</Button>}>
  <For each={items()}>{(item) => <Badge>{item}</Badge>}</For>
</Show>

<Input value={value()} onInput={(event) => setValue(event.currentTarget.value)} />
```

Do not use React-only `key`, `forwardRef`, `useId`, `useCallback`, `memo`, `Fragment`, or `props.children` patterns without checking the Solid equivalent.
