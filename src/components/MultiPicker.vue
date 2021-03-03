<template>
  <div class="multi-picker">
    <Loader v-if="isSubmitting"/>
    <div v-if="isInit">
      <div>
        <VDatePicker v-model="computedWeekendCollection" multiple :show-current="false"></VDatePicker>
      </div>
      <VBtn class="ma-2" @click="onClear" :disabled="!isDirty">Reset</VBtn>
      <VBtn class="ma-2" color="success" @click="onSave" :disabled="!isDirty">Save</VBtn>
    </div>
    <p class="red--text">{{ error }}</p>
  </div>
</template>
<script>
import Vue from 'vue';
import Loader from "@/components/Loader";

const apiUrl = 'http://test.unit.homestretch.ch/';

export default {
  name: 'MultiPicker',
  components: {Loader},
  data() {
    return {
      isInit: false,
      weekendCollection: [],
      initialDates: [],
      isDirty: false,
      isSubmitting: true,
      apiUrl: 'http://test.unit.homestretch.ch/',
      error: '',
    }
  },
  computed: {
    computedWeekendCollection: {
      get() {
        return this.weekendCollection;
      },

      set(value) {
        this.isDirty = true;
        this.error = '';
        Vue.set(this, 'weekendCollection', value);
      }
    }
  },
  methods: {
    async onSave() {
      if (!this.isDirty) return undefined;
      this.isSubmitting = true;
      const data = this.prepareData();
      try {
        const res = await this.$axios.post(apiUrl + 'save', data);

        this.setInitialState(res.data);

      } catch (e) {
        this.error = 'Что то пошло не так, попробуйте еще раз'
      }

      this.isSubmitting = false;
    },
    onClear() {
      if (!this.isDirty) return;
      this.computedWeekendCollection = this.initialDates;
      this.isDirty = false;
    },

    prepareData() {
      let initDates = this.initialDates.slice();
      const res = [];
      this.computedWeekendCollection.forEach(el => {
        res.push({date: el, value: true})
        initDates = initDates.filter(date => date !== el);
      });

      return res.concat(initDates.map(el => ({date: el, value: false})));
    },

    setInitialState(data) {
      this.computedWeekendCollection = data;
      this.initialDates = this.computedWeekendCollection;
      this.isDirty = false;
      this.isSubmitting = false;
      this.error = '';
    },

    async fetchData() {
      try {
        const res = await this.$axios.get(apiUrl);
        this.setInitialState(res.data);
        this.isInit = true;
      } catch (e) {
        console.error({e});
        this.error = 'Ничего не работает! Перезагрузитесь'
        this.isSubmitting = false;
      }
    }
  },

  beforeMount() {
    this.fetchData();
  }

}
</script>
<style lang="scss">
.multi-picker {
  .v-picker__title {
    display: none;
  }
}

</style>
