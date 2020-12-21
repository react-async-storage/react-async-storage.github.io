---
id: configuration
title: Configuration
sidebar_label: Configuration
slug: /configuration/
---

```ts
interface StorageOptions {
    allowStale?: boolean // default: false
    description?: string // default: undefined
    driver?: string | string[] // default: undefined, for ReactNative its set internally
    namespace?: string // default: "ReactAsyncStorage"
    preferCache?: boolean // default: true
    size?: number // default undefined
    storeName?: string // default: "defaultStore"
    version?: string // default: "1.0.0"
}
```

**Options**:

`allowStale`: set this to true to not automatically prune stale records.

`description`: add a description to the store. You can see it in the application tab of the browser.

`driver`: set the driver for localForage, for more details see the [localForage documentation](https://localforage.github.io/localForage/#settings-api-config). This option is ignored in react-native.

`namespace`: the namespace used by storage - only the first value passed will be used (i.e. you can set namespace only one.) Defaults to `ReactAsyncStorage`.

`preferCache`: whether or not to use in-memory caching (memoization), which increases performance. Defaults to true.

`size`: limit the phsical size allocated to the given store. For more details see the [localForage documentation](https://localforage.github.io/localForage/#settings-api-config)

`storeName`: the namespace used by the particular Storage instance. This option has to be set if you are creating multiple instances. Otherwise it defaults to `defaultStore`

`version`: version of the storage instance.
