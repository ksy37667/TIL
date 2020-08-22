## 🔍React의 이벤트 처리(Event Handling)

회원가입과 게시판 API를 다 만들었는데 프론트도 혼자 만들어보고 싶어서 React를 공부하던 중 이해가 되지 않는 부분이 있어서 정리하고 넘어가려고 한다.

`this.sayHello.bind(this)` 바로 이녀석인데 우선 전체 소스코드를 보자.

``` javascript
var obj = {
  prop: 'Hello',
  sayHello: function() {
    console.log(this.prop);
  }
};
obj.sayHello(); 
//  "Hello"
```

여기까진 쉽게 이해할 수 있었다.    
`obj` 의 `sayHello()`를 불러와서 `this.prop`를 출력하는 평범한 코드이다.

그렇다면 다른 방식으로 출력해보자

```javascript
var sayHello2 = obj.sayHello;
sayHello2();

//  "undefined
```

여기서 잠깐 막혔다. 이렇게 되는 이유를 찾아보니 `sayHello2` 에 obj.sayHello가 선언될때 obj와의 관계가 상실되기 때문이라고 한다.

```javascript
var sayHello2 = obj.sayHello.bind(obj);
sayHello2

// "Hello"
```

이렇게하면 정상적으로 출력되는 것을 볼 수 있다.
내가 이해한대로 풀어서 설명해보면 이 코드에서의 `bind()`의 역할은 `sayHello`와 `obj`의 상실된 관계를 다시 묶어주어서 다시 `this` 함수를 작동할 수 있게 해준다.(우선 내가 이해한대로 적어보자면 이렇다.)


### 🔍React에서의 바인딩
* React에서의 바인딩도 결국 JavaScript이기 때문에 원리는 비슷하다.

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
      this.handleClick = this.handleClick.bind(this);
  }

  handleClick (){
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

* 여기선 JSX 문법 안에서 `this`를 주의해야 한다.
JavaScript에서 클래스 메서드는 기본적으로 바인딩이 되어있지 않기 때문에 `this.handleClick`을 바인딩하지 않고 `onClick`에 전달하였다면, `this`는 `undefined`가 된다.

* 그렇기 때문에 컴포넌트의 생성자에서 `this.handleClick = this.handleClick.bind(this);` 이런식으로 바인딩을 해주어야 한다.

* 그 외에 2가지 방법이 더 있다.
```javascript
  handleClick = () => {    
    console.log('this is:', this);  
  }
```
와 같이 `handleClick = () => {.....}` 바인딩을 할 수 있다.


```javascript
render() {
  return (      
    <button onClick={() => this.handleClick()}>         
    Click me
    </button>
);
```
* `arrow function`을 사용할 수도 있지만 이렇게 된다면 클래스가 랜더링 될 때마다 다른 콜백이 생성되는 문제가 있다. 예를 들어 콜백이 하위 컴포넌트에 props로서 전달된다면 그 컴포넌트들은 추가로 다시 렌더링을 수행할 수도 있다. 즉, 성능상 문제가 발생할 수 있기 때문에 생성자 안에서 바인딩을 하거나 클래스 필드 문법을 사용하는 것을 권장한다고 한다.


## 🔍 참고 문서
[이벤트 처리하기](https://ko.reactjs.org/docs/handling-events.html)