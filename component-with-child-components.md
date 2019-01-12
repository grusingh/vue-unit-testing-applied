## Recipe: Unit test component with child components

Stub child component(s). Verify that component can be rendered.  

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
import helpers from '#/helpers';
import TodoList from '@/components/TodoList.vue';

describe('TodoList.vue', () => {
  const todos = [
    { id: '1', title: 'test-todo1', status: 'pending' },
    { id: '2', title: 'test-todo2', status: 'pending' }
  ];

  it('should be able to render', async () => {
    const { wrapper } = await helpers.mount(TodoList, {
      propsData: {
        todos,
      },
      stubs: ['TodoItem']
    });
    
    const todoItemsWrapper = wrapper.findAll("[data-test='todolist__item']");
    
    expect(wrapper.attributes('data-test')).toBe('todolist');
    expect(todoItemsWrapper.length).toBe(todos.length);
  });
});
```
