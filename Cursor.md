### # Claude with MCPs
- Exploring codebases and drawing diagrams
- Running all kinds of automation processes, from image conversions to video encoding
> Claude, on the other hand, allows you to think outside that box.
- [**Sequential Thinking MCP**](https://smithery.ai/server/@smithery-ai/server-sequential-thinking) — Enables multi-step workflows, making Claude think and plan in structured sequences.
- [**ClaudeComputerCommander**](https://github.com/wonderwhy-er/ClaudeComputerCommander) — Allows Claude to explore, read, and write files, as well as run and inspect long-running processes.
<mark style="background: #BBFABBA6;">Once **ClaudeComputerCommander** is installed, you can use it to **analyze a codebase and generate diagrams**</mark>
This looks promising as I could use it to visual new code base [[Petking5]] and [[Time Tracking]] and maybe the new [[OpenSource Apps]]
#### Sample diagram created by Claud
![](https://miro.medium.com/v2/resize:fit:1400/1*_N7MCVbaHEMdXZndmwuvzQ.png)
#### This flow is good for
- **Exploring an interesting repository** to quickly understand its structure.
- **Reviewing large projects**, getting a **high-level overview** before diving into details.
- **Creating documentation** 
#### What make Claude better?
Read in full code, while Cursor use *index* to save cost
#### Rewrite entire codebase [[code migration]]
And Claude did that for me. The rewritten version is **3.5k lines of code across 33 files**, while the original project was **5k lines across 21 files**, all written by hand.
![](https://miro.medium.com/v2/resize:fit:1400/1*R-G64WW0n8IHyFAHEaJo-w.png)
> I don't think this is legit. He actually successfully migrate the entire code base (5k lines) with Claude?

> I don’t want to read code that does not work, I want to read code that does

### Yolo mode
> <mark style="background: #FF5582A6;">Is this statement valid? </mark>Windsurf and Cursor make you accept changes and look in to the code all the time. Claude just does the work. And when it works, only then you review code.

When configured correctly, YOLO mode is like *having a senior developer working alongside you in real-time*, fixing errors as they appear.


### Vibe code and testing
#### Don’t “vibe test” your vibe coding (NUMBER 1 RULE)
Write your own tests. (Or at least *pair program* them with your coding copilot).
 Brainstorm any [[edge and corner cases]]

#### Versioning, env config and Libs
Many of the packages that get pulled into your app <mark style="background: #FF5582A6;">aren’t versioned correctly</mark>

#### You actual [[best practice test approach]]
-> Fill in later when I actually have interest in this topic - `testing`


### The Problem with Raw Vibes (tester perspective)
> Vibe coding is missing a **backbone**. It’s missing **intention**

 <mark style="background: #CACFD9A6;">But can it _validate_? Can it _correct itself_? Can it _know_ when it’s broken?</mark>
![[Pasted image 20250729204604.png]]

LLM failed even the **basic cases**. That is the *gap*, without telling, they don't know.

### What Every Vibe Coder Should Add
- `.windsurfrules` or `.cursorrules`
- Regression tests for known **fail states**
> **Code that feels good isn’t always good.**

 Even manually need to <mark style="background: #FF5582A6;">validate the generated (unit) tests</mark>

### 3 hours vibe coding
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
`.cursor/rules/`directory with multiple files.

I often like telling the LLM to `think about 3 different strategies/approaches, pick the best one, and give your rationale for why you chose it.` So I created a rule for it, `7-possible-solutions-thinking.mdc`

#### Using AI to Critique and Improve Your Workflow
Actively adding new rules and asking for feedback from LLM
>Can you review for breadth and clarity and think of a few ways it could be improved, if necessary. Remember, these documents are to be used as context for AI-assisted coding workflows.

#### Step 3: Defining the "What" and the "How" (PRD & Plan)
[[vertical slice method]]
#code-prompt
> From this PRD, create an actionable, step-by-step plan using a modified vertical slice implementation approach that's suitable for LLM-assisted coding. Before you create the plan, think about a few different plan styles that would be suitable for this project and the implementation style before selecting the best one. Give your reasoning for why you think we should use this plan style. Remember that we will constantly refer to this plan to guide our coding implementation so it should be well structured, concise, and actionable, while still providing enough information to guide the LLM.
#### Step 4: Building End-to-End - Vertical Slices in Action