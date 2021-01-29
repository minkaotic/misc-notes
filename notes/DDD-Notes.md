# Domain Driven Design
DDD (coined by Eric Evans in the [book](https://www.hive.co.uk/Product/Eric-Evans/Domain-Driven-Design--Tackling-Complexity-in-the-Heart-of-Software/753867) of the same title) is comprised of a [philosophy](#ddd-philosophy), [strategy](#ddd-strategy), and some [patterns](#ddd-technical-patterns) that derive from them.

## DDD Philosophy
### Model
= a system of abstractions that describes selected aspects of a domain and can be used to solve problems related to that domain.

**Getting the model right:** if the model used in a software system matches the underlying domain well, everything 'snaps into place' better: "the software will accept enhancements more easily, and it has a much better chance of surviving and thriving intact for years" ([1](#sources)). Achieving and maintaining this alignment of the domain and the model - and thereby "maintaining the conceptual integrity of the model of your application" ([4](#sources)) - is at the heart of DDD.
- This isn't a one-off activity, but an **ongoing process** - "it requires patience to keep refining, refactoring, iterating, and accepting feedback until the model, the code, and the business coalesce into a cooperative synergy" ([1](#sources)).

### Ubiquitous Language
= a language structured around the domain model, capturing the terminology that describes the domain well, and which is used by all team members (technical or otherwise). ([2](#sources))

This language is developed in close collaboration with domain experts (who are its source of truth), and then should be used everywhere.
- Avoid ambiguity or inconsistency: be **rigorous** - don't use multiple terms for the same concepts or vice versa.

**Favour domain language over implementation language** - always aim to describe what a piece of code is trying to achieve (its purpose; domain activitiy), rather than reflecting how this is achieved. For example:
- Rather than `CreateCustomer()` (implementational), call the method `SignUpForSubscription()` (purpose)


## DDD Strategy
### Bounded Contexts
Break large, complex domain models up by dividing them into **bounded contexts** - which will have some concepts unique to each context as well as some shared concepts ([3](#sources)):
![bounded context diagram](/images/bounded-contexts.png)

"The maximum extent a model can be stretched without compromising its conceptual integrity is called a context." ([4](#sources)) In [Domain Driven Design](https://www.hive.co.uk/Product/Eric-Evans/Domain-Driven-Design--Tackling-Complexity-in-the-Heart-of-Software/753867), a *Context* is defined as:
> **"The setting in which a word or a statement appears that determines its meaning"**

- For example: "account" will mean something different in a banking context, or a user authentication context
- A "customer" might need to be modelled with a different set of properties in a sales context or a support context
- Similarly, a "deal" will be a different set of properties in a results grid or in a rewards journey

The act of determining the contexts in which certain terms begin meaning different things, or require being modelled with different properties, is called **Context Mapping**. "When our application spans multiple contexts, we need to manage also what happens in-between. The relationship between different bounded contexts often provides very important insight about our project." ([4](#sources))

The **direction** of the relationship between two contexts is also important: An **upstream context** is one that influences its **downstream** counterpart, while the opposite might not be true.

#### Contexts and team scaling
Agreeing on a model requires sophisticated forms of communication and involves "a common understanding of the problem and a similar view on the possible solution" ([4](#sources)). As an organisation grows, it becomes harder to share & preserve the conceptual integrity of the model between different teams.

"In scenarios where communication is not that easy, doing becomes a lot cheaper than agreeing." ➭ This leads to a duplication of concepts and approaches in the code base (such as multiple classes doing similar things, or the same problem being addressed in multiple opposing ways) ([4](#sources)):

![Contexts diverging at scale](/images/ddd-contextmapping-scale.jpg)

When views of the model diverge - and resulting code problems arise, it signals "that there are probably not-so-well-separated contexts in play, on the same area of the project. **Sometimes, this would make two separated contexts a more efficient way to structure our domain model, than forcing two different teams to continuously integrate their vision.**" ([4](#sources))

Where contexts have not been clearly separated, they end up partially overlapping, with their relationship being unclear. As a result, the related code will be at higher risk of turning turn into a [Big Ball of Mud](http://www.laputan.org/mud/), unless we do something about it.

**To resolve this:**
1. acknowledge that there are different models in play within our application
1. focus on progressive context mapping through learning more about the systems involved, working towards higher conceptual integrity


## DDD Technical Patterns
- **Repository:** encapsulates persistence and search - typically database
- **Service:** stateless API entity used for aggregate root CRUD
- **Aggregate Root:** an entity whose other child composite entities lack appropriate meaning without it - perhaps the most important aspect from an API perspective when talking about DDD
- **Value Object:** entity whose state does not change after instantiation (e.g. Color), particularly useful in multithreaded programming because using such eliminates concurrency issues
- **CQRS** - to do:
- [ ] https://martinfowler.com/bliki/CQRS.html
- [ ] https://en.wikipedia.org/wiki/Command%E2%80%93query_separation#Command_query_responsibility_segregation


## Sources:
1. https://techbeacon.com/app-dev-testing/why-you-need-domain-driven-design-even-though-you-think-you-dont
2. https://martinfowler.com/bliki/UbiquitousLanguage.html
3. https://martinfowler.com/bliki/BoundedContext.html
4. https://www.infoq.com/articles/ddd-contextmapping/
5. https://en.wikipedia.org/wiki/Domain-driven_design

Still To Do:
- [ ] https://www.infoq.com/minibooks/domain-driven-design-quickly/
