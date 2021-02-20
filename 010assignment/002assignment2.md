# Goal
Follow the tutorial of https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc?view=aspnetcore-5.0&tabs=visual-studio to set up a new application with a datastore. 

## Make it
Evaluate if **and how** the end result of this tutorial protects against the following attacks by default:
- HTTPS
- CSRF 
- XSS
- SQLi
- IDOR

If not, introduce a fix. 

## Break it
After your evaluation, you must deliberately (re-)introduce two vulnerabilities of the following categories: CSRF, XSS, SQLi, IDOR. Each vulnerability must be of a different category and a PoC must be provided in the assignment report.
You must then find a (free) Static Analysis Security Testing (SAST) tool for .NET, and verify that it finds the vulnerabilities that you have introduced.