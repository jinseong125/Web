JSX(JavaScript XML)란?
* XML/HTML 문법을 JavaScript 안에 직접 쓸 수 있게 해주는 문법 확장.
* 브라우저가 직접 읽는 건 아니고, Babel 같은 트랜스파일러가 React.createElement() 호출로 변환해 준다.

#### JSX를 사용하는이유
* 가독성: 컴포넌트의 구조를 HTML처럼 한눈에 보기 쉽고,
* 표현력: JavaScript 변수나 함수 호출 결과를 {}안에 바로 삽입가능.
* 안전성: 텍스트 노드나 속성 값에 자동으로 이스케이프 처리가 되어 XSS
          공격 위험이 줄어든다.
```
rendering
ReactDOM.createRoot(document.getElementById('...')).render(...);          


<div id="root1"></div>

<script type="text/babel">
  // ① type="text/babel":
  //    브라우저가 아닌 Babel(트랜스파일러)이 이 스크립트를 JSX → 순수 JS로 변환하도록 지시합니다.

  //----- JSX를 이용한 Element 만들기
  // ② JSX 문법으로 <h1> 요소를 생성하여 h1Element 변수에 담습니다.
  const h1Element = <h1>Hello World</h1>;

  // ④ .render(h1Element):
  //    앞서 만든 h1Element(JSX)를 실제 DOM에 그려 줍니다.
  ReactDOM.createRoot(document.getElementById('root1')).render(h1Element);
</script>
```
```
여러 태그를 단일태그로 생성하기 - <React.Fragment></React.Fragment>로 가능하지만,
<></>로 줄여서 사용 가능
예시 코드
div id="root2"></div>
<script type="text/babel">

 //----- 여러 태그를 단일 태그로 생성하기
 const divElement = <React.Fragment>
   <div>Hello World</div>
   <div>Nice to meet you</div>
  </React.Fragment>

  // <React.Fragment>의 축약버전
    const pElement = <>
    <p>Hello World</p>
    <p>Nice to meet you</p>
    </>
```
중괄호{}를 이용한 표현식/ 주석 작성하는 법.
1) // 태그 내부의 주석은 중괄호를 사용하지 않는다.(주로 시작 태그에 주석을 답니다.) 
2) {/*태그 외부의 주석은 중괄호를 사용해야 한다.*/}
3) {
    // 슬래시 2개 주석 (single line comment)은 중괄호와 다른 줄에 작성해야한다.
  }  
셀프 클로징 태그란? (*반드시 뒤에 슬래시(/)를 추가해야함.)
JSX를 쓸 때는 “내용 없으면 무조건 <… />” 
내용이 있으면 <Tag>…</Tag>, 없으면 <Tag />로 작성 가능.
```
삼항 연산자: 조건 ? 참문장 : 거짓문장
JSX 안에 태그도 넣을 수 있고, 변수 할당도 가능
논리 연산자(&&, ||)와 용도는 비슷하지만, 둘 중 하나만 확실하게 렌더링할 땐 삼항이 더 직관적
너무 복잡해지면 if/else나 별도 함수로 빼서 가독성 유지하기.
```

#### 객체 리터럴 형식으로 스타일 전달하기.
JSX에서 인라인 스타일을 객체 리터럴로 전달하는 법은
* style 속성에 중괄호 두번{{...}} 사용하기
- 첫 번째 중괄호{}: JSX에서 JS표현식 사용
- 두 번째 중괄호{}: JS 객체 리터럴

* 클래스형 컴포넌트 만들기: 컴포넌트 기능을 활용하기 위해서 React.component를 상속받도록 만든다.
```
<div id="root1"></div>
<script type="text/babel">

 class ClassComp extends React.Component {
  클래스형 컴포넌트 정의: React.Component를 상속받아 만듭니다.

  render() {
   render메소드: 이 컴포넌트가 화면에 그려줄 JSX를 반환하는 함수.

    return <h1>Hello Class Component</h1>
    JSX 반환: 태그안에 "Hello Class Component"라는 텍스트를 렌더링.
  }
  }
  //----- rendering: 컴포넌트는 태그 형식으로 렌더링합니다.
  ReactDOM.createRoot(document.getElementById('root1')).render(<ClassComp/>);
</script>
```
* 함수형 컴포넌트 만들기: 함수 선언식, 함수 
표현식 모두 사용 가능.
```
const FunctionComp = () => {
  // props가 필요없으면 매개변수 빈 괄호만 사용가능

  return (
    // 컴포넌트가 화면에 렌더링할 JSX를 return 한다.
    <>
    <div>Hello Function Component</div>
    <div>Nice to meet you</div>
    </> //여러요소를 입력해서 사용할때는 Fragment(<></>를 사용)
  )
}
// rendering
ReactDOM.createRoot(document.getElementById('root2')).render(<FunctionComp/>);
</script>
```

#### defaultProps
 컴포넌트에 전달된 props가 없거나 undefined일 때 기본으로
사용할 값을 정의해주는 기능.
```
props.children은 컴포넌트의 안쪽에 들어온 콘텐츠를 꺼내서 렌더링해 주는 기능.
const GiftCard1 = (props) => {
  return (
    <h1>{props.children}</h1>
  )
}

const GiftCard2 = (props) => {
  const {children} = props;
  return (
    <h1>{children}</h1>
  )
}

const GiftCard3 = ({children}) => {
  return (
    <h1>{children}</h1>
  )
}

ReactDOM.createRoot(document.getElementById('root3')).render([
  <GiftCard1>3만원</GiftCard1>,
  <GiftCard2>5만원</GiftCard2>,
  <GiftCard3><em>10만원</em></GiftCard3>,
])

</script>
```
#### props drilling은 하위 컴포넌트에 값을 연속해서 전달하는 방식입니다.
 좋은 방식이 아니므로 권장하지 않음.

#### key
- 리스트를 렌더링할 때 각 요소를 고유하게 식별하기 위해 사용하는 값.

- key로 index(i)로 쓰는게 가능은 하지만 추천은 X
 이유는 항목의 순서가 바뀌거나 삭제되면 리렌더링이 비효율적이 될 수 있음.

 #### key props
 - 리스트 렌더링 시 각 항목을 고유하게 식별하기 위해 key props사용.
 -key가 없어도 렌더링은 가능..

#### React event
- 사용자와의 상호작용(클릭, 입력, 키보드 입력 등 )을 처리 하기 위한 시스템.
- DOM의 이벤트를 리액트 문법에 맞게 처리한 것.

#### 일반 DOM (일반 HTML) 문법 
- <button onclick="handle()"> 
                    이름 - 소문자 (onclick)
#### React Event (리액트) 문법 
- <button onClick={handle}>    
                    이름 - 카멜표기법(onClick)

#### e.preventDefault()
- 기본 동작(브라우저가 자동으로 처리하는 행동)을 막는 메소드.
- <form>안에서 데이터만 JS로 처리할 때 자주 사용.

#### e.stopPropagation()
- 이벤트가 상위(부모) 요소로 전달되는 걸 막는 함수.

이벤트 처리 방법 - 리액트에서는 인라인이벤트 모델 사용

#### state
- 컴포넌트가 기억하고 관리해야 할 데이터.

#### state를 사용해야 할때.
- 버튼 누르면 숫자 증가.
- 텍스트 입력하면 상태 업데이트

```
<div id="root1"></div>
<script type="text/babel">

  //  클래스형 컴포넌트 정의
  class Spin1 extends React.Component {

    //  state 선언: 상태값 number를 0으로 초기화
    state = {
      number: 0,
    }

    // 🔺 증가 버튼 클릭 시 실행될 핸들러 함수
    increaseHandler = () => {
      this.setState({
        number: this.state.number + 1  // 기존 값에 +1
      })
    }

    // 🔻 감소 버튼 클릭 시 실행될 핸들러 함수
    decreaseHandler = () => {
      this.setState({
        number: this.state.number - 1  // 기존 값에 -1
      })
    }

    // 화면에 보여질 내용을 정의
    render() {
      const { number } = this.state;  // 구조 분해 할당으로 state 값 가져오기

      return (
        <>
          {/* 현재 숫자를 표시하는 부분, 숫자 값에 따라 색상 변경 */}
          <h1 style={{ color: number === 0 ? 'black' : number > 0 ? 'red' : 'blue' }}>
            {number}
          </h1>

          {/* ▲ 버튼 클릭 시 increaseHandler 실행 */}
          <button onClick={this.increaseHandler}>▲</button>

          {/* ▼ 버튼 클릭 시 decreaseHandler 실행 */}
          <button onClick={this.decreaseHandler}>▼</button>
        </>
      )
    }
  }

  //  Spin1 컴포넌트를 root1 위치에 렌더링
  ReactDOM.createRoot(document.getElementById('root1')).render(<Spin1 />);
</script>
```

#### Life Cycle
-크게 3단계 (마운트, 업데이트, 언마운트)로 나뉨
1. 마운트(Mount) - 컴포넌트가 처음 생성될 때
 - constructor() -> 처음 생성자 호출
 - render(     ) -> JSX 그리기
 - componentDidMount -> 화면에 나타난 뒤 실행됨.

2. 업데이트(Update) - props나 state가 바뀔 때
- shouldComponentUpdate() -> 리렌더링 할지 말지 결정 (최적화)
- render() -> 변경된 내용 반영
- componentDidUpdate -> 업데이트 완료 후 실행됨

3. 언마운트(Unmount) - 컴포넌트가 화면에서 사라질 때
- componentWillUnmount() = 타이머 제거, 이벤트 제거 등 정리 작업에 사용 
```
<div id="root13"></div>
<script type="text/babel">
  // MultiSwitch 컴포넌트 선언
  const MultiSwitch = () => {
    // 1 switches 상태 선언
    //    - 세 개의 스위치 상태를 담은 배열([false, false, false])로 초기화
    //    - false = OFF, true = ON
    const [switches, setSwitches] = React.useState([false, false, false]);

    // 2 toggleSwitch 함수 정의
    //    - id(인덱스)를 받아서 해당 위치의 불리언 값을 반전(!)시킴
    //    - setSwitches에 새 배열을 전달해서 상태 업데이트
    const toggleSwitch = id => {
      setSwitches(prev =>
        prev.map((value, index) =>
          // index === id인 항목만 !value, 나머지는 그대로 value 유지
          index === id ? !value : value
        )
      );
    };
```
    // 3 렌더링할 JSX
    ```
    return (
      <>
        {
          // switches 배열을 map 돌면서 버튼 3개 생성
          switches.map((sw, index) => (
            <button
              key={index}                     // React가 리스트를 효율적으로 렌더링하도록 key 설정
              onClick={() => toggleSwitch(index)} // 클릭하면 해당 인덱스를 넘겨 토글 실행
            >
              {`스위치${index + 1} ${sw ? 'ON' : 'OFF'}`}
              {/* sw가 true면 'ON', false면 'OFF' */}
            </button>
          ))
        }
      </>
    );
  };
  
  // 4 ReactDOM으로 컴포넌트 실제 화면에 마운트
  ReactDOM.createRoot(document.getElementById('root13')).render(<MultiSwitch />);
</script>
```
상태 선언 (useState)
const [switches, setSwitches] = React.useState([false, false, false]);
switches에는 3개의 불리언값이 담긴 배열이 들어가요.
처음에 모두 false(OFF)로 시작하도록 설정했습니다.
setSwitches는 이 상태를 바꿔줄 때 사용하는 함수예요.
토글 함수 (toggleSwitch)

const toggleSwitch = id => {
  setSwitches(prev =>
    prev.map((value, index) =>
      index === id ? !value : value
    )
  );
};
id는 클릭한 버튼의 인덱스(0, 1, 2)를 의미해요.

prev.map(...)로 배열을 순회하며,

index === id인 항목만 !value(반전)

나머지는 기존 value를 그대로 유지

이렇게 새로 만들어진 배열 전체를 setSwitches로 업데이트합니다.

버튼 렌더링
```
switches.map((sw, index) => (
  <button key={index} onClick={() => toggleSwitch(index)}>
    {`스위치${index + 1} ${sw ? 'ON' : 'OFF'}`}
  </button>
))
```
switches.map으로 3번 반복하면서 버튼을 그려요.

sw는 현재 요소의 값(false 또는 true), index는 그 위치(0,1,2)

버튼 텍스트는 스위치1 OFF처럼, sw ? 'ON' : 'OFF'로 결정

onClick에 toggleSwitch(index)를 걸어, 클릭된 버튼만 토글되도록 합니다.

렌더링 마운트

ReactDOM.createRoot(document.getElementById('root13')).render(<MultiSwitch />);
HTML의 <div id="root13">에 MultiSwitch 컴포넌트를 붙여 실제 화면에 보이게 해 줍니다.

// 메타 facebook -react를 만듬*
label, 