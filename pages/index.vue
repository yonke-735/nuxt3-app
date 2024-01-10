<script setup lang="ts">
interface coinType {
  id: string;
  symbol: string;
  name: string;
  nameid: string;
  rank: number;
  price_usd: string;
  percent_change_24h: string;
}
interface coinLore {
  data: coinType[];
}
// write a function to make a call to the coinlore api
const { data } = await useFetch("/api/tickers/?limit=10");

const tableData: coinLore = data.value as coinLore;

const colorMode = useColorMode();

const isDark = computed({
  get() {
    return colorMode.value === "dark";
  },
  set() {
    colorMode.preference = colorMode.value === "dark" ? "light" : "dark";
  },
});

const items = (row: any) => [
  [{
    label: 'Edit',
    icon: 'i-heroicons-pencil-square-20-solid',
    click: () => console.log('Edit', row.id)
  }, {
    label: 'Duplicate',
    icon: 'i-heroicons-document-duplicate-20-solid'
  }], [{
    label: 'Archive',
    icon: 'i-heroicons-archive-box-20-solid'
  }, {
    label: 'Move',
    icon: 'i-heroicons-arrow-right-circle-20-solid'
  }], [{
    label: 'Delete',
    icon: 'i-heroicons-trash-20-solid'
  }]
]
</script>

<template>
  <UContainer>
    <ClientOnly>
      <UButton
        :icon="
          isDark ? 'i-heroicons-moon-20-solid' : 'i-heroicons-sun-20-solid'
        "
        color="gray"
        variant="ghost"
        aria-label="Theme"
        @click="isDark = !isDark"
      />

      <template #fallback>
        <div class="w-8 h-8"></div>
      </template>
    </ClientOnly>
    <UButton variant="ghost">Test Button</UButton>
    <table border="1 px solid">
      <thead>
      <tr>
        <th>Name</th>
        <th>Symbol</th>
        <th>Price</th>
        <th>Details</th>
      </tr>
      </thead>
      <tr v-for="(currency) in tableData.data" :key="tableData.data[0].id">
        <td>{{ currency.name }}</td>
        <td>{{ currency.symbol }}</td>
        <td>{{ currency.price_usd }}</td>
        <td>
          <NuxtLink :to="'/currency/' + currency.id">{{ currency.id }}</NuxtLink>
        </td>
      </tr>
    </table>
    <!-- <div>{{ tableData }}</div> -->
    <UTable :rows="tableData.data">
      <template #actions-data="{ row }">
        <UDropdown :items="items(row)">
          <UButton
            color="gray"
            variant="ghost"
            icon="i-heroicons-ellipsis-horizontal-20-solid"
          />
        </UDropdown>
      </template>
    </UTable>
  </UContainer>
</template>
