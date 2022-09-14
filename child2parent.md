```js
import React, { useState } from 'react';

function App() {
  return (
    <div className="App">
      <Parent />
    </div>
  );
};

const Parent = (props) => {
  const [data, setData] = useState("initial data");
  
  return (
    <>
      <div>{data}</div>
      <Child setData={setData} />
    </>
  );
};

const Child = (props) => {
  return (
    <>
      <button onClick={() => props.setData("data from child")}>
        send data to parent
      </button>
    </>
  );
};
```
