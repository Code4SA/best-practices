# OpenUp Development Best Practices

We build a range of sites, microsites, tools and visualisations for ourselves and our clients. We recommend sticking to these technical best practices to make it easier for us to deliver quality, and build, deploy and maintain our projects. We do our best to follow industry best practices to make it easier for other groups to build on our projects.

These aren’t rules. If you the situation calls for something different, talk to someone and make the most informed decision you can.

If you have a better practice, speak up and motivate for it.

# Guidelines

- Always do your best to ensure a good user experience
- Repeatability and consistency are important for reuse
- Prefer reuse over building from scratch
- Make data-driven decisions

## Development environment

- It should be easy to set up a development environment
  - e.g. docker-compose up, perhaps with a couple more setup commands
  - A frontend developer should be able to set up an app and make a simple change within an hour or so based on instructions and a few commands in the README.
- It should be easy to reset a development environment
- It should be easy to get a full site up and running with demo/dev data
  - e.g. using fixtures that could be loaded with one command
  - Using production data for development is not just putting personal data at risk, but also a code smell suggesting we are not able to easily manipulate data for dev/test. Once you write integration tests, fixtures for dev is easy. Once you have dev fixtures, continuous deployment to deploy previews is suddenly in the realm of possibility.
- The dev environment should be as close to production as possible.
  - Document how to develop on subsystems with external dependencies like mail or search. Mock or dockerise services if at all possible.
- It should be quick and easy to observe changes made in the code
  - e.g. changes reflected on page reload, without rebuilding containers or bundles.
- It should be easy to run tests in a development environment.
- Code formatting isn't a discusstion. Use a linter and language best practises and don't argue about it.
  - Enforce this in CI, e.g. if the linter introduces diffs, the CI run fails.
  - Python
    - isort
    - black

# Best Practices

## Languages and Frameworks

- Our preferred language is Python. It’s mature, has rich libraries and frameworks, is widely used in the Open Data community, is performant and easy to learn.
- Use [OpenUp's Jekyll / GitHub pages template](https://github.com/code4sa/static-template) for static sites
- Use [OpenUp's Django template](https://github.com/code4sa/django-template) for sites that need server-side functionality
- Use pip to manage dependencies.
- Use virtualenv to sandbox your projects.
- Use PostgreSQL if you need a big database (ie. not Mysql), sqlite if you need a small one.
- Use PostGIS for geo data.
- Use Bootstrap.
- Use jQuery.

### Recommended Frameworks

- Use leaflet.js for maps.
- Use D3 for datavis.

## Source code

- Store code on Github under the OpenUp account - https://github.com/OpenUp/
- Always include a README.md and a LICENSE. MIT and Apache are recommended, see http://choosealicense.com/
- Deploy from the master branch
- Branch early, commit often
- Delete fully-merged branches from GitHub.
- Remember: our community judges us on the quality of our code!
- Follow [PEP8](https://www.python.org/dev/peps/pep-0008) as a python style guide so as to be consistent with other python libraries and tools.
- Avoid committing commented-out code. Code merged into the master branch should represent the current state and not possible future or past states. If you want to keep hold of another way of implementing something, commit that as a separate branch. That way it doesn't distract from understanding the current implementation, and it's actually easier to compare using standard tooling like `git diff`. If you think we should move to this alternative implementation in the future, make sure your branch is tied to a task in the project management system. See _Avoid TODOs in code_.
- Avoid TODOs in code. It doesn't have any connection to how we schedule work so it is purely clutter. If you think something should get done, add it to the project's project management system where it can be discussed and prioritised or rejected as part of the normal planning process.

## Sensitive Data (Credentials, etc.)

- Never commit sensitive credentials or other information into the repo. If it happens accidentally, remove them from the source and change them.
- Sensitive data should be stored in a Google Sheets document in the project’s Google Drive folder and shared with your team mates, to improve your bus factor.
- Sensitive data should be injected into the application through environment variables or some other mechanism that doesn’t involve checking it into source control.

Sensitive data includes:

- AWS credentials
- Database credentials
- Flask/Django secret keys for session cookies

This data is NOT sensitive:

- Google Analytics ids (it gets sent to the client in any case)
- New Relic license key

## Build, Test, Deploy

**Goal**: simple, consistent, well documented

- Document how to setup a local development environment in your README.md.
- If your code needs to be built, it should be possible with one command.
- Document how to deploy into production in your README.md. If it’s really hard/long/complex to do this, then you’re probably doing deployment the wrong way.
- Running tests should be one command (eg. nosetests)
- Production deployment should be one command (eg. git push, fab deploy). This will be critical when the server dies (and it will!)
- Version your dependencies. Use pip freeze to save dependencies into a single requirements.txt file.
- Don’t worry about separate production and development requirements files, just use one.
- Keep the master branch deployable and deployed. That means we only merge to master when we're ready to deploy (as in that day) and someone can assume that the master branch is safe to deploy because it has already or you're busy doing so.
- http://12factor.net/ is highly recommended reading
- Make it easy to get a working dev setup, e.g.
  - docker-compose for dev DB, search index
  - fixtures for dev/demo data
- automatically check HTML correctness, e.g. for [unmatched tags](https://github.com/OpenUpSA/biz-portal/commit/a57a551ec933cf674d4e4c5a7300299a69bc822e)

## Platforms

Prefer platforms that encourage simple, consistent deployments and make collaboration easy.

- Apps that don't require server logic should be hosted using [GitHub Pages](https://pages.github.com/).
- When deploying on our own infrastructure, prefer [Dokku](https://github.com/progrium/dokku)
  - Dokku provides "seamless deploys" - when pushing an update, it starts up the new app and can run some checks on it before updating nginx to point to the new instance, and only then stops the old instance.
  - Dokku manages config for us - production secrets are provided to the app as environment variables managed from the dokku command line.

## Databases

- Prefer using OpenUp's central PostgreSQL and Mysql databases. They are backed up automatically regularly.
- Create a new user and database for each application
- Always keep customer data backed up

## Monitoring

- Always use New Relic for monitoring. The Django app template has it built in.
. Uptime monitoring, e.g. http://www.uptimedoctor.com/
- Email alerts of exceptions, e.g. https://sentry.io

## Branding

- Include our logo, linking to www.OpenUp.org.za
- Include a link to https://twitter.com/OpenUpSA - make it easy for users to contact us
- Include a link to the Github repo for the project.
- If the data is available on data.code4sa.org, link to it.

## Development Process

- Perfect is the enemy of done.
- Time-box experiments. Commit to a go/no-go deadline after a day or two of work. At that point, honestly discuss with someone else whether you should continue or learn and discard.
- Try to have at least 2 developers involved on each project, at least with code reviews or design discussions. If you can’t convince one person, then your idea might suck.
- Focus on top down development. Try to get an end-to-end system up and running as soon as possible. It is tempting to dive in and write code to solve a meaty problem but you will end up spending way more time than you should.
- Developer time is expensive. As distasteful as it might seem to a developer, sometimes manual data capture/scraping/etc is cheaper than writing code. Hire an intern.
- Don't even think about not developing for mobile. Don't do it as an afterthought, bake it in from the start.
