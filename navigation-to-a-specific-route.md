## Recipe: Unit test navigation to a specific route

Verify that workflow redirects to expected route.

```javascript
<template>
  <li data-test="todoitem" @click="navigateToDetail">
    <span class="todoitem__title" data-test="todoitem__title">{{ todo.title }}</span>
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
  methods: {
    navigateToDetail() {
      this.$router.push({ name: 'todos-detail', params: { todo: this.todo } });
    },
  },
};
</script>
```

```javascript
import helpers from '#/helpers';
import TodoItem from '@/components/TodoItem.vue';

describe('TodoItem.vue', () => {
  it('should redirect to todos-detail when an item is clicked', async () => {
    const { wrapper } = await helpers.mount(TodoItem, {
      propsData: {
        todo: {
          title: 'test-title',
        },
      },
    });
    wrapper.find('[data-test="todoitem"]').trigger('click');
    expect(wrapper).toHaveRouteName('todos-detail');
  });
});
```
