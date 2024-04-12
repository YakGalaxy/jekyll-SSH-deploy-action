# Jekyll SSH Deploy Action

A [GitHub Action](https://github.com/features/actions) for building and deploying your Jekyll site via SSH.

[![Actions Status](https://github.com/appleboy/scp-action/workflows/scp%20files/badge.svg)](https://github.com/appleboy/scp-action/actions)

## Usage

1. Create a GitHub workflow file (e.g. `.github/workflows/SSH-deploy-action.yml`) as per [here.](https://github.com/YakGalaxy/jekyll-SSH-deploy-action/blob/main/SSH-deploy-action.yml
2. Ensure your source and target values are correct within your new `SSH-deploy-action.yml` file. 
3. Create your `HOST` , `USERNAME` and `PASSWORD` secrets within your repository on Github.com `(your-repository/settings/secrets/actions)`. 
4. Push your `SSH-deploy-action.yml` file to your repository's `main` branch. 

```yaml
name: Build and Deploy Jekyll via SSH
description: 💾 A Github Action to build and then deploy your Jekyll site via SSH  
  
on:
  workflow_dispatch: # Allows workflow to be run manually
  push:
    branches:
      - main  # Or your default branch

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
      
    - name: 💎 Setup Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: '3.2.3'  # Adjust as needed for your project

    - name: 🏗️ Build Jekyll site 
      run: |
        bundle install
        bundle exec jekyll build

    - name: 🚀 Deploy via SSH
      uses: appleboy/scp-action@master
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        password: ${{ secrets.PASSWORD }}
        port: ${{ secrets.PORT }}
        source: "./_site/*" # Adjust if required
        target: "/public_html" # Set to your deployment directory (for example /public_html)
        strip_components: 1 # This ensures that a subdirectory is not created
```

## Credits

- [Jekyll](https://github.com/jekyll/jekyll) - A blog-aware static site generator in Ruby.
- [actions/checkout](https://github.com/actions/checkout) - Action for checking out a repo.
- [SCP for GitHub Actions](https://github.com/appleboy/scp-action) - Action for copying files and artefacts via SSH.

## Contributing

Issues and Pull Requests are greatly appreciated. 

You can start by [opening an issue](https://github.com/jeffreytse/jekyll-deploy-action/issues/new) describing the problem that you're looking to resolve and we'll go from there.

## License

This software is licensed under the [MIT license](https://opensource.org/licenses/mit-license.php).
