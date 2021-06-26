

### React Life Cycle



아래는 react 라이프사이클을 이해하기 위해 간단히 직접 짜본 코드이다. 실행시켜보면, 어떤 식으로 동작하는지 알 수 있을 것이다.

```javascript


class ParentComp extends React.Component {
  
  static State = {
    click: false,
    text: 'parent',
  };
  
   constructor(props) {
    super(props);
    this.state = {text:'parent'};
  }
 
  componentDidMount() {
    console.log('Parent Component did mount');
  }
  
  componentDidUpdate() {
    console.log('Parent Comp did update');
  }
  
  onClick(text) {
    console.log('onClick!');
    this.setState({ click: true, text: text});
  }
 
  render() {
    return(
      <>
      <button onClick={() => this.onClick('parent').bind(this)}>{this.state.text}</button>
      <SubComp onClick={() => this.onClick('sub')} />
        </>
    );
  }
}

class SubComp extends React.Component {
  
   componentDidMount() {
    console.log('SubComp did mount');
  }
  
  componentDidUpdate() {
    console.log('SubComp did update');
  }
  render() {
   
    return(
      <>
      <br />
      <button onClick={this.props.onClick}>sub</button>
      </>
    );
  }
}

ReactDOM.render(
  <ParentComp />,
  document.getElementById('app')
);
```

