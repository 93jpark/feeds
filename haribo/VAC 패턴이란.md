## VAC 패턴을 도입하게 된 이유

왜 프로젝트에서 VAC 패턴을 도입하게 되었을까?

최근에 관심사 분리라는 개념을 접하고 그에 대한 고민을 많이 하면서 여러가지 디자인 패턴에 대해 공부하기 시작하였다. 그 중에서도 **비즈니스 로직과 View의 관심사를 체계적으로 분리해서 관리하는 VAC 패턴**이 관심사 분리의 목적에 아주 적합하다고 생각하였다.

<br/>

또 실무에서 작업을 하게 되면 UI 개발자와 FE 개발이 나뉘게 되는 경우가 있는데 FE와 UI 개발 영역이 겹치는 경우가 많다고 한다. 비록 팀프로젝트에서 UI 개발자는 없지만 실무에서도 적용시킬 수 있는 디자인 패턴을 사용해보고 싶은 이유도 있었다.

<br/>

이러한 이유들로 인해서 이번 프로젝트에 도입을 하게 되었고 `VAC`패턴을 적용하면서 겪게된 장단점과 `VAC`패턴에 대해 소개해보려고 한다.

그렇다면 VAC 패턴이 무엇인지 먼저 알아보자.

<br/>

## VAC 패턴이란?

**View Asset Component**의 약자로 **View 로직과 `JSX`의 의존성을 최소화**하는 것이 목적이다.

View 컴포넌트에서 `JSX` 영역을 `Props Object`로 추상화하고, `JSX`를 `VAC`로 분리해서 개발하는 설계 방법이다.

`VAC`는 다음과 같은 규칙을 따른다.

- 반복, 조건부 노출 등 스타일 제어 렌더링에 관련된 처리만을 수행
- `Props`를 통해서만 제어되며, 스스로의 상태를 관리하지 않는 `stateless`컴포넌트
- 이벤트에 함수를 바인딩 할 때 추가 처리 없이 적용(이벤트에 직접 바인딩)

> `VAC`는 `state`를 가질 수 없지만 `state`를 가진 컴포넌트를 자식으로 가지는 것은 가능하다.
> 이 경우 `VAC`는 부모 컴포넌트와 자식 컴포넌트 중간에서 개입하지 않고 단순히 `props`를 전달하는 역할만 하게 된다.

기존의 패턴들과 어떤 차이점이 있을지 비교해보자.

### 기존의 패턴

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/eb9df471-1cb9-421e-9eca-276c6171e791/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230326T171105Z&X-Amz-Expires=86400&X-Amz-Signature=50b4361e3e2ec15593e56b891a8d5826c3f0560d8199826f30fbc87d65e2da29&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 컴포넌트 내에서 `JSX`와 `Style`을 처리하고 관련된 비즈니스 로직에 대해 필요한 콜백 함수들을 바인딩한다.
- `Props`나 `State`가 단순히 바뀌더라도 `JSX` 영역에 수정이 발생할 때, UI 개발자가 다른 작업을 `JSX`에 한 상태라면 코드 충돌이 발생하게 된다.

이러한 단점을 보완하기 위한 `VAC` 패턴의 구조에 대해 살펴보자.

### VAC 패턴

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7d8468f8-7f80-4020-99e5-70731cda95bc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230326T171126Z&X-Amz-Expires=86400&X-Amz-Signature=a95c063b66cb6f97f39129f40e47f3fc0ebf1894a4c363ea7bfba8b637788862&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `JSX`를 별도의 컴포넌트로 분리해서 기존의 패턴의 단점을 최소화한다.
- 전달되는 `Props`와 `State`를 `Props Object`로 관리를 해서 `VAC`에 필요한 속성과 콜백 함수를 확인하기 쉽게 한다.

한마디로 View 로직과 `JSX`를 어떻게 분리하는지에 대한 가이드를 제공하는 디자인 패턴이라고 할 수 있다.

위의 구조에 따라 프로젝트에 적용시켜보았다.

<br/>

## 프로젝트에 적용하기

### View 컴포넌트

```jsx
const Sidebar = () => {
  const [selectPage, setSelectPage] =
    (useState < 'myArt') | ('createArt' > 'myArt');

  const navigate = useNavigate();
  const onNavigateGallery = () => {
    setSelectPage('myArt');
    navigate(ADMIN_ROUTE.MY_ART);
  };
  const onNavigateCreate = () => {
    setSelectPage('createArt');
    navigate(ADMIN_ROUTE.CREATE_ART);
  };

  const sideBarViewProps = {
    onNavigateGallery,
    onNavigateCreate,
    selectPage
  };

  // JSX를 VAC로 사용
  return <SidebarView {...sideBarViewProps} />; // Props Object
};

export default Sidebar;
```

View 컴포넌트에서는 `props`, `state`, `callback` 함수들을 구현하고 `Props Object` 를 통해서 전달한다.

View 로직을 재사용한다고 하면 `HOC`나 `Hook`등으로 분리해서 관리할 수도 있다.

### Vac 컴포넌트

```tsx
const SidebarView = ({
  onNavigateGallery,
  onNavigateCreate,
  selectPage
}: TSideBarViewProps) => {
  return <Container>...</Container>;
};
```

`VAC` 컴포넌트에서는 `JSX`만 보여준다.

`JSX`영역은 항상 `Props`를 받은 값 그대로 사용하게 되기 때문에 UI 개발자는 `Props`가 어떻게 구성되는지 파악할 필요가 없다. 전달한 `Props`만 가지고 `JSX`를 개발 할 수 있다.

프로젝트에 VAC를 적용하면서 주의할 점이 있다.

<br/>

## 주의할 점

다음과 같이 View 컴포넌트의 기능이나 상태 제어에 `VAC`가 관여해서는 안된다.

```jsx
// View Component
const SpinBox = () => {
  const [value, setValue] = useState(0);

  const props = {
    value,
    step: 1,
    handleClick: (n) => setValue(n)
  };

  // VAC에서 value를 제어하는 행위에 관여
  return <SpinBoxView {...props} />;
};
```

```jsx
// VAC
const SpinBoxView = ({ value, step, handleClick }) => (
  <div>
    <button onClick={() => handleClick(value - step)}>-</button>
    <span>{value}</span>
    <button onClick={() => handleClick(value + step)}>+</button>
  </div>
);
```

올바른 `VAC`는 핸들러를 이벤트에 바인딩만 할 뿐, **무엇을 하는지에 대해서 관여하지 않는다**.

```jsx
// VAC
const SpinBoxView = ({ value, onIncrease, onDecrease }) => (
  <div>
    <button onClick={onDecrease}>-</button>
    <span>{value}</span>
    <button onClick={onIncrease}>+</button>
  </div>
);
```

<br/>

## 실무에서 작업을 한다고 가정

1. FE개발자는 로컬에서 `View` 컴포넌트의 `Props Object`에 있는 로직을 수정한다.
2. UI개발자는 `VAC`에서 `style`을 적용한다.
3. FE개발자는 View 컴포넌트에서 기능을 수정하고, UI개발자는 `VAC`에서 `JSX`를 수정하여 충돌이 발생하지 않게 된다.

<br/>

## VAC props 네이밍

데이터 친화적인 형태보다는 **렌더링에 직관적인 형태**로 작성한다.

예시)

- `isMax`, `isMin` 보다는 `disabledDescrease`, `disabledIncrease` 로 작성
- `isLogin`, `isOwner`을 각각 전달 받아 `VAC` 내에서 `isLogin && isOwner` 형태로 사용하는 것 보다는 `showEditButton: isLogin && isOwner` 형태로 처음부터 조합해서 전달한다.

렌더링에서 어떤 역할을 하는지 유추하기 쉽다.

<br/>

## VAC 패턴과 Presentational Container 패턴의 차이점

`VAC` 패턴은 Container 컴포넌트에 로직을 위임하는 설계 방식을 따르기 때문에 `Presentational`과 `Container` 컴포넌트 패턴의 한 종류라고 할 수 있다.

비슷해보이지만 두 패턴의 차이는 컴포넌트가 **View 로직(UI 기능, 상태 관리)을 가질수 있는지 여부**에 따라 결정된다.

### **Presentational 컴포넌트**

- 비즈니스 로직과 View의 관심사 분리가 목적
- Container 컴포넌트에서 비즈니스 로직을 관리하고 Presentational 컴포넌트를 제어
- Presentational 컴포넌트는 View 로직(UI 기능, 상태 관리)과 렌더링을 담당

### **VAC**

- View 로직(UI 기능, 상태 관리)과 렌더링(JSX)의 관심사 분리가 목적
- View 컴포넌트가 `VAC`의 Container 컴포넌트 역할을 하며 JSX를 추상화한 `Props Object`를 관리하여 VAC를 제어
- VAC는 `JSX`, `Style`을 관리하여 렌더링 처리

`VAC`는 `Presentational` 컴포넌트보다 **더 구체적인 기준을 제시**하여 `JSX`를 처리하는 컴포넌트 관점에서 일관성 있는 설계를 하는데 도움을 준다.

<br/>

## 직접 적용해 본 후기

### 장점

- 비즈니스 로직과 UI가 분리되어 있어서 코드 파악이 쉬웠다.
- 변경에 용이하다. `CSS` 라이브러리를 바꾸는 경우 `VAC` 컴포넌트만 바꾸기만 하면 된다.
- `VAC` 컴포넌트는 내려온 `props interface`만 보고 컴포넌트를 설계하고 스타일할 수 있다. 어떻게 `props`가 내려오는지는 신경 쓸 필요가 없다.

### 단점

- `Props` 드릴링이 자주 있게 되서 불편한 경우가 생긴다.
- `DOM`을 직접 핸들링 하는 경우 적용하기 쉽지 않다.
- `Props`가 비대해질 수 있다.

비즈니스 로직이냐 아니냐라는 기준으로 인해 `Props`가 비대해지게 된다.

- `VAC` 컴포넌트 기준이 모호하다.

단순한 기능을 가진 컴포넌트들도 로직과 스타일을 분리를 해야 하는가 고민을 하게 된다.

<br/>

`VAC` 패턴에 대해 알아보고 `VAC` 패턴을 프로젝트에 적용해보았는데 모든 디자인 패턴이 그렇든 각각의 장단점이 있다고 생각한다. 하지만 관심사 분리라는 목표에 맞춰 생각한다면 또는 UI 개발자와 협업을 한다고 생각할 때 비즈니스 로직과 UI가 분리되어 있다는 점은 목적에 아주 적합하다고 생각한다.

한번쯤은 공부해보고 적용할만한 리액트 디자인 패턴인 것 같다.

<br/>

## 참고 자료

[[리액트] VAC 패턴 적용 후기 및 장단점](https://all-dev-kang.tistory.com/entry/리액트-VAC-패턴-적용-후기-및-장단점)

[React VAC Pattern - View 로직과 JSX의 의존성을 최소화 하자!](https://tv.naver.com/v/23162062)

[프레임워크의 선택, React vs Angular](https://tech.kakaoenterprise.com/109)

[React에서 View의 렌더링 관심사 분리를 위한 VAC 패턴 소개 | WIT블로그](https://wit.nts-corp.com/2021/08/11/6461)

[npm: react-vac](https://www.npmjs.com/package/react-vac)
