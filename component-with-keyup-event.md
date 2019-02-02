## Recipe: Unit test component with keup event

Test keyup or similar keyboard events inside component. 

```javascript
<template>
    <div data-test="newtodo">
        <input type="text" v-model="title" @keyup.enter="add" data-test="newtodo__title" />
    </div>
</template>

<script>
export default {
  data: () => ({
    title: '',
    status: 'pending',
  }),
  methods: {
    add() {
      this.$emit('add', {
        id: new Date().getTime(),
        title: this.title,
        status: this.status,
      });
      this.title = '';
    },
  },
};
</script>
```

```javascript
import helpers from '#/helpers';
import NewTodo from '@/components/NewTodo.vue';

describe('TodoItem', () => {
  const todoTitle = 'new todo';

  it('should be able to add todo by pressing enter', async () => {
    const { wrapper } = await helpers.shallowMount(NewTodo);
    wrapper.find("[data-test='newtodo__title']").setValue(todoTitle);
    wrapper.find("[data-test='newtodo__title']").trigger('keyup.enter');

    expect(wrapper.emitted().add[0][0].title).toBe(todoTitle);
  });
})
```
