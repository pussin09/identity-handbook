---
title: "Acceptance Criteria for Platform PRs and Issues"
description: "How to create and manage Pull Requests and Issues for the Login.gov Platform Teams."
layout: article
redirect_from:
category: Platform
subcategory: Team
---

## Overview

This document is used to define the minimal ***acceptance criteria (AC)*** for **Pull Requests** (a.k.a. **Merge Requests**) and **Issues** for the Login.gov Platform teams.

### Style and Sanitizing

FIXME!!! Does this belong up here?

To be kind to the reviewer!  More importantly: don't leak sensitive data into places it should not be!

0. Replace ***ANY/ALL*** sensitive strings with `<REDACTED>` strings. This includes tokens, keys, etc.
1. Contain all content within code blocks, using language syntax highlighting when possible
2. Remove all unnecessary sections of content, i.e. excess log messages
3. Use `[...]` to show gaps where extra content was removed between output you are including

#### Organizing Large Output with `<details>` Dropdown Blocks

If the command-line output demonstrating the PR's changes is particularly large in size (i.e. 30+ lines), it can be wrapped within a `<details>` HTML block, which will output a clickable, dropdown text link when the description is submitted:

   ```
   <details><summary> <tt>terraform plan</tt> output </summary>

	`` ``` ``

	`` ``` ``

	</details>
   ```
   
* Text within the `<tt>` tags will be marked as `code`; these tags can be removed if desired
* The line breaks between the `<details>` tags, and the actual code block, ***must*** remain in order for the code block formatting to render correctly.

## Pull Requests

FIXME!!! This is long and makes me sleepy.

1. A note of what is being changed/fixed by the PR.
2. Outputs demonstrating that the changes can be implemented without errors (more information below).
3. Any corresponding Issue(s) that this PR *addresses* (as, generally, a PR will only address part of an Issue's AC)
4. Information regarding any breaking changes that may/will result once the change is implemented, along with steps detailing how to mitigate/roll back these changes in case of an error

Aside from the command-line outputs as detailed below, all PRs ***must*** indicate:

1. Which ***environment(s)*** these changes have been verified to work in. Most of the time, this will be a personal sandbox environment, and can be tested there without Issue. If the changes impact an account, such as those within Terraform directories `core` or `all`, it is acceptable to *test, then roll back,* the changes in the code within a `sandbox`/`dev` account, as long as the DevOps team is notified (within Slack) when the testing is about to take place.
2. Any ***breaking changes*** that will occur as a result of this PR being merged, whether just to `main` or when a particular `stages/` branch is fast-forwarded to a new release. The author of the PR *must* inform any/all individuals whose work may be impacted by the change, and tag the PR with the `BREAKING` label. Most of the time, this will be the On Call engineer who deploys a release containing this PR's changes.
3. Steps detailing, if they are necessary, any ***additional operations*** required in order to fully implement the change(s). These primarily will include changes to resources within the Terraform `state`, such as `state rm`/`mv`, `import`, etc. which must be performed *manually* before/after the plan is applied.
4. Similarly -- if necessary -- steps detailing any ***mitigation steps***, which fall *outside* of normal mitigation steps, that will need to be taken/recognized in case the plan needs to be rolled back. These will almost always need to be listed if there are any breaking changes, as defined in #2 above.

### Terraform PRs

FIXME!!!

All PRs containing Terraform changes ***must*** include or point to the output of a `terraform plan` for each "state" type that will be changed when `terraform apply` is run.

### Chef Cookbook/Recipe PRs

FIXME!!! (or kill it?)

### Why do PRs matter?

Pull Requests (a.k.a. Merge Requests) are the magic tool that allows us to move quickly and safely through continuos change:

* They are a way to lift each other up to higher standards, learn from each other, and improve
* They protect against intentional or unintentional damage to the system
* They are a critical part of how we meet the [NIST 800-53 CM control family](https://csrc.nist.gov/projects/cprt/catalog#/cprt/framework/version/SP_800_53_5_1_0/home?element=CM)

For more on the art of the PR see
[The Art of Giving and Receiving Code Reviews (Gracefully)](https://www.alexandra-hill.com/2018/06/25/the-art-of-giving-and-receiving-code-reviews/).

## Issues

FIXME!!! HEY FELLOW AUTHORS!  Should we just replace this with a link to https://handbook.login.gov/articles/definition-of-ready.html ?

### User Stories

**User Stories** must contain the following:

1. A **Problem**, used to define the desired outcome of the Issue and framed as a *user story*, i.e. "As a `USER`, I want to `DESIRED_ACTIVITY`, so that I can `VALUE_PROVIDED`."
2. Suggested **Solutions**, which give hints to the implementer on how they might approach the problem.  These are only suggestions!
3. **Acceptance Criteria** which defines additional requirements that must be met.  The **ultimate** AC is "Met the story!"
4. Assume AC is tied to release to `prod` (or `gitlab production` for GitLab) unless otherwise specified.  Make sure it shiped!

An example:

> *Problem:*
> 
> As a **Service Provider**,<br>I want to **be able to successfully upload images via a web form**,<br>so that I can **attach a logo to my web application**.
> 
> *Solution(s):*
> 
> - [ ] Create a new per-application environment S3 bucket to hold the images
> - [ ] Verify least privelge policy/permissions on the S3 bucket
> - [ ] Test API actions involved in uploading to the bucket via the web app
> - [ ] Make sure the images are visible in the dashboard
> 
> *Acceptance Criteria:*
> 
> 1. Users *are* able to upload files to the bucket via the web form in `int`
> 2. Bucket policy/permissions fall within specifications for compliance/security

### Bug Reports

**Bug Report** style can be used for bugs and regressions.  (Feel free to submit these in a story style if you prefer.)

**Bug Report** style must contain the following elements:

1. A **Description** providing a basic summary of the problematic behavior this report is describing.
2. **Detail** about how the Issue was discovered, including:
   - **Steps To Reproduce**, which should be precise but succinct
   - **Expected Behavior** indicating what should happen as a result of the Steps To Reproduce
   - **Actual Behavior**, indicating the difference from Expected Behavior and/or more details of the behavior covered in the Description
3. **Suggested Actions** to help define the work to be done, similar to **Solutions** in User Story Issues
4. **Acceptance Criteria** can just reference the Expected Behavior or add more elements as needed

### Issue Acceptance

An Issue is only **Done** when ACs are met and **evidence** is recorded to prove that the value described in the story has been delivered.  What is **evidence**?   It can be any of:
* A note pointing to a functional test that proves the feature/function is working (This is the preferred choice!)
* A screenshot or text output showing the feature/function working
* Shell output showing commands and output to validate the intended functionality

__Remember: We are here to deliver usable working things, not lines of code, or Terraform plans, or reams of documents!  Evidence must focus on the value and not the work done.__

