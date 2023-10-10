# 📦 🚀 Semantic release
This is a minimal Node.js Express application with a single controller endpoint. The main purpose of this project is to demonstrate how easily semantic release can be set up using the semantic-release package and a GitHub Actions pipeline.

## 🛫 Getting Started
Follow these instructions to set up the project and configure semantic release.

### Prerequisites

- Node.js and npm installed on your machine.
- A GitHub repository for your project.


1. Clone the repository to your local machine:

   ```shell
   git clone https://github.com/lukasnerdware/semantic-release.git
   cd semantic-release
   ````
2. Install project dependencies:
    ```shell 
    npm install
    ```
    ### Usage
    ```shell 
    npm start
    ```
    Your application will be available at http://localhost:3000.

## 👨🏼‍⚖️ Conventional Commits
Enforcing Conventional Commits with Husky and Commitlint
1. Install Husky to enforce conventional commit messages and commitlint:
    ```shell
    npm install --save-dev @commitlint/{config-conventional,cli}
    npm install --save-dev husky
    npx husky install
    npx husky add .husky/commit-msg  'npx --no -- commitlint --edit ${1}'
    ``````
    This will ensure that commit messages follow the conventional commit guideline.

2. Create a commitlint.config.js file in your project root:
```javascript
module.exports = { extends: ['@commitlint/config-conventional'] };
``````

## 📓 Semantic Release Setup
1. Install semantic-release and required plugins:
````shell
npm install --save-dev semantic-release @semantic-release/changelog @semantic-release/git @semantic-release/github

````
2. Create a release.config.js file in your project root:
```javascript
module.exports = {
  branches: ['main'],
  plugins: [
    '@semantic-release/commit-analyzer',
    '@semantic-release/release-notes-generator',
    '@semantic-release/changelog',
    '@semantic-release/npm',
    '@semantic-release/github',
    '@semantic-release/git',
  ],
};
``````


3. Configure GitHub Actions for semantic release by creating a .github/workflows/release.yml file:
```yaml
name: Semantic Release

on:
  push:
    branches:
      - main

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Semantic Release
        run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}

``````

4. Commit the changes and push to your GitHub repository.
5. Ensure you give the Read and write permissions permision to your github workflow (Settings --> Actions --> General --> Workflow permissions)
1. Add NPM_TOKEN as Github Secret
   1. GO to Settings
   2. Security --> Secrets and variables --> New Repository secret
   3. Create NPM_TOKEN secret (requires npm account)
2. Now, whenever you merge changes into the main branch, the GitHub Actions pipeline will automatically trigger a semantic release based on your commit messages and versioning rules defined in release.config.js.

