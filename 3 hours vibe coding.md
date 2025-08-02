reference: [[Guided coding with LLM]]
>  I came up with this workflow by testing and building a full-stack personal finance app in my spare time
#### Step 1: Laying the Foundation
Trying to get your LLM to *glue it all* **(full stack app)** together for you cohesively just doesn't work.
Solid foundation and leveraging: [[UI framework]], [[Template]], [[Styling]]. So that the LLM working consistently across the app.

**Central "source of truth"** the LLM can always reference to see how the app is defined as it builds new features.
I could have this with something like: `app.module`, `AndroidManifest`, ...

#### Step 2: Getting the Most Out of Your AI Assistant
Create a **comprehensive set of rules** for your editor and LLM to follow.
1. Start *building something*
2. Look out for times when the LLM (repeatedly) _doesn't meet your expectations_ and define rules for them
3. Constantly ask the LLM to help you improve your workflow
#### Define rules
 jkl;`.cursor/rules/`directory with multiple files.

I often like telling the LLM to `think about 3 different strategies/approaches, pick the best one, and give your rationale for why you chose it.` So I created a rule for it, `7-possible-solutions-thinking.mdc`

#### Using AI to Critique and Improve Your Workflow
Actively adding new rules and asking for feedback from LLM
>Can you review for breadth and clarity and think of a few ways it could be improved, if necessary. Remember, these documents are to be used as context for AI-assisted coding workflows.

#### Step 3: Defining the "What" and the "How" (PRD & Plan)
[[vertical slice method]]
#code-prompt
> From this PRD, create an actionable, step-by-step plan using a modified vertical slice implementation approach that's suitable for LLM-assisted coding. Before you create the plan, think about a few different plan styles that would be suitable for this project and the implementation style before selecting the best one. Give your reasoning for why you think we should use this plan style. Remember that we will constantly refer to this plan to guide our coding implementation so it should be well structured, concise, and actionable, while still providing enough information to guide the LLM.
#### Step 4: Building End-to-End - Vertical Slices in Action
[![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcyvbcp8xrihvz71pg6kb.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcyvbcp8xrihvz71pg6kb.png)
A different approach to a team.
**Team:** create full feature list, break down task, infra setup, db design, UI design,....
**Vibe:** feature by feature (*independent full stack feature*)
-> <mark style="background: #BBFABBA6;">Once the basis for these features was working smoothly</mark>, we could improve the complexity of them, and add on other sub-features, <mark style="background: #D2B3FFA6;">with little to no issues</mark>!

#### Step 5: Closing the Loop - AI-Assisted Documentation
**Document for LLM are much more important:** keeping track of why things were built a certain way and how the current implementation works

Materials: #mind-map
+ Key files related to the implemented feature: `auth.grahql`, `auth.resolver`, `auth.service`,...
+ Provide the relevant sections of the *PRD and the Plan that described the feature.*
+ Reference *the rule file* with the *Doc creation task*
- It focus on the **core logic**, how the different parts connect (DB -> Server -> Client), and any **key decisions made**, *referencing* the specific files where the implementation details can be found.
`ai/docs/`
+ For Humans: a clear, human-readable record of the feature for onboarding or *future development*.
+ For the AI: It built up a knowledge base within the project that could be *fed back into the AI's context* in later stages. This helped *maintain consistency* and reduced the chances of the AI forgetting previous decisions or implementations.
 Have a [[companion]] that has a huge knowledge set that helps you refine ideas and test assumptions is amazing as well