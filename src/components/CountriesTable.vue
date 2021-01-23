<template>
  <table class="table">
    <thead>
      <tr>
        <th scope="col">#</th>
        <th  scope="col">Country</th>
        <th  scope="col">Confirmed</th>
        <th  scope="col">Deaths</th>
        <th  scope="col">Recovered</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in data" :key="index">
        <th scope="row">{{ index + 1 }}</th>
        <td>{{ item.Country }}</td>
        <td>{{ item.TotalConfirmed | numberFormat }}</td>
        <td>{{ item.TotalDeaths | numberFormat }}</td>
        <td>{{ item.TotalRecovered | numberFormat }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: ["data"],
};
</script>