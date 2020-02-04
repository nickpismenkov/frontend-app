<template>
    <div class="container">
        <div class="mt-5">
            <b-alert variant="success" show>{{ name }} <b-badge>{{ type }}</b-badge></b-alert>
        </div>
        <hr>
        <div class="mb-5">
            <div class="row mb-5">
                <div class="col-md-8 offset-md-2">
                    <div class="card">
                        <div class="card-header">
                            <p class="mb-0">Изменить объявление</p>
                        </div>
                        <div class="card-body">
                            <form @submit.prevent="update">
                                <input type="hidden" :class="{ 'is-invalid': errors.changes }">
                                <div class="invalid-feedback" v-if="errors.changes">
                                    <b>{{ errors.changes[0] }}</b>
                                    <hr>
                                </div>
                                <div class="form-group">
                                    <label>Новая категория</label>
                                    <b-form-select v-model="form.category" :options="options" :class="{ 'is-invalid': errors.category }"></b-form-select>
                                    <div class="invalid-feedback" v-if="errors.category">
                                        {{ errors.category[0] }}
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label>Новая страна</label>
                                    <b-form-select v-model="form.country" :options="countries" :class="{ 'is-invalid': errors.country }"></b-form-select>
                                    <div class="invalid-feedback" v-if="errors.country">
                                        {{ errors.country[0] }}
                                    </div>
                                </div>
                                <div class="form-group">
                                    <template v-if="form.country == 'russia'">
                                        <label>Новый город</label>
                                        <b-form-select v-model="form.city" :options="russia" :class="{ 'is-invalid': errors.city }"></b-form-select>
                                    </template>
                                    <template v-if="form.country == 'china'">
                                        <label>Новый город</label>
                                        <b-form-select v-model="form.city" :options="china" :class="{ 'is-invalid': errors.city }"></b-form-select>
                                    </template>
                                    <template v-if="form.country == 'usa'">
                                        <label>Новый город</label>
                                        <b-form-select v-model="form.city" :options="usa" :class="{ 'is-invalid': errors.city }"></b-form-select>
                                    </template>
                                    <div class="invalid-feedback" v-if="errors.city">
                                        {{ errors.city[0] }}
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label>Новый заголовок объявления</label>
                                    <input v-model="form.title" type="text" class="form-control" :class="{ 'is-invalid': errors.title }" placeholder="Новый заголовок объявления">
                                    <div class="invalid-feedback" v-if="errors.title">
                                        {{ errors.title[0] }}
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label>Новое описание объявления</label>
                                    <b-form-textarea
                                    v-model="form.description"
                                    placeholder="Новое описание объявления"
                                    rows="3"
                                    max-rows="6"
                                    :class="{ 'is-invalid': errors.description }"
                                    ></b-form-textarea>
                                    <div class="invalid-feedback" v-if="errors.description">
                                        {{ errors.description[0] }}
                                    </div>
                                </div>
                                <div class="row">
                                    <div class="col-md-1 mr-4">
                                        <nuxt-link class="btn btn-secondary" to="ads">Отмена</nuxt-link>
                                    </div>
                                    <div class="col-md-2 ml-1">
                                        <input type="submit" class="btn btn-warning" value="Изменить объявление">
                                    </div>
                                </div>
                            </form>
                        </div>
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
        this.id = this.$route.query.id;
        
        if(this.data.ads.length > 0) { 
            this.ads = this.data.ads;
        } else {
            this.ads = 0;
        }

        for(let i = 0; i < this.ads.length; i++) {
            if(this.ads[i].id == this.id) {
                this.item = this.ads[i];
            }
        }

        if(this.item == null) {
            this.$router.push({name: 'index'});
        }
    },
    data() {
        return {
            data: [],
            name: '',
            type: '',
            ads: [],
            id: null,
            item: null,
            form: {
                id: this.$route.query.id,
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
        async update() {
            try {
                await this.$axios.post('/ad/change', this.form);

                this.$router.push({'path': 'ads'});
            } catch (e) {}
        }
    }
};
</script>