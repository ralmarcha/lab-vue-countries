<template>
  <div v-if="countryInfo">
    <img
      :src="`https://flagcdn.com/w320/${countryInfo.alpha2Code.toLowerCase()}.png`"
      alt="country flag"
      class="mb-4"
    />
    <h1>{{ countryInfo.name.common }}</h1>
    <table class="table">
      <thead></thead>
      <tbody>
        <tr>
          <td style="width: 30%">Capital</td>
          <td>{{ countryInfo.capital[0] }}</td>
        </tr>
        <tr>
          <td>Area</td>
          <td>{{ countryInfo.area }} km <sup>2</sup></td>
        </tr>
        <tr>
          <td>Borders</td>
          <td v-if="countryInfo.borders.length === 0">
            <ul>
              <li
                class="list-group-item d-flex justify-content-between flex-column"
              >
                This Country has no borders
              </li>
            </ul>
          </td>
          <td v-else>
            <ul>
              <li
                class="list-group-item d-flex justify-content-between flex-column"
              >
                <RouterLink
                  :to="`/country/${border}`"
                  v-for="(border, index) in countryInfo.borders"
                  :key="index"
                  >{{ border }}</RouterLink
                >
              </li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { watch } from "vue";
import { onMounted } from "vue";
import { computed } from "vue";
import { useRoute } from "vue-router";

const countryInfo = ref(null);

const route = useRoute();

const getCountryByAlphaCode = async () => {
  const alpha3Code = route.params.alpha3Code;

  const response = await fetch(
    `https://ih-countries-api.herokuapp.com/countries/${alpha3Code}`
  );

  const finalResponse = await response.json();

  countryInfo.value = finalResponse;
  return { countryInfo };
};

onMounted(() => {
  getCountryByAlphaCode();
});

const countryCode = computed(() => {
  return route.params.alpha3Code;
});

watch(countryCode, (newCountryCode) => {
  getCountryByAlphaCode();
});
</script>
