## Unit Testing Setup

To keep code DRY we will create a `mount` helper utility function that will setup all application dependencies in unit testable way.

```javascript
// helpers/index.js
import { mount, createLocalVue } from '@vue/test-utils'
import Vuex from 'vuex'
import VueRouter from 'vue-router'
import VueI18n from 'vue-i18n'
import flushPromises from 'flush-promises'
import mockStore from './mockStore'
import mockRouter from './mockRouter'
import './jestMatchers'

function mockVeeValidate (localVue, mocks) {
  localVue.directive('validate', {})
  mocks.errors = {
    has: jest.fn(),
    first: jest.fn(),
    any: jest.fn()
  }

  mocks.$validator = {
    validate: jest.fn(),
    validateAll: jest.fn(),
    reset: jest.fn()
  }
}

function mockI18n (mountOptions, mocks) {
  const i18n = new VueI18n({
    locale: 'en',
    messages: {}
  })
  mountOptions.i18n = i18n
  mocks.$t = (val) => val
}

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

  mockVeeValidate(localVue, mocks)
  mockI18n(mountOptions, mocks)
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

[home](/index.md) | [previous](/vuex-router.md) | [next](/component-with-no-child-components.md)
