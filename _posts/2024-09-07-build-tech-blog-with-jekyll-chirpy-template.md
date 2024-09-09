---
title: Build tech blog with Jekyll Chirpy template
date: 2024-09-07 17:00:00 +0900
categories: [blog]
tags: [blog, jekyll, chirpy, static site generator]
author: wonhee
toc: true
comments: true
---

## Summary

In this post, I will introduce the various options I explored 

and the pros and cons I encountered while building a tech blog.

I will also explain why I ultimately chose the combination of Jekyll + Chirpy template.

## My Criteria

The criteria I considered when choosing a platform and template for a tech blog were as follows.

1. Must support Markdown natively.
2. The design has to be visually appealing.
3. The Table of Content should appear on the right side, and it would be great if it highlighted when scrolling.
4. Comment functionality should be natively supported.
5. If it’s a one-time payment, I don’t mind it being a paid option.
6. There must be a wide variety of custom templates available for blogs.
7. The template should be actively maintained.
8. Migration should be easy.
9. There should be a tag section for posts, and when clicking on each tag, the related posts should be sorted and displayed.

## Tech Blog Platform Options

Below are the platforms I experimented with to build a tech blog,

along with their pros and cons based on [my criteria](/posts/BuildTechBlogWithJekyllChirpyTemplate/#my-criteria).

### WordPress

There’s a domain-provided model (https://wordpress.com) and the option to set up a self-hosted version.

I had already purchased a domain that I wanted to run on AWS (wonhee.net), and I could easily launch a pre-optimized WordPress environment with AWS Lightsail.

While WordPress offers many plugins and is user-friendly, the fact that documents are stored in a database makes migration inconvenient.

I felt that a simple yet powerful Markdown-based static site generator would be more suited for a tech blog,

so I discontinued using WordPress during customization process.

![alt text](/assets/img/image2.png)

#### Pros
- Can be built using a domain-provided model.
- If you have server knowledge, you can run it on cloud providers like AWS, Azure, or GCP.

#### Cons
- Not Markdown-based.
- Too heavy for a tech blog.
- Documents are stored in a database, making migration cumbersome.

#### Ref
- [https://wordpress.org](https://wordpress.org)
- [https://wordpress.com](https://wordpress.com)
- [https://lightsail.aws.amazon.com](https://lightsail.aws.amazon.com)

### VitePress

This is a solution that my company uses as a document generator.

Since it's now being used for all official Vue documents, it’s trustworthy.

The design is clean, and it's actively being developed, which made it appealing.

However, since it's specialized for document sites, customizing it for a blog seemed necessary.

While some blog-customized solutions were available as open source, they didn’t quite meet my expectations.

![alt text](/assets/img/image3.png)

![alt text](/assets/img/image6.png)

#### Pros
- Actively being developed.
- The design is clean and well-made.
- Easy to customize if you know Vue.

#### Cons
- Optimized for document sites, not well-suited for blogs.

#### Ref
- [https://vitepress.dev](https://vitepress.dev)

### Hugo

We also use Hugo in the company as a document generator.

It has a fast build speed, and changes are instantly reflected.

While there are multiple blog templates available,

I couldn’t find a template that matched my needs when compared to Jekyll.

![alt text](/assets/img/image4.png)

#### Pros
- Fast build speed.
- Immediate content reflection, making it easy to work with.

#### Cons
- The number of templates is significantly less compared to Jekyll.

#### Ref
- [https://gohugo.io](https://gohugo.io)
- [https://github.com/QIN2DIM/awesome-hugo-themes](https://github.com/QIN2DIM/awesome-hugo-themes)

### Jekyll (+ Chirpy theme)

Since Jekyll is officially supported by GitHub Pages,

I found many useful templates on the [https://github.com/topics/jekyll-theme](https://github.com/topics/jekyll-theme) page.

Previously, deploying Jekyll or similar platforms to GitHub Pages required complex configuration.

But nowadays, GitHub Actions makes deployment easier, reducing setup hassles.

![alt text](/assets/img/image7.png)

Among the various templates, I chose the [https://github.com/cotes2020/jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) template,

which satisfied all [my criteria](/posts/BuildTechBlogWithJekyllChirpyTemplate/#my-criteria). I’m very happy with its design and features.

![alt text](/assets/img/image5.png)

#### Pros
- GitHub provides a ready-made GitHub Actions example for deploying Jekyll, so you just need to use it.
- Meets all the criteria mentioned in [my criteria](/posts/BuildTechBlogWithJekyllChirpyTemplate/#my-criteria).
- A wide variety of blog-related templates are available.
- The design is beautiful.

#### Cons
- It’s based on Ruby, so some knowledge of the Ruby ecosystem is needed for customization.

#### Ref
- [https://jekyllrb.com](https://jekyllrb.com)
- [https://github.com/topics/jekyll-theme](https://github.com/topics/jekyll-theme)

## Conclusion

Although I ended up choosing Jekyll + Chirpy theme,

Gatsby, a React-based platform, seems to be gaining popularity.

For now, I’ll run my tech blog in its current form, but I might explore Gatsby if changes are needed in the future.