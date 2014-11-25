# When to use blocks, delegates or data source

## Block
- Asynchronous
- User inputs with multiple options (Process workflow)
- Data source driven inputs
- Returns many values
- If there’s no tracked stated or if it’s defined in the same method

## Delegate
- Synchronous
- Doesn’t return values (void)
- Provides control over performing an action (return BOOL)
- User input with one action
- If tracked state is shared (in a property)

## Data source
- Returns ONE value (if more than one use a block)
