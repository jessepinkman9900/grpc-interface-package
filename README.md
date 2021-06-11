# Permission
## Schema
- [Permission Schema](./docs/permission.md)


## Usage
#### 1. mongodb running on Atlas
- NOTE: nothing to do here, can use this uri to explore db on Mongodb Compass
- uri: `spring.data.mongodb.uri=mongodb+srv://spring-core:pwd@graphql.neo5p.mongodb.net/central?retryWrites=true&w=majority`
#### 2. run application
```shell script
./gradlew bootRun
```
#### 3. access interactive GraphQL editor
- on localhost
  - access `localhost:8080/graphiql`
  - try out the queries in the order given (due to inter-dependency)

## Queries And Mutations
- [Acl](./docs/entityIterationAcl.md)
