# Datomancer

A static data-driven site built with **Jekyll**, **Bootstrap 5** and **D3.js**, exploring topics with interactive visualizations and articles.

## 🚀 Features

- **Interactive D3 charts**  
- **Responsive, modern UI**  
- **Content management via Markdown**  
- **Fast, zero-backend deployment**  

## 🛠️ Tech Stack

- **Jekyll** – static site generator  
- **Bootstrap 5** – responsive layout & components  
- **SCSS** – custom theming and utilities  
- **D3.js (v7)** – data visualization library  
- **Cloudflare Pages** – CI/CD and hosting  
- **GitHub** – source control

## ⚙️ Installation & Local Development

1. **Clone this repo**  
2. **Then start up the project locally**
```bash
cd datomancer
bundle install
bundle exec jekyll serve
```

## Project Stucture

All posts are stored int eh `_posts` folder by year, month, and then post.

Example: `_posts/2025/05/2025-05-05-game-of-robes`

Inside that directory is the markdown file for the post. You could also use HTML with Jekyll. This allows for us to create either templated posts where we use Jekyll layouts (i.e. `post.html`), or custom HTML. This can provide structure where we do not need a lot of custom design, or complex custom designs where it would help a story or post.

Images for the post above are stored at `assets/images/posts/2025/05/2025-05-05-game-of-robes`.

## Publishing & Deployment 🚚

This repository is connected to Cloudflare pages for deployment. Commits to `master` publish to the live site.

## Todo

## Done

Made with 🪄 by the Datomancer team