## Project specific mount function
Every project has a specific set of dependencies. It is recommended to create a project specific `mount` function that will setup all these dependencies for unit testing and thus help in keeping code DRY.  


```javascript
// helpers/index.js
import { mount, shallowMount, createLocalVue } from '@vue/test-utils';
import Vuex from 'vuex';
import VueRouter from 'vue-router';
import flushPromises from 'flush-promises';
import mockStore from './mockStore';
import mockRouter from './mockRouter';

async function mountComponent(factory, component, options = {}, mockHooks = {}) {
  const localVue = createLocalVue();
  localVue.use(Vuex);

  const { beforeMockStore } = mockHooks;
  const { store, ...mockedStoreReferences } = mockStore.mock(beforeMockStore);
  const mountOptions = {
    ...options,
    store,
    localVue
  };

  if (!(options.mocks && options.mocks.$route)) {
    localVue.use(VueRouter);
    mountOptions.router = mockRouter.mock();
  }

  const wrapper = factory(component, mountOptions);
  await flushPromises();

  return {
    wrapper,
    store,
    ...mockedStoreReferences // actions, getters, mutations
  };
}

export default {
  mount: (...args) => mountComponent(mount, ...args),
  shallowMount: (...args) => mountComponent(shallowMount, ...args),
};
```

