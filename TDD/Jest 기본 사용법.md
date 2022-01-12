### Jest란 무엇인가?
facebook에서 만든 테스칭 프레임워크로서 최소한의 서정으로 동작하면 TEST CASE를 만들어 어플리케이션 코드가 에러 없이 돌아가는지 확인해준다. 단위 테스트를 위해서 이용합니다.

### Jest 설치하기 
```js
$ npm install jest --save-dev
```
> 개발 환경에서만 사용될거기 때문에 dev 디펜던시로 설치해준다

```js
//pakage.json
"test": "jest"
```
> pakage.json에 있는 test 스크립트를 jest로 변경해준다. jest로 변경해주고 npm test를 해주면 jest가 test파일만 찾아서 테스트를 실행시켜준다.

![](https://images.velog.io/images/rlagudwog/post/a981fd4d-6f5e-434e-b699-6588454d674e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-12%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.09.09.png)
> 테스트 폴더와 파일들을 만들어준다.

### Jest 파일구조, 사용법
![](https://images.velog.io/images/rlagudwog/post/6758009a-4efa-452f-8e6e-eb2452cd3e6b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-12%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.23.06.png)
```js
describe("user에 관련된 api", () => {
  test("user가 문제없이 실행되면 상태코드 200", () => {
    expect(statusCode).toBe(200)
  })

  test(it)("user가 토큰이없다면 401", () => {
    expect(statusCode).toBe(401)
  })
})
```
> describe는 여러관련 테스트를 그룹화하는데 사용되고 test는 개별 테스트를 할때 사용된다. test를 사용해도되고 it를 사용해도 똑같이 작동한다.
expect함수는 혼자서는 거의 사용되지 않으면 matcher와 같이 사용된다. 위의 코드에서 보면 expect뒤에 toBe라는 함수가 있는데 이부분이 바로 matcher이다.

