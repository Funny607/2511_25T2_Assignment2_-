# COMP2511 Assignment 2 Part 2

## Summary

This assignmentʼs goal is for you to practice the structural and behavioural modelling of a high-level architecture based on a case study inspired by realistic requirements from industry. These models will be developed using the C4 architectural notation.

## Case Study Description

### Background

Modern businesses crucially depend on obtaining and processing quality data for efficient decision-making. One of the major issues is that such data is often scattered across multiple sources which makes data acquisition a very expensive process. For example, in the area of finance, there are multiple data sources such as:

- Stock market feeds
- News providers
- Regulatory reports
- Macroeconomic announcements

Often the mechanisms to acquire the data are very different. In some cases, data can be downloaded, in other cases it has to be scraped from web pages. In other cases, it needs to be extracted from PDF reports and in some cases, it is available via API calls.

In this case study, we assume that you are working as part of a financial institution that wants to build a platform that ingests data from different **price information feeds**. This data is then supplied to its customers via alerts and notifications.

## User requirements

The list of user requirements are as follows:

- You need to design a data serving platform for a financial institution that regularly ingests, stores, and serves price information from different data sources on behalf of its customers
- A data source in this case is a price feed which can be historical or real-time prices. Historical data is often in simple text files (like CSV file) that can be downloaded. An example of a data source is [Yahoo Finance](https://au.finance.yahoo.com/)
- It is also possible for data to come from other sources like Australian Stock Exchange or a simulated market.
- In general, customers prefer avoiding going to a data source directly and prefer to get it from the financial institution.
- Customers can be traders, investors, and individuals. Customers would like **prices timeseries** that are customised to their needs.
- Customers like to get prices timeseries in a specific **data format**. For example, an Excel spreadsheet that contains a clean timeseries with a particular frequency (like daily, hourly, minute-level prices).
- Customers also want to be able to specify alerts/notifications for particular **events**. For example, a customer may specify “Please send me an alert when Stock A price rises above $1.”
- **Only the following four data sources may be used to ingest price data:**
  - [Investing.com](https://www.investing.com)
  - [Stooq.com](https://stooq.com)
  - [Yahoo Finance](https://au.finance.yahoo.com)
  - [Alpha Vantage](https://www.alphavantage.co)
  This restriction ensures a common foundation for all student submissions and simplifies integration and evaluation. Your architecture must clearly reflect this limitation.

## Architecture needed

As a first step in building a solution, you need to design an architecture that offers the following advantages:

- It has to support any type of price feeds with different data acquisition methods. We assume changes in the methods of acquisition of the data for example a change in the URL of a website or in the address of an API
- It has to support pre-processing and cleaning of the data to fit customers' data format requirements
- It has to support alerts to notify customers when a specific type of event arises.

## Assignment Requirements

### Overall Tasks

In this assignment, there are 3 tasks:

1. Define 2–3 most important **use cases** that correspond to the requirements of the case study.
2. Define two architecture diagrams (**Context and Container**) that supports the use cases using the C4 notation.
3. For one of your use cases, define a **sequence diagram** showing how components in your Container diagram interact over time

### Task 1: Defining the Use cases (8 marks)

Based on the analysis of the requirements, define your most important use cases (**2 or 3 use cases approximately**). These use cases represent the high priority requirements that need to be supported by the architecture. Each use case must:

- Have a clear name and support a very specific user function
- Be defined in a way that only illustrates the userʼs perspective (not technology or implementation dependent)

As an alternative to use cases, you can use define your most important requirements using **user stories**.

### Task 2: Defining the Architecture (20 marks)

To support the use cases, you need to produce 2 architecture diagrams:

- **C4 System Context Level diagram**: showing your system + users + neighbouring systems. This is useful for Business stakeholders, execs and non-tech users.
- **C4 Container Level diagram**: showing major application/components like web apps, APIs, DBs. This is useful for developers, tech leads and architects.

Please make sure that:

- The names of common entities (components and relations) in the 2 diagrams are matching as one is a further decomposition of the other.
- Good C4 design rules have been followed.

### Task 3: Defining the behaviour - (12 marks)

For one of the use cases identified in Task 1, create a sequence diagram that illustrates how the architecture is supporting the use case. Make sure that:

- Each component (vertical line) in the sequence diagram corresponds to a C4 component, either external (from System context level) or internal (from the Container diagram).
- Invocations between components have to be clearly labelled according to good design rules in sequence diagrams.

## Marking criteria |

| Criteria                            | Description                                                                                                                                                                                                                                                                                               |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Defining the Use Cases (8 marks)**    | • Are the use cases clearly defined and articulated?<br>• Do use cases align with the intended purpose of the application?                                                                                                                                                                                |
| **Defining the Architecture (20 marks)** | • Has the architecture followed good C4 design practices?<br>• Is the architecture clearly explained for a business user?<br>• Can the architecture serve as a good implementation blueprint for a developer?<br>• Is the architecture flexible, open to changes and extensible with new functionalities? |
| **Defining the Behaviour (12 marks)**    | • Are the interactions clearly explained according to good modelling practices?<br>• Is the sequence diagram aligned with the use case description?<br>• Is the sequence diagram aligned with the architecture description?                                                                               |

## Resources

### Information on the C4 model:

- Home page: [Home | C4 model](https://c4model.com)
- C4 modelling:[ Visualising software architecture with the C4 model - Simon Brown, Agile on the Beach 2019 ](https://www.youtube.com/watch?v=x2-rSnhpw0g&t=785s)(watch from min 9)

### Information on modelling tools

**Recommended tool:**

- [Excalidraw — Collaborative whiteboarding made easy](https://excalidraw.com)

**Alternatives (with tutor approval):**

- [Draw.io / diagrams.net](https://app.diagrams.net): Flowchart Maker & Online Diagram Software is the simplest diagramming tool out there that supports C4 diagrams, UML diagrams, and sequence diagrams.
- [PlantUML](https://plantuml.com): Open-source tool that uses simple textual descriptions to draw beautiful UML diagrams. This is good for code to diagram and diagram to code but has a steep learning curve as you will need to learn the diagramming syntax and DSL.
