<template>
    <div class="container">
        <div class="col-md-6 offset-md-3">
            <div class="card mt-5">
                <div class="card-header">
                    <p class="mb-0">Войти</p>
                </div>
                <div class="card-body">
                    <form @submit.prevent="login">
                        <div>
                            <input type="hidden" :class="{ 'is-invalid': errors.email }">
                            <div class="invalid-feedback" v-if="errors.email">
                                {{ errors.email[0] }}
                                <hr>
                            </div>
                        </div>
                        <div class="form-group">
                            <label>Почта</label>
                            <input v-model="form.email" type="email" class="form-control" placeholder="Почта">
                        </div>
                        <div class="form-group">
                            <label>Пароль</label>
                            <input v-model="form.password" type="password" class="form-control" placeholder="Пароль">
                        </div>
                        <div class="form-group">
                            <input type="submit" class="btn btn-primary w-100" value="Войти">
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    middleware: 'guest',
    data() {
        return {
            form: {
                email: '',
                password: ''
            }
        }
    },
    methods: {
        async login() {
            try {
                await this.$auth.login({ data: this.form });

                this.$router.push(this.$route.query.redirect ? this.$route.query.redirect : '/');
            } catch (e) {}
        }
    }
};
</script>