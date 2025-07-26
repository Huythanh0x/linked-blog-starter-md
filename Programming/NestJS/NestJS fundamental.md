### App module
may like [[`Application`]] class in Android
- Entry point of the application's [[dependency injection]]
- Manage dependencies between different parts of the application
- Organize the [[application's structure]]
- Config global configs([[Database]], [[Redis]])
```ts
@Module({}
	imports: [] //Usually others module
	providers: [] //Usually the service
})
```

> In M project
- The imports set up core infrastructure (TypeORM, Redis, GraphQL)
- The imports also bring in feature modules (Auth, Users, Chat)
- The providers define application-wide services (VertexAI, GCS, PubSub)
### App module - Imports
- Import other modules
```ts
imports: [
	TypeOrmModule.forRootAsync(...), // Database connection
	RedisModule.forRootAsync(...), // Redis connection
	GraphiQLExplorerModule.forRoot(...), // GraphQL playground
	GraphQLModule.forRoot(...), // GraphQL configuration
	AuthModule, // Authentication module
	UsersModule, // Users functionality
	LoggerModule, // Logging functionality
	ChatModule, // Chat functionality
	// etc...
]
```

### App module - Providers
- [[Service]], [[Factory]], [[Repository]] that used to be injected to component
- [[Singleton instance]]
```ts
providers: [
	VertexAIService, // AI service implementation
	GCSService, // Google Cloud Storage service
	PubSubService // Publish/Subscribe service
]
```

### App module - Exports
- Make it available to other modules
```ts
// In chat.modules.ts
@Module({
	providers: [ChatService], // The actual service implementation
	exports: [ChatService] // Make it available to other modules
})
export class ChatModule {}
  

// In app.module.ts
@Module({
	imports: [ChatModule], // Import to use ChatService
})
```

### Database and Schema
**Database**:
- The top-level container that holds all data
**Shema**:
- A Namespace/container within a database

### [[multi-tenant architecture]] with schema-based separation
- Each tenant get their own schema in the same database
- Data isolation is achieved through schemas
```ts
const dataSourceOptions: DataSourceOptions = {
	type: 'postgres',
	database: ENV_CONFIGS.POSTGRESQL.RDS_DATABASE, // Same database
	schema: tenantConnection.schemaName, // Different schema per tenant
	// ...
};
```

##### Flow of db query
1. System identifies the tenant
2. Uses appropriate schema connection
3. All queries automatically scope to tenant's schema
4. Data remains isolated between tenants