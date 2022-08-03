# tr-ssb

Experiments with Secure Scuttlebutt (SSB).

The idea is to have a disjoint network of SSB, that we can use for experiments without sending out anything over the wider SSB network. 

## Getting started

- Install nodejs and yarn
- Clone this repo
- If you're using `nvm` run `nvm install` and `nvm use`
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


