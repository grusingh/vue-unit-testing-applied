## Unit test component with no child components

Verify that component can be rendered. 

```javascript
<template>
  <li data-test="todoitem">
    <span data-test="todoitem__title">{{ todo.title }}</span>
  </li>
</template>

<script>
export default {
  props: {
    todo: {
      type: Object,
      required: true,
    },
  },
};
</script>
```

```javascript
import helpers from '#/helpers'
import TodoItem from '@/components/TodoItem.vue'

describe('TodoItem', () => {
  it('should be able to render', async () => {
    const { wrapper } = await helpers.mount(TodoItem, {
      propsData: {
        todo: {
          title: 'test-title',
        },
      },
    });
    expect(wrapper.attributes('data-test')).toBe('todoitem');
    expect(wrapper.find('[data-test="todoitem__title"]').text()).toBe('test-title');
  })
})
```
