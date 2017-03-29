---
title: Maven version ranges
published: true
description: Understand how maven version ranges are working
cover_image: 
tags: maven, apache
---

Hey folks 👋

this week I stumbled across problems with maven dependency version ranges.

So here we are - a short article about maven dependency version ranges and possible advantages/disadvantages based on my experience.

# What are version ranges? 😳
Maven allows you to define a range of version numbers (In a specific format) to resolve automatically the latest version of a defined dependency by the next build execution.

# What version ranges am I able to specify? 🛠

| Range        | Meaning
| ------------- |:-------------:|
| 1.0      | x >= 1.0 * The default Maven meaning for 1.0 is everything (,) but with 1.0 recommended. Obviously this doesn't work for enforcing versions here, so it has been redefined as a minimum version. |
| (,1.0]      | x <= 1.0      |
| (,1.0)      | x < 1.0      |
| [1.0]      | x == 1.0      |
| [1.0,)      | x >= 1.0      |
| (1.0,)      | x > 1.0      |
| (1.0,2.0)      | 1.0 < x < 2.0      |
| [1.0,2.0]      | 1.0 <= x <= 2.0      |
| (,1.0],[1.2,)      | x <= 1.0 or x >= 1.2. Multiple sets are comma-separated      |
| (,1.1),(1.1,)      | x != 1.1      |

For example:

```
<dependency>
    <groupId>com.example</groupId>
    <artifactId>healthcheck</artifactId>
    <version>[0.0.5,0.3.0)</version>
</dependency>
```

# What's the current version? 🔮

You're able to get the actual version by running `mvn dependency:list |grep healthcheck` 

The result will provide the actual version number. It could look like this: `[INFO] com.example:healthcheck:jar:0.0.7:compile`

# Why should I use it? 👍
It's magic. An update to newer dependencies within the defined ranges without hitting the keyboard. So less work to keep an eye for any updates and "magic" gain of possible new features/fixes of the dependency.

# Why not? 👎
You never can't be sure that newer versions are compatible to your code and are unbroken. So it could be possible that you're build/compilation will magically be broke caused of newer dependencies.

# Conclusion 🎬
Well in my opinion the usage of version ranges is a matter of taste and also depends of the used technology. For example in the JavaScript 🌎 version ranges are more recommended and popular. 

For Maven especially I like to avoid version ranges to keep my dependency list clean and short to have a better overview about used versions, certain sub-dependencies and avoid possible site effect of magic updates. 🔮

# Hints 🚀
👉 You can resolve newer dependency versions by using `mvn versions:display-dependency-updates`

👉 Replace version ranges automatically by run `mvn versions:resolve-ranges` or based on specific groupId's `mvn versions:resolve-ranges -Dincludes=com.example:*` (',' separation possible)

👉 Be careful resolving SNAPSHOT dependencies by version ranges. They could be under construction and manipulate your expected behavior.

#### RELATED:
[Official apache maven docs](http://maven.apache.org/enforcer/enforcer-rules/versionRanges.html)
