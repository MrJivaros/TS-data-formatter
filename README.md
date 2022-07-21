# Description
This repo aims to provide small utility functions to check the type of data. Really used to check the type of data returned by APIs



```ts
export function boolVal(val: unknown): boolean {
  if (typeof val === 'boolean') return val;
  if (val === 0 || val === '0' || val === 'f' || val === 'false') return false;
  if (val === 1 || val === '1' || val === 't' || val === 'true') return true;
  if (val === undefined || val === null) throw new Error('Missing boolean value');
  return !!val;
}
```
```ts
export function boolValOrUndef(val: unknown): boolean | undefined {
  return val === undefined || val === null ? undefined : boolVal(val);
}
```

```ts
export function strVal(val: unknown): string {
  if (val === undefined || val === null || val === '')
    throw new Error('Missing string value');
  if (typeof val === 'string') return val;
  return (val as object).toString();
}
```

```ts
export function strValOrUndef(val: unknown): string | undefined {
  return val === undefined || val === null || val === '' ? undefined : strVal(val);
}
```


```ts
export function numberVal(val: unknown): number {
  if (typeof val === 'number') return val;
  if (val === undefined || val === null) throw new Error('Missing number value');
  if (typeof val === 'string') return parseInt(val, 10);
  throw new Error(`Cannot convert the value to integer (${typeof val})`);
}

```


```ts
export function numberValOrUndef(val: unknown): number | undefined {
  return val === undefined || val === null ? undefined : numberVal(val);
}
```

```ts
export function bufferVal(val: unknown): Buffer {
  if (val === undefined || val === null || !(val instanceof Buffer))
    throw new Error('Missing buffer value');
  return val;
}
```

```ts
export function bufferValOrUndef(val: unknown): Buffer | undefined {
  return val === undefined || val === null ? undefined : bufferVal(val);
}
```

```ts
export function dateTimeValAsIsoString(val: unknown): string {
  return toDate(val).toISOString().substring(0, 19) + 'Z'; // without milliseconds
}
```

```ts
export function dateTimeValAsIsoStringOrUndef(val: unknown): string | undefined {
  return val === undefined || val === null ? undefined : dateTimeValAsIsoString(val);
}
```

```ts
export function dateValAsIsoString(val: unknown): string {
  return toDate(val).toISOString().substring(0, 10);
}
```

```ts
export function dateValAsIsoStringOrUndef(val: unknown): string | undefined {
  return val === undefined || val === null ? undefined : dateValAsIsoString(val);
}

```

```ts
export function toDate(val: unknown): Date {
  if (val instanceof Date) return val;
  if (val === undefined || val === null) throw new Error('Missing value for date');
  if (typeof val === 'string' || typeof val === 'number') return new Date(val);
  throw new Error(`Cannot convert the value to date (${typeof val})`);
}
```

```ts
export function prettyPrintedDate(val?: unknown) {
  return val ? toDate(val).toString() : '';
}
```

```ts
export function nbVal(val: unknown): number {
  if (typeof val === 'number') return val;
  if (val === undefined || val === null) throw new Error('Missing number value');
  if (typeof val === 'string' && isNumeric(val)) return Number(val);
  throw new Error(`Cannot convert the value to number '${typeof val}'`);
}

```


```ts
export function nbValOrUndef(val: unknown): number | undefined {
  return val === undefined || val === null ? undefined : nbVal(val);
}

```


```ts
export function dateVal(val: unknown): Date {
  if (val instanceof Date) return val;
  if (val === undefined || val === null || val === '')
    throw new Error('Missing number value');
  if (typeof val === 'string') return new Date(val);
  if (typeof val === 'number') return new Date(val);
  throw new Error(`Cannot convert the value to date '${typeof val}'`);
}

```

```ts
export function dateValOrUndef(val: unknown): Date | undefined {
  return val === undefined || val === null || val === '' ? undefined : dateVal(val);
}

```


```ts
export function listVal<T>(val: unknown, valueFormater: (val: unknown) => T): T[] {
  if (!Array.isArray(val)) throw new Error(`Invalid array '${typeof val}'`);
  return val.map(valueFormater);
}
```

```ts
export function listValOrUndef<T>(
  val: unknown,
  valueFormater: (val: unknown) => T
): T[] | undefined {
  return val === undefined || val === null ? undefined : listVal(val, valueFormater);
}
```

```ts
/**
 * @see https://stackoverflow.com/a/175787/3786294
 */
function isNumeric(str: string): boolean {
  return (
    // use type coercion to parse the _entirety_ of the string (`parseFloat` alone does
    // not do this)...
    !isNaN(str as any as number) && !isNaN(parseFloat(str))
  ); // ... and ensure strings of whitespace fail
}
```


```
