# Combating CodeQL No Results Found Checklist

## Discovery Checklist
- `What does this project / application do?`
  - Is it a Web Application or [something else](./codeql-debugging/codeql-no-results-found.md#question-what-does-this-project--application-do)?
- `How big is the Application / Project?`
  - [Smaller applications will inherently have less security issues](./codeql-debugging/codeql-no-results-found.md#question-how-big-is-the-application--project)
- `What languages are you trying to analysis?`
  - [Third-Party Integration needed?](./code-scanning-integrations.md)
- `What frameworks are you using?`
  - Focus on Web, Networking, Databases, and Frontend frameworks
  - [CodeQL Language and Framework support list](https://codeql.github.com/docs/codeql-overview/supported-languages-and-frameworks/) 
- Are they using a [Framework or Library that needs Extra Support](./codeql-technologies-support)?
- `What Threat Model are you working under?`
  - Is it a Web Application or [something else](./codeql-debugging/technical-knowledge/codeql-debugging/codeql-no-results-found.md#step-3---understanding-their-application-threat-model)?
- `Is all of the application compiling?`

## Questions / Discovery

### Step 1 - Understanding Context

This the biggest issues customers are facing but don't know it is when it comes to understand the application context and threat model of the application.


##### Question: `What does this project / application do?`

This is to make sure we (GitHub) understand what this application does as the focus on our analysis in analysis from Sources to Sinks and in some contexts we won't out of the box find any issues.

*Contexts (alert bells):*

- "it's a Library / Module / Package"
  - [Scanning Libraries Guide](./codeql-scanning-libraries.md)
- "it's a CLI tool"
  - [CodeQL Enabling Extra Queries](./codeql-enabling-extra-queries.md#local-query-variates)
- "it's a Desktop Client"
- "it's a Custom Framework"


##### Question: `How big is the Application / Project?`

Make sure to gather information around how big or small the code bases that are being scanned are (exact LoC is the best but rough guesses also helps).
Very Small to Small (see table below) code bases will have very little to no security issues on adverage contained within them.

*Size ranges:*

| Name       | Size (LoC)       |
| :--------- | :--------------- |
| Very Small | 0-2k LoC         |
| Small      | 2-10k LoC        |
| Medium     | 10-300k LoC      |
| Large      | 300k-999k LoC    |
| Very Large | 1M and above LoC |

*Suggest the following:*

- For the developer team to create a Draft PR with a security issue in the PR; Or/And
    - This will show the team that thew workflow with the CodeAL is working and reassure them
- For the developer team to analysis another larger repository or application
    - This will show on averaged more results to show the developers


### Step 2 - Technologies, Libraries, and Frameworks

<!-- **`Question: What Technologies, Libraries, and Frameworks are you using in your applications?`** -->
##### Question: `What languages are you trying to analysis?`

This question should have been asked during the qualification stage of a Trail but if the customer is using a (primarily web) framework that isn't in our [list of supported libraries and frameworks](https://codeql.github.com/docs/codeql-overview/supported-languages-and-frameworks/), this could point you to the fact CodeQL doesn't understand the applications Sources or Sinks.

Also, if they are trying to scan non-supported CodeQL languages please checkout our [list of integrations with tools](./code-scanning-integrations.md) to fill the gap.

If the customer is using a languages that isn't supported by CodeQL Natively, please provide [feedback in the corresponding customer-feedback issue.](https://github.com/github/customer-feedback/issues/2823)

<!-- Engage with Services -->


##### Question: `Does your repository use unsupported CodeQL languages?`

If the the repository contains multiple languages that rely heavily on each other then CodeQL will not be able to correctly perform the analysis on the code that is supported.

*Examples:*

- Not an issue: `Java` backend with `JavaScript` frontend
- Possible issue: `Java` project with `Kotlin` or/and `Scala`
- Possible issue: `Java` using Native interface to call `C` / `CPP` libraries

<!-- TODO: What should the SE do at this point? -->

If the customer is using a languages that isn't supported by CodeQL Natively, please provide [feedback in the corresponding customer-feedback issue.](https://github.com/github/customer-feedback/issues/2823)


##### Question: `What frameworks are you using?`

Using the Dependency Graph and talking to the developers, check to see if their frameworks are present on our [list of supported libraries and frameworks](https://codeql.github.com/docs/codeql-overview/supported-languages-and-frameworks/), this could point you to the fact CodeQL doesn't understand the applications Sources or Sinks.

*The 4 primarily fields of interest are:*

- `What web framework are you using?`
- `What network libraries are you using?`
- `What database drivers or ORM are you using?`
- `What frontend framework are you using?`

Make sure that you confirm with the customer the importance of these types of libraries in a security context.

If the framework is not present in the list, please add the [framework to the customer-feedback issue (in the corresponding language issue)](https://github.com/github/customer-feedback/issues/2930) that needs to be supported for this customer in order to get a proper analysis.
In some cases this might be a documentation issue and not a supported by CodeQL issue.


##### Framework/Library Extra Support

Check the list of [technologies that CodeQL needs a little more support around](./codeql-technologies-support) to make sure that they don't have one of the technologies that can be an issue with CodeQL.

Generally these frameworks and libraries without extra support will not break the analysis and doesn't produce any errors.


### Step 3 - Understanding their Application Threat Model

Something to consider with the customer is the threat model they are working under.
CodeQL assumes that the application is primarily a Web Application (or web based Micro-Service) and that some sources of input are not considered as user defined input (compared to other SAST vendors).
Other vendors will assume sources of user defined input such as CLI Arguments, Environment Variables, and File contents as sources of user data where CodeQL doesn't assume this but have a set of queries (for some languages) to combat this issue.

A great example of this is:

> a customer might scan a sample snippet of code with the sources of data comes from CLI arguments, ends up in a sink (SQL driver, command execution, etc.) and CodeQL don't report any results.

This is because CodeQL does not consider by default that the CLI argument is a source of user defined data which in most cases is not a security issue (trusted source of data) but in other cases (Desktop Applications, CLI tools, concerned about insider threats, ext.) might consider them as sources.

Here is an example of how to enabled more Local queries:

<!-- TODO: Add Local queries samples -->
- [CodeQL Configuration to add Java Local queries](https://github.com/GeekMasher/security-queries/blob/main/config/codeql-local.yml)


### Step 4 - Build issues, Logs, and Source Code

#### Reviewing Analysed Source Code

To be able to review the source code that CodeQL has analysed, you'll need to follow these two guides:

- [Capturing the CodeQL Database](./codeql-database-debugging.md#Capturing-the-CodeQL-Database)
- [Validating Analyzed Source Code](./codeql-database-debugging.md#Validating-Analyzed-Source-Code)


<!-- TODO: Reviewing the CodeQL Logs -->


### Step 5 - Missing Sources and Sinks

#### Enabling External API query

The External API Query was a cross-language query that allows for users and customers to find missing sinks in there applications.
This can be because of a multitude of issues but primarily it could be the following:

- Framework / Libraries not supported by CodeQL
- Heavy Inner Sourcing and using internal Libraries

To enable the queries, [use the following guide](./codeql-enabling-external-apis.md).


## References

- [Lack of results: triaging steps](https://docs.google.com/document/d/1wiKZGpzr_sJXWuvq0sHSxxbLRk8JSC1Es59h2I8mBZg/edit#)
