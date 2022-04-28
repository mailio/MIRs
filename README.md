# Mailio Implementation Rationale

## Mission

The goal of the MIRs project two fold: 
1. to document reasons for individual functionalities 
2. to document Mailio communication protocol 

## Build the status page locally

### Install prerequisites

1. Open Terminal.

2. Check whether you have Ruby 2.1.0 or higher installed:

```
ruby --version
```

3. If you don't have Ruby installed, install Ruby 2.1.0 or higher.

4. Install Bundler:

```
gem install bundler
```

5. Install dependencies:

```
bundle install
```

### Build your local Jekyll site

1. Bundle assets and start the server:

```
bundle exec jekyll serve
```

2. Preview your local Jekyll site in your web browser at [http://localhost:4000](http://localhost:4000).

More information on Jekyll and GitHub pages [here](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll).


### About

This documentation has been adopted from [Ethereum EIPs](https://github.com/ethereum/EIPs) 