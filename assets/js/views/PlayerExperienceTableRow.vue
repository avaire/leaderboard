<template>
    <tr>
        <td class="rank">#{{ user.rank }}</td>
        <td class="avatar">
            <figure class="image is-48x48">
                <img class="avatar is-rounded" :src="buildAvatarUrl(user)" :alt="user.username" @error="loadDefaultAvatar">
            </figure>
        </td>
        <td class="username">
            <span>
                <strong>{{ user.username }}</strong>
                <span class="discriminator">#{{ user.discriminator }}</span>
            </span>
        </td>
        <td class="xp-bar has-text-centered">
            <div class="xp"><strong>{{ xpInCurrentLevel }}</strong> out of <strong>{{ totalXPForLevel }}</strong> XP (Total: <strong>{{ formatNumber(user.experience - 100) }}</strong>)</div>
            <div class="bar">
                <progress class="progress is-primary" :value="percentage" max="100">{{ percentage }}%</progress>
            </div>
        </td>
        <td class="level has-text-centered">
            <span class="level">Level<br>{{ level }}</span>
        </td>
    </tr>
</template>

<style type="text/css" scoped>
    td {
        padding: 0;
    }
    td.rank {
        font-size: 1.5rem;
        width: 5%;
    }
    td.avatar {
        width: 5%;
    }
    td.avatar figure {
        margin-left: 0px;
        margin-right: 0px;
    }
    td.username {
        font-size: 1.15rem;
        width: 25%;
    }
    td.username > span {
        vertical-align: -webkit-baseline-middle;
    }
    td.username span.discriminator {
        font-size: 0.95rem;
    }
    td.xp-bar {
        width: 55%;
    }
    td.level {
        vertical-align: -webkit-baseline-middle;
        width: 10%;
    }

    @media only screen and (max-width: 820px) {
        td.xp-bar {
            display: none;
        }
        td.username {
            width: 80%;
        }
    }
</style>

<script>
    import {
        getLevelFromExperience,
        getExperienceFromLevel,
        getPercentageToNextLevel,
        getExperienceInCurrentLevel,
        getExperienceBetweenCurrentAndNextLevel,
    } from '../level/LevelManager';

    export default {
        props: {
            user: Object,
            serverModifier: Number,
        },
        methods: {
            loadDefaultAvatar(event) {
                event.target.src = '/img/default-profile.png';
            },
            buildAvatarUrl(user) {
                return `https://cdn.discordapp.com/avatars/${user.id}/${user.avatar}.png`;
            },
        },
        computed: {
            modifier() {
                return this.serverModifier < 0 ? 0.3715 : this.serverModifier;
            },
            level() {
                return this.formatNumber(getLevelFromExperience(this.user.experience, this.modifier));
            },
            totalXPForLevel() {
                return this.formatNumber(getExperienceBetweenCurrentAndNextLevel(this.level, this.modifier));
            },
            xpInCurrentLevel() {
                return this.formatNumber(getExperienceInCurrentLevel(this.level, this.user.experience, this.modifier));
            },
            percentage() {
                return getPercentageToNextLevel(this.level, this.user.experience, this.modifier);
            },
        },
    }
</script>
