---
title: Project Lombok
published: true
description: Making your curious about writing less code.
tags: lombok, java, boilerplate
---

I've been using and appreciating Lombok for some time. It's one of the first dependencies I drop into my Java projects. This article is just about making you curious writing less, more clean code and of course save some time. 

⏳

# What kind of problems solves Project Lombok? 👀
Well I think this quote is nailing it.

> "Boilerplate" is a term used to describe code that is repeated in many parts of an application with little alteration. One of the most frequently voiced criticisms of the Java language is the volume of this type of code that is found in most projects. This problem is frequently a result of design decisions in various libraries, but is exacerbated by limitations in the language itself. Project Lombok aims to reduce the prevalence of some of the worst offenders by replacing them with a simple set of annotations.
> - [Michael Kimberlin](https://twitter.com/mkimberlin)

# Show me some code! 🙏
```
package com.codebud7.example;

import lombok.Builder;
import lombok.Getter;
import lombok.ToString;
import lombok.extern.java.Log;

@Log // or: @Slf4j @CommonsLog @Log4j @Log4j2 @XSlf4j
public class LombokDemoApplication
{
    public static void main(String[] args)
    {
        Article article = Article.builder()
            .title("Project Lombok")
            .published(false)
            .description("It's magic!")
            .build();

        log.info(article.toString());
    }
}

@Getter
@Builder // or: @Setter
@ToString
class Article
{
    private String title;
    private Boolean published;
    private String description;
}
```
This is just a small example of some powerful Lombok features. 

On top of the LombokDemoApplication class you can see the `@Log` annotation. Instead of declaring an instance of a logger framework of your choice Project Lombok is doing this for you to save common declarations.

Let's have a look at the Article class. Use `@Getter` and you never will write 
`public int getFoo() {return foo;}` again. The `@Builder` annotation produces complex builder APIs for your classes and `@ToString` is printing your class name, along with each field, in order, separated by commas by default.

# Next steps 🎓
So if you're curious and want to know some more details about Project Lombok you should visit the following sites:

👉 [The official project Lombok docs](https://projectlombok.org/index.html)
👉 [Nice introduction to more Lombok features by baeldung](http://www.baeldung.com/intro-to-project-lombok)
