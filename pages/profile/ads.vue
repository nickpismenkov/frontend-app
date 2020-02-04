<template>
    <div class="container">
        <div class="mt-5">
            <b-alert variant="success" show>{{ name }} <b-badge>{{ type }}</b-badge></b-alert>
        </div>
        <hr>
        <div class="mb-5">
            <template v-if="ads">
                <div>
                    <b-button variant="primary">
                        Мои объявления <b-badge variant="light">{{ ads.length }}</b-badge>
                    </b-button>
                    <b-list-group class="mt-2">
                        <b-card v-for="ad in ads" v-bind:key="ad.id" v-bind:title="ad.title" v-bind:sub-title="ad.country + ' | ' + ad.city" class="mb-2">
                            <b-card-text>
                            <b>Описание:</b><br> {{ ad.description }}
                            </b-card-text>

                            <b-card-text><b>Категория:</b><br> {{ ad.category }}</b-card-text>

                            <div class="row">
                                <div class="col-md-1 mr-4">
                                    <form @submit.prevent="change(ad.id)">
                                        <input type="submit" class="btn btn-outline-warning" value="Изменить">
                                    </form>
                                </div>
                                <div class="ml-1">
                                    <form @submit.prevent="del(ad.id)">
                                        <input type="submit" class="btn btn-outline-danger" value="Удалить">
                                    </form>
                                </div>
                            </div>
                        </b-card>
                    </b-list-group>
                </div>
            </template>
            <template v-else>
                <h3>Нет объявлений</h3>
            </template>
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
            ads: []
        }
    },
    methods: {
        async del(id) {
            try {
                await this.$axios.post('/ad/delete', {'id': id});

                this.data = await this.$axios.$get('/me');
                this.name = this.user.name;
                this.type = this.data.type;
                
                if(this.data.ads.length > 0) { 
                    this.ads = this.data.ads;
                } else {
                    this.ads = 0;
                }
            } catch (e) {}
        },
        async change(id) {
            try {
                this.$router.push({'path': 'change', query: {id: id}});
            } catch (e) {}
        }
    }
};
</script>