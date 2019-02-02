## Recipe: Unit test component with router link

When using `vue-test-utils` library in this scenario we will have to stub [router-link](https://vue-test-utils.vuejs.org/guides/using-with-vue-router.html#testing-components-that-use-router-link-or-router-view). Because in `mockRouter.js` we are using an instance of VueRouter and real application routes, no extra setup is required for this test. 

```javascript
<template>
  <div class="about" data-test="about">
    <h1>Coded with love by <a href="">@sarngru</a></h1>
    <router-link :to="{ name: 'todos' }">Back to Todos</router-link>
  </div>
</template>
```

```javascript
import helpers from '#/helpers';
import About from '@/views/About.vue';

describe('About.vue', () => {
  it('should be able to render', async () => {
    const { wrapper } = await helpers.mount(About);
    expect(wrapper.attributes('data-test')).toBe('about');
  });
});
```
