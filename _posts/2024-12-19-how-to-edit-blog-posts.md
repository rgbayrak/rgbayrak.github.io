---
layout: post
title: How to Edit and Create Blog Posts
date: 2024-12-19 12:00:00
description: A guide on how to modify existing blog posts and create new ones
tags: tutorial blog editing
categories: guides
---

## How to Edit Existing Blog Posts

### 1. Find the Post File
All blog posts are stored in the `_posts/` directory. Each post is a markdown file with the naming convention:
```
YYYY-MM-DD-title.md
```

### 2. Edit the Front Matter
Each post starts with front matter (the section between `---` markers):

```yaml
---
layout: post
title: Your Post Title
date: 2024-12-19 12:00:00
description: Brief description of your post
tags: tag1 tag2 tag3
categories: category-name
---
```

### 3. Edit the Content
After the front matter, you can write your content using:
- **Markdown** for formatting
- **HTML** for advanced formatting
- **Liquid templates** for dynamic content

## How to Create New Blog Posts

### 1. Create a New File
Create a new `.md` file in the `_posts/` directory with the format:
```
YYYY-MM-DD-your-title.md
```

### 2. Add Front Matter
Start your file with the front matter:

```yaml
---
layout: post
title: Your New Post Title
date: 2024-12-19 12:00:00
description: Brief description of your post
tags: your tags here
categories: your category
---
```

### 3. Write Your Content
Use markdown to write your content:

```markdown
# Main Heading
## Subheading

This is a paragraph with **bold text** and *italic text*.

- List item 1
- List item 2
- List item 3

[Link text](https://example.com)

![Image alt text](path/to/image.jpg)
```

## Available Front Matter Options

- `layout: post` - Required for blog posts
- `title:` - Your post title
- `date:` - Publication date
- `description:` - Brief description (appears in previews)
- `tags:` - Tags for categorization
- `categories:` - Categories for organization
- `thumbnail:` - Path to thumbnail image
- `featured: true` - Mark as featured post
- `external_source:` - For posts from external sources

## Tips for Editing

1. **Preview your changes** by running your Jekyll site locally
2. **Use descriptive titles** that reflect the content
3. **Add relevant tags** to help readers find your posts
4. **Include images** to make posts more engaging
5. **Keep descriptions concise** but informative

## Example of a Complete Post

Here's what a complete blog post structure looks like:

```markdown
---
layout: post
title: My Amazing Research Discovery
date: 2024-12-19 12:00:00
description: I discovered something incredible in my research today
tags: research discovery science
categories: research
thumbnail: /assets/img/research.jpg
---

# My Amazing Research Discovery

Today I made an incredible breakthrough in my research...

## The Discovery

The key finding was...

## Implications

This discovery has several important implications...

## Conclusion

In conclusion, this research opens up new possibilities...
```

That's it! You now know how to edit existing posts and create new ones for your blog. 