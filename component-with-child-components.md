## Recipe: Unit test component with child components

Stub child component(s). Verify that component can be rendered.  

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
