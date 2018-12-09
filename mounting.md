## How to mount a component

[Vue-test-utils](https://vue-test-utils.vuejs.org/api/options.html#mounting-options) provides two options to mount a component for unit testing.

* [mount](https://vue-test-utils.vuejs.org/api/#mount)
* [shallowMount](https://vue-test-utils.vuejs.org/api/#shallowmount)

The difference between these two options is that `shallowMount` will stub all child components, where as `mount` will render all child components. 

**Note:** If you are using a Vue UI component library such as Vuetify in your application then `shallowMount` will stub out all UI components. In this case you can use `mount` and pass it options to stub out any child components.

```javascript
const wrapper = mount('Component',
  { 
   stubs: [ 'ChildComponentName' ] 
  }
)
```
