# Nama  : Midun Hakiki
# Nim   : 312210583
# Kelas : IT.22.B1
# Prodi : Teknik Informatika
====================================================



# ChallengeGov

Welcome to the ChallengeGov Data Portal

# Requirements
- PostgreSQL 10 - database
- Elixir 1.9 - server language
- Erlang 21.2 - server language
- node.js 11.10 - front end language
- yarn 1.22.5 - package manager

# Install & Setup
# Requirements

Install PostgreSQL according to your OS of choice, for MacOS Postgres.app is recommended.

Alternatively, use the Challenge_gov docker-compose file to run psql locally in a container.

    docker-compose run --service-ports psql

To install Elixir, Erlang, and NodeJS it is recommended to use the asdf version manager. Install instructions are copied here for MacOS, for other OSs see asdf docs. This also assumes you have HomeBrew installed.

    git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.7.0
    echo -e '\n. $HOME/.asdf/asdf.sh' >> ~/.bash_profile
    echo -e '\n. $HOME/.asdf/completions/asdf.bash' >> ~/.bash_profile

    brew install \
      coreutils automake autoconf openssl \
      libyaml readline libxslt libtool unixodbc

Once asdf is set up, install each language. NodeJS may require setting up keys, and should display help to guide you.

    asdf plugin-add erlang
    asdf install erlang 21.2.5

    asdf plugin-add elixir
    asdf install elixir 1.8.0

    asdf plugin-add nodejs
    bash ~/.asdf/plugins/nodejs/bin/import-release-team-keyring
    asdf install nodejs 11.13.

    asdf plugin-add yarn 
    asdf install yarn 1.22.5

# ChallengeGov Setup

Create the file config/dev.local.exs and set a secret key base, and if needed, include local PostgreSQL connection information. It will look something like (replacing with your local configuration):

    use Mix.Config

    config :challenge_gov, Web.Endpoint,
      secret_key_base: "<OUTPUT OF `mix phx.gen.secret`>"

    config :challenge_gov, ChallengeGov.Repo,
      username: "postgres",
      password: "postgres",
      database: "challenge_gov_dev",
      hostname: "localhost",
      pool_size: 10

Create the file config/test.local.exs and set a secret key base, and if needed, include local PostgreSQL connection information. It will look something like (replacing with your local configuration):

    use Mix.Config

    config :challenge_gov, Web.Endpoint,
        secret_key_base: "<OUTPUT OF `mix phx.gen.secret`>"

    config :challenge_gov, ChallengeGov.Repo,
      username: "postgres",
      password: "postgres",
      database: "challenge_gov_test",
      hostname: "localhost",
      pool_size: 10

Start with cloning the application. Once cloned, in your terminal run the following commands inside the cloned folder.

    mix local.hex --force
    mix local.rebar --force
    mix deps.get
    mix compile

This sets up your basic elixir environment. Next setup the database. The following commands will create and migrate locally, as well as migrate seeds.

Super-Admin (optional) -- If you would like to seed the database with a default super-admin user, please set the following environment variables:

  - FIRST_USER_EMAIL
  - FIRST_USER_FN
  - FIRST_USER_LN

Setup and seed the database

  - mix ecto.create
  - mix ecto.migrate
  - mix run priv/repo/seeds_updated.exs

Once the database is setup, make sure to install javascript dependencies.

    cd assets/
    yarn
    cd ..

Now you can run the server.

    mix phx.server

# Development

Gain access to the portal by going to http://localhost:4000/ and clicking on 'Dev Accounts' to select and sign in under different user roles.

# Testing

The ChallengeGov runs each pull request (and every commit on the master branch) through CI. Make sure to add tests as you extend the application. We also run Credo and the built in formatter in CI to ensure code quality.

# Deployment

Passing CI on master deploys to the dev environment via Cloud Foundry as part of the Drone build.

# Importing Data

Run importers in order of Open, Closed, ClosedImported. Afterward set the challlenges_seq_id to the max challenge-id.

Commands to run:

    $ iex -S mix run
    > Mix.Tasks.OpenChallengeImporter.run("")
    > Mix.Tasks.ClosedChallengeImporter.run("")
    > Mix.Tasks.ClosedImportedChallengeImporter.run("")

In psql after imports:

    SELECT setval('challenges_id_seq', max(id)) FROM challenges;

# Learn more about Phoenix

  - Official website: http://www.phoenixframework.org/
  - Guides: https://hexdocs.pm/phoenix/overview.html
  - Docs: https://hexdocs.pm/phoenix
  - Source: https://github.com/phoenixframework/phoenix

# Thank You Happy Learning...