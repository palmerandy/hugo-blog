---
title: "Finding your hello world"
date: 2020-05-20T18:02:11+11:00
draft: false
tags: ["hello world", "blazor", "react", "typescript", "github pages", ".NET"]
---

I've never liked creating *Hello World* as a way to understand a new technology.  Recently I have been employing a new technique of building a small application that I have a passion for.

For me my passion is running, so instead of building *Hello World* I build a *Running Pace Calculator*.  Below is a mock-up of the funcionality, it is quite simple it that you input your goal time (in hours, minutes and seconds) and the distance and it calculates how fast you would need to run in kilometer splits.  

![Alt text](/images/pace-calculator-mockup.png "Pace Calculator Mock Up")

It could be more complicated but it forces me to learn both the what and how to solve a few important problems:

- What sort of testing does this technlogy support out of the box?  
- How to deploy and host this technology as part of a CI/CD pipeline.  For static web applications I'm finding GitHub Pages an excellent free hosting provider.
- How to validate user input.

More importantly it lets me learn the pros and cons of a technology with firsthand expeirence.  

Here are some recent technologies I created the above Running Pace Calculators with:

### Blazor WebAssembly

- Link to my Blazor WASM based [Running Pace Calculator](https://palmerandy.github.io/Running-Pace-Calculator/).
- Link to <a href="https://github.com/palmerandy/Pace-Calculator" target="_"><i class="fab fa-github fa-lg" aria-hidden="true"></i> GitHub Repository</a>


#### My take on Blazor:
If you are an experienced C# developer you will feel right at home writing C# and having it run in the browswer.  It feels like all of the stars have aligned for Microsoft to make an excellent open source, cross platform, web standard based, Single Page App framework.  I particuarly like the Componet model, Microsoft have learnt from those first to market.

### React
- Link to my React based [Running Pace Calculator](https://palmerandy.github.io/Running-Calculator-React/)
- Link to <a href="https://github.com/palmerandy/Running-Calculator-React" target="_"><i class="fab fa-github fa-lg" aria-hidden="true"></i> GitHub Repository</a>

#### My take on React:
This is my prefered Javascript Single Page App library as it's a library!  I used [Create React App](https://github.com/facebook/create-react-app) and got up and running very quickly.  For large apps it's a no brainer to use TypeScript.

My biggest problem was not being able to easily set a value of a select element from the in built testing framework - more information in my <a href="https://github.com/palmerandy/Running-Calculator-React/issues/4" target="_"><i class="fab fa-github fa-lg" aria-hidden="true"></i> GitHub issue</a>
