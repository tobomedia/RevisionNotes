# NestJS Cheatsheet

*A comprehensive reference for NestJS's most commonly used features*

📚 **Official Documentation:** <a href="https://docs.nestjs.com/" target="_blank">NestJS Documentation</a>

## Table of Contents

### 🚀 Getting Started
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)

### 🎯 Core Concepts
- [Controllers](#controllers)
- [Services & Providers](#services--providers)
- [Modules](#modules)
- [DTOs & Validation](#dtos--validation)

### 🔧 Request Lifecycle
- [Middleware](#middleware)
- [Guards](#guards)
- [Interceptors](#interceptors)
- [Pipes](#pipes)
- [Exception Filters](#exception-filters)

### 🗄️ Data & External Services
- [Database Integration (TypeORM)](#database-integration-typeorm)
- [Configuration](#configuration)
- [WebSockets](#websockets)

### 🔐 Security & Authentication
- [Authentication & Authorization](#authentication--authorization)

### 🛠️ Advanced Features
- [Custom Decorators](#common-decorators)
- [Testing](#testing)

### 📚 Resources
- [Quick Reference Links](#quick-reference-links)

---

## Getting Started

📖 <a href="https://docs.nestjs.com/first-steps" target="_blank">First Steps Documentation</a>

```bash
# Install NestJS CLI
npm i -g @nestjs/cli

# Create new project
nest new project-name

# Generate resources
nest generate controller cats
nest generate service cats
nest generate module cats
nest generate resource cats  # Creates controller, service, module, DTO, entity
```

## Project Structure

```
src/
├── app.controller.ts      # Root controller
├── app.module.ts         # Root module
├── app.service.ts        # Root service
├── main.ts              # Entry point
├── modules/             # Feature modules
│   ├── cats/
│   │   ├── cats.controller.ts
│   │   ├── cats.service.ts
│   │   ├── cats.module.ts
│   │   ├── dto/
│   │   └── entities/
└── common/              # Shared utilities
    ├── decorators/
    ├── filters/
    ├── guards/
    ├── interceptors/
    └── pipes/
```

## Controllers

📖 <a href="https://docs.nestjs.com/controllers" target="_blank">Controllers Documentation</a>

```typescript
import { Controller, Get, Post, Put, Delete, Body, Param, Query } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  // GET /cats
  @Get()
  findAll(@Query() query: any) {
    return this.catsService.findAll(query);
  }

  // GET /cats/:id
  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.catsService.findOne(+id);
  }

  // POST /cats
  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }

  // PUT /cats/:id
  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return this.catsService.update(+id, updateCatDto);
  }

  // DELETE /cats/:id
  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.catsService.remove(+id);
  }

  // Custom route with multiple decorators
  @Get('search')
  @HttpCode(200)
  @Header('Cache-Control', 'none')
  search(@Query('name') name: string) {
    return this.catsService.search(name);
  }
}
```

## Services & Providers

📖 <a href="https://docs.nestjs.com/providers" target="_blank">Providers Documentation</a>

```typescript
import { Injectable, NotFoundException } from '@nestjs/common';

@Injectable()
export class CatsService {
  private cats: Cat[] = [];

  findAll(): Cat[] {
    return this.cats;
  }

  findOne(id: number): Cat {
    const cat = this.cats.find(cat => cat.id === id);
    if (!cat) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    return cat;
  }

  create(createCatDto: CreateCatDto): Cat {
    const cat = {
      id: Date.now(),
      ...createCatDto,
    };
    this.cats.push(cat);
    return cat;
  }

  update(id: number, updateCatDto: UpdateCatDto): Cat {
    const catIndex = this.cats.findIndex(cat => cat.id === id);
    if (catIndex === -1) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    
    this.cats[catIndex] = { ...this.cats[catIndex], ...updateCatDto };
    return this.cats[catIndex];
  }

  remove(id: number): void {
    const catIndex = this.cats.findIndex(cat => cat.id === id);
    if (catIndex === -1) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    this.cats.splice(catIndex, 1);
  }
}

// Custom provider patterns
@Injectable()
export class ConfigService {
  private readonly config = process.env;

  get(key: string): string {
    return this.config[key];
  }
}

// Factory provider
const connectionProvider = {
  provide: 'CONNECTION',
  useFactory: (configService: ConfigService) => {
    return createConnection(configService.get('DATABASE_URL'));
  },
  inject: [ConfigService],
};
```

## Modules

📖 <a href="https://docs.nestjs.com/modules" target="_blank">Modules Documentation</a>

```typescript
import { Module } from '@nestjs/common';

// Feature module
@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService], // Make service available to other modules
})
export class CatsModule {}

// Module with imports
@Module({
  imports: [
    TypeOrmModule.forFeature([Cat]), // Import specific features
    ConfigModule,                     // Import other modules
  ],
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}

// Dynamic module
@Module({})
export class DatabaseModule {
  static forRoot(options: DatabaseOptions): DynamicModule {
    return {
      module: DatabaseModule,
      providers: [
        {
          provide: 'DATABASE_OPTIONS',
          useValue: options,
        },
        DatabaseService,
      ],
      exports: [DatabaseService],
      global: true, // Make module global
    };
  }
}

// Root module
@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
    }),
    DatabaseModule.forRoot({
      host: 'localhost',
      port: 5432,
    }),
    CatsModule,
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

## DTOs & Validation

📖 <a href="https://docs.nestjs.com/techniques/validation" target="_blank">Validation Documentation</a>  
📖 <a href="https://docs.nestjs.com/pipes" target="_blank">Pipes Documentation</a>

```typescript
import { IsString, IsInt, IsOptional, IsEmail, MinLength, MaxLength } from 'class-validator';
import { Transform, Type } from 'class-transformer';

// Create DTO
export class CreateCatDto {
  @IsString()
  @MinLength(2)
  @MaxLength(50)
  name: string;

  @IsInt()
  @Type(() => Number)
  age: number;

  @IsString()
  breed: string;

  @IsEmail()
  @IsOptional()
  ownerEmail?: string;

  @Transform(({ value }) => value.toLowerCase())
  @IsOptional()
  color?: string;
}

// Update DTO (extends PartialType)
import { PartialType } from '@nestjs/mapped-types';

export class UpdateCatDto extends PartialType(CreateCatDto) {}

// Custom DTO with validation
export class QueryCatDto {
  @IsOptional()
  @IsString()
  name?: string;

  @IsOptional()
  @Type(() => Number)
  @IsInt()
  limit?: number = 10;

  @IsOptional()
  @Type(() => Number)
  @IsInt()
  offset?: number = 0;
}

// Using DTOs in controller
@Controller('cats')
export class CatsController {
  @Post()
  @UsePipes(new ValidationPipe({ transform: true }))
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```

## Middleware

📖 <a href="https://docs.nestjs.com/middleware" target="_blank">Middleware Documentation</a>

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`${req.method} ${req.originalUrl} - ${new Date().toISOString()}`);
    next();
  }
}

// Functional middleware
export function logger(req: Request, res: Response, next: NextFunction) {
  console.log(`Request...`);
  next();
}

// Apply middleware in module
@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes({ path: 'cats', method: RequestMethod.GET });
    
    // Or apply to specific controllers
    consumer
      .apply(logger)
      .exclude(
        { path: 'cats', method: RequestMethod.GET },
        'cats/(.*)',
      )
      .forRoutes(CatsController);
  }
}
```

## Guards

📖 <a href="https://docs.nestjs.com/guards" target="_blank">Guards Documentation</a>

```typescript
import { Injectable, CanActivate, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { Reflector } from '@nestjs/core';

// Authentication guard
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    const token = request.headers.authorization;
    
    if (!token) {
      throw new UnauthorizedException('No token provided');
    }
    
    // Validate token logic here
    return this.validateToken(token);
  }

  private validateToken(token: string): boolean {
    // Token validation logic
    return token === 'valid-token';
  }
}

// Role-based guard
export const Roles = (...roles: string[]) => SetMetadata('roles', roles);

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const roles = this.reflector.get<string[]>('roles', context.getHandler());
    if (!roles) {
      return true;
    }

    const request = context.switchToHttp().getRequest();
    const user = request.user;
    
    return roles.some(role => user?.roles?.includes(role));
  }
}

// Using guards
@Controller('cats')
@UseGuards(AuthGuard)
export class CatsController {
  @Get()
  @Roles('admin', 'user')
  @UseGuards(RolesGuard)
  findAll() {
    return this.catsService.findAll();
  }
}
```

## Interceptors

📖 <a href="https://docs.nestjs.com/interceptors" target="_blank">Interceptors Documentation</a>

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map, tap } from 'rxjs/operators';

// Logging interceptor
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');
    const now = Date.now();
    
    return next
      .handle()
      .pipe(
        tap(() => console.log(`After... ${Date.now() - now}ms`)),
      );
  }
}

// Transform interceptor
@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(
      map(data => ({
        success: true,
        data,
        timestamp: new Date().toISOString(),
      }))
    );
  }
}

// Cache interceptor
@Injectable()
export class CacheInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const isCached = true; // Cache logic here
    
    if (isCached) {
      return of([]); // Return cached data
    }
    
    return next.handle();
  }
}

// Using interceptors
@Controller('cats')
@UseInterceptors(LoggingInterceptor, TransformInterceptor)
export class CatsController {
  @Get()
  findAll() {
    return this.catsService.findAll();
  }
}
```

## Pipes

📖 <a href="https://docs.nestjs.com/pipes" target="_blank">Pipes Documentation</a>

```typescript
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';

// Validation pipe
@Injectable()
export class ParseIntPipe implements PipeTransform<string, number> {
  transform(value: string, metadata: ArgumentMetadata): number {
    const val = parseInt(value, 10);
    if (isNaN(val)) {
      throw new BadRequestException('Validation failed');
    }
    return val;
  }
}

// Custom validation pipe
@Injectable()
export class CatValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    if (metadata.type === 'body' && value.age < 0) {
      throw new BadRequestException('Age must be positive');
    }
    return value;
  }
}

// Using pipes
@Controller('cats')
export class CatsController {
  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    return this.catsService.findOne(id);
  }

  @Post()
  @UsePipes(new ValidationPipe(), CatValidationPipe)
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```

## Exception Filters

📖 <a href="https://docs.nestjs.com/exception-filters" target="_blank">Exception Filters Documentation</a>

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException, HttpStatus } from '@nestjs/common';

// HTTP exception filter
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
        message: exception.message,
      });
  }
}

// All exceptions filter
@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();

    const status = exception instanceof HttpException
      ? exception.getStatus()
      : HttpStatus.INTERNAL_SERVER_ERROR;

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
      });
  }
}

// Using filters
@Controller('cats')
@UseFilters(HttpExceptionFilter)
export class CatsController {
  @Get(':id')
  @UseFilters(new HttpExceptionFilter())
  findOne(@Param('id') id: string) {
    throw new NotFoundException('Cat not found');
  }
}
```

## Database Integration (TypeORM)

📖 <a href="https://docs.nestjs.com/techniques/database" target="_blank">Database Documentation</a>  
📖 <a href="https://docs.nestjs.com/recipes/sql-typeorm" target="_blank">TypeORM Integration</a>

```typescript
// Entity
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class Cat {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  age: number;

  @Column()
  breed: string;

  @Column({ nullable: true })
  ownerEmail?: string;
}

// Repository service
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';

@Injectable()
export class CatsService {
  constructor(
    @InjectRepository(Cat)
    private catsRepository: Repository<Cat>,
  ) {}

  async findAll(): Promise<Cat[]> {
    return this.catsRepository.find();
  }

  async findOne(id: number): Promise<Cat> {
    const cat = await this.catsRepository.findOne({ where: { id } });
    if (!cat) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    return cat;
  }

  async create(createCatDto: CreateCatDto): Promise<Cat> {
    const cat = this.catsRepository.create(createCatDto);
    return this.catsRepository.save(cat);
  }

  async update(id: number, updateCatDto: UpdateCatDto): Promise<Cat> {
    await this.catsRepository.update(id, updateCatDto);
    return this.findOne(id);
  }

  async remove(id: number): Promise<void> {
    const result = await this.catsRepository.delete(id);
    if (result.affected === 0) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
  }
}

// Module setup
@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: 'localhost',
      port: 5432,
      username: 'user',
      password: 'password',
      database: 'nestjs',
      entities: [Cat],
      synchronize: true, // Don't use in production
    }),
    TypeOrmModule.forFeature([Cat]),
  ],
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

## Configuration

📖 <a href="https://docs.nestjs.com/techniques/configuration" target="_blank">Configuration Documentation</a>

```typescript
// Using ConfigModule
import { ConfigModule, ConfigService } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: ['.env.local', '.env'],
      load: [databaseConfig],
    }),
  ],
})
export class AppModule {}

// Configuration factory
export const databaseConfig = () => ({
  database: {
    host: process.env.DATABASE_HOST,
    port: parseInt(process.env.DATABASE_PORT, 10) || 5432,
    username: process.env.DATABASE_USERNAME,
    password: process.env.DATABASE_PASSWORD,
    database: process.env.DATABASE_NAME,
  },
});

// Using ConfigService
@Injectable()
export class AppService {
  constructor(private configService: ConfigService) {}

  getDatabaseHost(): string {
    return this.configService.get<string>('database.host');
  }
}

// Custom configuration service
@Injectable()
export class MyConfigService {
  constructor(private configService: ConfigService) {}

  get port(): number {
    return this.configService.get<number>('PORT', 3000);
  }

  get isDevelopment(): boolean {
    return this.configService.get('NODE_ENV') === 'development';
  }
}
```

## Testing

📖 <a href="https://docs.nestjs.com/fundamentals/testing" target="_blank">Testing Documentation</a>

```typescript
// Unit test
import { Test, TestingModule } from '@nestjs/testing';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

describe('CatsController', () => {
  let controller: CatsController;
  let service: CatsService;

  const mockCatsService = {
    findAll: jest.fn(() => []),
    findOne: jest.fn((id) => ({ id, name: 'Test Cat', age: 1 })),
    create: jest.fn((dto) => ({ id: 1, ...dto })),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [CatsController],
      providers: [
        {
          provide: CatsService,
          useValue: mockCatsService,
        },
      ],
    }).compile();

    controller = module.get<CatsController>(CatsController);
    service = module.get<CatsService>(CatsService);
  });

  it('should be defined', () => {
    expect(controller).toBeDefined();
  });

  it('should return all cats', () => {
    expect(controller.findAll()).toBe(mockCatsService.findAll());
    expect(service.findAll).toHaveBeenCalled();
  });
});

// E2E test
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from './../src/app.module';

describe('CatsController (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  it('/cats (GET)', () => {
    return request(app.getHttpServer())
      .get('/cats')
      .expect(200)
      .expect('Content-Type', /json/);
  });

  it('/cats (POST)', () => {
    return request(app.getHttpServer())
      .post('/cats')
      .send({ name: 'Test Cat', age: 1, breed: 'Persian' })
      .expect(201);
  });
});
```

## WebSockets

📖 <a href="https://docs.nestjs.com/websockets/gateways" target="_blank">WebSockets Documentation</a>

```typescript
import {
  WebSocketGateway,
  SubscribeMessage,
  MessageBody,
  WebSocketServer,
  ConnectedSocket,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';

@WebSocketGateway({
  cors: {
    origin: '*',
  },
})
export class EventsGateway {
  @WebSocketServer()
  server: Server;

  @SubscribeMessage('events')
  handleEvent(@MessageBody() data: string): string {
    return data;
  }

  @SubscribeMessage('identity')
  async identity(@MessageBody() data: number): Promise<number> {
    return data;
  }

  @SubscribeMessage('events')
  handleEvent(@MessageBody() data: unknown, @ConnectedSocket() client: Socket): string {
    return 'Hello world!';
  }

  // Emit to all clients
  handleConnection(client: Socket, ...args: any[]) {
    this.server.emit('userConnected', { userId: client.id });
  }

  handleDisconnect(client: Socket) {
    this.server.emit('userDisconnected', { userId: client.id });
  }
}
```

## Authentication & Authorization

📖 <a href="https://docs.nestjs.com/security/authentication" target="_blank">Authentication Documentation</a>  
📖 <a href="https://docs.nestjs.com/security/authorization" target="_blank">Authorization Documentation</a>

```typescript
// JWT Strategy
import { ExtractJwt, Strategy } from 'passport-jwt';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable } from '@nestjs/common';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: 'secret',
    });
  }

  async validate(payload: any) {
    return { userId: payload.sub, username: payload.username };
  }
}

// Auth service
@Injectable()
export class AuthService {
  constructor(
    private usersService: UsersService,
    private jwtService: JwtService,
  ) {}

  async validateUser(username: string, pass: string): Promise<any> {
    const user = await this.usersService.findOne(username);
    if (user && user.password === pass) {
      const { password, ...result } = user;
      return result;
    }
    return null;
  }

  async login(user: any) {
    const payload = { username: user.username, sub: user.userId };
    return {
      access_token: this.jwtService.sign(payload),
    };
  }
}

// Auth module
@Module({
  imports: [
    UsersModule,
    PassportModule,
    JwtModule.register({
      secret: 'secret',
      signOptions: { expiresIn: '60s' },
    }),
  ],
  providers: [AuthService, JwtStrategy],
  exports: [AuthService],
})
export class AuthModule {}
```

## Common Decorators

📖 <a href="https://docs.nestjs.com/custom-decorators" target="_blank">Custom Decorators Documentation</a>

```typescript
// Custom parameter decorator
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const User = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    return request.user;
  },
);

// Custom property decorator
export const CurrentUser = createParamDecorator(
  (data: string, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user;
    return data ? user?.[data] : user;
  },
);

// Using custom decorators
@Controller('profile')
export class ProfileController {
  @Get()
  getProfile(@User() user: UserEntity) {
    return user;
  }

  @Get('name')
  getName(@CurrentUser('name') name: string) {
    return { name };
  }
}

// Metadata decorator
export const IS_PUBLIC_KEY = 'isPublic';
export const Public = () => SetMetadata(IS_PUBLIC_KEY, true);

// Using metadata
@Controller('auth')
export class AuthController {
  @Public()
  @Post('login')
  login(@Body() loginDto: LoginDto) {
    return this.authService.login(loginDto);
  }
}
```

---

## Quick Reference Links

### Essential Resources
- 📚 <a href="https://docs.nestjs.com/" target="_blank">NestJS Documentation</a> - Complete documentation
- 🎮 <a href="https://devtools.nestjs.com/" target="_blank">NestJS DevTools</a> - Development tools
- 📦 <a href="https://docs.nestjs.com/cli/overview" target="_blank">NestJS CLI</a> - Command-line interface
- 🔧 <a href="https://github.com/nestjs/nest/tree/master/sample" target="_blank">NestJS Samples</a> - Example applications

### Popular Integrations
- 🗄️ <a href="https://docs.nestjs.com/recipes/sql-typeorm" target="_blank">TypeORM</a> - SQL database ORM
- 🍃 <a href="https://docs.nestjs.com/recipes/mongodb" target="_blank">Mongoose</a> - MongoDB integration
- 🔐 <a href="https://docs.nestjs.com/security/authentication" target="_blank">Passport</a> - Authentication
- 📊 <a href="https://docs.nestjs.com/graphql/quick-start" target="_blank">GraphQL</a> - GraphQL support
- 🔄 <a href="https://docs.nestjs.com/techniques/queues" target="_blank">Bull Queue</a> - Job queues
- 📧 <a href="https://nest-modules.github.io/mailer/" target="_blank">Mailer</a> - Email sending

### Advanced Topics
- 🔄 <a href="https://docs.nestjs.com/microservices/basics" target="_blank">Microservices</a> - Microservice architecture
- 🏗️ <a href="https://docs.nestjs.com/recipes/cqrs" target="_blank">CQRS</a> - Command Query Responsibility Segregation
- 🎯 <a href="https://docs.nestjs.com/recipes/cqrs#events" target="_blank">Event Sourcing</a> - Event-driven architecture
- 🚀 <a href="https://docs.nestjs.com/techniques/performance" target="_blank">Performance</a> - Optimization techniques
- 🔒 <a href="https://docs.nestjs.com/security/helmet" target="_blank">Security</a> - Security best practices

### Community Resources
- 💬 <a href="https://discord.gg/G7Qnnhy" target="_blank">Discord</a> - Community chat
- 🐦 <a href="https://twitter.com/nestframework" target="_blank">Twitter</a> - Updates and news
- 📚 <a href="https://github.com/juliandavidmr/awesome-nestjs" target="_blank">Awesome NestJS</a> - Curated resources
- 🎓 <a href="https://courses.nestjs.com/" target="_blank">NestJS Courses</a> - Official courses

---

*This cheatsheet covers NestJS 10+ features. For the latest updates, always refer to the <a href="https://docs.nestjs.com/" target="_blank">official documentation</a>.*