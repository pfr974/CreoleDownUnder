+++
title = 'Brief notes on React components'
date = 2024-03-07T14:25:25+11:00
draft = false
summary = "React components"
tags = ['React']
+++

(*Those are brief and personal notes as I follow along the [Udemy course by Maximilian Schwarzmüller](https://www.udemy.com/course/react-the-complete-guide-incl-redux/).*)

# React Components

Components are the key elements of any React application. They are the blocks an application/website can be broken down into. Thinking about it, this comes more naturally to me than when I think of something like MVC. When you look at a websitem you can identify differents parts: header, footer, sidebar, body, etc.

A react component wraps together HTML, CSS and Javascript to create a reusable element for an application. The reusable aspect is important here. That's what makes React so powerful. A good component should be reusable everywhere in the application. Think of something like a button or a header.

Moreover, within a component, we should find related code. This makes the codebase more maintainable and easier to understand. It's easier to go through one single component that has all the code related to a submission form than going through a lenghty js file. This concept is called **separation of concerns**. 