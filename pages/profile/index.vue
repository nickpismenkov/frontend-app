<template>
    <div class="container">
        <div class="mt-5">
            <b-alert variant="success" show>{{ name }} <b-badge>{{ type }}</b-badge></b-alert>
        </div>
        <hr>
        <div class="row mb-5">
            <div class="col-md-8 offset-md-2">
                <div class="card">
                    <div class="card-header">
                        <p class="mb-0">Добавить объявление</p>
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="create">
                            <div class="form-group">
                                <label>Категория</label>
                                <b-form-select v-model="form.category" :options="options" :class="{ 'is-invalid': errors.category }"></b-form-select>
                                <div class="invalid-feedback" v-if="errors.category">
                                    {{ errors.category[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label>Страна</label>
                                <b-form-select v-model="form.country" :options="countries" :class="{ 'is-invalid': errors.country }"></b-form-select>
                                <div class="invalid-feedback" v-if="errors.country">
                                    {{ errors.country[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <template v-if="form.country == 'russia'">
                                    <label>Город</label>
                                    <b-form-select v-model="form.city" :options="russia" :class="{ 'is-invalid': errors.city }"></b-form-select>
                                </template>
                                <template v-if="form.country == 'china'">
                                    <label>Город</label>
                                    <b-form-select v-model="form.city" :options="china" :class="{ 'is-invalid': errors.city }"></b-form-select>
                                </template>
                                <template v-if="form.country == 'usa'">
                                    <label>Город</label>
                                    <b-form-select v-model="form.city" :options="usa" :class="{ 'is-invalid': errors.city }"></b-form-select>
                                </template>
                                <div class="invalid-feedback" v-if="errors.city">
                                    {{ errors.city[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label>Заголовок объявления</label>
                                <input v-model="form.title" type="text" class="form-control" :class="{ 'is-invalid': errors.title }" placeholder="Заголовок объявления">
                                <div class="invalid-feedback" v-if="errors.title">
                                    {{ errors.title[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label>Описание объявления</label>
                                <b-form-textarea
                                v-model="form.description"
                                placeholder="Описание объявления"
                                rows="3"
                                max-rows="6"
                                :class="{ 'is-invalid': errors.description }"
                                ></b-form-textarea>
                                <div class="invalid-feedback" v-if="errors.description">
                                    {{ errors.description[0] }}
                                </div>
                            </div>
                            <br>
                            <div class="form-group">
                                <input type="submit" class="btn btn-primary w-100" value="Создать объявление">
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    middleware: 'auth',
    async mounted() {
        this.data = await this.$axios.$get('/me');
        this.name = this.user.name;
        this.type = this.data.type;
        
        if(this.data.ads.length > 0) { 
            this.ads = this.data.ads;
        } else {
            this.ads = 0;
        }
    },
    data() {
        return {
            data: [],
            name: '',
            type: '',
            ads: [],
            form: {
                category: null,
                country: null,
                city: null,
                title: '',
                description: ''
            },
            options: [
                { value: null, text: 'Выберите категорию' },
                { value: 'auto', text: 'Авто' },
                { value: 'property', text: 'Недвижимость' },
                { value: 'other', text: 'Другое' }
            ],
            countries: [
                { value: null, text: 'Выберите страну' },
                { value: 'russia', text: 'Россия' },
                { value: 'china', text: 'Китай' },
                { value: 'usa', text: 'США', },
            ],
            russia: [
                { value: null, text: 'Выберите город' },
                { value: 'moscow', text: 'Москва' },
                { value: 'sochi', text: 'Сочи' }
            ],
            china: [
                { value: null, text: 'Выберите город' },
                { value: 'hong kong', text: 'Гонконг' },
                { value: 'beijing', text: 'Пекин' }
            ],
            usa: [
                { value: null, text: 'Выберите город' },
                { value: 'boston', text: 'Бостон' },
                { value: 'chicago', text: 'Чикаго' }
            ]
        }
    },
    methods: {
        async create() {
            try {
                await this.$axios.post('/ad/create', this.form);

                this.$router.push({'path': 'profile/ads'});
            } catch (e) {}
        }
    }
};
</script>