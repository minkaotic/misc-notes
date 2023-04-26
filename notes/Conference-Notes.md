# DEVELOPER WEEK EUROPE 2023
## Agile Evolution: An Enterprise Transformation That Shows That You Can Too
(Martin Hinshelwood)
https://developerweekeurope2023.sched.com/event/1KC2f/open-talk-agile-evolution-an-enterprise-transformation-that-shows-that-you-can-too

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
- 




