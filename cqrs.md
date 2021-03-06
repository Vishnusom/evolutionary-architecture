


## CQRS Naming Convention
It's hard to find proper CQRS naming convention, hence I've decided to find what's useful and add it here. Some useful guideline is:
- separate read and write
- separate command and events
- use `verb-noun` for commands
- use `noun-verb` for events in past tense

E.g. Write Service

| Command | Event |
| --------|-------|
| CreateEvent | EventCreated |
| ChangeCustomerLocale | CustomerLocaleChanged |
| MakeCustomerPreferred | CustomerPreferredMade |
| EditCustomerDetails | CustomerDetailsEdited | 

Note that it is best to avoid `REST` terms like `UPDATE`, because there's a loss of information here (what is being updated?). In other words, be as specific as possible and group necessary fields that needs to be updated. E.g. `UpdateUserPreference`, `UpdateUserAddress`. If possible, all `WRITE` services should be logged, since they change the state of the application, unlike `READ` services.

E.g. Read Service

| Command | Event |
| --------|-------|
| GetCustomer | - |
| GetCustomerByName | - |
| GetPreferredCustomer | - |

For `boolean/flags`, you can use the term `Mark/Set` to indicate status change. Don't use ambiguous term such as `Toggle` - they do not provide information of the end state of the action. E.g. when delivering a food - `MarkFoodDelivered`, or order `MarkOrderConfirmed`.

For `update`, note that it can give more meaningful context compared to traditional CRUD. Say we have a separate Ops team to handle user orders, and the user called the customer service to amend the changes made to the order (which is not possible through the webui), we can create an event called `AmendCustomerOrder`, rather than the `UpdateCustomerOrder`, which can serve as a meaningful metrics to find out how many customers made mistakes or other useful data.


## TODO
Summarize it. And how to implement one.

In what situation can CQRS be used, and when it should not be used?

## Glossary

- `command` is the only way to change the state of the system. Commands are responsible for introducing all changes to the system. Command should not return any value. `command` is just a plain object that is used by `command handler` as input value (params) for the operation it represents.
- `query` is a read operation. It reads the state of the system, filters, aggregrates and transforms data to deliver the most useful format. Multiple execution should not affect the state of the system.

https://www.future-processing.pl/blog/cqrs-simple-architecture/
https://medium.com/@fagnerbrack/the-power-of-commands-and-events-ec6d2b3f856d

https://medium.com/@odedia/listen-to-yourself-design-pattern-for-event-driven-microservices-16f97e3ed066
https://blog.couchbase.com/saga-pattern-implement-business-transactions-using-microservices-part-2/
https://medium.com/@domagojk/patterns-for-designing-flexible-architecture-in-node-js-cqrs-es-onion-7eb10bbefe17
https://medium.com/@fagnerbrack/the-power-of-commands-and-events-ec6d2b3f856d
https://dzone.com/articles/saga-pattern-how-to-implement-business-transaction-1
https://medium.com/capital-one-tech/microservices-when-to-react-vs-orchestrate-c6b18308a14c
https://matthiasnoback.nl/2015/01/collecting-events-and-the-events-aware-command-bus/

## References
- https://github.com/efacilitation/eventric/wiki/Literature
- https://ordina-jworks.github.io/domain-driven%20design/2016/02/02/A-Decade-Of-DDD-CQRS-And-Event-Sourcing.html
- https://stackoverflow.com/questions/4688696/cqrs-how-to-model-a-scenario-execution-system/4699693#4699693
- https://stackoverflow.com/questions/29146167/timeseries-temporal-data-in-ddd-on-write-side-in-cqrs
- http://scottlobdell.me/2017/01/practical-implementation-event-sourcing-mysql/
- https://martinfowler.com/eaaDev/EventSourcing.html
- https://dev.to/barryosull/event-sourcing-what-it-is-and-why-its-awesome
- https://martinfowler.com/eaaDev/AgreementDispatcher.html
- https://www.hugopicado.com/2017/05/06/what-event-sourcing-is-not.html
- https://dzone.com/articles/our-experience-domain-events
- https://news.ycombinator.com/item?id=19072850
