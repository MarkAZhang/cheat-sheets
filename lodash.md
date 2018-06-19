### Setting state without modifying original object.(without lodash/fp)

```
const newState = _.setWith(_.clone(state), 'users.user01.name.first', 'John Doe', _.clone);
```
