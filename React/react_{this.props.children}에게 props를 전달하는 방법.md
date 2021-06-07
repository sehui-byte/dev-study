

### How to pass props to children

이에 대해 stackoverflow에 대해 설명되어있다.

- https://stackoverflow.com/questions/32370994/how-to-pass-props-to-this-props-children



1. class component 인 경우

```javascript
 render() {
    const { children } = this.props;
    const childrenWithProps = React.Children.map(children, child => {
      if (React.isValidElement(child)) {
        return React.cloneElement(child, { ...this.state, onChange: this.onChange });
      }
      return child;
    });
    return childrenWithProps;
  }
}
```



2. functional component인 경우

만약 `functional component`일 경우 parameter로 넘길 수 있다.