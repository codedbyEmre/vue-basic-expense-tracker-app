<template>
  <header class="bg-primary text-white py-2 border-bottom border-4 border-dark">
      <div class="container-md">
          <div class="d-flex align-items-center justify-content-between">
            <h2>Expense Tracker</h2>
            <h4 class="total">Balance: 
              <!-- Income is more -->
              <span v-if="total > 0" class="badge bg-success">
                <i class="fas fa-lira-sign mx-1"></i>
                <span class="h3">{{total.toLocaleString()}}</span> 
              </span>
              <!-- Expense is more -->
              <span v-else-if="total < 0" class="badge bg-danger">
                <i class="fas fa-lira-sign mx-1"></i>
                <span class="h3">{{total.toLocaleString()}}</span> 
              </span>
              <!-- The balance is zero  -->
              <span v-else class="badge bg-secondary">
                <i class="fas fa-lira-sign mx-1"></i>
                <span class="h3">{{total}}</span> 
              </span>
            </h4>
          </div>
      </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  props: ['total']
}
</script>

<style>

</style>