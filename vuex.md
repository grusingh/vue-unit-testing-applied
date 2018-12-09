## Handling Vuex

This recipe utilizes store modules of application under test and mocks out their actions, mutations and getters. 

Steps to mock Vuex store:
* Separate store creation from store modules declarations
* Helper function to create store for unit testing
* Override `getters` to provide default state

### Separate store creation from store modules declarations
This separation will enable helper functions in `vue-test-utils-helper` library to discover, and mock all actions, mutations and getters.  

```javascript
// src/modules/index.js - imports all store modules in one central place
import storeModule1 from './storeModule1'
import storeModule2 from './storeModule2'

export default
{
  storeModule1,
  storeModule2,
}
```

### Helper function to create store for unit testing

```javascript
// tests/unit/helpers/mockStore.js - create store for unit testing 
import Vuex from 'vuex'
import modules from '@/modules'
import mockedGetters from './mockedGetters'
import { mockStoreActionsAndGetters, mockStoreMutations } from 'vue-test-utils-helpers'

export default {
  mock (beforeMockStore) {  
    const mocked = mockStoreActionsAndGetters({
      modules,
      mockedGetters,
      jestFn: jest.fn
    })

    const { mutations } = mockStoreMutations({ modules, jestFn: jest.fn })

    // hook to allow overrides before store is created
    beforeMockStore && beforeMockStore(mocked)

    const store = new Vuex.Store({
      modules,
      strict: true
    })

    // return store and references to mocked actions, getters and mutations
    return {
      store,
      ...mocked,
      mutations
    }
  }
}
```

### Override `getters` to provide default state

```javascript
// test/unit/helpers/mockedGetters.js - override return value for getters 

export default {
  isAuthenticated: jest.fn().mockReturnValue(true),
  'auth/loggedInUser': jest.fn().mockReturnValue({
    name: 'foo',
    role: 'admin'
  })
}
```