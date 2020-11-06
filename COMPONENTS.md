```java
import org.axonframework.commandhandling.CommandHandler;
import org.axonframework.eventsourcing.EventSourcingHandler;
import org.axonframework.modelling.command.AggregateIdentifier;

import static org.axonframework.modelling.command.AggregateLifecycle.apply;

public class GiftCard {

    @AggregateIdentifier // 1.
    private String id;

    @CommandHandler // 2.
    public GiftCard(IssueCardCommand cmd) {
        // 3.
       apply(new CardIssuedEvent(cmd.getCardId(), cmd.getAmount()));
    }

    @EventSourcingHandler // 4.
    public void on(CardIssuedEvent evt) {
        id = evt.getCardId();
    }

    // 5.
    protected GiftCard() {
    }
    // omitted command handlers and event sourcing handlers
}
```

1. The `@AggregateIdentifier` is the external reference point to into the `GiftCard Aggregate. This field is a hard requirement, as without it Axon will not know to which Aggregate a given Command is targeted. Note that this annotation can be placed on a field and a method.

2. A `@CommandHandler` annotated constructor, or differently put the 'command handling constructor'.
   This annotation tells the framework that the given constructor is capable of handling the IssueCardCommand.
   
   The `@CommandHandler` annotated functions are the place where you would put your decision-making/business logic.
   
3. The static `AggregateLifecycle#apply(Object...)` is what is used when an Event Message should be published.

   Upon calling this function the provided `Object`s will be published as `EventMessages within the scope of the Aggregate they are applied in.

4. Using the `@EventSourcingHandler is what tells the framework that the annotated function should be called when the Aggregate is 'sourced from its events'.

   As all the Event Sourcing Handlers combined will form the Aggregate, this is where all the state changes happen.
   
   Note that the Aggregate Identifier **must** be set in the `@EventSourcingHandler of the very first Event published by the aggregate.
   
   This is usually the creation event. Lastly, `@EventSourcingHandler annotated functions are resolved using specific rules.
   
   These rules are the same for the `@EventHandler annotated methods, and are thoroughly explained in Annotated Event Handler.
   
5. A no-arg constructor, which is required by Axon.
   Axon Framework uses this constructor to create an empty aggregate instance before initializing it using past Events.
   Failure to provide this constructor will result in an exception when loading the Aggregate.