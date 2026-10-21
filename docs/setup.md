# Setup

## 1. Developer account

1. Sign up at developer.servicenow.com ("Sign Up and Start Building") and verify the email.
2. On first sign-in the onboarding asks "Do you code?". Answer: **No (guided experience)**. This lab is administration and ITSM, which is done through forms and wizards, not an IDE. The choice can be changed later.

## 2. Request the instance

1. In the Developer Site, click **Request instance** (top right).
2. Pick the latest release (Brazil) and click **Setup Brazil instance**.
3. When the dialog shows "Ready to use", click **Manage instance** to see the instance URL and the admin password.
4. Store the URL and the password in a password manager. They do not go in this repo, in Jira or in chat.

## 3. First login

1. Open the instance URL and sign in as `admin`.
2. Direct login to the instance URL is what counts as activity. Visiting the developer site does not.

## Keeping the instance alive

* Log in directly at least every 10 days.
* If the instance shows as hibernating, use **Wake Up Instance**.
* If it is reclaimed, request a new one and rebuild from the exported update sets.

## Verification (day 0)

* [ ] Logged in to the instance as admin and saw the platform home page
* [ ] Instance URL and password saved outside the repo
* [ ] Repo created with README and docs/
