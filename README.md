# Generic RubyGem Release Script

A simple, reliable, **one‑command release tool** for your Ruby gems.  
It automates all the tedious bits — version detection, tag creation, gem build, and RubyGems publishing — so you can
release with confidence (and a little style).

* [Generic RubyGem Release Script](#generic-rubygem-release-script)
    * [What It Does](#what-it-does)
    * [Setup](#setup)
    * [Requirements](#requirements)
    * [Example](#example)
    * [Safety Features](#safety-features)
    * [Bonus: For the Curious](#bonus-for-the-curious)
    * [License](#license)

---

## What It Does

When invoked in your gem's root directory, this script:

1. Ensures the working directory is clean (no uncommitted changes)
2. Verifies you're on the `main` or `master` branch
3. Detects your gem name and version automatically from the `.gemspec`
4. Checks that the corresponding Git tag doesn't already exist
5. Asks for confirmation before continuing
6. Builds the gem
7. Creates and pushes a version tag (`vX.Y.Z`)
8. Pushes the built gem to [RubyGems.org](https://rubygems.org)
9. Prints next steps with a prefilled GitHub release URL

---

## Setup

You can host this script in its own repository (for example `my-company/release-script`) and call it remotely from your
projects.

1. **Add a minimal `bin/release`** to any gem project:

   ```shell
   #!/usr/bin/env bash
   set -euo pipefail

   curl -sSL https://raw.githubusercontent.com/<your-username>/release-script/master/release | bash
   ```

2. Make sure it's executable:

    ```shell
    chmod +x bin/release
    ```

3. Commit this tiny launcher (`bin/release`) to each repo you want to release from.

## Requirements

The script assumes you already have:

- Git installed and configured (with access to push tags)
- Ruby and RubyGems installed
- Your gemspec defined correctly with name and version
- A valid RubyGems API key configured locally, e.g.:
    ```shell
    gem signin
    ```

## Example

In your gem project (e.g., my_gem):

```shell
git checkout main
bin/release
```

Typical output:

```text
Starting release process...
Preparing to release version 1.2.3 for my_gem
Building gem...
Creating and pushing git tag...
Publishing to RubyGems...
✅ Release v1.2.3 completed successfully!
```

Afterward, it'll remind you to create a GitHub release and update your changelog.

## Safety Features

The script won't run if:

- You have uncommitted changes;
- You're not on main/master;
- The git tag for the detected version already exists;
- It always asks for confirmation before any irreversible actions.

No surprises — only clean, consistent releases.

## Bonus: For the Curious

The detection logic uses Ruby itself to read the .gemspec:

```ruby
spec = Gem::Specification.load('my-gem.gemspec')
spec.name # => "my-gem"
spec.version # => "1.2.3"
```

That keeps your release metadata single‑sourced and accurate.

## License

Released under the MIT License.
