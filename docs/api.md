---
id: api
title: Api Reference
sidebar_label: Api Reference
slug: /api/
---

# StorageWrapper

The storage wrapper is the main class with which the user interacts in this library. Its a high level abstraction on top of localForage which adds extra functionalities. By and large the methods of the `StorageWrapper` are similar to those of `localForage` and to an extent - `React-Native AsyncStorage`. 

## `getItem`

Retrieves a value from storage. The value is automatically deserialized into a javascript value, so you do not need to call `JSON.parse()`. 

**Signature**:

```ts
getItem<T>(
    key: string, 
    options?: {
        fallback?: T
        allowNull?: boolean
    },
    callback?: (error: Error, result: T | null) => void
): Promise<T | null>
```

**Parameters**

`key`: The key of the value being retrieved (**required**)

`options`: The second parameter is an optional object that has two optional keys: 
- `fallback`: By default `getItem`returns `null` when no value is found for the given key. You can customize this by passing an optional fallback value as part of the options object.
- `allowNull`: By default `getItem` will not throw an error if a value is not found for the given key and no fallback is provided. To change this, you can set `allowNull` to `true`, which will cause the function to throw a `CacheError` if no value is found. 

`callback`: For legacy compatibility - you can pass a node style callback. 

<br />
<br />

## `setItem`

Sets a value in storage. The value is automatically serialized, so you do not need to call `JSON.stringify()`. 
 
_NOTE_: If you want to serialized functions or classes with methods you will need to serialized them before calling setItem, because these cannot be serialized by the library.

**Signature**:

```ts
setItem<T>(
    key: string,
    value: () => T | T,
    maxAge?: number | [number, TimeUnit],
    callback?: (error: Error, result: CacheRecord<T>) => void,
): Promise<void>
```

**Options**

`fallback`: By default `getItem`returns `null` when no value is found for the given key. You can customize this by passing an optional fallback value as part of the options object.

`allowNull`: By default `getItem` will not throw an error if a value is not found for the given key. To change this, you can set `allowNull` to `true`, which will cause the function to throw a `CacheError` if no value is found. 

_NOTE_ if a `fallback` is provided no error will be thrown and the fallback value will be returned. 

`preferCache`: By default the in-memory cache is prefered. Setting `preferCache` to `false`, will cause `getItem` to retrieve the value directly from storage. 

<br />
<br />