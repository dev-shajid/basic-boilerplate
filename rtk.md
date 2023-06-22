# React Redux Toolkit

Basic template for redux-toolkit

**1. Install Redux Toolkit and React-Redux:**
```
npm install @reduxjs/toolkit react-redux
```

**2. Create a Redux store: (store.js)**
```javascript
import { combineReducers, configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

const rootReducer = combineReducers({
  counter: counterReducer,
});

export default configureStore({ reducer: rootReducer })
```


**3. Create your Redux reducers: (counterSlice.js)**
```javascript
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  // initial state
};

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
  },
});

export const { increment, decrement } = counterSlice.actions;

export default counterSlice.reducer;
```

**4.Wrap the App in the store Provider**
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from '.redux/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

**5.Use the useSelector and useDispatch hooks to interact with your Redux store: (Component)**
```javascript
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from './counterSlice';

function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}

export default Counter;
```