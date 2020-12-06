<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
     <q-input
        dark
        v-model="search"
        label="Search"
        borderless >
        <template v-slot:before>
          <q-icon name="my_location" />
        </template>

        <template v-slot:hint>
          Field hint
        </template>

        <template v-slot:append>
         <q-btn
            round
            dense
            flat
            icon="add" />
        </template>
      </q-input>
    </div>
    <div class="col text-white text-center">
      <div class="text-h4 text-weight-light">
        Saitama
      </div>
      <div class="text-h6 text-weight-light">
        Sunny
      </div>
      <div class="text-h1 text-weight-thin q-my-lg relative-position">
        <span>8</span>
        <span class="text-h4 relative-position degree">&deg;</span>
      </div>
    </div>

    <div class="col text-center">
      <img src="https://www.fillmurray.com/100/100" alt="Bill">
    </div>

    <div class="col skyline">

    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data() {
    return {
      search:''
    }
  },
};
</script>

<style lang="scss" scoped>
  .q-page {
    background: linear-gradient(to bottom, #0b486b, #f56217);
  }
  .degree {
    top: -44px;
  }
  .skyline {
    flex: 0 0 100px;
    background: url(../assets/city2.png);
    background-size: contain;
    background-position:  center bottom;
  }
</style>
