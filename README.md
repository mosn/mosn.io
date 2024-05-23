# mosn.io

Source for mosn.io site <https://mosn.io>.

Powered by [hugo](https://gohugo.io) with [docsy theme](https://github.com/google/docsy).

# Install Hugo

You need a [recent extended version](https://github.com/gohugoio/hugo/releases) (we recommend version 0.53 or later) of [Hugo](https://gohugo.io/) to do local builds and previews of sites (like this one) that use Docsy. If you install from the release page, make sure to get the `extended` Hugo version, which supports [SCSS](https://sass-lang.com/documentation/file.SCSS_FOR_SASS_USERS.html); you may need to scroll down the list of releases to see it.

For comprehensive Hugo documentation, see [gohugo.io](https://gohugo.io/).

# Environment setup
You can follow the manual steps given below.

1. Ensure pre-requisites are installed 
2. Clone this repository

```
git clone git@github.com:mosn/mosn.io.git
```

3. Install `PostCSS` required by Docsy by running the following commands from the root directory of your project:

```
npm install --save-dev autoprefixer
npm install -D postcss
npm install --save-dev postcss-cli
```

# Run server locally
1. Clear up your local module cache

```
hugo mod clean
```

2. Start the server

```
hugo server --disableFastRender
```

3. Navigate to `http://localhost:1313`

# Update docs
1. Create new branch 
2. Commit and push changes to content 
3. Submit pull request to master branch 
4. Staging site will automatically get created and linked to PR to review and test

# Notice

This website is built under hugo version v0.55.5-A83256B9C/extended. There may be unknown errors when compiling on other versions.