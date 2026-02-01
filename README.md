# 👋 Hello Kube

[![Hugo](https://img.shields.io/badge/Hugo-0.146+-blue.svg)](https://gohugo.io)
[![Theme](https://img.shields.io/badge/Theme-PaperMod-green.svg)](https://github.com/adityatelange/hugo-PaperMod)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Website](https://img.shields.io/badge/Website-hellokube.dev-orange.svg)](https://hellokube.dev)

A developer blog focused on **Kubernetes**, **containers**, and **cloud-native technologies**. Built with Hugo and the PaperMod theme.

🌐 **Live Site**: [hellokube.dev](https://hellokube.dev)

---

## 📖 About

Hello Kube is a technical blog sharing practical tutorials and hands-on guides for developers working with:

- 🚢 **Kubernetes** - Minikube, KinD, K3d, and production patterns
- 📦 **Containers** - Docker, Podman, and container runtimes
- ⚡ **Event Streaming** - Apache Kafka and messaging systems
- 🛠️ **Developer Tools** - WSL2, local development environments
- ☁️ **Cloud Native** - Best practices and real-world experiences

---

## 🚀 Quick Start

### Prerequisites

- [Hugo](https://gohugo.io/installation/) (v0.146.0 or higher)
- [Git](https://git-scm.com/)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/hguerrero/hellokube-web.git
   cd hellokube-web
   ```

2. **Initialize the theme submodule**
   ```bash
   git submodule update --init --recursive
   ```

3. **Run the development server**
   ```bash
   hugo server
   ```

4. **View the site**
   
   Open your browser to [http://localhost:1313](http://localhost:1313)

---

## 🛠️ Development

### Project Structure

```
hellokube-web/
├── archetypes/          # Content templates
├── assets/              # Images and other assets
├── content/             # Blog posts and pages
│   ├── posts/          # Blog articles
│   ├── about.md        # About page
│   └── search.md       # Search page
├── static/              # Static files (images, etc.)
├── themes/              # Hugo themes
│   └── PaperMod/       # PaperMod theme (submodule)
├── hugo.yaml           # Site configuration
└── README.md           # This file
```

### Creating New Posts

```bash
hugo new posts/my-new-post.md
```

Edit the front matter and content:

```yaml
---
title: "My New Post"
date: 2026-02-01
categories: ["Kubernetes"]
tags: ["tutorial", "k8s"]
author: "Hugo Guerrero"
---

Your content here...
```

### Building for Production

```bash
hugo --minify
```

The static site will be generated in the `public/` directory.

---

## 🎨 Theme

This site uses the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, which provides:

- ⚡ Fast and lightweight
- 🌙 Dark/Light mode toggle
- 🔍 Built-in search functionality
- 📱 Fully responsive design
- 💬 Disqus comments integration
- 📊 Google Analytics support
- 🎨 Syntax highlighting for code blocks
- 📖 Table of contents generation

---

## ⚙️ Configuration

Key configuration options in `hugo.yaml`:

- **Site Settings**: Base URL, title, language
- **Theme**: PaperMod configuration
- **Analytics**: Google Analytics integration
- **Comments**: Disqus integration
- **Menu**: Navigation items
- **Social Icons**: Twitter, GitHub, LinkedIn, YouTube
- **Search**: Fuse.js search configuration

---

## 📝 Content Guidelines

### Post Front Matter

```yaml
---
title: "Your Post Title"
date: 2026-02-01
categories: ["Category1", "Category2"]
tags: ["tag1", "tag2", "tag3"]
author: "Hugo Guerrero"
draft: false
---
```

### Categories

- Kubernetes
- Containers
- Docker
- WSL2
- Development
- Cloud Native

### Writing Tips

- Use clear, descriptive titles
- Include code examples with syntax highlighting
- Add relevant tags for discoverability
- Include practical, hands-on examples
- Test all commands and code snippets

---

## 🚢 Deployment

### GitHub Pages

1. Configure GitHub Pages in repository settings
2. Set source to `gh-pages` branch
3. Push changes to trigger deployment

### Netlify

1. Connect your repository to Netlify
2. Set build command: `hugo --minify`
3. Set publish directory: `public`
4. Deploy!

### Manual Deployment

```bash
hugo --minify
# Upload contents of public/ to your web server
```

---

## 🤝 Contributing

Contributions are welcome! If you find a typo, broken link, or have suggestions:

1. Fork the repository
2. Create a feature branch (`git checkout -b fix/typo`)
3. Commit your changes (`git commit -m 'Fix typo in post'`)
4. Push to the branch (`git push origin fix/typo`)
5. Open a Pull Request

---

## 📧 Contact

- **Author**: Hugo Guerrero
- **Website**: [hellokube.dev](https://hellokube.dev)
- **Twitter**: [@hellokube](https://twitter.com/hellokube) | [@hguerreroo](https://twitter.com/hguerreroo)
- **GitHub**: [@hguerrero](https://github.com/hguerrero)
- **LinkedIn**: [hugoguerrero](https://www.linkedin.com/in/hugoguerrero/)
- **YouTube**: [@hguerreroo](https://www.youtube.com/@hguerreroo)

---

## 📄 License

Content is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

Code snippets and examples are licensed under [MIT License](LICENSE)

---

## 🙏 Acknowledgments

- [Hugo](https://gohugo.io) - The world's fastest framework for building websites
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod) - A fast, clean, responsive Hugo theme
- All the amazing developers in the Kubernetes and cloud-native community

---

**Happy coding, and may your pods always be running!** 🎉
