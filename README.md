# REDUX-BUCHELI-COURSE-IMMUTABLE-UPDATES---COMPLEX-STATE

## REDUX-LESSONS-INSTRUCTOR-ANDRES-R.-BUCHELI

## Usage:

Now that you have defined which changes can occur to your application’s state, you need a reducer to execute those changes.

Remember, the store‘s reducer function is called each time an action is dispatched. It is passed the action and the current state as arguments and returns the store‘s next
state. 

The second rule of reducers states that when the reducer is updating the state, it must make a copy and return the copy rather than directly mutating the incoming state. When
the state is a mutable data type, like an array or object, this is typically done using the spread operator (...).

Below, the todosReducer for a todo app demonstrates this in action:

```
const initialState = {
  filter: 'SHOW_INCOMPLETE',
  todos: [
    { id: 0, text: 'learn redux', completed: false },
    { id: 1, text: 'build a redux app', completed: true },
    { id: 2, text: 'do a dance', completed: false },
  ]
};
 
const todosReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'filter/setFilter':
      return {
        ...state,
        filter: action.payload
      };
    case 'todos/addTodo': 
      return {
        ...state,
        todos: [...state.todos, action.payload]
      } ;
    case 'todos/toggleTodo':
      return {
        ...state,
        todos: state.todos.map(todo => {
          return (todo.id === action.payload.id) ? 
            { ...todo, completed: !todo.completed } : 
            todo;
        })
      }
    default:
      return state;
  }
};
```

* The todosReducer uses the initialState as the default state value.
* When a 'filter/setFilter' action is received, it spreads the old state‘s contents (...state) into a new object before updating the filter property with the new filter from
action.payload.
* When a 'todos/addTodo' action is received, it does the same except this time, since state.todos is a mutable array, its contents are also spread into a new array, with the
new todo from action.payload added to the end.
* When a 'todos/toggleTodo action is received, it uses the .map() method to create a copy of the state.todos array. Additionally, the todo being toggled is found using
action.payload.id and it is spread into a new object and updated.

It should be clarified that the state.todos.map() method only makes a “shallow” copy of the array, meaning the objects inside share the same references as the originals. 
Therefore, mutations to the objects within the copy will affect the objects within the original. For now, we can make do with this solution, but you will learn how to bypass 
this issue in a later lesson on the Redux Toolkit.

Now, let’s create a reducer for the Recipes app! In the store.js file, after the initialState and your action creators, you can see that this function has already been started 
for you. In the output terminal, you will see the results of printTests() which dispatch some actions to the store. Your task is to complete it such that it can handle each of 
the five action creator types that you had created in the last exercise.
