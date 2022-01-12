### controllers
#### @Controller
>컨트롤러는 들어오는 요청을 처리하고 응답을 클라이언트에 반환할 책임이 있다.

![](https://images.velog.io/images/rlagudwog/post/b943bf4a-49cd-4505-a4cf-fc991f5a0d34/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-12%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.02.25.png)
> 클라이언트에서 HTTP 요청을 보내면 컨트롤러는 어떤 요청을 수신하는지 제어할 수 있다. 라우팅의 역할을 수행한다고 생가하면 될거같다.

```js
// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return 'hello kimbro'
  }
}

```
> 위의 코드는 nest controller 파일이다. nest에서 컨트롤러를 정의하데에는 @Controller() 데코레이터가 사용된다. 데코레이터안에 자신이 원하는 path를 넣어주면 path 맞는 요청에만 응답을 반환해준다.

![](https://images.velog.io/images/rlagudwog/post/71a80825-451f-4933-be23-839e065ad85d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-12%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.16.49.png)

> 현재는 어떤 분기도 입력하지 않아 3000번 port로 get요청을 보내면 'hello kimbro'를 출력하는것을 볼 수 있다.

```js
// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('kimbro')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return 'hello kimbro'
  }
}

```
> 위의 코드처럼 데코레이터 안에 원하는 path를 넣어줘서 분기를 나눠주면 아래왜 같이 3000번 port에 kimbro로 get요청을 보내야지 'hello kimbro'를 출력하느것을 볼 수 있다. 결론적으로 express의 route 개념과 비슷하다고 생각하면 편할거 같다. 

![](https://images.velog.io/images/rlagudwog/post/21332a5e-9217-4824-a080-3bc57a66c6e3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-12%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.19.19.png)

#### HTTP 요청 메서드
> HTTP 요청 메서드에는 GET, POST, PUT, DELETE등이 있는데 nest에서는 이 요청 메서드도 데코레이터를 이용해서 만들어 줄 수 있다. 

```js
// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('kimbro')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return 'hello kimbro'
  }
}

```
> 위의 getHello함수 위에 @Get 요청메서드가 있는것을 볼 수 있다. 컨트롤러 데코레이터와 같이 앤드포인트를 지정해서 앤드포인트에 맞는 요청을 처리해줄 수 있다.

```js
// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('kimbro')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get('/hello')
  getHello(): string {
    return 'hello kimbro'
  }
}

```
> @Get 데코레이터에 'hello'라는 앤드폰인트를 넣어주면 아래의 사진과 같이 'hello kimbro'를 출력하는것을 볼 수 있다. 

![](https://images.velog.io/images/rlagudwog/post/32240d7a-e91b-4fe6-aea6-dee7b14283cd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-12%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.44.25.png)
```js
// app.controller.ts
import { Controller, Get, Post } from '@nestjs/common';
import { AppService } from './app.service';
import { Request } from 'express';

@Controller('kimbro')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get('/hello')
  getHello(): string {
    return 'hello kimbro'
  }

  @Post('/hello')
  createHello(@Req() req: Request ) {
    console.log(req.body)
  }
}

```
> 우리가 get 요청만 하는것이 아니고 post, delete 같은 정보를 수정하는 메서드를 많이 사용한다. 그럼 그 데이터들은 body, headers와 같은 요청에 실어서 보내주면 위와같이 @Req() 데코레이터를 사용해서 받을 수 있다.

![](https://images.velog.io/images/rlagudwog/post/8847be97-5d9c-464d-8f38-ea631b6a0faa/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-12%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.37.01.png)
> @Req() 데코레이터 말고도 여러가지 옵션들있다. 공식문서에 친절하게 나와있기때문에 공식문서를 참고하면 좋을거 같다. 