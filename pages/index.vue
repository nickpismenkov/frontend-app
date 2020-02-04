<template>
  <div class="container mt-5">
    <hr>
    <div>
    <h3>Фильтр</h3>
      <form @submit.prevent="filter">
        <div class="row">
          <div class="col-md-5">
              <div class="form-group">
                <label>Страна</label>
                <b-form-select v-model="form.country" :options="form.countries"></b-form-select>
              </div>
          </div>
          <div class="col-md-5">
            <div class="form-group">
              <template v-if="form.country == 'russia'">
                  <label>Город</label>
                  <b-form-select v-model="form.city" :options="form.russia"></b-form-select>
              </template>
              <template v-if="form.country == 'china'">
                  <label>Город</label>
                  <b-form-select v-model="form.city" :options="form.china"></b-form-select>
              </template>
              <template v-if="form.country == 'usa'">
                  <label>Город</label>
                  <b-form-select v-model="form.city" :options="form.usa"></b-form-select>
              </template>
            </div>
          </div>
          <div class="col-md-2">
            <label>Фильтровать</label>
            <input type="submit" class="btn btn-outline-success" value="Фильтровать">
          </div>
        </div>
      </form>
      <br>
      <template v-if="ads">
        <div>
          <b-button variant="warning">
              Объявления <b-badge variant="danger">{{ ads.length }}</b-badge>
          </b-button>
          <b-list-group class="mt-2">
            <b-card v-for="ad in ads" v-bind:key="ad.id" v-bind:title="ad.title" v-bind:sub-title="ad.country + ' | ' + ad.city" class="mb-2">
              <b-card-text>
              <b>Описание:</b><br> {{ ad.description }}
              </b-card-text>

              <b-card-text><b>Категория:</b><br> {{ ad.category }}</b-card-text>
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
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  async mounted() {
    if(this.$route.query.country && this.$route.query.city){
      this.data = await this.$axios.$get('/' + this.form.country + '/' + this.form.city);
    } else if(this.$route.query.country) {
      this.data = await this.$axios.$get('/' + this.form.country);
    } else {
      this.data = await this.$axios.$get('/');
    }
    
    if(this.data.data && this.data.data.length > 0) { 
        this.ads = this.data.data;
    } else {
        this.ads = 0;
    }
  },
  data() {
    return {
        data: [],
        ads: [],
        form: {
          country: null,
          city: null,
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
    }
  },
  methods: {
    async filter() {
      try {
          if(this.form.country && this.form.city){
            this.data = await this.$axios.$get('/' + this.form.country + '/' + this.form.city);
          } else if(this.form.country) {
            this.data = await this.$axios.$get('/' + this.form.country);
          } else {
            this.data = await this.$axios.$get('/');
          }
          
          if(this.data.data && this.data.data.length > 0) { 
              this.ads = this.data.data;
          } else {
              this.ads = 0;
          }
      } catch (e) {}
    }
  }
}
</script>