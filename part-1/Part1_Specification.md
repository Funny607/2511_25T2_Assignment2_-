# Part 1: Assignment Specification <!-- omit in toc -->

This page contains the tasks you’ll need to complete part 1 for this assignment, and how you’ll be assessed.

- [1. Assignment Parts](#1-assignment-parts)
- [2. Getting Started](#2-getting-started)
- [3. Tasks](#3-tasks)
  - [Task 1) Code Analysis and Refactoring (25 marks) ⚙️](#task-1-code-analysis-and-refactoring-25-marks-️)
    - [a) From DRY to Design Patterns (5 marks)](#a-from-dry-to-design-patterns-5-marks)
    - [b) Inheritance Design (5 marks)](#b-inheritance-design-5-marks)
    - [c) Open-Closed Goals (5 marks)](#c-open-closed-goals-5-marks)
    - [d) Open Refactoring (10 marks)](#d-open-refactoring-10-marks)
  - [Task 2) Evolution of Requirements (30 marks) 🔧](#task-2-evolution-of-requirements-30-marks-)
    - [Software Delivery - Task Lifecycle](#software-delivery---task-lifecycle)
    - [a) Slimes (15 marks)](#a-slimes-15-marks)
      - [Technical Specification](#technical-specification)
      - [Another Example](#another-example)
    - [b) Sun Stone \& More Buildables (15 marks)](#b-sun-stone--more-buildables-15-marks)
      - [Technical Specification](#technical-specification-1)
    - [c) Logic Switches (20 marks)](#c-logic-switches-20-marks)
      - [a) Logical Entities](#a-logical-entities)
      - [b) Logical Rules](#b-logical-rules)
      - [Technical Specification](#technical-specification-2)
- [4. Titbits](#4-titbits)
  - [4.1. Git Practices 🧭](#41-git-practices-)
  - [4.2. Assumptions 👾](#42-assumptions-)
  - [4.3. Test Design 🐝](#43-test-design-)
  - [4.4. Dungeon Map Helper](#44-dungeon-map-helper)
- [5. Design](#5-design)
  - [5.1. Pair Blogging](#51-pair-blogging)
  - [5.2. Individual Blogging](#52-individual-blogging)
- [6. Assessment](#6-assessment)
  - [6.1. Marking Criteria](#61-marking-criteria)
  - [6.2. Submission](#62-submission)
  - [6.3. Dryruns](#63-dryruns)
    - [6.3.1.Ensuring your code **compiles** with our tests through GitLab CI/CD pipelines.](#631ensuring-your-code-compiles-with-our-tests-through-gitlab-cicd-pipelines)
    - [6.3.2. Ensuring your code **compiles and passes** with our tests through GitLab CI/CD pipelines.](#632-ensuring-your-code-compiles-and-passes-with-our-tests-through-gitlab-cicd-pipelines)

# 1. Assignment Parts

**This assignment has two parts, of which this is the first**. The first part assesses the first half of the course, focusing on design principles and patterns. The second part, to be released in Monday Week 8, will assess system architecture.

# 2. Getting Started

You can access your pair repository through the [course website](https://cgi.cse.unsw.edu.au/~cs2511/25T2/assignments)

> For this assignment, you will need to make two sets of blog posts and add these to your repo:
>
> - Your pair blog post, which contains the answers to these questions and links to your code changes.
> - Your individual blog post, which serves as a record of your individual contribution.
>   [You can view instructions and templates for this blogging here](./Blog_Instructions.md).

# 3. Tasks

This part of the assignment consists of two core tasks. **We strongly recommend reading the entire specification before starting**, as earlier tasks may introduce design decisions or code that influence later tasks.

## Task 1) Code Analysis and Refactoring (25 marks) ⚙️

In this task, you will need to analyse the design of the monolith, including the patterns and smells present in the implementation, and apply refactoring techniques discussed in the course in order to improve the quality of the code.

> ### Instructions for Task 1:
>
> Put all of your answers to the following theory questions in your **pair blog post**.
>
> For each refactoring change, **before you start coding** take some time to plan and write up a design in your pair blog post. This should include:
>
> - What fields/methods you will need to add/change in a class
> - What new classes/packages you will need to create
>
> **After you finish coding**, make a Merge Request into the `main` branch of your repository with the following:
>
> - A meaningful MR title that encompasses the changes
> - A brief description which outlines the changes being made
> - Make sure to keep the MR as small as possible - don't make any extra changes that aren’t absolutely necessary. Try to keep the number of files the changes touch to a minimum.
> - Make sure all the Continuous Integration checks (regression tests, linting, coverage) remain passing.
>
> Your partner will need to **code review** and either:
>
> - Leave comments in the MR with things to fix (you can do code reviews sync or async), requiring you to **iterate on the MR** and **resubmit** for review; or
> - Approve the MR.
>
> Once the MR is approved, copy and paste the link to your MR into your blog post.
>
> Make sure to do this for each part of the question - **if you don't you will lose marks**.
>
> Each group member must make a non-trivial contribution to Task 1 (e.g. being the primary designer and programmer for a subtask, or evidence of substantial collaboration) in order to meet learning outcomes. This will be determined through commit history and individual blogging. If you feel these do not reflect your contribution, make a note in your individual blog.

### a) From DRY to Design Patterns (5 marks)

- i. Look inside `app/src/main/java/dungeonmania/entities/enemies`. Where can you notice an instance of repeated code? Note down the particular offending lines/methods/fields.
- ii. What Design Pattern could be used to improve the quality of the code and avoid repetition? Justify your choice by relating the scenario to the key characteristics of your chosen Design Pattern.
- iii. Using your chosen Design Pattern, refactor the code to remove the repetition.

### b) Inheritance Design (5 marks)

Currently, there is a flaw in the inheritance structure of collectables. A note from a member of the previous engineering team says:

> I don't understand why some collectable items like Wood and Treasure have durability and do things like apply buffs, especially when they're not being used in battle anyway.

- i. List one design principle that is violated by collectable objects based on the description above. Briefly justify your answer.
- ii. Refactor the **inheritance structure** of the code, and in the process remove the design principle violation you identified.

### c) Open-Closed Goals (5 marks)

Look inside the goals package at the code provided.

- i. Do you think the design is of good quality here? Do you think it complies with the open-closed principle? Do you think the design should be changed?
- ii. If you think the design is sufficient as it is, justify your decision. If you think the answer is no, pick a suitable Design Pattern that would improve the quality of the code and refactor the code accordingly.

### d) Open Refactoring (10 marks)

Make any other refactoring improvements you see fit to the codebase. This can include resolving code smells, violations of design principles, using Design Patterns discussed or any other general improvements to the health and quality of the code.

Some areas and questions you can consider:

- Consider violations of the Law of Demeter and the Liskov substitution principle.
- Some classes such as `Exit` contain a number of common empty and unused methods. This might be a symptom of a deeper issue with the inheritance structure.
- The current implementation of the crafting system for buildable entities contains a significant amount of hard coding. Think about how you can improve this.
- If you run `gradle test`, you will see a warning about "deprecated" methods in the codebase. What is the significance of this message? What should you, as the programmer, do with the code?

The above list isn’t exhaustive; there are plenty of other areas to improve the quality of the code.

Some hints for this task:

> 🥇 You don't need to have a perfect design to achieve full marks for this task. Some code smells or design problems may be disproportionately difficult or even impossible to refactor, given the nature of the assignment. It is recommended that you aim for "low-hanging fruit" before judging if you need to attempt more difficult refactors.
>
> 🧠 Don't make solutions to problems that don’t exist yet! :) Avoid over-engineering or over-abstracting in places where you might want to improve the design for some future change in requirements - instead improve the design of the current system. This will inherently make your software open-closed.
>
> You’ll also want to split this task into one MR for each refactoring change you make.

## Task 2) Evolution of Requirements (30 marks) 🔧

In this task, you and your partner/group will need to complete the tasks **Slimes** and **Sunstone & More Buildables**. If you are in a group of three, you must complete the additional component: **Logic Switches**.

If you are in a group of 3, your mark will be out of 50 and scaled to 30.

> Each group member must make a non-trivial contribution to Task 2 (e.g. being the primary designer and programmer for a subtask, or evidence of substantial collaboration) in order to meet learning outcomes. This will be determined through commit history and individual blogging. If you feel these do not reflect your contribution, make a note in your individual blog.

You do not need to do any frontend modifications for any of these tasks, and will not be assessed on any frontend material.

You should create a new folder for your tests called `task2` and place all your tests inside of it.

### Software Delivery - Task Lifecycle

For each part of this task, you will need to undertake the following process. This will help your tutor assess your design during marking.

1.  **Requirements Engineering**. Analyse the task requirements, including the technical and product specifications. If you need to, make some assumptions and document these in your pair blog post.
2.  **Detailed Design**. In your pair blog post, plan out a detailed design for the task. This should include:
    - What fields/methods you will need to add/change in a class
    - What new classes/packages you will need to create
3.  **Design Review**. Have your partner review the design, and go back and iterate on the design if needed.
4.  **Create a Test List**. Once the design is approved, write a **test list** (a list of all the tests you will write) for the task. Map out each of the conceptual cases you want to test. This can be written in your blog post, or if you want make function stubs for JUnit tests and put up a Merge Request (link in your blog).
5.  **Test List Review**. Have someone else in your team review the test list to make sure the test cases all make sense and cover the input space.
6.  **Create the Skeleton**. Stub out anything you need to with class/method prototypes.
7.  **Write the tests**, which should be failing assertions currently since the functionality hasn't been implemented.
8.  **Development**. Implement the functionality so that your tests pass.
9.  Run a **usability test** (check your functionality works on the frontend).
10. Where needed, **refactor** your code to improve the style and design while keeping the tests passing.
11. Put up a **merge request** with your changes into main. The CI should be passing. The merge request should have a meaningful title and contain a description of the changes you have made. In most cases you should just be able to link them to your design/test list blog.
12. **Code Review** from your partner, iterate where needed then they should approve the MR.

Feel free to split tasks further where you see fit, especially for larger tasks (e.g. Logic Switches). Keep your Merge Requests small and make the iteration cycle short, incrementally building your MVP as you go.

> 💡 If you've done Task 1 well and have a nice healthy codebase, this task should be relatively straightforward! If you're finding parts of this task difficult to integrate with the existing design, that's probably a sign you need to do some more refactoring.

> ♻️ A note about **backwards compatibility**:
>
> - **All** the regression tests we have provided to you in the starter code should remain passing.
> - All of the MVP configuration files (in the provided config files) do not currently contain the fields listed in the technical specifications for Tasks 2a)-c). Rather than retroactively adding these fields to the existing configuration files, you will need to design your implementation to accommodate for this and maintain backwards compatibility. **All configuration files in our Task 2 autotests will contain all values from Task 2** and the MVP.

### a) Slimes (15 marks)

In this section, you will modify the codebase to introduce a new type of enemy: slimes. Slimes roam the map, and when two or more slimes collide on the same tile, they merge into one slime, with one slime absorbing the others' special attributes.

Slimes travel in a single, predetermined direction. When they encounter a wall or door, they instinctively change direction, going back in the direction they came from.

> For example, Slime A starts by moving right. On the next tick, it notices that there is a wall to its right. It changes direction, moves left, and continues moving left from then on until it hits another wall or door.

There are multiple kinds of slimes. Importantly, **the type of the slime that is absorbed becomes the type of the resulting slime**. For example, if a red slime absorbs a green slime, it transforms into a green slime with red slime effects appended to it.

Below are all the slime types you will encounter:

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">Entity</span></th>
    <th><span style="font-weight:bold">Image</span></th>
    <th><span style="font-weight:bold">Description</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Green Slime</td>
    <td><img src='/images/green_slime.png' /></td>
    <td>Green slimes are often found in lush forest areas. Their tranquil nature makes them curious but non-aggressive, meaning they initially move upwards and deal no damage until absorbed. When absorbed by another slime, they halve the damage that slime inflicts.
    <details style="margin-top:5px">
      <summary>Example</summary>
      <p>
        If a green slime is absorbed by a red slime with 10 points of damage, the red slime transforms into a green slime with 5 points of damage.
      </p>
    </details>
  </td>
  </tr>
  <tr>
    <td>Red Slime</td>
    <td><img src='/images/red_slime.png' /></td>
    <td>Red slimes frequently inhabit hot, volcanic regions. Constant exposure to heat has made them more aggressive. They begin by moving downwards and, when they encounter the player, they inflict as much damage as possible. When absorbed by another slime, they increase the amount of damage dealt by the resulting red slime.
    <details style="margin-top:5px">
      <summary>Example</summary>
      <p>
        If a red slime with 10 points of damage is absorbed by another red slime with 15 points of damage, the resulting slime remains red and has 25 points of damage.
      </p>
    </details>
    </td>
  </tr>
  <tr>
    <td>Blue Slime</td>
    <td><img src='/images/blue_slime.png' /></td>
    <td>Blue slimes are extremely rare, and their natural habitat is unknown. According to legend, when they come into contact with the player, the player is miraculously healed rather than harmed. They begin by moving to the right and, if they encounter any slimes along the way, they undo the effects of the most recent slime absorption. However, when a blue slime is absorbed, its mystical healing properties are no longer retained.
    <details style="margin-top:5px">
      <summary>Example 1</summary>
      <p>Consider a case where a red slime absorbs a green slime, and then a blue slime:</p>
      <ul>
        <li>The green slime is absorbed by a red slime with 10 points of damage. The red slime transforms into a green slime with 5 points of damage.</li>
        <li>The blue slime is then absorbed by that green slime. The green slime transforms into a blue slime with 10 points of damage.</li>
      </ul>
    </details>
    <details style="margin-top:5px">
      <summary>Example 2</summary>
      <p>
        In the case where a blue slime is absorbed by a red slime with 10 points of damage, the red slime becomes a blue slime with 10 points of damage.  
        In other words, the slime’s colour changes, but its stats do not as there is no previous absorption to undo.
      </p>
    </details>
    </td>
  </tr>
</tbody>
</table>

**Slimes merge in a predetermined order: red, green, blue.**

- When slimes occupy the same tile, **red slimes** absorb other red slimes first with the red slime dealing the most damage absorbing the one dealing less damage, followed by green slimes and then blue slimes.
- If there are no red slimes, **green slimes** absorb other green slimes first, followed by blue slimes.
- If there are no red or green slimes, **blue slimes** absorb each other.

#### Technical Specification

The list of inputs in [Section 4.1.1. of the MVP specification](./MVP.md#411-input-specification---entities-mvp) now includes the following entities:

<table>
<thead>
  <tr>
    <th>Entity</th>
    <th>JSON Prefix</th>
    <th>Creatable from Dungeon Map?</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Green Slime</td>
    <td>
    <code>green_slime</code>
  </td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Red Slime</td>
    <td>
    <code>red_slime</code>
  </td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Blue Slime</td>
    <td>
    <code>blue_slime</code>
  </td>
    <td>Yes</td>
  </tr>
</tbody>
</table>

The list of configuration fields in [Section 4.2.1. of the MVP specification](./MVP.md#421-configuration-fields-mvp) now includes the following fields:

<table>
<thead>
  <tr>
    <th style="font-weight:bold">JSON Format</th>
    <th style="font-weight:bold">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>slime_attack</code></td>
    <td>Attack damage of the red slime.</td>
  </tr>
  <tr>
    <td><code>slime_heal</code></td>
    <td>Health restored by the blue slime.</td>
  </tr>
  <tr>
    <td><code>slime_health</code></td>
    <td>Health of slimes.</td>
  </tr>
</tbody>
</table>

#### Another Example

Below is an example of a merge between a red, a green, and two blue slimes based on the following technical specifications:

- `slime_attack: 10`
- `slime_heal: 5`
- `slime_health: 10`

1. **Initial Slimes**:

   - **Red Slime**: `slime_attack: 10`, `slime_health: 10`
   - **Green Slime**: `slime_health: 10`
   - **Blue Slime**: `slime_heal: 5`, `slime_health: 10`
   - **Blue Slime**: `slime_heal: 5`, `slime_health: 10`

2. **Merge Process**:

   - The red slime absorbs the green slime and becomes a green slime with `slime_attack: 5`, `slime_health: 10`.
   - The green slime absorbs the blue slime and becomes a blue slime with `slime_attack: 10`, `slime_health: 10`.
   - The blue slime absorbs the blue slime and becomes a blue slime with `slime_attack: 5`, `slime_health: 10`.

3. **Result**:
   - Blue Slime: `slime_attack: 5` and `slime_health: 10`. Importantly, it does not have any healing abilities as it has been absorbed.

On the next tick, the resulting blue slime collides with a green slime with stats `slime_attack: 15`, `slime_health: 10`.

> One possible way it could have reached 15 attack is by three red slimes merging, followed by absorbing a green slime.

The green slime absorbs the blue slime, resulting in a blue slime with `slime_attack: 30`, `slime_health: 10`.

### b) Sun Stone & More Buildables (15 marks)

In this task, the following collectable entities need to be added:

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">Entity</span></th>
    <th><span style="font-weight:bold">Image</span></th>
    <th><span style="font-weight:bold">Description</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Sun Stone</td>
    <td><img src='/images/sunstone.png' /></td>
    <td>A special form of treasure, hard and treasuable. It can be picked up by the player. Can be used to open any door. Since it is classed as treasure it counts towards the treasure goal, but it cannot be used to bribe mercenaries. Can also be used interchangeably with treasure or keys when building entities, though if the player possesses enough treasure or keys those should be preferred when crafting. When used for opening doors, or when replacing another material such as a key or treasure in building entities, it is retained after use. When used as a listed ingredient in crafting, it is consumed. Sunstones will always be prioritised to be used as a listed ingredient before serving as a replacement material.</td>
  </tr>
</tbody>
</table>

The following buildable entities have also been added:

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">Entity</span></th>
    <th><span style="font-weight:bold">Image</span></th>
    <th><span style="font-weight:bold">Description</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Sceptre</td>
    <td><img src='/images/sceptre.png' /></td>
    <td>Can be crafted with (1 wood OR 2 arrows) + (1 key OR 1 treasure) + (1 sun stone). A player with a sceptre does not need to bribe mercenaries to become allies, as they can use the sceptre to control their minds without any distance constraint. They can do so by interacting with the particular mercenary they wish to mind control. Mind control has the same effects as bribing, but the effects only last for a certain number of ticks. This number of ticks begins counting down immediately after the sceptre is used.
    <details margin-top="5px">
        <br>
        <summary>Example</summary>
        In a case where the sceptre's duration is 2 ticks:
        <ul>
            <li>Tick 1 - player's turn: Player mind controls enemy. </li>
            <li>Tick 1 - enemy's turn: Enemy is mind controlled. </li>
            <li>Tick 2 - player's turn: Player moves. </li>
            <li>Tick 2 - enemy's turn: Enemy is mind controlled. </li>
            <li>Tick 3 - player's turn: Player moves. </li>
            <li>Tick 3 - enemy's turn: Enemy is no longer mind controlled. </li>
        </ul>
        </details>
    <br>
    Other than the standard behaviour of allying mercenaries, sceptres do not have any direct effect in battle.
    The behaviour after a sceptre is used is not specified and will not be tested: it can be destroyed, retained, etc.
   </td>
  </tr>
    <tr>
    <td>Midnight Armour</td>
    <td><img src='/images/midnight_armour.png' /></td>
    <td>Can be crafted with (1 sword + 1 sun stone) if there are no zombies currently in the dungeon. Midnight armour provides extra attack damage as well as protection, and it lasts forever.</td>
  </tr>

</tbody>
</table>

> **Where otherwise unspecified, if there are multiple valid options for crafting an item, the precedence of items consumed is undefined.**

#### Technical Specification

The list of inputs in [Section 4.1.1. of the MVP specification](./MVP.md#411-input-specification---entities-mvp) now includes the following entities:

> You may find `NameConverter.java` particularly useful in ensuring your response objects match these strings for automarking.

<table>
<thead>
  <tr>
    <th>Entity</th>
    <th>JSON Prefix</th>
    <th>Creatable from Dungeon Map?</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Sun Stone</td>
    <td>
    <code>sun_stone</code>
  </td>
    <td>Yes</td>
  </tr>

  <tr>
    <td>Sceptre</td>
    <td>
    <code>sceptre</code>
  </td>
    <td>No, since this entity must be built by the player.</td>
  </tr>
  <tr>
    <td>Midnight Armour</td>
    <td>
    <code>midnight_armour</code>
  </td>
    <td>No, since this entity must be built by the player.</td>
  </tr>
</tbody>
</table>

The list of configuration fields in [Section 4.2.1. of the MVP specification](./MVP.md#421-configuration-fields-mvp) now includes the following fields:

<table>
<thead>
  <tr>
    <th style="font-weight:bold">JSON Format<br></th>
    <th style="font-weight:bold">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td> <code>mind_control_duration</code>
  </td>
    <td>The amount of time mind controlling via a sceptre lasts for.</td>
  </tr>
  <tr>
    <td> <code>midnight_armour_attack</code>
  </td>
    <td>Attack bonus wearing midnight armour gives to the player.</td>
  </tr>
  <tr>
    <td> <code>midnight_armour_defence</code>
  </td>
    <td>Defence bonus wearing midnight armour gives to the player.</td>
  </tr>
</tbody>
</table>

You will not be given any test cases where `mind_control_duration` is less than or equal to 0.

The following interface methods in [Section 4.3.2. of the MVP specification](./MVP.md#432-interface-methods) are now updated:

<table>
<thead>
  <tr>
    <th style="font-weight:bold">Method Prototype<br></th>
    <th style="font-weight:bold">Description<br></th>
    <th style="font-weight:bold">Exceptions<br></th>
  </tr>
</thead>
<tbody>
</tbody>
  <tr>
    <td><code>public DungeonResponse build(String buildable) throws InvalidActionException</code</td>
    <td>
    Builds the given entity, where buildable is one of <code>bow</code>, <code>shield</code>, 🆕<code>sceptre</code>, or 🆕<code>midnight_armour</code>
    </td>
    <td>
    IllegalArgumentException:
    <ul><li>If <code>buildable</code> is not one of <code>bow</code>, <code>shield</code>, 🆕<code>sceptre</code>, or 🆕<code>midnight_armour</code></li></ul> <br>
    InvalidActionException:
    <ul><li>If the player does not have sufficient items to craft the buildable, 🆕or unbuildable for <code>midnight_armour</code> because there are zombies currently in the dungeon.</li></ul>
    </td>
  </tr>
  <tr>
    <td><code>public DungeonResponse interact(String entityId) throws IllegalArgumentException</code</td>
    <td>
    Interacts with a mercenary (where the Player bribes 🆕or mind controls the mercenary) or a zombie spawner, where the Player destroys the spawner.
    </td>
    <td>
    IllegalArgumentException:
    <ul>
    <li> If <code> entityId</code> is not a valid entity ID</li></ul><br>
    </ul>
    InvalidActionException:
    <ul>
    <li>If the player is not within specified bribing radius to the mercenary, when they are bribing</li>
    <li>🆕If the player does not have enough gold and does not have a sceptre and attempts to bribe/mind-control a mercenary</li>
    <li>If the player is not cardinally adjacent to the spawner, if they are destroying a spawner</li>
    <li> If the player does not have a weapon and attempts to destroy a spawner</li>
    </ul>
    </td>
  </tr>
</table>

### c) Logic Switches (20 marks)

**NOTE: This task is for groups of THREE to complete only. It will not go towards your marks otherwise.**

There are three new entities in this extension:

<table>
<thead>
  <tr>
    <th><span style="font-weight:bold">Entity</span></th>
    <th><span style="font-weight:bold">Image</span></th>
    <th><span style="font-weight:bold">Description</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Light Bulb</td>
    <td><img src='/images/light_bulb.png' /></td>
    <td>Light bulbs cannot be collected, but can be lit up by satisfying the logical condition. They are always created as a logical entity.
    </td>
  </tr>
  <tr>
    <td>Switch Door</td>
    <td><img src='/images/switch_door.png' /></td>
    <td>Switch doors behave the same as normal doors, except they have a different opening mechanism. They are always created as a logical entity, and can be opened when their logical condition is satisfied.
    They cannot be opened with a key. The door should only remain open as long as its condition for opening is still true.
    </td>
  </tr>

  <tr>
    <td>Wire</td>
    <td><img src='/images/wire.png' /></td>
    <td>Wires cannot be collected, but act as a conductor to pass current between switches and logical entities. Wires themselves do not follow logical rules. Any moveable entity can walk onto a wire.</td>
  </tr>
</tbody>
</table>

If a switch is activated, it sends a current to any cardinally adjacent wires and logical entities. Current can be conducted through **wires and activated switches only** (conductors), meaning that a logical entity may be activated directly by a switch if cardinally adjacent, or indirectly by a cardinally adjacent current-carrying wire. This allows for the creation of dungeons with logic circuits. For example:

![](images/logic_circuit.png)

<center style="opacity:0.7">Current from the switch on (3, 1) is propagated through the wires on (4, 1) and (5, 1) to activate the bulb on (6, 1).</center>
</br>

All logical entities will be created in an off state.

#### a) Logical Entities

Some entities are "logical" entities with a specified configuration value. These entities will only activate if their logical condition is fulfilled. They include:

- Light Bulbs (see above)
- Switch Doors (see above)
- Bombs also now have the option to be created as logical entities with a specified configuration value. Bombs which have this attribute will only explode when their logical condition is fulfilled. Bombs created without this value will interact with switches in the same way as they do in the MVP, and do not interact with other logical entities or wires.

Switches and wires are not logical entities, but are conductors - they activate cardinally adjacent wires and logical entities when they are switched on or conducting. Light bulbs and switch doors can be activated but do not transmit current through themselves.

![](images/logic_conducting.png)

<center style="opacity:0.7">Assume that (1, 0) contains a boulder-activated switch.</center>
</br>

All switches and logical entities will be created in an inactive state. Wires and logical entities will also remain activated as long as there is a current from a switch running through them - if at some point there were multiple switches powering a component and one of them is turned off, the component will remain activated if there is still current that can reach it. If the source of power for a logical component has been switched off or destroyed, then the logical component will be deactivated.

Any other entities do not have conductive or logical behaviours when placed next to a conducting wire or switch.

#### b) Logical Rules

Logical entities will obey one of the following rules:

- **OR** - the entity will be activated if there is 1 or more cardinally adjacent activated conductor

- **AND** - the entity is activated if all cardinally adjacent conductors are activated. Furthermore, there must be at least 2 cardinally adjacent conductors.
![](images/logic_and.png)
<center style="opacity:0.7">The bulb can only be activated if the switch on (2, 1) and the wires on (3, 0) and (4, 1) are all activated. Hence, both switches must be switched on in order to activate the bulb.</center>
</br>

- **XOR** - the entity will be activated if there is 1 and only 1 cardinally adjacent activated conductor
![](images/logic_and.png)
<center style="opacity:0.7">The bulb can only be activated by switching on just the switch on (2, 1). If the switch on (2, 0) is turned on, then the cardinally adjacent wires on (3, 0) and (4, 1) will also conduct which do not satisfy the XOR condition. </center>
</br>

- **CO_AND** - the entity will only be activated if there are 2 or more cardinally adjacent activated conductors, which are **all activated on the same tick**, e.g. if a switch activates two wires that are both cardinally adjacent to a logical entity with the `co_and` condition, it should be activated.
![](images/logic_coand.png)
<center style="opacity:0.7">Activating the switch on (3, 1) will also activate the bulb's cardinally adjacent wires on (4, 0) and (4, 2) on the same tick, and satisfy the co_and condition. If any or both of the switches on (1, 0) and (1, 2) are activated before the middle switch then the bulb will NOT turn on as the adjacent conductors are activated on different ticks.</center>
</br>

Note that all conductor activation should be checked before before logical conditions are computed within the same tick. Activation order of the logical entities is undefined.

In the case that another conductor powers an already activated component, the current is not 'refreshed'. The `co_and` case should rely on the tick the adjacent conductor is initially powered from a deactivated state.

#### Technical Specification

The list of inputs in [Section 4.1.1. of the MVP specification](./MVP.md#411-input-specification---entities-mvp) now includes the following entities:

<table>
<thead>
  <tr>
    <th>Entity</th>
    <th>JSON Prefix</th>
    <th>Creatable from Dungeon Map?</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Light Bulb (off)</td>
    <td>
    <code>light_bulb_off</code>
  </td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Light Bulb (on)</td>
    <td>
    <code>light_bulb_on</code>
  </td>
    <td>No, since light bulbs will always be created off.</td>
  </tr>
    <tr>
    <td>Wire</td>
    <td>
    <code>wire</code>
  </td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Closed Switch Door</td>
    <td>
    <code>switch_door</code>
  </td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Open Switch Door</td>
    <td>
    <code>switch_door_open</code>
  </td>
    <td>No, since switch doors will always be created as closed.</td>
  </tr>
</tbody>
</table>

- All logical entities will be created with the field <code>logic</code> which will be one of <code>and</code>, <code>or</code>, <code>xor</code>, or <code>co_and</code>. Note that light bulbs and switch doors will always be created with a logic field. Regular doors will never be created with a logic field, nor will floor switches or wires.
- Bombs may be created with the field <code>logic</code>. If they have this field, they are expected to be able to interact with other logical entities as described. Bombs created without this field function as they do in the MVP and do not need to interact with other logical entities.

# 4. Titbits

## 4.1. Git Practices 🧭

- We will not be assessing your commit messages, though for the sake of good practice you should write meaningful commits.
- The main branch should always remain stable - the pipeline should always be passing. You are welcome to comment out some of the example tests we have provided until you can get them working locally.
- When putting up Merge Requests, all changes in the diff should be relevant to the task which they relate to (not polluted with irrelevant changes, as this makes it difficult for reviewers);
- Code reviews should occur as comments on Merge Requests - if you are doing the code review synchronously leave comments on the MR as your minutes/action items

## 4.2. Assumptions 👾

As you develop your implementation of the tasks, you will undoubtedly come across scenarios where some behaviour is not properly defined by the specification. This is a complex assignment - if we defined every possible scenario, the specification would go on forever. Unlike previous courses, this one is about design. We are not trying to catch you out by testing every imaginable edge case. The autotests are there to verify that you have followed the specification and completed the work, not to trip you up on obscure edge cases.

Here are the steps you should follow if you are unsure about something in the spec:

1. Double check the spec, do a ctrl/command-f to check it isn't mentioned elsewhere, in a lot of situations this will be the case.
2. Check the Approved Assumptions post on the forum, all of the approved assumptions from previous terms are there and new ones will be added if they come up.
3. Make a post in the forum **(please search first)** asking the Course Staff whether you are able to make an assumption about the behaviour in this case. We will either:

- Approve the assumption and add to the Approved Assumptions page
- Update the specification if appropriate, or
- Respond explaining how the behaviour is defined in the scope of the specification.

Any ambiguities/assumptions that we have listed as approved **we will not be testing** in automarking.

## 4.3. Test Design 🐝

When it comes to writing tests for the new features, you should write functional tests on the controller.

All of the existing tests in the monolith are written in this way, and you can structure your tests similarly.

## 4.4. Dungeon Map Helper

We’ve provided a dungeon map helper [here](https://cs2511-dungeonmania-map-generator.vercel.app/) for you here to assist with visualising and creating test maps.

Please note it is not comprehensive (not all entities and fields are provided), and it may contain some bugs!

# 5. Design

## 5.1. Pair Blogging

You must complete the pair blog according to the template, including documenting your design decisions for each coding task.

## 5.2. Individual Blogging

As well as your pair blog, you will need to fill out an individual blog post each week to describe your contributions. This may include summarising your activities, adding links to your merge requests, and reflecting on the challenges you faced in the past week.

The individual blog is not given a mark weighting but is mandatory for all students.
We will use these blog posts to determine individual marks in the case where pair contribution is not equal.

# 6. Assessment

## 6.1. Marking Criteria

<table>
<thead>
<tr>
    <td style="font-weight:bold; width:15%;">Task</td>
    <td style="font-weight:bold; width:19%;">Subtask</td>
    <td style="font-weight:bold; width:66%;">Criteria</td>
</tr>
</thead>
<tbody>

<tr>
    <td rowspan="4">Task 1 ⚙️<br>25 marks</td>
    <td>a) From DRY to Design Patterns (5 marks)</td>
    <td>
        <ul>
            <li>Has the smell been pinpointed?</li>
            <li>Has a suitable Design Pattern been selected and justified?</li>
            <li>Was the pattern well implemented?</li>
        </ul>
    </td>
</tr>
<tr>
    <td>b) Inheritance Design (5 marks)</td>
    <td>
        <ul>
            <li>Was the design principle identified, with clear justification?</li>
            <li>Was the design flaw resolved?</li>
        </ul>
    </td>
</tr>
<tr>
    <td>c) Open-Closed Goals (5 marks)</td>
    <td>
        <ul>
            <li>Is the stance justified according to the design?</li>
            <li>If applicable, was any refactoring done to improve the design?</li>
        </ul>
    </td>
</tr>
<tr>
    <td>d) Open Refactoring (10 marks)</td>
    <td>
        For 3 marks, <b>some</b> additional smells/flaws are identified and resolved.<br>
        For 5 marks, <b>listed</b> additional smells/flaws (or equivalent) are identified and resolved.<br>
        For 10 marks, <b>substantially more</b> additional smells/flaws are identified and resolved, equivalent to double that of the 5 mark criteria.
    </td>
</tr>
<tr>
    <td>Task 2 🔧<br>30 marks</td>
    <td>
        For each subtask completed:<br><br>
        a) Slimes (15 marks)<br><br>
        b) Sunstone & More Buildables (15 marks)<br><br>
        If you are in a group of 3:<br>
        c) Logic Switches (20 marks)
    </td>
    <td>
        <b>Software Correctness</b> (40% of the marks for the subtask)<br>
        This section will be automarked.<br><br>
        <b>Software Design</b> (50% of the marks for the subtask)
        <ul>
            <li>Is the design seamlessly integrated into the existing infrastructure?</li>
            <li>Does the design adhere to principles discussed in the course?</li>
            <li>Does the design contain any smells?</li>
        </ul>
        <b>Software Testing</b> (10% of the marks for the subtask)
        <ul>
            <li>Have functional tests on the controller been written?</li>
            <li>Do the tests cover a range of cases?</li>
            <li>Are the tests well designed?</li>
        </ul>
    </td>
</tr>
<tr>
    <td rowspan="2"">Easy Marks 🧂<br>5 marks</td>
    <td>Code Hygiene 🚿</td>
    <td>
        You will receive <strong>0/5 marks</strong> in the <strong>Easy Marks</strong> section if you fail to meet <em>any</em> of the following requirements:
        <ul>
            <li>All regression tests must pass</li>
            <li>Code coverage must remain above 80%</li>
            <li>The linter must pass</li>
        </ul>
        In simple terms, the pipeline on the <code>main</code> branch of your repository must remain green (i.e. passing) for the entire assignment.<br><br>
        If pipelines are running slowly, you’re expected to run linting and tests locally. If they pass locally, you may force merge. The purpose of the pipeline is to help you keep <code>main</code> in a working state. It’s not a hard requirement for merging.<br><br>
        <strong>Slow pipelines are not a valid excuse for failing to meet these requirements or for not merging to <code>main</code>.</strong>
    </td>
</td>
</tr>
<tr>
    <td>Merge Requests 🧭</td>
    <td>
        You will get 0/5 marks in this Easy Marks section if you:
        <ul>
            <li>Have Merge Requests that are too large and contain too many changes</li>
            <li>Don’t link your Merge Requests in your blog post</li>
        </ul>
        This is to make it easier for your marker to award you for your work. If you stick to the blog template, you will be fine here 🙂
    </td>
</tr>
</tbody>
</table>

## 6.2. Submission

We will take the contents of your `main` branch as your final submission. There are no late submissions. See [this](#54-late-penalties).

## 6.3. Dryruns

To ensure your code **compiles with our automarker** and **basic functionality works as expected** we have provided you a basic dryrun test. If your code does not compile with our automarker, you will receive 0 for the automarking section. Any code changes to fix the compilation issue will result in a non-negotiable 20% penalty. **It is your responsibility to ensure your code compiles with our automarker**.

You can find the dryrun files here: [https://cgi.cse.unsw.edu.au/~cs2511/public/dryrun-files/dryrun_ass2/ ](https://cgi.cse.unsw.edu.au/~cs2511/public/dryrun-files/dryrun_ass2/).

**Make sure you read all of the output!**

### 6.3.1.Ensuring your code **compiles** with our tests through GitLab CI/CD pipelines.

Every time you push code to the `main` branch on GitLab or create a merge request on GitLab, an automated pipeline will run that first:

1. Compiles your code with your own tests
2. Runs your tests
3. Replaces your tests with our dryrun tests found [here](https://cgi.cse.unsw.edu.au/~cs2511/public/dryrun-files/dryrun_ass2/)
4. Checks if your code compiles with our tests

Note: If any of the previous steps fail, it will not run the next step.

**You should try to maintain a green pipeline on the `main` branch at all times.**

### 6.3.2. Ensuring your code **compiles and passes** with our tests through GitLab CI/CD pipelines.

Every time you push code to the `main` branch on GitLab or create a merge request on GitLab, you can click on the Green or Red indicator next to your commit. This should bring you to a page that looks like the following:

![](/images/gitlab-pipelines.png)

You can request a manual run of the pipeline by clicking on the `Run` button (see red arrow in image above). This will run the pipeline with the dryrun tests.

The pipeline **will always pass** as to not affect your overall `main` branch pipeline status. This means you will have to **manually check the output of the dryrun test**. You should view the logs and output and determine if you have passed the correct tests.

**Only task 2 will be tested with the dryrun tests.**

> Note: This behaviour is different to assignment-i.
