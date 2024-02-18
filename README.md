# NestJS 배워보기

노누리님의 (블로그 글)[https://velog.io/@nuri00/Nest.js%EB%A1%9C-CRUD-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0]을 참고해서 nestJS를 공부해서 정리한 내용들입니다.



# NestJS 설치

```bash
$ npm i -g @nestjs/cli
$ nest new --strict learn-nestjs
```

# 파일 둘러보기

## main.ts

```tsx
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap()
```

NestJS App의 entry point

bootstrap() → app.module  파일로부터 AppModule을 import 해서 NestFactory가 App 객체를 생성하고 3000포트로 HTTP 요청을 받음

## Module

app.module.ts에서 AppModule 클래스를 확인할 수 있음.

```tsx
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

@Module()이라는 decorator가 호출되고 있음. 

decorator는 imports, controllers, providers 속성으로 이뤄진 객체를 인자로 받음.

- controllers: HTTP 요청을 받아 응답을 보내는 컨트롤러 클래스
- pvoviders: 컨트롤러가 사용하는 일반 클래스(주로 서비스 클래스)
- imports: 해당 모듈이 의존하고 있는 다른 모듈을 나열

module → App를 기능 단위로 쪼개놓은 것, 서로가 의존 가능

Hence,

NestJS는 IoC 컨테이너 역할을 하며 여러 모듈을 의존성 주입을 통해 엮어준다고 생각하면 됨.

## Controller

HTTP 요청을 받아 처리하고 응답을 해주는 역할

```tsx
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

@Controller() 데코레이터 호출을 통해 NestJS가 해당 클래스가 컨트롤러라는 인식을 하게 됨.

클래스 내의 각 메서드에서는 @Get(), @Post(), @Delete()와 같은 HTTP 방식에 해당하는 데코레이터를 붙여줌.

## Service

app.sevice.ts에서 AppService 클래스를 확인 가능.

```tsx
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

@Injectable() 데코레이터가 있는 클래스는 인스턴스를 생성하여 다른 생성자를 통해서 주입을 해줄 수 있음.