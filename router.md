### Handling Vue Router
This recipe utilizes routes of application under test and enables assertions on navigation redirects. 

Steps to mock Vue Router:
* Separate router creation from route declarations
* Helper function to create router for unit testing

### Separate router creation from route declarations
This separation will enable helper functions in `vue-test-utils-helper` library to discover and mock all routes.  

### Helper function to create router for unit testing

```javascript
// tests/unit/helpers/mockRouter.js - create router for unit testing
import routes from '@/routes'
import VueRouter from 'vue-router'
import { mockRouterComponents } from 'vue-test-utils-helpers'

export default {
  mock () {
    const clearedRoutes = mockRouterComponents(routes)
    return new VueRouter({
      mode: 'abstract',
      routes: clearedRoutes
    })
  }
}
```
