Task 3: Assess a Design

Does this design meet the specification of the software system needed by InnovateNow?

Specification Requirements:
1. Users can post short descriptions of novel ideas
2. Ideas can be polled by the general public and/or a select group of elite audience
3. Feedback should indicate whether ideas have potential
4. Innovators can collect demographic and customer information through polling results

Analysis of the Design:

Requirements Met:
- Posting ideas: The relationship between User and Idea (1 User to 0..n Ideas) allows users to post multiple ideas
- Polling functionality: The Poll entity exists and is associated with Ideas (0..n Ideas to 0..* Polls)
- Different audiences: The Audience entity exists and is associated with Polls (1 Poll to 0..n Audiences), which could represent general public vs. elite audience
- Demographic collection: The Demographics entity exists and is associated with Users

Issues and Shortcomings:

1. Ambiguous Poll-Idea Relationship:
   - The relationship shows 0..n Ideas to 0..* Polls with aggregation from Poll to Idea
   - This suggests a Poll can aggregate multiple Ideas, but the specification implies one Poll per Idea
   - Issue: The specification suggests each Idea should have its own Poll, not multiple Ideas in one Poll
   - Recommendation: Should be 1 Idea to 0..1 Poll (an Idea can have zero or one Poll)

2. Demographics Relationship is Backwards:
   - The relationship shows 1 User to 0..n Demographics with navigability from Demographics to User
   - This suggests one User can have multiple Demographics records, which makes sense (demographics can change over time)
   - However, the navigability arrow from Demographics to User is unusual - typically Demographics would be a component of User
   - Issue: The relationship direction suggests Demographics "uses" User rather than User "has" Demographics
   - Recommendation: Should use composition (filled diamond) from User to Demographics, indicating Demographics cannot exist without a User

3. Missing Feedback/Response Entity:
   - The specification requires collecting "feedback" on ideas, but there's no Feedback or Response entity in the design
   - Issue: How is feedback stored? Is it part of Poll? How do we track individual responses?
   - Recommendation: Add a Response or Feedback entity that connects Poll, User, and Idea to store individual feedback

4. Audience Concept is Unclear:
   - The design shows Poll composed of Audiences, and Audience aggregates User
   - Issue: Does "Audience" represent the type (general public vs. elite) or the collection of users? The specification mentions "general public and/or a select group of elite audience" - this could be:
     - Option A: Two types of audiences (general vs. elite) - needs an AudienceType attribute
     - Option B: Audience is just a collection mechanism
   - Recommendation: Clarify whether Audience should have a type attribute or if there should be separate GeneralAudience and EliteAudience classes

5. No Way to Track Poll Results:
   - There's no entity to store individual votes/responses
   - Issue: How do we know what feedback was given? How do we aggregate results?
   - Recommendation: Add a Response/Vote entity that connects User, Poll, and Idea

Does this design adhere to "High Cohesion" and "Low Coupling"?

Cohesion Analysis:

Good Cohesion:
- User-related concepts (User, Demographics) are grouped together
- Polling-related concepts (Poll, Audience) are grouped together
- Idea is a clear, focused entity

Cohesion Issues:

1. Demographics Separation:
   - Demographics is separate from User but represents user attributes
   - Issue: Low cohesion - user data is split across two entities when it could be one
   - Impact: Requires joins to get complete user information
   - Recommendation: Consider making Demographics an attribute of User, or use composition to show it's part of User

2. Audience as Separate Entity:
   - Audience aggregates User but seems to be just a collection mechanism
   - Issue: If Audience is just "a group of users for a poll," it might be better represented as a many-to-many relationship between Poll and User
   - Impact: Adds unnecessary indirection
   - Recommendation: If Audience doesn't have its own attributes/behavior, consider removing it and using a direct Poll-User relationship with an "audience type" attribute

Coupling Analysis:

Good Coupling:
- The design uses associations rather than tight inheritance hierarchies
- Entities are relatively independent

Coupling Issues:

1. Poll-Idea Aggregation:
   - Poll aggregates Idea (open diamond), suggesting Poll depends on Idea
   - Issue: If the relationship should be 1-to-1 (one Poll per Idea), this creates unnecessary coupling
   - Impact: Changes to Idea structure might affect Poll unnecessarily
   - Recommendation: Use simple association or composition from Idea to Poll (Idea "has a" Poll)

2. Audience-Poll Composition:
   - Poll is composed of Audiences (filled diamond), meaning Audiences cannot exist without Poll
   - Issue: This might be too restrictive - what if we want to reuse audience definitions?
   - Impact: High coupling - Audience is tightly bound to Poll
   - Recommendation: If audiences can be predefined or reused, use aggregation instead of composition

3. Missing Abstraction Layers:
   - Direct relationships between all entities create a web of dependencies
   - Issue: No service layer or interfaces to reduce coupling
   - Impact: Changes in one entity might ripple through the system
   - Recommendation: Consider introducing service classes or interfaces to mediate relationships

Recommended Design Improvements:

1. Fix Relationships:

Idea --[composition, 1 to 0..1]--> Poll
  (An Idea has zero or one Poll, Poll cannot exist without Idea)

Poll --[association, 1 to 0..n]--> User
  (A Poll has many Users who can participate, with an "audienceType" attribute: GENERAL or ELITE)

User --[composition, 1 to 1]--> Demographics
  (A User has exactly one Demographics record, Demographics cannot exist without User)

2. Add Missing Entities:

Response/Vote Entity:
- Connects: User, Poll, Idea
- Attributes: responseText, rating, timestamp
- Relationship: Many Responses belong to one Poll, one User, and one Idea

3. Simplify or Clarify Audience:

Option A: Remove Audience entity, add audienceType to Poll-User relationship
Option B: Keep Audience but add type attribute (GENERAL, ELITE)

4. Improve Cohesion:

- Make Demographics a component of User (composition)
- Group polling-related entities (Poll, Response) together
- Consider a PollService to handle polling logic separately from entities

5. Reduce Coupling:

- Use interfaces for services (IPollingService, IFeedbackCollector)
- Separate data entities from business logic
- Consider a mediator pattern for coordinating Poll, Idea, and User interactions

Conclusion:

The design captures the basic entities needed but has several issues:
- Missing feedback/response storage mechanism
- Unclear or incorrect relationship cardinalities
- Demographics relationship direction is counterintuitive
- Audience concept needs clarification
- Some coupling issues that could be improved

With the suggested improvements, the design would better meet the specification and exhibit higher cohesion and lower coupling.
