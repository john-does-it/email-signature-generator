
# Email Signature Generator

A web application built with SvelteKit that helps users create professional email signatures with ease.

## Features

- Multi-language support (English, French, Dutch)
- Customizable signature templates
- Real-time preview
- Social media integration
- Responsive design
- Easy copy-paste functionality

## Tech Stack

- SvelteKit 2.16.0
- TypeScript 5.0.0
- Paraglide.js 2.0.13 for i18n
- Vite 6.2.5
- Testing Suite (Vitest 3.0.0 + Playwright 1.49.1)
- Deployment: Vercel adapter (Node.js adapter also available)

## Getting Started

```bash
# Clone the repository
git clone https://github.com/john-does-it/email-signature-generator.git

# Navigate to project directory
cd email-signature-generator/my-app

# Install dependencies
npm install

# Start development server
npm run dev

# Open in browser (optional)
npm run dev -- --open
```

## Deployment

The project uses `@sveltejs/adapter-vercel` by default for Vercel deployment. If you want to deploy elsewhere, you can switch to another adapter:

```bash
# For Node.js environments (already included in devDependencies)
npm i -D @sveltejs/adapter-node

# Update svelte.config.js to use the new adapter
```

## License & Attribution

This project is open source and available under the MIT License.

### Important Note on Usage

If you use this project as a base for your work, please include the following attribution:

```markdown
This project is [fork/inspired by] the Email Signature Generator:
- Website: https://https://johndoesit-email-signature.vercel.app
- GitHub: https://github.com/john-does-it/email-signature-generator
```

## Contributing

Feel free to submit a Pull Request

## Contact

For questions or support, please [open an issue](https://github.com/john-does-it/email-signature-generator/issues) on GitHub.
