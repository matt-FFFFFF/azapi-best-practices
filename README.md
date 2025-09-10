# AzAPI Terraform Provider: Best Practices for Success

This repository contains a comprehensive guide for using the AzAPI Terraform provider effectively. The guide is built as a static website using Hugo and is designed to be deployed on GitHub Pages.

## 🌐 Website

Visit the live guide at: https://matt-ffffff.github.io/azapi-best-practices/

## 📚 What's Included

This guide covers:

- **Getting Started**: Installation, authentication, and basic usage
- **Understanding AzAPI**: Architecture and core concepts  
- **Core Concepts**: Resource types, state management, and JSON body construction
- **Resource Management**: Best practices for lifecycle management
- **Security & Compliance**: Security considerations and implementation
- **Performance Optimization**: Techniques for optimal performance
- **Testing Strategies**: Approaches for testing AzAPI configurations
- **Common Patterns**: Reusable patterns and examples
- **Troubleshooting**: Common issues and solutions

## 🚀 Features

- **Book-style Layout**: Clean, navigable structure with table of contents
- **Code Highlighting**: Syntax highlighting for HCL, JSON, and other languages
- **Mermaid Diagrams**: Interactive diagrams for architecture and flows
- **Search Functionality**: Built-in search across all content
- **Mobile Responsive**: Works great on desktop and mobile devices
- **GitHub Pages Compatible**: Automatically deployed via GitHub Actions

## 🛠️ Local Development

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.146.0 or later)
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

## 📖 Content Structure

```
content/
├── _index.md              # Homepage
└── docs/
    ├── _index.md          # Documentation index
    ├── getting-started.md # Getting started guide
    ├── understanding-azapi.md
    ├── core-concepts.md
    ├── resource-management.md
    └── ...                # Additional chapters
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests to improve the guide.

## 📄 License

This project is open source and available under the MIT License.

## 🏗️ Built With

- [Hugo](https://gohugo.io/) - Static site generator
- [Hugo Book Theme](https://github.com/alex-shpak/hugo-book) - Clean book-style theme
- [Mermaid](https://mermaid-js.github.io/) - Diagram and flowchart generation
- [GitHub Actions](https://github.com/features/actions) - CI/CD for automated deployment
- [GitHub Pages](https://pages.github.com/) - Static site hosting