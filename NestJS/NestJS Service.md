
## Service
```js
//app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```
> 위 코드는 app.service.ts의 코드이다. service는 @Injectable() 데코레이터를 사용해서 만들어 줄 수 있다. @Injectable() 데코레이터를 사용하는 class는 서비스 뿐만이 아니고 리포지토리, 헬퍼, 등 종속성 주입을 할 수 있게 만들어 준다. 

## 컨트롤러와 서비스를 나누는 이유 
```js
//app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('kimbro')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get('/hello')
  getHello(): string {
    return this.appService.getHello();
  }
}
```
```js
//app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```
> 위의 코드는 컨트롤러, 아래 코드는 서비스 코드이다. localhost:3000/kimbro/hello에 get요청을 보내게 되면 'Hello World!'가 출력될것이다. url로 요청을 보내면 컨트롤러에서 처음으로 요청을 받게된다. 받게되면 그에 해당하는 함수가 실행되고 서비스로 넘어가서 'Hello World!'를 출력하는것이다. 

```js
//app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('kimbro')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get('/hello')
  getHello(): string {
    return 'Hello World!'
  }
}
```

> 그렇다면 위의 코드처럼 컨트롤러에 'Hello World!' 리턴해주면 서비스 파일도 안만들고 더 효율적인거 아닌가라는 생각을 할 수 있다. 맞다 지금은 코드가 몇 줄 없기때문에 저렇게 컨트롤러에서 바로 리턴해줘도 상관없을것이다. 하지만 코드가 조금만 많아지고 컨트롤러에 작업을 계속 한다면 가독성도 떨어지고 협업하는데 많이 힘들것이다. 그래서 서비스 파일을 만들어서 컨트롤러에는 분기만 정해주고 서비스에는 비즈니스 로직을 작성해주면 조금더 가독성이 좋은 코드를 짤 수 있다. 

## service 종속성 주입
```js
// app.module.ts
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
> 서비스를 컨트롤러에서 사용하려면 app.module.ts에  providers에 종속성 주입을 해줘야 사용할 수 있다.

## controller에서 service
```js
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('kimbro')
export class AppController {
  appService: AppService
  constructor(appService: AppService) {
  	this.appService = appService
  }

  @Get('/hello')
  getHello(): string {
    return this.appService.getHello();
  }
}
```

> 컨트롤러에서 서비를 사용하려면 위의 코드 처럼  생성자 함수에 appService 파라미터에 AppService 객체를 타입으로 지정해주고 appService 파라미터를 AppController 클래스 안에서 사용하기 위해서 this.appService 프로퍼티에 appService 파라미터를 할당해준다. 하지만 타입스크립트에서는 선언한 값만 객체의 프로퍼티로 사용가능하기 때문에 위에 appService: AppService로  선언해준다. 하지남 위의 코드처럼 작성하면 너무 많은 정보를 입력하기 때문에 밑에 코드 처럼 작성해줄 수 있다.

```js
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('kimbro')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get('/hello')
  getHello(): string {
    return this.appService.getHello();
  }
}

```
> 위의 코드처럼 접근 제한자를 이용해서 소스를 간단하게 만들어줄 수 있다. 접근 제한자를 생성자 파라미터에 선언하면 접근 제한자가 사용된 생성자 파라미터는 암묵적으로 클래스 프로퍼티로 선언된다. Private를 사용하면 클래스 내부에서만 사용 가능하다.