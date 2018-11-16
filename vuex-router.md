## Handling Vuex and Vue-Router

Vuex and Vue-Router make it harder to unit test components that rely on them. We will be using utility functions to make it easier to work with these libraries.

### Vuex
We will be using full blown Vuex store but will mock out all the actions and getters. To keep code DRY will piggy backing on actual store withing the application.

```javascript
// src/modules/index.js - imports all store modules and used when creating store
import storeModule1 from './storeModule1'
import storeModule2 from './storeModule2'

export default
{
  storeModule1,
  storeModule2,
}
```

```javascript
// mockStore.js - helper utility method to create store for unit testing 
import Vuex from 'vuex'
import modules from '@/modules'
import mockedGetters from './mockedGetters'
import { mockStoreActionsAndGetters } from './mockStoreActionsAndGetters'

export default {
  mock (beforeMockStore) {
    const mocked = mockStoreActionsAndGetters({
      modules,
      mockedGetters,
      jestFn: jest.fn
    })

    beforeMockStore && beforeMockStore(mocked)

    const store = new Vuex.Store({
      modules,
      strict: true
    })

    return {
      store,
      ...mocked
    }
  }
}
```

```javascript
// mockStoreActionsAndGetters.js - utility method that mocks out all actions and getters
function mockStoreActionsAndGetters({
    modules: modulesDictionary,
    mockedGetters, // object containing default values for store getters
    jestFn
}) {
    let actionsDictionary = {}
    let gettersDictionary = {}

    Object.keys(modulesDictionary).forEach(moduleKey => {
        const moduleValue = modulesDictionary[moduleKey]
        const isNamespaced = Object.keys(moduleValue).includes('namespaced')

        Object.keys(moduleValue.actions).forEach(key => {
            const mockActionKey = isNamespaced ? `${moduleKey}/${key}` : key
            const mockFn = jestFn().mockResolvedValue({})
            actionsDictionary[mockActionKey] = mockFn
            moduleValue.actions[key] = mockFn
        })

        Object.keys(moduleValue.getters).forEach(key => {
            const mockedGetterKey = isNamespaced ? `${moduleKey}/${key}` : key
            let mockFn = jestFn()
            // use custom mock functions when available
            if (Object.keys(mockedGetters).includes(mockedGetterKey)) {
                mockFn = mockedGetters[mockedGetterKey]
            }
            gettersDictionary[mockedGetterKey] = mockFn
            moduleValue.getters[key] = mockFn
        })
    })

    // Vuex mutates the passed in actions when new store is created, therefore we preserve reference to actions & getters objects.
    return {
        actions: actionsDictionary,
        getters: gettersDictionary
    }
}

export { mockStoreActionsAndGetters }
```

```javascript
// mockedGetters.js - object containing default values for store getters
export default {
  loading: jest.fn().mockReturnValue(false),
  'namespace/someGetter': jest.fn().mockReturnValue('')
}
```


