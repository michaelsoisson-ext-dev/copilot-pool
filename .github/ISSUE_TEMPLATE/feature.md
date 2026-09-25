# [FEATURE] Add a user logout button

## Short description

Add a logout button so authenticated users can safely sign out of the application.

## Priority

Medium

## Affected component

Frontend

## Context and business need

As an authenticated user,  
I want to log out of the application,  
so that I can protect my account when I finish using the application.

## Expected behavior

1. The logout button is visible to authenticated users.
2. When the user clicks the button, the current session is deleted.
3. The user is redirected to the login page.
4. The user cannot access protected pages after logging out.
5. An error message is displayed if the logout operation fails.

## Acceptance criteria

- [ ] A logout button is displayed for authenticated users.
- [ ] The user session is deleted after logout.
- [ ] The user is redirected to the login page.
- [ ] Protected pages cannot be accessed after logout.
- [ ] Error cases are handled correctly.
- [ ] Automated tests are added or updated.
- [ ] The code follows the project's conventions.

## Technical constraints

- Use the existing authentication service.
- Do not modify the login flow.
- Reuse the existing button component.
- Preserve the current application design.

## Suggested implementation plan

- [ ] Analyze the existing authentication service.
- [ ] Add the logout button to the user menu.
- [ ] Connect the button to the logout method.
- [ ] Redirect the user to the login page.
- [ ] Add or update automated tests.

## Tests to implement

- [ ] Test that the logout button is displayed for authenticated users.
- [ ] Test that the session is deleted after logout.
- [ ] Test that the user is redirected to the login page.
- [ ] Test the logout error case.

## Dependencies and related links

No known dependencies.

## Pre-development checks

- [x] The requirement is sufficiently clear.
- [x] The acceptance criteria are defined.
- [x] Known dependencies are listed.
