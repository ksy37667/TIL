## useState

 useState를 사용하면 `state 변수`를 선언할 수 있다. 클래스에서의 this.state가 제공하는 기능과 똑같다고 생각하면 된다. 



* 보통의 클래스를 사용할 때
```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        count: 0    
    };
}
```

함수형 컴포넌트는 `this`를 사용할 수 없기 때문에 `this.state`처럼 할당하거나 읽을 수 없습니다. 대신 `useState` 를 사용하면 됩니다.

```javascript
import React, { useState } from 'react';

function Example(){
    const [const, setCount] = useState(0);
}
```

