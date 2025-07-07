---
title: lt - a TUI client for Linear.app
slug: introducing-lt
description: The TUI client no one asked for
tags:
  - technical
added: 2025-07-07T03:05:33.401Z
---

I'll be the first to admit that the intersection between the sets of people that prefer terminal UI (TUI) apps and use Linear.app for issue tracking is tiny. It has at least a population of 1 - me.

Seeing that finding product market fit should be a walk in the park, I wrote a \[TUI client for Linear.app called lt]\([https://github.com/markmarkoh/lt](https://github.com/markmarkoh/lt)).

![](</assets/2025-07-02 20.50.09.gif>)

I needed an excuse to learn a new language, so I read the first half of the Rust book and then dove in.

Here are the highlights and lowlights:

#### Highlights

Rust + Neovim, with rust-analyzer, makes for a pretty solid development experience.

Rust is a lovely language. The pattern matching feels very natural, and the Option/Result types are lovely and simple.

Crates.io is nice and it was easy enough to find crates that I needed, like one for GraphQL and one for writing TUI apps.

I've heard horror stories of async Rust, and maybe my use of async was very isolated, but it wasn't difficult to get async working.

Publishing an executable to be installable via Homebrew was simple (using a tap)

#### Lowlights

I find the generic and typical docs.rs Rust documentation to be difficult to parse and follow. At least the docs are uniform.

While I was able to find packages to do the heavy lifting, the main graphql package hasn't been updated in years, and the TUI library called ratatui has a website that needs some love viz-a-viz information architecture. 
