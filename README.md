# AzAPI Terraform Provider: Best Practices for Success

[![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This repository contains a comprehensive guide for using the AzAPI Terraform provider effectively. The guide is built as a static website using Hugo and is designed to be deployed on GitHub Pages.

## 🌐 Website

Visit the live guide at: https://matt-ffffff.github.io/azapi-best-practices/

## 🛠️ Local Development

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.150.0 or later)
- Git

### Running Locally

1. Clone the repository:
   ```bash
   git clone https://github.com/matt-FFFFFF/azapi-best-practices.git
   cd azapi-best-practices
   ```

2. Initialize submodules:
   ```bash
   git submodule update --init --recursive
   ```

3. Start the development server:
   ```bash
   hugo server
   ```

4. Open http://localhost:1313 in your browser

### Building for Production

```bash
hugo --minify
```

The built site will be in the `public/` directory.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests to improve the guide.

## 📑 License

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg

## 🏗️ Built With

- [Hugo](https://gohugo.io/) - Static site generator
- [Hugo Book Theme](https://github.com/alex-shpak/hugo-book) - Clean book-style theme
- [Mermaid](https://mermaid-js.github.io/) - Diagram and flowchart generation
- [GitHub Actions](https://github.com/features/actions) - CI/CD for automated deployment
- [GitHub Pages](https://pages.github.com/) - Static site hosting
