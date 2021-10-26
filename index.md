# Event Loop. Render Queue 

The focus of this presentation is the Render phase of the Event Loop in particular. But at first we will go over some general information about the Event Loop and its related concepts.

## Event Loop. The Basics. Well Known Facts and Concepts

![Image](https://i.stack.imgur.com/E4wh6.gif)

JavaScript is single-threaded, meaning that there is only one call-stack and heap, which are not basically "part of the JS itself", but of the Engine that executes the code, like V8, Rhino, SpiderMonkey, etc. Having a single-thread would be cumbersome if there wasn't the idea of asynchronous programming. 

There are APIs in the browser that have been used by almost any JavaScript developer out there (e.g. “setTimeout”). Those APIs, however, are not provided by the Engine, but are provided by the browser. 


##

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/maryprtavian/event-loop-render-queue/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

### Sources used
