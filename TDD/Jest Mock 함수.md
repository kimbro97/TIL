## jest.fn() 이란?
jest.fn()은  Mock함수를 생성하는 함수 이다. 이 Mock함수가 하는 일은 단위 테스트를 작성할 때, 해당 코드가 의존하는 부분을 가짜로 대체하는 일을 해준다.  

## 단위 테스트가 독립적이어야 하는 이유
의존적인 부분을 구현하기가 까다로울 경우가 있으면, 의존적인 부분의 상태에 따라서 테스트 하고자 하는 부분의 테스트 결과가 영향을 받을 수 있기 때문이다.


## jest.fn() 기본 사용법
 ```js
const mockFunction = jest.fn() // Mock 함수를 만들어준다.
```
```js
mockFunction()
mockFunction('hello') // 가짜 함수 호출 인자를 넘겨서도 호출이 가능하다
```
```js
mockFunction.mockReturnValue('가짜 함수 반환')
// mockReturnValue 메소드를 이용해서 반환 값을 정해준다.
consol.log(mockFunction) // 가짜 함수 반환
```
```js
mockFunction('hello')
mockFunction()
expect(mockFunction).toBeCalledWith('hello')
expect(mockFunction).toBeCalledTimes(2) // 가짜 함수가 몇번 호출되었고 어떤 인자가 넘어왔는지 검증
```