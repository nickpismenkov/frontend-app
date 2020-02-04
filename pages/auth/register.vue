<template>
    <div class="container">
        <div class="col-md-6 offset-md-3">
            <div class="card mt-5">
                <div class="card-header">
                    <p class="mb-0">Регистрация</p>
                </div>
                <div class="card-body">
                    <form @submit.prevent="register">
                        <div class="form-group w-100">
                            <b-form-group label="Тип аккаунта">
                                <b-form-radio-group
                                    id="btn-radios-1"
                                    v-model="form.user_type"
                                    :options="options"
                                    buttons
                                    button-variant="outline-primary"
                                    name="radio-btn-outline"
                                ></b-form-radio-group>
                            </b-form-group>
                        </div>
                        <div class="form-group">
                            <label>Имя</label>
                            <input v-model="form.name" type="text" class="form-control" :class="{ 'is-invalid': errors.name }" placeholder="Имя">
                            <div class="invalid-feedback" v-if="errors.name">
                                {{ errors.name[0] }}
                            </div>
                        </div>
                        <div class="form-group">
                            <label>Почта</label>
                            <input v-model="form.email" type="email" class="form-control" :class="{ 'is-invalid': errors.email }" placeholder="Почта">
                            <div class="invalid-feedback" v-if="errors.email">
                                {{ errors.email[0] }}
                            </div>
                        </div>
                        <div class="form-group">
                            <label>Пароль</label>
                            <input v-model="form.password" type="password" class="form-control" :class="{ 'is-invalid': errors.password }" placeholder="Пароль">
                            <div class="invalid-feedback" v-if="errors.password">
                                {{ errors.password[0] }}
                            </div>
                        </div>
                        <div class="form-group">
                            <input type="submit" class="btn btn-success w-100" value="Зарегистрироваться">
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
            options: [
                { text: 'Компания', value: 'company' },
                { text: 'Физическое лицо', value: 'individual' }
            ],
            form: {
                name: '',
                email: '',
                password: '',
                user_type: 'company'
            }
        }
    },
    methods: {
        async register() {
            try {
                await this.$axios.post('/auth/register', this.form);

                this.$auth.login({data: this.form});

                this.$router.push({name: 'index'});
            } catch (e) {}
        }
    }
};
</script>