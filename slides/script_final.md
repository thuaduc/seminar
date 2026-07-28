Hi, my name is Duc.

The topic of my seminar is **agentic AI for code generation**.

I'd like to start by asking you a couple of questions.

Who here has ever written a line of code?
And who wrote code before ChatGPT was released?
Finally, who has used an AI coding agent, such as Claude Code or Codex, to write code?

I think we can all agree that the way we write code has changed dramatically.
We use to do everything manually.
We wrote the code, compiled it, ran it, tested it, and debugged everything ourselves.
IDEs introduced features such as auto-completion.
Pressing Tab could finish a line of code for us, but we still had to do most of the work manually.

Next came large language models like ChatGPT.
They could generate an entire function or even an entire file from a single prompt.
However, we still had to copy the generated code and do the rest manually.

Then, in 2024, the first generation of **agentic coding systems**.
Instead of only generating code, these systems can work directly on an entire repository.
They locate the relevant files, modify the code, generate and run tests, compile the project, execute linters.
In the end, they deliver the feature or bug fix you requested.

This rapid evolution is what motivated my seminar.
The goal of this survey is to understand how modern agentic coding systems work.
In particular, I wanted to answer three questions.
First, how are agentic coding systems built?
Second, how do we measure their performance?
Third, what are the current challenges of agentic AI

Before we begin, I'd like to define a few important terms.

A **scaffold** is everything surrounding the language model that enables it to act.
This includes prompts, tools, control flow, and the overall orchestration.
A **tool call** is a single action that the scaffold allows the model to perform.
For example, opening a file, editing code, or running a command.
An **oracle** is an external component that verifies whether the task has been completed correctly.
Examples include a compiler, a test suite, or even a human evaluator.
A **critic** is another model, or a specialized component, that evaluates candidate outputs and scores their quality.



Definition. 

The popular coding systems share the same high-level architecture.
They operate as an iterative loop.
The process begins by understanding the user's request.
This could be a bug report or a feature request.

Next, the system creates a plan.
Based on this plan, it generates code and applies the changes to the repository.
Afterward, it executes the code it generated.
Finally, it enters a refinement stage.
Based on the execution results, the agent decides whether it should stop, revise the plan, or generate another solution.

Every stage uses an interface that stay between the language model and the system.
By itself, a language model can only receive input and generate output.
It cannot directly interact with files or execute programs.

The interface acts as the model's eyes, hands, and tools, allowing it to manipulate the repository.
In practice, there are two common designs.

The first is the **Action-Computer Interface**, or **ACI**.
It provides functions for opening files, scrolling through code, editing files, searching the repository, and executing commands.

The second design is called **Code-as-Action**.
Instead of using DEDE-like operations, the model interacts entirely through terminal commands.
DE
NoNow let's take a closer look at the stages of the loop.
No
WeWe'll start with the **planning stage**, the most important stage.

The planning stage has two main objectives. First, the agent needs to determine **which files** are relevant to the task. In a large repository with thousands of functions and hundreds of files, finding the correct location is already a challenge. Second, it must decide **what changes** need to be made. Should it modify a function, change an interface, or update a function signature?

To solve this problem, the agent needs a strategy for exploring possible solutions. There are several approaches, including **Chain of Thought**, **Tree of Thoughts**, and **Graph of Thoughts**.

Let's look at **Chain of Thought** as an example.

Suppose the issue is: *"The application crashes when checking out with an empty cart."*

The model starts by reasoning about possible causes. It might suspect that an empty cart results in a divide-by-zero error. It then uses its tools to locate the relevant function and opens `cart.py`.

Suppose it finds the following line:

`return sum(price) / len(items)`

The model now reasons that `len(items)` becomes zero when the cart is empty. It inspects the surrounding code to confirm that there is no guard handling this case.

Based on that reasoning, it updates its plan. It decides to modify `cart.py` by adding a condition that handles the edge case.

I'll skip **Tree of Thoughts** and **Graph of Thoughts** for now, but we can come back to them during the Q&A if you're interested.

After we have a plan, the next step is **code generation**.

One common approach is called **infilling**. The scaffold opens a gap in the source code, and the model fills in only the missing part. 

This works well because language models are trained with **next-token prediction**, so completing a missing code fragment is what the model are trained for.
Another advantage is that the scaffold already knows exactly where the edit belongs, so the generated code is inserted into the correct location. 

The downside is that it edits only one region at a time, which makes it less efficient for big changes.

Another approach is **diff generation**. Instead of filling a single gap, the model generates a patch describing all the edits for the entire file. 

This makes it possible to perform multiple edits at once.
But the model has to specify the exact location of every change. 
If the patch is even slightly incorrect, it gonna fail to apply.

A third approach is **Best-of-N generation**. 
Instead of generating one solution, the model produces several candidate patches. 

An external **oracle** then selects the best one. This increases the chance of finding a correct solution, but it also increases the computational cost because it requires more model calls and more execution time.

After generating the code, the next stage is **execution**.

Here, the agent validates its changes by compiling the project, running the relevant test cases, and executing tools such as linters and type checkers. 
The execution results are then passed to the final stage of the loop: **refinement**.

The goal of refinement is to decide what happens next.

There are several refinement mechanisms. 
These first three all work the same way: 
The model reads back its own output and tries to fix it.

These methods do improve performance. 
But they have a ceiling: If the model could always recognize its own mistakes simply by reading its output, it would have generated the correct solution in the first place.

To overcome this limitation, many systems rely on external feedback. 
They feed the test, the traces back into the model to help with reasoning and make a better decision. 

External tests can genuinely surprise the model. 
But they are also limited. 
Tests catch only the cases they run, not a guarantee of correctness

The systems I surveyed differ in these design choices. 
They use different interfaces, planning strategies, execution pipelines, and refinement mechanisms. 
**SWE-agent**, **Agentless**, and **AutoCodeRover**, are research systems. 
**OpendHands** and **Devin** are commecialized.

So far, we've looked at how agentic coding systems work. 
The next question is: **how do we measure their performance?** 
How do we know whether one system is actually better than another?

Anthropic released Sonnet 5 and open AI released GPT5.6. 
And well, they use a lot of different benchmark to proof the power of their model. 
In the seminar, I took a closer look into **SWE-bench**, 
which is one of the oldest and most popular benchmark for coidng agent. 

SWE-bench consists of more than **2,000 real software engineering tasks**. These tasks are created from merged pull requests that closed real GitHub issues. 
They come from twelve Python projects, such as Django, scikit-learn, and Matplotlib.

For each task, the AI system receives two things: the issue description in natural language and the repository before the fix. 
Its goal is to generate the correct patch.

A task is considered solved if the previously failing tests now pass,
while all the tests that were already passing continue to pass, so no regressions.

Over time, several splits of SWE-bench have been introduced.

The first is **SWE-bench Lite**, which mainly contains issues that require changes to only a single file.

Later, OpenAI introduced **SWE-bench Verified**. In this version, every task was reviewed by software engineers. They checked whether the issue description was clear, whether the tests were fair.

Multilingual versions have also been added to include additional programming languages.

Eventually, OpenAI replaced the Verified benchmark with **SWE-bench Pro**, which focuses on more heavy software engineering tasks, multiple files, and larger code changes.

So why was the Verified version retired?

To understand that, let's take a deeper look at how it was constructed.
When human annotators reviewed the tasks, they also estimated how long an experienced software engineer would need to solve each issue. Based on this estimate, the tasks were divided into three categories.

**Easy** tasks could be solved in less than 15 minutes.
**Medium** tasks required between 15 minutes and one hour.
**Hard** tasks required more than one hour.

Now let's look at the progress over time.

Between April 2024 and April 2025, the best reported solve rate on SWE-bench Verified increased from roughly **20%** to about **65%**.
 To this date, the frontier model have a resolved rate of more than 80%.

This looks like progress. **AI agents solved software engineering?**

Actually, **not quite**. If we break the results down by difficulty, we see that nearly all of the improvement comes from the **easy** and **medium** tasks.

These two categories make up roughly **91%** of the benchmark, 
Performance is only around **20%** for hard task.


So we learn that agentic coding systems perform very well on short, well-defined problems.
However, they still struggle with long-horizon, multiple file changes

What are the challenges? What prevent agentic coding systems from solving these harder problems?

1. Intent capture is the one stage with no feedback. The agent reads the issue once, decides what it means, and never revisits that. Nothing ever asks it to check. If we trying to solved a problem but the problem is wrong, then we can not we right.

2. The second challenge is that the repo doesn't fit in the context window. The agent can not look at all files. Its also hard to always find the correct files.


3. Passing the tests is not being right

Third: the oracle we lean on is not the thing we actually want. A test suite is a sample of intended behaviour, not the behaviour itself. So a patch can turn the tests from failing to passing and still be wrong.