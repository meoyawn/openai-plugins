# Icons

Zaidan uses `lucide-solid`.

```tsx
import { Search, ArrowRight } from "lucide-solid"
```

Do not import from `lucide-react` or pass icon names as strings.

## Button and badge icons

Use `data-icon` for icons inside buttons, badges, tabs, toggles, and similar components.

```tsx
<Button>
  <Search data-icon="inline-start" />
  Search
</Button>

<Button>
  Next
  <ArrowRight data-icon="inline-end" />
</Button>
```

Zaidan styles use `has-data-[icon=inline-start]` and `has-data-[icon=inline-end]` to adjust spacing.

## Sizing

Do not add size classes to icons inside Zaidan components unless explicitly requested.

```tsx
// Incorrect.
<Button>
  <Search class="mr-2 size-4" />
  Search
</Button>

// Correct.
<Button>
  <Search data-icon="inline-start" />
  Search
</Button>
```

Standalone decorative icons outside components may use `size-*` when width and height are equal.

## Icon props

Pass icon components, not string keys.

```tsx
function StatusBadge(props: { icon: typeof Check; label: string }) {
  const Icon = props.icon
  return (
    <Badge>
      <Icon data-icon="inline-start" />
      {props.label}
    </Badge>
  )
}
```
