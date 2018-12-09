## Unit test component with no child components

Verify that component can be mounted by passing in only required props. 

```javascript
Vue.component('TodoItem', {
  template: '<div data-qa="todoapp-todoitem">{{ todo.name }}</div>',
  props: ['todo']
})
```

```javascript
import helpers from '#/helpers'
import TodoItem from '@/components/TodoItem.vue'

describe('TodoItem', () => {
  it('mount', async () => {
    const { wrapper } = await helpers.mount(TodoItem, {
      propsData: {
        todo: {}
      }
    })
    expect(wrapper.attributes('data-qa')).toBe('todoapp-todoitem')
  })
})
```
