<template>
    <div>
        <b-navbar toggleable="lg" type="light" variant="light">
            <div class="container">
                <b-navbar-brand to="/">Доска Объявлений</b-navbar-brand>

                    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

                    <b-collapse id="nav-collapse" is-nav>

                    <b-navbar-nav class="ml-auto">
                        <template v-if="authenticated">
                            <b-nav-item-dropdown>
                                <template slot="button-content">
                                    {{ user.name }}
                                </template>
                                <b-dropdown-item to="/profile">Добавить</b-dropdown-item>
                                <b-dropdown-item to="/profile/ads">Мои объявления</b-dropdown-item>
                                <b-dropdown-item @click.prevent="signOut">Выйти</b-dropdown-item>
                            </b-nav-item-dropdown>
                        </template>
                        <template v-else>
                            <nuxt-link class="nav-link" to="/auth/login">Войти</nuxt-link>
                            <nuxt-link class="nav-link" to="/auth/register">Зарегистрироваться</nuxt-link>
                        </template>
                    </b-navbar-nav>
                </b-collapse>
            </div>
        </b-navbar>
    </div>
</template>

<script>
export default {
    methods: {
        signOut() {
            this.$auth.logout();
        }
    }
};
</script>