# Directus Interface: Conditional Fields
Directus interface to create conditional field groups that will show/hide other fields based on the selected option.

## :question: How does it work

### Creating the field using this interface

When creating a field using this interface you can **create a list of key-value pairs**:
- **`key`**: represents the name of the conditional field
- **`value`**: represents an array of fieldnames that will only be displayed when the key is selected in the dropdown menu.
  - Press `enter` in the `value` field to add the fieldname

### New collection value.

A dropdown menu will appear with the `key` as display text.
- When selecting an entry **the fields linked to that key will be displayed**.
- All other fields, in all other options, **will be hidden**
  - Fields, available in multiple key-value pairs, will also show when the type is selected.

## Usage

1. Build for production

    ```
    npm run build
    ```

2. Copy the contents of the **`dist`** folder to `public/extensions/custom/interfaces/conditional-fields/`

## :gear: Options

Checkout common interface options(passed via `props`) [here.](https://github.com/directus/v8-archive/blob/master/app/src/interfaces/README.md)

| Option             | Default | Interface      | Desc                                                                  |
| ------------------ | ------- | -------------- | --------------------------------------------------------------------- |
| choices            |         | [key-value ]() | Conditional field groups. Key represents the display name, value represents an array of fields to display when the choice is selected.                                                     |
| placeholder        |         | [text-input]() | Placeholder text.                                                     |
| formatting         | `true`  | [toggle]()     | Replace underscores with spaces & capitalize the key                  |
| icon               |         | [icon]()       | Material Icon to display on the input.                                |
