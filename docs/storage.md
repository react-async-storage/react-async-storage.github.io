---
id: storage
title: Storage API Reference
hide_title: true
sidebar_label: Storage API
slug: /storage/
---

# StorageWrapper

The storage wrapper is the main class with which the user interacts in this library.

## `hasItem`

Checks whether a given key exists in storage. This method is _synchroneous_.

<br />

**Signature**:

```ts
storage.hasItem(key: string): boolean
```

## `getItem`

Retrieves a value from storage. The value is automatically deserialized into a javascript value, so you do not need to call `JSON.parse()`.

**Signature**:

```ts
storage.getItem<T>(
    key: string,
    options?: {
        fallback?: T
        allowNull?: boolean
    },
    callback?: (error: Error | null, result?: T | null) => void
): Promise<T | null>
```

**Parameters**

`key`: The key of the value being retrieved (**required**)

`options`: The second parameter is an optional object that has two optional keys:

- `fallback`: By default `getItem`returns `null` when no value is found for the given key. You can customize this by passing an optional fallback value as part of the options object.
- `allowNull`: By default `getItem` will not throw an error if a value is not found for the given key and no fallback is provided. To change this, you can set `allowNull` to `true`, which will cause the function to throw a `CacheError` if no value is found.

`callback`: Node style callback.

<br />

## `setItem`

Sets a value in storage. The value is automatically serialized, so you do not need to call `JSON.stringify()`.

> Note: If you want to serialized functions or classes with methods you will need to serialized them before calling setItem, because these cannot be serialized by the library.

**Signature**:

```ts
storage.setItem<T>(
    key: string,
    value: () => T | T,
    maxAge?: number | [number, TimeUnit],
    callback?: (error: Error | null, result?: CacheRecord<T>) => void,
): Promise<void>
```

**Parameters**

`key`: The key to set for the given value (**required**)

`value`: The value that is being stored. This can be either a value or a function that returns the given value. (**required**)

`maxAge`: An optional maximum ttl after which the value will become stale. You can set this to either a millisecond value, or pass in an array in a more human readable format, e.g.:

```ts
storage.setItem("key", {}, [1, "day"]);
```

The first value of the array is a number representing an amount, the second value is a time-unit, with the accepted values being:

```ts
type TimeUnit =
  | "second"
  | "minute"
  | "hour"
  | "day"
  | "week"
  | "month"
  | "year";
```

`callback`: Node style callback. _NOTE_ the value being passed to the callback for setItem is not the raw value but rather an instance of `CacheRecord`.

<br />

## `removeItem`

Removes a value from storage.

**Signature**:

```ts
storage.removeItem(
    key: string,
    callback?: (error: Error) => void,
): Promise<void>
```

**Parameters**

`key`: The key of the value to remove (**required**)

`callback`: Node style callback.

<br />

## `mergeItem`

Merges a stored value with the value passed in. This method uses `Lodash Merge` to merge values.

**Signature**:

```ts
storage.mergeItem<T>(
    key: string,
    value: T,
    callback?: (error: Error | null, result?: T) => void,
): Promise<void>
```

**Parameters**

`key`: The key of the value to merge (**required**)

`value`: The value to merge with the stored value (**required**)

`callback`: Node style callback.

<br />

## `multiGet`

Convenience method to batch retrieve values from storage.

The values are returned as a 3-dimensional array (array of arrays), with the first value in the array being the key and the second value being the stored value.

**Signature**:

```ts
storage.multiGet(
    keys: string[],
): Promise<[string, any][]>
```

**Parameters**

`keys`: An array of keys of values to retrieve (**required**)

<br />

## `multiSet`

Convenience method to batch set values in storage.

**Signature**:

```ts
storage.multiSet(
    values: {
        key: string
        value: any
        maxAge?: MaxAge
    }[],
): Promise<void>
```

**Parameters**

`values`: An array of objects, each representing a value to be stored:

- `key`: The key to set for the given value (**required**)

- `value`: The value that is being stored. This can be either a value or a function that returns the given value. (**required**)

- `maxAge`: An optional maximum ttl after which the value will become stale. See the entry for [setItem](#/setItem) for details

<br />

## `multiRemove`

Convenience method to batch removes values from storage.

**Signature**:

```ts
storage.multiRemove(
    keys: string[],
): Promise<void>
```

**Parameters**

`keys`: An array of keys of values to be removed (**required**)

<br />

## `multiMerge`

Convenience method to batch merge values.

Returns a 3-dimensional array (array of arrays), with the first value in the array being the key and the second value being the merged result.

**Signature**:

```ts
storage.multiMerge(
    values: {
        key: string
        value: any
    }[],
): Promise<[string, any][]>
```

**Parameters**

`values`: An array of objects, each representing a value to be merged:

- `key`: The key of the value to be merged with (**required**)

- `value`: The value to merged with the stored value. (**required**)

<br />

## `clear`

Clears all keys in the given store.

> Note: only the store namespace is cleared. If you are using multiple stores, only the one on which you call `clear` will be affected.

**Signature**:

```ts
storage.clear(): Promise<void>
```

<br />

## `keys`

Returns all the keys in the store.

> Note: keys are retrieved only for the given store's namespace.

**Signature**:

```ts
storage.keys(): Promise<string[]>
```

<br />
