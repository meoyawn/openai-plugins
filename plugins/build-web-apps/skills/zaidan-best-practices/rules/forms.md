# Forms & Inputs

## Field layout

Use `FieldGroup` and `Field`; do not build form layout from raw `div` stacks.

```tsx
<FieldGroup>
  <Field>
    <FieldLabel for="email">Email</FieldLabel>
    <Input id="email" type="email" />
  </Field>
  <Field>
    <FieldLabel for="password">Password</FieldLabel>
    <Input id="password" type="password" />
  </Field>
</FieldGroup>
```

Use `orientation="horizontal"` or `orientation="responsive"` for settings pages. Use `class="sr-only"` for visually hidden labels.

## Control selection

| Need | Use |
| --- | --- |
| Text input | `Input` |
| Multi-line text | `Textarea` |
| Dropdown with JS behavior | `Select` |
| Native/no-JS dropdown | `NativeSelect` |
| Searchable dropdown | `Combobox` |
| Boolean setting | `Switch` |
| Boolean form choice | `Checkbox` |
| One choice from several | `RadioGroup` |
| Segmented 2-7 choices | `ToggleGroup` |
| OTP code | `InputOTP` |
| Numeric range | `Slider` |

## InputGroup

Never put a raw `Input` or `Textarea` inside `InputGroup`.

```tsx
// Incorrect.
<InputGroup>
  <Input placeholder="Search..." />
</InputGroup>

// Correct.
<InputGroup>
  <InputGroupInput placeholder="Search..." />
</InputGroup>
```

Buttons and adornments inside inputs use `InputGroupAddon` and `InputGroupButton`.

```tsx
<InputGroup>
  <InputGroupInput placeholder="Search..." />
  <InputGroupAddon align="inline-end">
    <InputGroupButton size="icon-xs" aria-label="Search">
      <Search data-icon="inline-start" />
    </InputGroupButton>
  </InputGroupAddon>
</InputGroup>
```

## ToggleGroup

Use `ToggleGroup` for segmented choices; do not loop `Button` with manual active state.

```tsx
<Field orientation="horizontal">
  <FieldTitle id="density-label">Density</FieldTitle>
  <ToggleGroup multiple={false} defaultValue="comfortable" spacing={2} aria-labelledby="density-label">
    <ToggleGroupItem value="compact">Compact</ToggleGroupItem>
    <ToggleGroupItem value="comfortable">Comfortable</ToggleGroupItem>
    <ToggleGroupItem value="spacious">Spacious</ToggleGroupItem>
  </ToggleGroup>
</Field>
```

Use `multiple` for multi-select groups. Use `multiple={false}` for a single selected value.

## FieldSet

Use `FieldSet` and `FieldLegend` for related checkboxes, radios, or switches.

```tsx
<FieldSet>
  <FieldLegend variant="label">Preferences</FieldLegend>
  <FieldDescription>Select all that apply.</FieldDescription>
  <FieldGroup>
    <Field orientation="horizontal">
      <Checkbox id="marketing" />
      <FieldLabel for="marketing" class="font-normal">
        Marketing emails
      </FieldLabel>
    </Field>
  </FieldGroup>
</FieldSet>
```

## Validation and disabled states

Both field and control attributes are needed.

```tsx
<Field data-invalid>
  <FieldLabel for="email">Email</FieldLabel>
  <Input id="email" aria-invalid />
  <FieldDescription>Invalid email address.</FieldDescription>
</Field>

<Field data-disabled>
  <FieldLabel for="email-disabled">Email</FieldLabel>
  <Input id="email-disabled" disabled />
</Field>
```

Use `FieldError` when displaying validation messages produced by a form library or local validation.

## Solid-controlled input

```tsx
const [email, setEmail] = createSignal("")

<Input
  id="email"
  value={email()}
  onInput={(event) => setEmail(event.currentTarget.value)}
/>
```

Use `event.currentTarget`, not `event.target`.
