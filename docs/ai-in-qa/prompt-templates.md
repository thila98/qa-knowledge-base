# Prompt Templates for QA

These are prompts I use regularly when working with AI tools like Claude and ChatGPT for QA tasks. I have tested and refined these over time. Feel free to copy and adapt them for your own work.

---

## 1. Test case generation

This is the prompt I use most often. I use it when I get a new feature to test and need a solid starting point for my test suite. It works well with both Claude and ChatGPT.

**The prompt:**
You are a Senior QA Engineer with 10 years of experience in software testing.
Given the following feature description, generate comprehensive test cases.
FEATURE:
[paste your feature description here]
Generate test cases in the following categories:

Functional - happy path, core user flows
Negative - invalid inputs, error handling, rejection scenarios
Boundary - edge values, minimum and maximum limits
Security - SQL injection, XSS, unauthorised access attempts
UI/UX - labels, layout, responsiveness, usability
Integration - how this feature connects to other modules or systems

For each test case provide exactly these fields:
TC_ID | Category | Title | Precondition | Test Steps | Expected Result | Priority | Status
Rules:

TC_ID format: TC_001, TC_002 etc
Priority: High / Medium / Low
Status: Not Executed
Test Steps: number each step
Generate at least 15 test cases
Start directly with TC_001, no introduction text


**Why this works:**

The key is telling the AI exactly what role to take (Senior QA Engineer), what categories to cover, and what format to output. Without the format instruction the output is often inconsistent and hard to import into test management tools like Qase or TestRail.

**Example output:**

I used this prompt for a login feature and got 40 test cases back in under 15 seconds, covering everything from valid login to SQL injection to session timeout. I still reviewed and refined them but the coverage was excellent as a starting point.

---

## 2. Requirement gap analysis

I use this before I start writing test cases. When I receive a new requirement or user story I paste it into this prompt first to find gaps and ambiguities before testing begins. This saves a lot of back and forth with developers later.

**The prompt:**
You are an experienced QA Engineer reviewing requirements for testability.
Review the following requirement and identify:

Missing information that would block testing - what questions need answers before I can test this?
Ambiguous statements - words or phrases that could be interpreted in more than one way
Edge cases not mentioned - scenarios the requirement does not address but should
Assumptions being made - things the requirement assumes but does not state
Missing acceptance criteria - what does done actually look like?
Dependencies - other features or systems this relies on that should be tested together

Requirement:
[paste your requirement here]
Format your response as a numbered list under each category above.

**Why this works:**

Requirements are often written by product managers or business analysts who think about the happy path. A QA mindset looks for what can go wrong. This prompt bridges that gap before any code is written.

---

## 3. Bug report writing

When I find a bug during testing I sometimes have rough notes - steps I did, what broke, what I expected. This prompt turns those rough notes into a properly formatted bug report in seconds.

**The prompt:**
Help me write a clear and professional bug report using the following information.
What I did:
[describe the steps you took]
What happened:
[describe what went wrong - be specific]
What should have happened:
[describe the expected behaviour]
Environment:
[browser, OS, device, app version, test environment]
Write a complete bug report with these sections:

Title: short and descriptive, starts with the affected feature
Environment: where the bug was found
Steps to Reproduce: numbered, specific, reproducible
Expected Result: what should happen
Actual Result: what actually happened
Severity: Critical / High / Medium / Low with brief justification
Notes: any additional context, screenshots needed, related issues


**Why this works:**

Good bug reports get fixed faster. Developers should never have to ask for more information. This prompt ensures nothing is missing and the report is clear to anyone reading it, not just you.

---

## 4. Test data generation

When I need realistic test data for different scenarios I use this prompt. It is especially useful for negative and boundary testing where you need specific types of invalid input.

**The prompt:**
Generate realistic test data for testing [feature name] in a [type of application] application.
I need the following:

5 valid data sets for positive/happy path testing
3 invalid data sets for negative testing - each with a different type of invalid input
2 boundary value examples - one at the minimum limit and one at the maximum
1 example with special characters or unusual input that might cause issues
1 example with empty or null values

For each data set explain why you chose those values and what scenario they test.
The data should look realistic and not obviously fake. Use plausible names, emails, phone numbers etc.

**Why this works:**

Using realistic looking test data catches more bugs. If you test a name field with "John" you might miss bugs that appear with longer names, names with apostrophes like O'Brien, or names with accented characters. This prompt makes sure you cover those cases.

---

## 5. Automation script review

Before I commit an automation script I sometimes paste it into Claude and ask for a review. It often catches things I missed or suggests cleaner ways to write assertions.

**The prompt:**
Review the following test automation script and provide feedback on:

Logic errors - any steps that are wrong or assertions that will not work as expected
Missing test coverage - scenarios or assertions that should be there but are not
Code quality - readability, naming conventions, structure
Best practices - anything that goes against standard automation testing practices
Maintenance concerns - anything that will be hard to maintain or likely to break

Also tell me: does this test actually verify what it claims to verify?
Script:
[paste your script here]
Language/Framework: [e.g. Python/pytest, Java/TestNG, JavaScript/Playwright]

**Why this works:**

It is easy to write a test that passes but does not actually verify the right thing. Getting a fresh perspective on your script before it goes into the pipeline catches issues early.

---

## 6. API test scenario generator

When I am about to test a new API endpoint I use this to make sure I think of all the scenarios before I start writing tests.

**The prompt:**
I am testing the following API endpoint. Generate a comprehensive list of test scenarios.
Endpoint details:

Method: [GET / POST / PUT / DELETE]
URL: [endpoint URL]
Request body: [paste request body or write N/A]
Authentication: [type of auth required]
Expected success response: [status code and response structure]

Generate test scenarios covering:

Happy path - valid request with all required fields
Missing required fields - one test per required field
Invalid field values - wrong data types, formats, lengths
Authentication - missing token, expired token, wrong permissions
Boundary values - minimum and maximum values for numeric and string fields
Edge cases - empty strings, null values, very long strings, special characters
Duplicate requests - what happens if the same request is sent twice

For each scenario include: scenario name, input, expected status code, expected response.

---

## Tips for getting better results from AI prompts

**Be specific about the output format**
If you want a table, say you want a table. If you want numbered steps, say so. Vague prompts get vague answers.

**Tell the AI what role to take**
Starting with "You are a Senior QA Engineer" gives much more relevant output than just asking a question directly. The AI adjusts its perspective based on the role you give it.

**Give context about your product**
Adding details like "this is a B2B SaaS application" or "users are enterprise finance teams" helps the AI generate more relevant scenarios.

**Ask for refinements rather than starting over**
If the output is not quite right, tell the AI what is wrong and ask it to fix that specific thing. This is faster than rewriting the whole prompt.

**Break complex requests into steps**
Instead of one huge prompt, break it into smaller steps. Generate the test cases first, then ask for the test data, then ask for the automation script. Each step builds on the previous one.
