# Step 1: Events

## TL;DR

Add a String email parameter to AttendeeRegisteredEvent.java:

```java
package dddhexagonalworkshop.conference.attendees.domain.events;

public record AttendeeRegisteredEvent(String email) {
}
```

## Learning Objectives

- Understand the role of Events in capturing business-significant occurrences.
- Implement an AttendeeRegisteredEvent to notify other parts of the system.

## What We Are Building

An AttendeeRegisteredEvent record that captures the fact that an attendee has successfully registered for the conference.

## Why Domain Events Matter

Domain Events are statements of fact that the business cares about.

Building around events solves several critical problems in distributed systems:

**Business Communication:** Events represent facts that have already happened in the business domain. Events are immutable statements of truth.

**System Decoupling:** When an attendee registers, multiple things might need to happen:
- Send a welcome email
- Update conference capacity
- Notify the billing system
- Generate a badge

By publishing an event like `AttendeeRegisteredEvent`, we enable different parts of the system to react independently without tight coupling. Each component can listen for this event and perform its own actions.

Having an audit trail is another advantage: Events naturally create a history of what happened in your system, which is valuable for debugging, compliance, and business analytics.

## Implementation

A Domain Event is a record of some business-significant occurrence in a Bounded Context. It's obviously significant that an attendee has registered because that's how conferences make money, but it's also significant because other parts of the system need to respond to the registration.
For this iteration, we'll use a minimal event with only the attendee's email address. Update  `AttendeeRegisteredEvent.java` with the email:

```java
package dddhexagonalworkshop.conference.attendees.domain.events;

public record AttendeeRegisteredEvent(String email) {
}
```

## Key Design Decisions

**Why a record?** Records are perfect for events because:
- They're immutable by default (events should never change)
- They provide automatic equals/hashCode implementation
- They're concise and readable

**Why only email?** In this iteration, we're keeping it simple. In real systems, you might include:
- Timestamp of registration
- Attendee ID
- Conference ID
- Registration type (early bird, regular, etc.)

## Testing Your Implementation

There is a JUnit test, `AttendeeRegisteredEventTest.java` which can be run in your IDE or from the command line. Update the test class to check that the `AttendeeRegisteredEvent` can be instantiated with an email address and that it behaves correctly as a record.  

```java
package dddhexagonalworkshop.conference.attendees.domain.events;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;

public class AttendeeRegisteredEventTest {

   @Test
   public void testAttendeeRegisteredEventContainsAllFields() {
       AttendeeRegisteredEvent event = new AttendeeRegisteredEvent("gandalfthegrey@istari.net");
       assertNotNull(event);
       assertNotNull(event.email());
       assertEquals("gandalfthegrey@istari.net", event.email());
   }
}

```

To run the test stop Quarkus with "ctrl + C" and then "./mvnw test -Dtest=AttendeeRegisteredEventTest":


```bash
ctrl + C

/mvnw test -Dtest=AttendeeRegisteredEventTest
```

The test should pass, confirming that the `AttendeeRegisteredEvent` record is correctly defined and can be instantiated with an email address.

## Next Steps

Next up is implementing a Command to trigger the registration worklow: [Step 2: Commands](02-Commands.md)

