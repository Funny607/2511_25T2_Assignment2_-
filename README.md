# Assignment-ii <!-- omit in toc -->

## Due: Week 10 Friday, 3pm [Sydney Local Time](https://www.timeanddate.com/worldclock/australia/sydney) (8th August 2025)

- [1. Part 1 \[60 Marks\]](#1-part-1-60-marks)
- [2. Part 2 \[40 Marks\]](#2-part-2-40-marks)
- [3. Other Information](#3-other-information)
  - [3.1. Late Penalties](#31-late-penalties)
  - [3.2. Extenuating Circumstances](#32-extenuating-circumstances)
  - [3.3. Other Expectations](#33-other-expectations)
  - [3.4. Plagiarism](#34-plagiarism)
  - [3.5. Copyright Notice](#35-copyright-notice)
- [4. Credits](#4-credits)

Key notes:

- There are **two** parts to this assignment.
- **No submissions will be accepted after the due date. This includes students with special considerations and ELP.** See [this](#3-other-information).
- We will take the contents of your `main` branch as your final submission for Part 1. See [this](#3-other-information).
- There are no "Short Extensions" from Special Consideration for this assignment. See [this](#32-extenuating-circumstances).
- Ensure your Part 1 submission compiles with our dryrun [here](./part-1/Part1_Specification.md#63-dryruns).
- Please visit the [course website](https://cgi.cse.unsw.edu.au/~cs2511/25T2/els-spec-cons) for Special Consideration or Equitable Learning Plans.
- Part 2 must be submitted on [Moodle](https://moodle.telt.unsw.edu.au/mod/turnitintooltwo/view.php?id=7987670).

# 1. Part 1 [60 Marks]

In this assignment, you are provided an existing developed system. You will first need to analyse and refactor the code, then adapt the system to an evolution in requirements.

The aims of this part can be broken down into five major themes:

1. **Acclimatising to an existing system.** For many of you this will be the largest codebase you have worked on to date - which can be very daunting! Being able to work with a system that you haven’t built from scratch and don’t fully understand is a vital skill, since software is never developed in isolation.
2. **Refactoring Techniques.** Like when you go camping, you always want to leave the code in a better state than you found it in. We’ve intentionally put in a series of design flaws, with accompanying design smells for you to discover and refactor.
3. **Design Patterns.** Being able to see patterns in existing code, and to use patterns to improve code quality is an essential skill in a Software Engineer. You’ll have the chance to do this and apply the theoretical ideas discussed in lectures.
4. **Evolution of Requirements.** Software is never static - it always evolves and grows as do its requirements. You’ll need to build on the existing system to accommodate for these changes, in doing so undergo an iterative design and development process.
5. **Dealing with The Unknown.** There are many unknowns in this assignment that you and your partner will encounter. You will need to explore and investigate together, clarify and ask for help where needed and approach these unknowns with grace in order to succeed.

This spec is split into **two** sections.

1. [Product Specification - MVP](./part-1/MVP.md) contains the requirements the existing MVP we've provided was built to.
2. [Part 1 Specification](./part-1/Part1_Specification.md) contains your tasks you'll need to complete for this assignment.

We have also included some extra documentation which may be useful to you.

- [Setting up](./part-1/Setup.md)
- [Customisations](./part-1/Customisations.md)
- [Blog Instructions](./part-1/Blog_Instructions.md)
- [Dungeon Map Creation Tool](https://cs2511-dungeonmania-map-generator.vercel.app) (for assistance only, may be incomplete)

# 2. Part 2 [40 Marks]

> This part of the assignment will be released on Week 8, Monday.

Part 2 focuses on content introduced from Week 7 onward. Two tutorials and labs will explore key topics related to this part. In this part, you will model the high-level architecture of a system,
focusing on both structural and behavioural aspects. The case study is inspired by realistic, industry-relevant requirements to provide a practical design experience. These models will be developed
using the C4 architectural notation and behavioural modelling.

- [Part 2 Specification](./part-2/Part2_Specification.md)

# 3. Other Information

## 3.1. Late Penalties

**No submissions will be accepted after the due date. This includes students with special considerations and ELP.**

## 3.2. Extenuating Circumstances

Mark leniency must be approved through Special Consideration or pre-arranged through an Equitable Learning Plan. For more information, please visit the [course website](https://cgi.cse.unsw.edu.au/~cs2511/25T2/els-spec-cons#23-assignment-ii). If you have any questions, please email [cs2511@cse.unsw.edu.au](mailto:cs2511@cse.unsw.edu.au).

## 3.3. Other Expectations

While it is up to you as a pair to decide how work is distributed between you, for the purpose of assessment there are certain key criteria all partners must attain:

- Code contribution;
- Non-code contribution;
- Usage of Git/GitLab;
- Individual blog posts; and
- Academic conduct.

While, in general, both students will receive the same mark for the assignment, if you as an individual fail to meet these criteria your final assignment mark will be reduced.

> If you believe a your partner is not contributing as they should contribute, you must inform your tutor at the end of that corresponding week.
>
> For example, if your partner has not contributed in Week 7, you need to report this before the end of Week 7. You must not wait beyond this. If you fail to report in time, we may not be able to address the issue and/or apply redistribution of marks.

## 3.4. Plagiarism

Your program must be entirely your group's work. Plagiarism detection software will be used to compare all submissions pairwise (including submissions for similar assignments in previous years, if applicable) and serious penalties will be applied, including an entry on UNSW's plagiarism register. Relevant scholarship authorities will be informed if students holding scholarships are involved in an incident of plagiarism or other misconduct.

You are also not allowed to submit code obtained with the help of ChatGPT, GitHub Copilot, Gemini or similar automatic tools.

- Do not copy ideas or code from others outside your group.
- Do not use a publicly accessible repository or allow anyone outside your group to see your code, except for the teaching staff of COMP2511.
- Code generated by ChatGPT, GitHub Copilot, Gemini and similar tools will be treated as plagiarism.

The course reserves the right to ask you to explain your code or design choices to a member of staff as part of your submission, or complete a similar assessment.

Please refer to the on-line resources to help you understand what plagiarism is and how it is dealt with at UNSW:

- [Academic Integrity and Plagiarism](https://www.student.unsw.edu.au/plagiarism/integrity)
- [UNSW Plagiarism Policy](https://www.unsw.edu.au/content/dam/pdfs/governance/policy/2022-01-policies/plagiarismpolicy.pdf)

## 3.5. Copyright Notice

Reproducing, publishing, posting, distributing or translating this assignment is an infringement of copyright and will be referred to UNSW Student Conduct and Integrity for action.

# 4. Credits

If no specific license is specified it's public domain permissible (i.e. usable in commercial/non-commercial products) but no explicit license was found.

- Frontend + monolith built by cs2511: Braedon Wooding, Nick Patrikeos, George Litsas, Noa Challis, Chloe Cheong, Sienna Archer, Tina Ji, Webster Zhang, Adi Kishore, Amanda Lu
- The one ring: Created by Jordan Irwin (AntumDeluge)
- Mercenary: Animated Ranger by Calciumtrice, usable under Creative Commons Attribution 3.0 license.
- Portals: Portals made by RodHakGames - RHG
- Boulder: This work, made by Viktor Hahn (Viktor.Hahn@web.de), is licensed under the Creative Commons Attribution 4.0 International License. Creative Commons — Attribution 4.0 International — CC BY 4.0
- Alagard Font: Made by Pix3M, usable under Creative Commons Attribution 3.0 license.
- Armor + Shield: Made by Zeno
- Tileset + Some Random Entities: Made by egordorichev, these assets are public domain and free to use on whatever you want, personal or commercial (aka CC0 license).
- Coin/Treasure: By La Red Games
- Zombie Toast: By LHTeam (LazyHamsters )
- Toaster: By Reakain; LICENCE: This asset pack can be used in both free and commercial projects. You can modify it to suit your own needs. Credit is not necessary, but very appreciated. You may not redistribute it or resell it.
- Spider: By Elthen
- Assignment authored by Robert Clifton-Everest, Braedon Wooding, Ashesh Mahidadia, Fethi Rabhi, Nick Patrikeos and Amanda Lu.
