# DEVELOPER WEEK EUROPE 2023
## Agile Evolution: An Enterprise Transformation That Shows That You Can Too
(Martin Hinshelwood)

https://developerweekeurope2023.sched.com/event/1KC2f/open-talk-agile-evolution-an-enterprise-transformation-that-shows-that-you-can-too

-> actually ended up being a talk about Microsoft's journey to agile, focusing on practices of Azure Devops team!

### Intro
- **DevOps:** "Union of people, process and products to enable continuous delivery of value to end users"
- Agile + Devops essentially the same thing, just looking at it from different perspectives
- Company experiencing 'epic failure' is a great point in time for bringing in change (Ken Schwaber)
- Example: MS Windows Vista - massive bug smash, poor quality product -> massive ccost in brand recognition
- Example: MS Windows 8 - mismatch to customer desires -> cost 12 billion dollars in brand recognition (people moving to Mac)
- Made MS realise they had to close the feedback loop, which was 6 years at the time
- Assumptions of value and usability would be built up over those 6 years
- (both Agile and DevOps focus on closing those feedback loops)
- Azure Devops team were catalyst for MS moving to much greater degree of agility
- long journey, 'first sprint' in 2010; 2012 was the last 'waterfall' release

![image](https://user-images.githubusercontent.com/6144523/234564201-5da86272-52dc-46c3-83d7-f2c786d4101e.png)

![image](https://user-images.githubusercontent.com/6144523/234558356-464b96b6-2463-4c91-a916-16b8369aabea.png)

![image](https://user-images.githubusercontent.com/6144523/234558935-fa704da4-602e-4ef8-a8f3-7bf4294745bf.png)

### Be Customer Obsessed
- qualitatively: have an easy way for customers to submit feedback embedded in your product; review customer questions in common support forums incl Stack Overflow
- quantitatively: data on usage / journey drop offs / etc.
- "Definition of done:" - checklist to ensure required organisational quality standards -> "Live in production, collecting telemetry supporting or diminishing the starting hypothesis"
- Application insights: high volume ingestion / text search queries over structured or semi-structured data / fast queries over very large data set
- collect data broadly, but carefully
- customers will tell you they want a feature, when in reality they won't end up using it anywhere near as much - so data collection on usage really important
- Also: Measure what's important in terms of engineering standards (KPIs), such incident metrics
- USAGE: : acquisition, engagement, satisfaction, churn, feature usage
- VELOCITY: time to build, time to self test, time to deploy, time to learn
- LIVE SITE HEALTH: time to detect // communicate // mitigate, customer immpact, SLAs, Customer support metrics
- What NOT to monitor? original estimate, completed hours, lines of code, team capacity, team burndown, team velocity, # of bugs found (really all that matters is bug *trend* - is the number going up or down?)

### Iterating over the pain
- long journey from 2-year delivery to 3 week sprints
- "Find the part of your process in getting value to customers that slows you down or hurts _the most_. Make it _incrementally better_ each sprint. Re-evaluate and improve the next most painful."
- "Stabilization sprints" (dedicated to paying back technical debt or fixing bugs) are a very bad idea: they will ramp up tech debt as people default to "we'll fix it later" so bug trend actually went up significantly (my thought: contradicts 'build quality in')
- Feature flags to control exposure; reduces integration debt
- MS now use this a lot, even on a user level to get feedback when users raise specific issues

### Production first mindset
- admit when you made mistakes; transparency is super important
- includes transparancy in communication on incidents
- automate COMPLETELY (not just partially) - all the way to production, but work out how to control the blast radius

ME: Does 'automate to production completely' rule out manual product testing (to supplement automated test suites) along the way?
-> Speaker: yes absolutely it does, there is no room for UAT: get criteria of what is needed to be happy to deploy, and automate them
Book a sesh to discuss further: https://nkdagility.com/book-online/

Further reading: https://nkdagility.com/learn/agile-delivery-kit/whitepapers/enterprise-evolution-that-shows-that-you-can-too/

![image](https://user-images.githubusercontent.com/6144523/234567709-f9ab7a56-4050-460b-bbd5-4491878691aa.png)

--------------

## Code Review Is Too Little Too Late - Shift Left the Code Review Process
(Omer Rosenbaum)

https://developerweekeurope2023.sched.com/event/1KRuW/open-talk-code-review-is-too-little-too-late-shift-left-the-code-review-process

- same mistakes repeating themselves & depending on people with knowledge catching it each time
![image](https://user-images.githubusercontent.com/6144523/234853899-855268f1-0a8a-4e84-8b95-a5412a41ecc1.png)

### Why should we review code?
- Enforce coding standards
- Find bugs
- Share knowledge
- Ensure code is clear / readable
- Verify code does what it is supposed to do

### Where does it come in the lifecycle?
![image](https://user-images.githubusercontent.com/6144523/234850508-5f2f22e1-6bbd-4cfa-b2df-51c92e951fd2.png)

Is this too late? -> It depends!

![image](https://user-images.githubusercontent.com/6144523/234850912-12f0a858-0709-45dc-a942-2f8f3ab2c5b5.png)

- Best practices / coding standards often captured in wikis -> but relying on people memorising this feels old school and not really that helpful -> use linters instead!
- Can expand use of linters to avoid bugs too (i.e. not doing dangerous casts; not using functions that might have side effects)
- But Linters are limited to "don't do X" (+ sometimes "use Y instead")
- Code-coupled docs that can pop up relevant documentation in place -> great for documenting more complex best practices; these can be used to automate feedback into future codings
- both Linters and Code-coupled docs allow to "shift CR benefits left"

My own takeaways:
- don't waste CR resource on things like style guide lines and coding standards
- automate whatever can into linting
- we haven't really invested much time into making linters (or formatting defaults in VS) actually our friend
- give Swimm a go - https://swimm.io/

--------------

## Are We Still Building For Developers?
(Gertrude Westrin)

https://developerweekeurope2023.sched.com/event/1KoE6/open-talk-are-we-still-building-for-developers

- "Developer Advocacy" / internal developer enablement
- about how efficient your devs can be; giving them what they need, when they need it
- Rather than 10x devs - "what you want is someone who can come in and make 10 developers more productive" (Kelsey Hightower)

### Getting started
1. Establish stakeholders (of a project) - reach out and ask lots of questions
2. Demos
3. Get feedback
4. Clear CTA
5. Incentivise

--------------

## Secret Shortcuts of Loading Web Performance
(Nikola Mitrovic)

https://developerweekeurope2023.sched.com/event/1LELC/open-talk-secret-shortcuts-of-loading-web-performance?iframe=no

### What is Loading Performance?
![image](https://user-images.githubusercontent.com/6144523/234886109-6ecb2baa-596b-48e0-a809-04dece4deb74.png)

Javascript is the biggest cause of poor loading performance on the web today

### Standard optimization techniques
- minimizing & compressing files (esp JavaScript)
- using tree-shaking
- inlining critical CSS
- using next-gen image formats
- apply caching headers
- use server-side rendering

- code splitting = chopping bundle up into lots of smaller JS bundles, rather than one epic on
- In react -> Route-based code splitting or Component-based code splitting
- Reduce usage of libraries

--------------

## It’s Not Them, It’s You: How to Succeed as a Remote Leader
(Inna Weiner)

https://developerweekeurope2023.sched.com/event/1JhCx/featured-talk-its-not-them-its-you-how-to-succeed-as-a-remote-leader

- Remote (implying some centre of gravity) vs distributed vs hybrid

### High-context vs low-context cultures
![image](https://user-images.githubusercontent.com/6144523/234902579-4c3b3d5f-60b6-4477-a635-586617cfd218.png)





