---
layout: narrative
title: The CSS Still Has to Work
author: Alex Weisberger
---

This blog is a static site generated with Jekyll published via Netlify. In a bit of delicious irony, the first time I deployed the site and viewed it all of the CSS was missing. 

It a reminder that not everything related to reliability has to be about formal methods or testing practices. Remember, in this blog, nothing is off limits. I'm interested in any and every possible solution to the reliability problem, not just the current popular techniques. 

(Then, when I went to buy the concerningreliability.com domain name, it used a card that I didn't want, but that's besides the point)

Some takeaways:

- Prod vs. dev environment differences erode confidence when dev was used in testing.
- The user interface is important to users

# Environment Differences

Environment differences can be problematic. There was at least [one study about how prevalent configuration errors are](http://opera.ucsd.edu/paper/sosp11-yin.pdf), and this is frightening. This actually just happened at my current company the other day, an error was deployed that only happened in production, related to setting up an external service. How do you catch these errors?

# The UI is important to users

The CSS has to work for a product to be fully functional. I'll speak for myself when I say that I leave out CSS from almost all of my mental real estate, and I almost always focus on system behaviors when thinking about reliability. But, the appearance of the user interface is still important, and either way when it is completely missing it is a clear mistake.

# Even static sites are distributed systems



