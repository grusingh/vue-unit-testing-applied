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
import authentication from './authentication';

export default {
  authentication,
};
```

### Helper function to create store for unit testing

```javascript
// tests/unit/helpers/mockStore.js - create store for unit testing 
import Vuex from 'vuex';
import modules from '@/modules';
import { mockStoreGetters, mockStoreActions, mockStoreMutations } from 'vue-test-utils-helpers';

export default {
  mock(beforeMockStore) {
    const { getters } = mockStoreGetters({ modules, mockedGetters: {}, jestFn: jest.fn });
    const { mutations } = mockStoreMutations({ modules, jestFn: jest.fn });
    const { actions } = mockStoreActions({ modules, jestFn: jest.fn });
    const mocked = { getters, mutations, actions };

    // hook to allow overrides before store is created
    beforeMockStore && beforeMockStore(mocked);

    const store = new Vuex.Store({
      modules,
      strict: true,
    });

    return {
      store,
      ...mocked,
    };
  },
};
```

### Override `getters` to provide default state

```javascript
// test/unit/helpers/mockedGetters.js - override return value for getters 

export default {
  'isLoggedIn': jest.fn().mockReturnValue(true),
  'name': jest.fn().mockReturnValue('guest')
}
```
