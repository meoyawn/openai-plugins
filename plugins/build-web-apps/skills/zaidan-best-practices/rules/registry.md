# Registry & CLI

Zaidan uses shadcn registry mechanics with a SolidJS registry namespace.

## components.json

Existing Solid projects should point shadcn at the Zaidan registry:

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "kobalte",
  "rsc": false,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "src/styles/globals.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "iconLibrary": "lucide",
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "registries": {
    "@zaidan": "https://zaidan.carere.dev/r/{style}/{name}.json"
  }
}
```

Rules:

- `style: "kobalte"` is the active Zaidan style key for component registry URLs.
- `iconLibrary: "lucide"` maps to `lucide-solid` in Solid source.
- `tailwind.css` must point at the actual global Tailwind v4 file.
- Zaidan style/theme/font registry items may add CSS files and CSS variables; inspect diffs before overwriting.

## CLI workflow

Use the project runner. Examples use Bun.

```bash
bunx shadcn@latest info
bunx shadcn@latest add @zaidan/button
bunx shadcn@latest add @zaidan/button --dry-run
bunx shadcn@latest add @zaidan/button --diff
bunx shadcn@latest add @zaidan/button --view
```

Before adding:

1. Read `components.json`.
2. Check whether the component already exists in the configured `aliases.ui` path.
3. Read Zaidan docs at `https://zaidan.carere.dev/ui/<component>` or registry JSON at `https://zaidan.carere.dev/r/kobalte/<component>.json`.
4. For primitive behavior, read Kobalte or Corvu docs.

After adding:

1. Read all generated files.
2. Fix aliases if the registry item used repo-local paths.
3. Check for React remnants: `className`, `htmlFor`, `React`, `useState`, `useEffect`, `asChild`, `render`, `lucide-react`, `recharts`.
4. Check the relevant composition, form, icon, chart, and styling rules.

Never use `--overwrite` unless the user explicitly approves replacing local files. Prefer `--dry-run` and `--diff`.

## Customization registry items

Zaidan uses registry items for design system changes:

```bash
bunx shadcn@latest add @zaidan/style-vega
bunx shadcn@latest add @zaidan/blue @zaidan/neutral
bunx shadcn@latest add @zaidan/font-inter
```

Inspect CSS diffs. If the style item creates `src/styles/base.css`, ensure the global CSS imports it in a Tailwind v4-compatible way.

## Authoring inside the Zaidan repo

Only apply this section when editing `github.com/carere/zaidan` itself.

Registry file: `src/registry/kobalte/registry.json`.

Item shape:

```json
{
  "name": "tooltip",
  "type": "registry:ui",
  "dependencies": ["@kobalte/core"],
  "registryDependencies": [],
  "files": [
    {
      "path": "src/registry/kobalte/ui/tooltip.tsx",
      "type": "registry:ui"
    }
  ]
}
```

Rules:

- Keep `items` alphabetized by `name`.
- Use `registry:ui` for single-file UI components, `registry:hook` for hooks, `registry:block` for multi-file blocks, `registry:theme` / `registry:style` for CSS-variable items.
- Verify every file path exists.
- Put npm packages in `dependencies`; put Zaidan registry items in `registryDependencies`.
- Existing Zaidan registry dependencies often use full URLs such as `https://zaidan.carere.dev/r/kobalte/button.json`.
- Run the repo's registry build/format/check commands when available.
