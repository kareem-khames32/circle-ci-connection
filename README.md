# circle-ci-connectiona
# Use the latest 2.1 version of CircleCI pipeline process engine. See: https://circleci.com/docs/2.0/configuration-reference
version: 2.1
# Use a package of configuration called an orb.
orbs:
  # Declare a dependency on the welcome-orb
  welcome: circleci/welcome-orb@0.4.1
# Orchestrate or schedule a set of jobs
jobs:
  print_hello:
    docker:
      - image: circleci/node:13.8.0
    steps:
      - run: echo hello
  print_world:
    docker:
      - image: circleci/node:13.8.0
    steps:
      - run: echo world
  build:
    docker:
      - image: cimg/base:2020.01
    steps:
      - checkout
      - run:
          name: "echo an env var that is part of our project"
          command: |
            echo $MY_ENV_VAR
workflows:
  test-env-vars:
    jobs:
      - build
  # Name the workflow "welcome"
  welcome:
    # Run the welcome/run job in its own container
    jobs:
      - welcome/run
      - print_hello
      - print_world:
          requires:
            - print_hello
