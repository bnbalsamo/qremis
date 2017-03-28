# What is qremis?

Qremis is a [PREMIS](http://www.loc.gov/standards/premis/) fork with different design goals. Qremis's design goals differ from PREMIS's in the following ways:

- Qremis prioritizes machine readability
- Qremis is built to work in environments built on modern tools which support concurrency, high availability, and scalability.
- Qremis is driven by tooling - features or changes are driven by _code_ that can be run to produce results.
- Qremis is a _data_ standard, not a standard for any particular serialization. The guiding principals for element definition are shared with the guiding principals for good object design in object oriented programming.
- Qremis is designed to be primarily edited via appending new data.

# What is qremis' relation to PREMIS?

PREMIS is "upstream" of qremis, in the software development sense.
```
When talking about a branch or a fork, the primary branch on the original repository is often referred to as the "upstream", since that is the main place that other changes will come in from.
```
[Source](https://help.github.com/articles/github-glossary/)

Qremis aims to merge changes to relevant portions of PREMIS into it's own data model in a reasonable timeframe, when they become available, but maintains the freedom to disregard or refactor changes which would violate design paradigms or standing workflows.

Qremis (at the moment) aims to be compatible (though not necessarily losslessly) with PREMIS metadata records.

# Who runs qremis?

Hi - I'm [Brian](mailto:balsamo@uchicago.edu) the primary qremis maintainer (at the moment).

The qremis project is driven primarily by me, the thoughts of the folks over at the [PREMIS implementors group mailing list](https://www.loc.gov/standards/premis/pig.html) and anyone who presents a convincing argument (or, better yet, pull request) in this github repository. If the project grows to the point where this organizational structure is insuffecient further organization will be developed.
