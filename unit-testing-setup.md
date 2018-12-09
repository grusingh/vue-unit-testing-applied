## Project specific mount function
Each project has a specific set of requirements and dependencies. Therefore its recommended to create a project specific `mount` function that will setup all these dependencies for unit testing and help in keeping code DRY.  


```javascript
// helpers/index.js
import { mount, createLocalVue } from '@vue/test-utils'
import Vuex from 'vuex'
import VueRouter from 'vue-router'
import flushPromises from 'flush-promises'
import mockStore from './mockStore'
import mockRouter from './mockRouter'
import './jestMatchers'

function mockVueRouter (mountOptions, localVue, mocks) {
  if (!mocks.$route) {
    localVue.use(VueRouter)
    mountOptions.router = mockRouter.mock()
  }
}

async function mountComponent (component, options = {}, mockHooks = {}) {
  const localVue = createLocalVue()
  localVue.use(Vuex)

  const { beforeMockStore } = mockHooks
  const { store, actions, getters } = mockStore.mock(beforeMockStore)
  const mocks = {
    ...options.mocks
  }
  const mountOptions = {
    ...options,
    store,
    localVue,
    mocks
  }

  mockVueRouter(mountOptions, localVue, mocks)

  const wrapper = mount(component, mountOptions)
  await flushPromises()

  return {
    wrapper,
    actions,
    getters
  }
}

export default {
  mount: mountComponent
}
```

