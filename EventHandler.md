```js

class LoggingButton extends React.Component {
  handleClick = () => {
    console.log('this is: ', this);
  }
  
  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

```js
class LoggingButton extends React.Compoennt (
  handleClick() {
    console.log('this is: ', this);
  }
  
  render() {
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
```

#### Event Handler에 인자 전달하기
```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

```js
const clickHandler = ( params, e) => {
  console.log(params);
  e.preventDefault();
  // do something...
}

return (
  <button onClick={(e)=>{clickHandler(params, e)}}> Do Something!</button>
)
```
