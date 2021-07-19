# Enabling CodeQL External APIs Queries

This document is focused on helping the field debug the possibility of missing sinks in a customer application to tackle the issue of no results being found during a Trial.
One of the primary reasons is that CodeQL isn’t discovering particular sinks due to the nature of SAST not knowing what a particular function or pattern does.

The Untrusted Data to External API was created to find unknown sinks in an applications source code to possibly identify sinks CodeQL isn’t aware of.
This includes both internally written code (libraries, modules, packages, etc) and open source libraries & frameworks.


<!-- TODO: ## Recording -->


## Enabling the Queries

This step requires either write access to the repository affected by the issue or in conversation with a user that can write to the affected repository.

### 1. Create a new branch in the repository

Creating a branch to perform these changes to allow us to separate the results from the query and the current production security issues.

### 2. Create and edit a CodeQL configuration to enable the queries

Create or update a file in the following location `.github/codeql/codeql-config.yml` in the repository you want to enable this query on. Update the content with the Appendix CodeQL Configuration which will enable this query for 5 languages (CPP, CSharp, Java, JavaScript, and Python) that currently support this query.

```yml
name: "CodeQL External API Configuration"

# This disabled all other queries except for the ones defined in this
#  configuration file.
disable-default-queries: true

queries:
  # Query Suite
  - uses: ./.github/codeql/external-api.qls
```

**References:**

- [Using a custom configuration file](https://docs.github.com/en/free-pro-team@latest/github/finding-security-vulnerabilities-and-errors-in-your-code/configuring-code-scanning#using-a-custom-configuration-file)

### 3. Create a CodeQL Query Suite

The next file to create is a CodeQL Query suite (a collection of queries) at the following location: `.github/codeql/external-api.qls`.
Inside the file you will need to define with queries to use / import and replace the `{{LANG}}` with the language you require.

```yml
- description: "CodeQL External API Suite"

- import: codeql-suites/{{LANG}}-security-extended.qls
  from: codeql-{{LANG}}

- qlpack: codeql-{{LANG}}
- include:
    id:
      - {{LANG}}/untrusted-data-to-external-api
```

##### Examples Query Suites

- [C/CPP](https://github.com/GeekMasher/security-queries/blob/main/cpp/suites/codeql-external-api.qls)
- [Java](https://github.com/GeekMasher/security-queries/blob/main/java/suites/codeql-external-api.qls)
- [JavaScript / TypeScript](https://github.com/GeekMasher/security-queries/blob/main/java/suites/codeql-external-api.qls)
- [Python Suite](https://github.com/GeekMasher/security-queries/blob/main/python/suites/codeql-external-api.qls)


### 4. Update the CodeQL Workflow to enable the configuration

Edit and update the CodeQL Workflow to make sure that the configuration file will be used in the next analysis update adding a `config-file` variable of the `github/codeql-action/init@v1`.

```yaml
- uses: github/codeql-action/init@v1
 with:
   config-file: ./.github/codeql/codeql-config.yml
```

### 5. Run the build/analysis and verify the results

Trigger a new Action to run the newly updated workflow which will then publish the results into Code Scanning.

Once completed navigate to the GitHub Code Scanning tab of the repository and make sure to select CodeQL at the security tool.
Drop down and select the branch to which your analysis was run on.
You should see a new category of results called `Untrusted Data to External API` which can also be filtered to only those specific results by setting the Rule filter to `java/untrusted-data-to-external-api`.


## Alternative (GHAS Cloud)

You can just use the following configuration to easier enable the queries for most / all languages.


```yaml
# ...
- uses: github/codeql-action/init@v1
 with:
   config-file: GeekMasher/security-queries/config/codeql-external-api.yml@main
# ...
```


## Identification of Sinks

This step requires the user to have technical knowledge in the areas of reading code (developer background) and understanding how to identify a security sink in code.

### Reviewing Instances

The part typically is achieved when you have a session(s) with the developer and security teams that have context of their internal libraries or a list of open source dependencies. This is because the unknown sinks that these queries find need to have context to add value to an organisation and most of the time these sinks are irrelevant in regard to security.

When reviewing a sink, there are different sinks types you want to identify such as the following:

- Database Query sinks (SQL Injection and NoSQL Injection)
- Returning output to the user without escaping (Cross-site Scripting)
- File paths (Path Injection, Path Manipulation, etc)

*Note: This is a limited set of sinks types you want to identify, Services should be involved for complex and thorough reviewing of these sinks.*

Once these unknown sinks have been discovered, you have either the option to write engage with 


## Actionable Paths

If the identified libraries are built internally by the customer or are Open Source, there are multiple things to consider and action upon. Take a look at the best option for your use case.


### 1. Request Field Architecture Team (free Trails, based on availability)

This option is best for: **Demostrating and Speed**

For demonstrating and supporting the customer to a degree, Field Architecture can support a customer to explain what needs to be done and how this would be done.

[Request support here](https://github.com/github/advanced-security-field/issues/new?assignees=&labels=request&template=ghas-expert-meeting.md)


### 2. Request Services Support (paid, PS hours required)

This option is best for: **Speed and Completeness**

If there are a large number of issues and your customer is looking to fully support a library or framework, Services should be engaged for these advance customisations.


### 3. Request CodeQL Product Support

This option is best for: **Completeness**

Alongside these two other solutions and primarily only for Open Source projects, please search for and (if it doesn’t exist) create a Customer Feedback request to make sure the product is aware of the support needed for a particular library or framework.

This can take time to get Product time to evaluate and add support for a new Open Source library or framework which can delay or impact a POC’s timeline.

- [Customer Feedback: CodeQL and GHAS Code Scanning languages support](https://github.com/github/customer-feedback/issues/2823)
- Ask for support in `#codeql-support` Slack channel
