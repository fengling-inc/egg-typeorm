# egg-typeorm

[TypeORM](https://typeorm.io/#/) for [Egg.js](https://eggjs.org/).

[![NPM version][npm-image]][npm-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/@zhuowenli/egg-typeorm.svg?style=flat-square
[npm-url]: https://npmjs.org/package/@zhuowenli/egg-typeorm
[download-image]: https://img.shields.io/npm/dm/@zhuowenli/egg-typeorm.svg?style=flat-square
[download-url]: https://npmjs.org/package/@zhuowenli/egg-typeorm

## Install

```bash
yarn add @zhuowenli/egg-typeorm mysql
```

## Usage

### Plugin

```ts
// {app_root}/config/plugin.ts
const plugin: EggPlugin = {
    typeorm: {
        enable: true,
        package: '@zhuowenli/egg-typeorm',
    },
};
```

### Configuration

```ts
// {app_root}/config/config.default.ts
config.typeorm = {
    type: 'mysql',
    host: 'localhost',
    port: 3306,
    username: 'root',
    password: '123456',
    database: 'test',
    synchronize: true,
    logging: false,
    entities: ['app/entity/**/*.ts'],
    migrations: ['app/migration/**/*.ts'],
    subscribers: ['app/subscriber/**/*.ts'],
    cli: {
        entitiesDir: 'app/entity',
        migrationsDir: 'app/migration',
        subscribersDir: 'app/subscriber',
    },
};
```

### Create entity files

```bash
├── controller
│   └── home.ts
├── entity
    ├── Post.ts
    └── User.ts
```

### Entity file

```ts
// app/entity/User.ts

import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm'

@Entity()
export class User {
    @PrimaryGeneratedColumn()
    id: number

    @Column()
    name: string
}
```

### Use with FindOptions

```ts
// in controller
export default class UserController extends Controller {
    public async index() {
        const { ctx } = this;
        ctx.body = await ctx.repo.User.find();
    }
}
```

### Use with QueryBuilder

```ts
// in controller
export default class UserController extends Controller {
    public async index() {
        const { ctx } = this;
        const firstUser = await ctx.repo.User.createQueryBuilder('user')
            .where('user.id = :id', { id: 1 })
            .getOne();
        ctx.body = firstUser;
    }
}
```

## License

[MIT](LICENSE)
