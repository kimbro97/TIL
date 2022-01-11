## Nest 프로젝트 생성
NestJS의 프로젝트 폴더를 만드는 것은 리액트 폴더를 만드는것처럼 명령어 하나로 생성할 수 있다. 
```js
$ npm i -g @nestjs/cli
$ nest new project-name
```
먼저 NestJS cli를 글로벌로 설치하고 nest 명령어를 사용해서 프로젝트를 만들 수 있다. 
![](https://images.velog.io/images/rlagudwog/post/17596c7c-cf90-45c6-b565-a57ba0fae8b7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2012.47.31.png)

명령어를 실행키면 위와 같은 package manager를 선택하는 부분이 나오느데 자신이 주로 사용하는것을 선택하면된다.
![](https://images.velog.io/images/rlagudwog/post/1088f2e5-1e39-4298-afc6-fcd1caa339cc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2012.48.11.png)![](https://images.velog.io/images/rlagudwog/post/861fe690-dc9b-4cf9-88d5-ab114eaaa178/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2012.48.24.png)
이렇게 프로젝트 폴더가 쉽게 만들어지는것을 볼 수 있다. 

## src 폴더
프로젝트 폴더가 생성되고 노드 노듈과 몇가지 파일들이 설치되고 src 폴데어 여러 코어 파일이 생성된다
![](https://images.velog.io/images/rlagudwog/post/d9197420-9d0f-4ef5-8854-ca878fd6fd96/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.12.44.png)
### main.ts
먼전 main.ts파일부터 살펴보면 핵심기능 NestFactory를 사용하여 3000번 port에서 서버를 랜더링하는것을 볼 수 있다. 
```js
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
![](https://images.velog.io/images/rlagudwog/post/0f718b79-36f6-4c1e-a74f-85a4caaf798f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.56.37.png)
NestFactory는 애플리케이션 인스턴스를 생성하기 위한 핵심 class입니다.  main.ts에서 HTTP 리스너를 시작하게되면 애플리케이션이 인바운드 HTTP 요청을 기다릴 수 있다.
### app.module.ts
AppModule은 프로젝트의 루트 모델이다. 나중에 프로젝트를 진행하면서 많은 모듈을 만들게 되는데 모든 모듈들은 AppModule에서 import해서 사용할 수 있게 된다.![](https://images.velog.io/images/rlagudwog/post/40d8605f-f739-4289-a3eb-b00238ffd32d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.09.18.png)
```js
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
### app.controller.ts
AppCtroller는 하나의 라우트가 되는 기본 컨트롤러이다. express로 생각을 해보다면 route를 이용해서 api를 분기해주는거라고 생각하면 편할거 같다.
```js
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}

```
### app.service.ts
AppService는 단일 메소드를 사용하는 기본 서비스 파일이다. 여기서는 api의 비즈니스 로직을 작성하는 공간이라고 생각하면 될거 같다.
```js
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```