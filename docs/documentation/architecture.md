# Architecture

NEM Library works perfect in a [multitier architecture](https://en.wikipedia.org/wiki/Multitier_architecture), because 
it is the layer the between your business logic and the data tier. It enables the developers focus on the product rather than the 
NEM Blockchain specific API details due its higher abstraction.

![Tiers](../img/nem-library-architecture.png)

- **NEM Blockchain**: Where the truth resides.
- **NEM Library**: Abstraction layer.
- **Your Application**: Where the business logic resides.
- **Final Users**: People who benefit the NEM Blockchain Technology.

NEM Library has a functional programming approach to deal with the immutability of the NEM Blockchain. So, the developer will find
himself/herself comfortable with the easy way to fetch the state, and create new Transactions in order to change that state.

![Functional](../img/nem-library-functional-approach.png)

### Brief description of NEM Library organization

NEM Library follows the next rules:

- **Infrastructure**: The HTTP requests are done following the [Repository Pattern](https://msdn.microsoft.com/en-us/library/ff649690.aspx), and they return NEM Domain immutable 
models via the [Observable Pattern](https://en.wikipedia.org/wiki/Observer_pattern). So, the state fetched from the NEM Blockchain cannot be changed via code. 

- **NEM Domain models**: The NEM Domain models are [immutable](https://en.wikipedia.org/wiki/Immutable_object) by definition, the developer cannot change its attributes. 
Instead, the developer have to create new Transactions and dispatch them to NEM Blockchain via TransactionHTTP, in order to change the NEM Blockchain state.

## Characteristics

- **Standardized Contracts**: Guaranteeing interoperability and harmonization of data models.

- **Loose Coupling**: Reducing the degree of component coupling fosters.

- **Abstraction**: Increasing long-term consistency of interoperability and allowing underlying components to evolve independently.

- **Reusability**: Enabling high-level interoperability between modules and potential component consumers.

- **Stateless**: Increasing availability and scalability of components allowing them to interoperate more frequent and reliable.

- **Composability**: For components to be effectively composable they must be interoperable.

A key objective is for interoperability to become a natural design of the NEM Library, 
ideally to extend that components to work with other products or systems.

## Reactive

NEM Library uses [RxJS](http://reactivex.io/) as Reactive Library. See its docs [here](http://reactivex.io/rxjs/)

- **Functional**: Developers can avoid intricate stateful programs using clean input/output functions over
observable streams.

- **Less is more**:ReactiveX's operators often reduce what was once an elaborate challenge into a few lines
of code.

- **Async error handling**: Traditional try/catch is powerless for errors handling in asynchronous computations, but
ReactiveX will offer developers the proper tools to handling these sort of errors.

- **Concurrency made easy**: Observables and Schedulers in ReactiveX allow the programmer to abstract away
low-level threading, synchronization, and concurrency issues.

- **Frontend**: Simple manipulation of UI events and API responses on the Web using RxJS

- **Backend**: Embrace ReactiveX's asynchronicity, enabling concurrency and implementation independence.

- **Connection Pool**: NEM Library will offer a Connection Pool feature that will distribute load throughout the
network ensuring maintained connectivity, higher speeds and higher availability.