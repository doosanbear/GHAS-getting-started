# Code Scanning queries

## Code Scanning ships with 3 pre-defined query suites out of the box. These are:

#### 1. code-scanning: A careful selection of highly-precise, low noise security queries.
#### 2. security-extended: All of our security queries, including code-scanning.
#### 3. security-and-quality: All of our security queries from security-extended, as well as our Code Quality queries.


  - code-scanning is the query suite used by default, and the intended use is that this suite is safe to roll out across potentially several thousand repositories without overwhelming anyone with large volumes of alerts, and all the questions and resistance that follows. It is, specifically, designed for scale. Individual teams can then "peel the onion" at their own pace, enabling the wider suites when they are comfortable doing so. This approach works well in large organizations, as it respects the fact that different teams are often at different stages of the DevSecOps journey.
  - For a better apples-to-apples comparison with other SAST tools, we recommend always starting with code-scanning, and then expanding to security-and-quality, as some vendors will tag some of our code quality alerts as "security issues" (eg. empty catch blocks are considered a code quality issue in CodeQL, but a security issue among some vendors).
  - All queries are open sources, and available in our public repo for CodeQL: https://github.com/github/codeql
  - A CSV file detailing what queries are included in each of the suites for each language is generated every time changes in this repo are merged into main. 
  - [Query list](https://github.com/github/codeql/actions/workflows/query-list.yml)
