<template>
    <div>
        <section class="section content">
            <div class="container has-text-centered" v-if="isLoading">
                <h1 class="title is-3">Loading...</h1>

                <p class="button is-loading transparent"></p>
            </div>

            <div class="container has-text-centered" v-else-if="isError">
                <h4 class="title is-4">Found no matching server with an ID of {{ this.$route.params.id }}</h4>
            </div>

            <div class="container has-text-centered" v-else-if="!enabled">
                <h1 class="title is-1">{{ name }}</h1>
                <h4 class="title is-4">This server does not have the leveling feature enabled :(</h4>
            </div>

            <div class="container has-text-centered" v-else>

                <div class="columns">
                    <div class="column">
                        <h1 class="title is-1">{{ name }}</h1>
                        <p>Showing the top 100 players on the <strong>{{ name }}</strong> leaderboard.</p>
                    </div>
                </div>

                <div class="columns">
                    <div class="column">
                        <table class="table is-striped is-fullwidth is-hoverable">
                            <tbody>
                                <player-experience-table-row
                                    v-for="user of leaderboardUsers"
                                    :key="user.id"
                                    :serverModifier="modifier"
                                    :user="user"
                                />
                            </tbody>
                        </table>
                    </div>
                </div>

            </div>
        </section>

        <footer class="footer">
            <div class="content has-text-centered">
                <p>
                    Created by <a href="https://senither.com/">Alexis Tan</a>, powered by <a href="https://bulma.io/">Bulma</a> and <a href="https://vuejs.org/">VueJS</a>.
                    <br>Get the <a href="https://github.com/avaire/leaderboard">source code</a> on <a href="https://github.com/avaire/leaderboard">GitHub</a>.
                </p>
            </div>
        </footer>
    </div>
</template>

<script>
    import PlayerExperienceTableRow from './PlayerExperienceTableRow';

    export default {
        mounted() {
            axios(`https://api.avairebot.com/v1/leaderboard/284083636368834561`).then(response => {
                if (response.status == 200) {
                    this.name = response.data.name;
                    this.enabled = response.data.enabled;
                    this.modifier = response.data.modifier;
                    this.leaderboard = response.data.leaderboard;
                }

                this.isLoading = false;
            }).catch(error => this.$router.push({ name: 'home' }));
        },
        data() {
            return {
                name: null,
                enabled: false,
                modifier: -1,
                leaderboard: [],
                isLoading: true,
                isError: false,
            };
        },
        computed: {
            leaderboardUsers() {
                let index = 1;
                let leaderboardUsers = [];
                for (let user of this.leaderboard) {
                    user.rank = index++;
                    leaderboardUsers.push(user);
                }
                return leaderboardUsers;
            }
        },
        components: {
            PlayerExperienceTableRow
        }
    }
</script>
