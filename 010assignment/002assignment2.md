# Goal
Evaluate a (free) Static Analysis Security Testing (SAST) tool for React OR .NET. SAST tools are source code analyzers which help in secure coding. While Linters are tools that can scan and analyze your code for bugs and common programming mistakes, SAST tools go one step further and analyze your code for security bugs. 
The goal of this assignment is to scan a React or .NET application of your choice using a SAST tool of your choice and interpret the results. I expect you to take (or make) a reasonably large application and find and discuss at least three security issues (of which one major) found by the SAST tool. 

You may deliberately introduce vulnerabilities in case the application you are using does not lead to any useful results. I do expect you to provide a fix for the vulnerabilities discussed (the fix may be commented out in the source code, or may be described in your document). 

# Assignment clarification
`Reasonably large` is not further defined and you can ignore those words: "I expect you to find or make an application and find and discuss at least three ..."

You can take whatever application you want. However, a submission with three bugs present in only three lines of code is not very realistic. The more realistic, the better the grade. You will likely get the most realistic results when using an application that contains the typical architectural parts: front-end, business logic, and data storage. 

As a final hint for those who are still in doubt: ask yourself why you don't take an opensource project that is actually used by people in real life and run a SAST tool against that? It will be much easier than making one yourself, and even if you deliberately  introduce bugs, they will most likely still be realistic.  

