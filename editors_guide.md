# Editors' Guide

Onboarding at DataONE is managed by a team of editors.  We are piloting
a system of a rotating Editor-in-Chief (EiC).

# EiC Responsibilities

The EiC serves for 3 months or a time agreed to by all members of the editorial
board. The EiC plays the following roles

- Watch all issues posted to the onboarding repo:
-  Assigns compendia to other editors, including self, to handle. Mostly this just rotates among editors, unless the EiC thinks an editor is particularly suited to a package, or an editor rejects due to being too busy/conflict of interest.
- Raises scope/overlap issue with all editors if they see an ambiguous case.  This
may also be done by handling editors (see below). 
 - Responds to pre-submission inquiries and `meta` issues posted to the onboarding
 repo, similarly pinging channel if discussion needed.  But editors should all feel free to chime in on these if they want.
 - Monitors pace of review process and reminds other editors to move packages
 along as needed.

# Handling Editor's Checklist

## Upon submission:

-   Tag issue appropriately with `compendium`, `topic:`, etc. tags. Add `1/editor-checks` tag
    and assign a main editor if you haven't already.
-   Check that template has been properly filled out
-   Check against policies.
    Initiate discussion if needed for edge cases.
    If reject, close issue, direct authors to other groups/repos as appropriate.
-   Check that mandatory parts of template are complete.  If not, close issue,
    direct authors toward appropriate instructions.
-   Run run automated tests: `devtools::check()`, `goodpractice::gp()`, `devtools::spell_check()`. Run
    `covr::package_coverage()` using `NOT_CRAN` if needed, as well. Report
    relevant outputs in the issue thread.
    -   For compendia with compiled code or linking to external libraries or languages,
        check on multiple platforms, using win-builder, r-hub, or other editors
        as needed.
-   If initial checks show major gaps, request changes before assigning reviewers.
-   If the package raises a new issue for ROpenSci policy, open a discussion to discuss it with other
    editors
    
## Assign reviewers:

-   Switch numbered tag to `2/seeking-reviewers`
-   Ask author to add a DataONE review badge to their README. Badge URL is xxx. Full link should be:

```
xxx
```

-   Use the [email template](https://github.com/benmarwick/onboarding-reproducible-compendia/blob/master/review_request_template.md) if needed for inviting reviewers
    -   When inviting reviewers, include something like "if I don't hear from
        you in a week, I'll assume you are unable to review," so as to give a
        clear deadline when you'll move on to looking for someone else.
-   Assign a due date 3 weeks after all reviewers have been found.
-   Once two or more reviewers are found, assign reviewer by tagging in the issue with the
    following format to activate the automated reminders bot:
   
      Reviewer: @githubname1 
      Reviewer: @githubname2
      Due date: YYYY-MM-DD

-   Switch numbered tag `to 3/reviewers-assigned` once reviewers are assigned.
-   Invite authors and reviewers to DataONE Slack if they aren't on already.

## During review:

-   Check in with reviewers and authors occasionally. Offer clarification and help as needed.
-   In general aim for 3 weeks for review, 2 weeks for
    subsequent changes, and 1 week for reviewer approval of changes.
-   Upon all reviews being submitted, change the review status tag to
    `4/review-in-awaiting-changes` to update the reminder bot.
-   Upon changes being made, change the review status tag to `5/awaiting-reviewer-response`.
    
## After review:

-  Change the status tag to `6/approved`.
-   Add review/er information to the review database.

-   Ask author to:
    -   Add DataONE footer to README
-   Close the onboarding issue. 

## Compendium promotion:

-  Ask authors to write either a blog post or a tech-notes post for the compendium,
   as appropriate.
-   Write a short summary for a regular "new compendia round-up" blog post.
-   Tweet, etc.

