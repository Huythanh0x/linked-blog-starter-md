### Multi-tenant
- Default tenant database
	- 
- Default Service Owner
- Default Owner

### Db configs
- Import constant configs (amin, password, host, ...)
- [[ORM Entities]]
- [[Migration]] scripts

### [[Docker compose]]

### DB design

### GraphQL schema
- DTO (all fields are [[optional]])
- Response 

### Migration script
> on UP() and DOWN()

- **root**
- **seeds* // add seed data (default user / admin user / default row)

### Schema based project
> create `schema.table`

### [[onModuleInit]]
Automatically runs when the application starts -> initialize db connection

### AbstractDTO & AbstractEntity
```ts
export class AbstractDto {
	id: string;
	createdAt: Date;
	updatedAt: Date;
}
```

```ts
import { BaseEntity, CreateDateColumn, DeleteDateColumn, PrimaryGeneratedColumn, UpdateDateColumn } from 'typeorm';
import { AbstractDto } from './abstract.dto';
import { Constructor } from './types';

export interface IAbstractEntity<DTO extends AbstractDto, O = never> {
	id: string;
	createdAt: Date;
	updatedAt: Date;
	toDto(options?: O): DTO;
}

export abstract class AbstractEntity<DTO extends AbstractDto = AbstractDto, O = never> 
extends BaseEntity 
implements IAbstractEntity<DTO, O> {

	private dtoClass: Constructor<DTO, [AbstractEntity, O?]>;

	@PrimaryGeneratedColumn('uuid')
	id: string;
	
	@CreateDateColumn({ type: 'timestamp' })
	createdAt: Date;
	
	@UpdateDateColumn({ type: 'timestamp' })
	updatedAt: Date;
	
	@DeleteDateColumn({ type: 'timestamp' })
	deletedAt: Date;
	
	toDto(options?: O): DTO {
		const dtoClass = this.dtoClass;
			if (!dtoClass) {
				throw new Error(`You need to use @UseDto on class (${this.constructor.name}) be able to call toDto function`);
			
			}
		return new this.dtoClass(this, options);
	}

	constructor(partial?: Partial<DTO>) {
		super();
		Object.assign(this, partial);
	}
}
```

### XXXConnectionDTO
> Basically the set up to connect to different schema

### forFeature & forRoot
> Registers entities for a <mark style="background: #ADCCFFA6;">specific [[module]]</mark>, making them available for dependency injection [[within that module's scope]]
```ts
@Module({
	imports: [TypeOrmModule.forFeature([XXXConnectionsEntity])],
	providers: [],
	exports: [],
})
```

### Db relationship
![[Pasted image 20250623140030.png]]