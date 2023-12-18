---
layout: default
---

## Welcome to My Website

I'm Walker, a Computer Engineering student at Auburn University with a passion for technology, programming, and cybersecurity. I enjoy working on various projects, from web development to computer graphics, and I'm always eager to learn and apply new skills.


### Blog Posts

Here's a list of my latest blog posts:

{% for post in site.posts %}
  - [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}

### Contact

Feel free to reach out to me:

- **GitHub:** [@TheSlabby](https://github.com/TheSlabby)
- **LinkedIn:** [@walker-mcgilvary](https://www.linkedin.com/in/walker-mcgilvary) 
