<script setup>
import { ref } from 'vue';
import Form from './components/Form.vue';

const users = ref([]);

const handleFormSubmit = (formData) => {
  users.value.push(formData);
};
</script>

<template>
  <main>
    <Form @submit="handleFormSubmit" />
    <div v-if="users.length > 0" class="mt-5">
      <DataTable :value="users">
        <Column field="username" header="Username"></Column>
        <Column field="password" header="Password"></Column>
        <Column field="isAustralian" header="Australian Resident">
          <template #body="slotProps">
            {{ slotProps.data.isAustralian ? 'Yes' : 'No' }}
          </template>
        </Column>
        <Column field="gender" header="Gender"></Column>
        <Column field="reason" header="Reason"></Column>
      </DataTable>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>