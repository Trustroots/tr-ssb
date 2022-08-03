# tr-ssb

Experiments with SSB

## Getting started

- Install nodejs and yarn
- Clone this repo
- `yarn` to install dependencies
- `yarn start` to run the server
- Open a new terminal
  - `yarn whoami` to see your new identity
  - Run commands like `yarn ssb-server COMMANDS -- --path ./data`
    - Sorry this is a headache, please open a PR to make this easier
  - Write your profile like so:
    - `yarn ssb-server publish --type about --about "@PROFILE_ID_FROM_WHOAMI" --name "PROFILE_NAME" --description "PROFILE_DESCRIPTION" -- --path ./data`
  - Accept an invite like so:
    - `yarn ssb-server invite.accept "INVITE_CODE" -- --path ./data`
- Live long and prosper
