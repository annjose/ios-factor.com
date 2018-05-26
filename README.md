# ios-factor

Inspired by the famous [twelve-factor app framework](https://www.12factor.net/), a methodology to write web services. This project uses the same structure and similar principals, re-written and applied to iOS app.

This project was started by [@KrauseFx](https://twitter.com/KrauseFx)

## Background

Over the past years, the iOS development process has shifted drastically:

- from supporting a single device, to a wide range of available iOS powered iPhones and iPads and various platforms like tvOS and watchOS
- from directly drawing user interfaces using `drawRect` to AutoLayout
- from iOS apps mostly running locally, to iOS apps heavily relying on backend services
- from 2 weeks iOS app review times to less than a day
- from installing an app on your phone using iTunes to distributing apps through the official TestFlight channel
- from uploading 5 screenshots per language to iTunes Connect to 110 per language, as well as app previews
- from slow releases whenever something is ready, to shipping every single week
- from instant roll outs to A/B testing, slow rollouts and automatic regression detection


### I. Codebase

### II. Dependencies

> Explicitly declare and isolate dependencies

Ideally, an `ios-factor` iOS app never relies on implicit existence of system-wide packages. It declares all dependencies, completely and exactly via a dependency declaration manifest. This includes the exact versions of Xcode, CocoaPods and fastlane. 

The benefit of explicit dependency declaration is that it simplifies setup for developers new to the app, as well as having a reliable build system that is also able to run past builds again in a reproducible fashion. A new developer can check out the app’s codebase onto their development machine, requiring only the language runtime and dependency manager installed as prerequisites. They will be able to set up everything needed to run the app’s code with a deterministic build command.

Since iOS development can't be containerized like it's already the case for web development, we're limited to third party tools trying to fullfill this requirement until Apple helps us out.

For the time being, you can use various third party tooling to explicitly declare those dependencies.

#### Swift-based tooling

TODO

#### Xcode version

You can use an [.xcode-version](https://github.com/fastlane/ci/blob/master/docs/xcode-version.md) file in the root of your iOS project to declare the exact version of Xcode to be used for a given iOS app.

This way, you can configure your CI-system to automatically install and use a given Xcode version. 

To automate the installation of Xcode, you can use the third party tool [xcode-install](https://github.com/krausefx/xcode-install).

#### Ruby-based tooling

Ruby uses [bundler](https://bundler.io) to define the exact dependencies to be used for a build in a so-called `Gemfile`:

```ruby
source "https://rubygems.org"

gem "fastlane", ">= 2.96.1", "<= 3.0.0"
gem "cocoapods", "=1.5"
```

To `Gemfile` and the automatically generated `Gemfile.lock` must be checked into version control. Any build system can then run `bundle install` to install all Ruby-based build dependencies.

### III. Config

> Inject configuration during build time

An app’s config is everything that is likely to vary between deploys (App Store, TestFlight, local development). This includes:

- API keys for backend services (internal as well as external services)
- URLs for remote resources (the APIs your app uses)
- Feature toggles

Apps sometimes store config as constants in the code. This is a violation of ios-factor, which requires strict separation of config from code. Config varies substantially across deploys, code does not.

A litmus test for whether an app has all config correctly factored out of the code is whether the codebase could be made open source at any moment, without compromising any credentials.

There are many ways on how you can inject config values during build time

- [cocoapods-keys](https://github.com/orta/cocoapods-keys) to securely apply secret keys to your iOS app during build time
- Custom built solution using a build phase
- TODO: Are there any alternatives?

### IV. Backing services

I'll have to think about how we can apply this to mobile.

### V. Build, release, run

> Strictly separate build and run stages

A codebase is transformed into a (non-development) deploy through three stages:

TODO

### VI. Processes

### VII. Port binding
### VIII. Concurrency
### IV: Disposability
### X: Dev/prod parity

> Keep development, staging and production as similar as possible

Historically, there have been substantial gaps between development (a developer making live edits to the app on their local machine) and production (an app deployed on the App Store accessed by end users). These gaps manifest in three areas:

- The time gap: A developer may work on code that takes days, weeks, or even months to go into production
- The personnel gap: Developers write code, release engineers or just a single engineer deploy new versions
- The tools gap: Developers may be using a staging server that runs a different version than the production one. Developers might be using a different Xcode release than the one that was used for deployment

The ios-factor app is designed for [continuous deployment](https://avc.com/2011/02/continuous-deployment/) by keeping the gap between development and production small. Looking at the three gaps described above:

- Make the time gap small: a developer may write code and have it deployed just a few days later
- Make the personnel gap small: developers who wrote code are closely involved in deploying it and watching its behavior in production. This is best possible by completely automating the release process of your iOS app and put the know-how on how to do it in code declarations instead of documentation.
- Make the tools gap small: keep development and production as similar as possible. Follow the principals of the `Dependencies` section of ios-factor and make use of an `.xcode-version` file as well as define all other dependencies.


## Open TODOs

- versioned APIs backwards compatible
- remote config, feature toggles
- tooling like react native: how do you sync the native code with the JS code
- OTA updates for config should be possible

## Disclaimer

This project is licensed under the terms of the MIT license. See the [LICENSE](LICENSE) file.

This project are in no way affiliated with Apple Inc or Google Inc. 