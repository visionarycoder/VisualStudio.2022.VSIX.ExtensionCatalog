# GitHub Copilot Instructions

## Technology Preferences

- Prefer Microsoft technologies
- Prefer Microsoft associated libraries and NuGet packages.
- Prefer .Net Foundation supported projects when recommending NuGet packages or solutions.

## Language and Framework

- Use C# for both client-side and server-side code.
- Use the lastest version of .Net for all projects.
- Use the lastest language version in the project definition.
- Use the latest code quality standards in the project definition.

## Architecture

- Always use Volatility-based decomposition.

### Volatility-Based Decomposition

#### Component Definition

- **Manager Classes**: Execute workflow activities.
- **Engine Classes**: Implement business rules.
- **Access Classes**: Abstract data persistence.

#### Component Structure

- Each component will have at least 2 projects: Contract and Service.
- The Contract project will define one or more interfaces with all related models.
- The Service project will implement the public interface.
- The service project can only return models defined in its Contract project.
- Access projects may have an Orm project.  Orm models may not be shared via the Contract project.

#### Layers

There are typically 5 layers in a VBD system.

- Client
- Api (optional)
- Manager
- Engine
- Access
- Resource

#### Communication

- Components can only call "down" through the layers: Client -> Manager -> Engine -> Access -> Resource
- Components cannot call "sideways" to directly communicate with peer components in the same layer.
- Components should never leak internal activities to the caller.

- Clients can call 1 or more Managers.
- Clients can publish messages to a message bus.

- Managers can call 1 or more Engines.
- Managers can call 1 or more Accessors.
- Managers can publish messages to a message bus.
- Managers can subscribe to message published on a message bus.

- Engines can call 1 or more Accessors.

- Accessors interact with the peristence mechanisms.  

- Persistence mechanisms may be a database, a file system, a 3rd party OpenAPI endpoint, etc.  

#### Communication Exceptions

- A manager can communicate with another manager via the message bus.

#### Infrastructure

##### IFx

A software library typically called the Ifx is a library of helpers and stock resources.
The Ifx is typically specific to a given solution.

##### Utilities

Utilities are packages typically shared across solutions.  

## Database Preferences

### Structured Data

- Use **SQLite** for local development.
- Use **SQL Server** for deployed environments.

### Unstructured Data

- Use Azurite for local development.
- Use Azure CosmosDB or Azure Blog storage.

## Testing

- Prefer xUnit or MSTest for writing unit tests in C#
- Use Moq for mocking dependencies in tests
- Use an in-memory database like SQLite for testing to avoid side effects on the production database.
- Define clear interfaces for module interactions.
- Use incremental integration to isolate and fix defects more easily.
- Use realistic data for testing.
- Automate integration tests to run frequently.
- Integrate tests into the CI/CD pipeline.

## Patterns

### Request / Response

- Use the ServiceMessaging library.
- All calls between components should use the ServiceMessageRequest and ServiceMessageResponse base classes to simplify communications across boundaries.
- Use the ServiceMessageFactory to create all service messages.

### Unit of Work Pattern

- Implement the Unit of Work pattern to manage transactions and ensure data consistency.

### Saga Pattern

- Use the Saga pattern to manage long-running transactions and ensure data consistency across multiple services.
- Implement compensating transactions to undo changes if part of the transaction fails.
