AvaIre Leaderboard
==================

AvaIre Leaderboard is a small stateless web app that can be used by people who self-host [AvaIre](https://github.com/avaire/avaire) to quickly and easily set up a web leaderboard for their server, the leaderboard will show the top 100 people for the server defined in the `config.js` file, along with their progress, current XP, and total XP.

## Getting Started

AvaIre Leaderboard is built using [VueJS](https://vuejs.org/), [SASS](https://sass-lang.com/), and [Laravel Mix](https://laravel-mix.com/), to host the leaderboard, make sure that both [NodeJS](https://nodejs.org/en/) and [Yarn](https://yarnpkg.com/en/) is installed in order to transpile the assets so the application can run.

Start by cloning the repository to some location where the `public` directory will be visible to people accessing a domain or IP address.

    git clone https://github.com/avaire/leaderboard.git

Next, install all the required dependencies using [Yarn](https://yarnpkg.com/en/).

    yarn

While that is going on, you can edit the [`assets/js/config.js`](https://github.com/avaire/leaderboard/blob/master/assets/js/config.js) file with your bots IP/Hostname, API port, server ID the leaderboard should be used for, and the update rate, when you're satisfied with your settings you can save the config file, finally we'll generate all the assets.

    yarn prod

And you're done!

If you want to edit the CSS or JS for the site, you can run Yarn in watch mode by running `yarn watch` which will build all the assets whenever one of the asset files are saved.

## License

AvaIre Leaderboard is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
