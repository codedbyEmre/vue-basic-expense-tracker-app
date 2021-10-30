<template>
  <Header :total = 'totalIncomes' />
  <div class="container-md">
    <div class="input-group mt-5 mb-3">
      <input type="text" class="form-control py-2" placeholder="Description" aria-label="Description" aria-describedby="button-addon2" v-model="description">
      <input type="number" class="form-control py-2" placeholder="Amount" aria-label="Amount" aria-describedby="button-addon2" v-model="amount">
      <input type="date" class="form-control py-2" aria-label="Recipient's username" aria-describedby="button-addon2" v-model="date">
      <button class="btn btn-outline-secondary py-2" type="button" id="button-addon2" @click="addIncome">
        <i class="fas fa-plus mx-1"></i>
        Add
      </button>
    </div>

    <div v-if="showAlert == true">
      <div class="alert alert-danger py-2" role="alert">
        <i class="fas fa-exclamation-circle mx-1"></i>
        Incomplete information. Please fill out all the required parts.
      </div>
    </div>

    <!-- Table -->
    <table class="table table-striped table-hover table-bordered mt-5">
      <thead>
        <tr class="text-center">
          <th scope="col">#</th>
          <th scope="col">Type</th>
          <th scope="col">Description</th>
          <th scope="col">Amount</th>
          <th scope="col">Date</th>
          <th scope="col">Operation</th>
        </tr>
      </thead>
      <tbody>
        <tr class="text-center" v-for="(income,index) in sortedIncomesByDate(incomes)" :key="index">
          <td>{{index}}</td>
          <td>
            <span v-if="income.amount < 0" class="badge bg-danger py-2 rounded-pill">
              <i class="fas fa-arrow-down"></i>
              Expense
            </span>
            <span v-else class="badge bg-success py-2 rounded-pill">
              <i class="fas fa-arrow-up"></i>
              Income
            </span>
          </td>
          <td>{{income.description}}</td>
          <td>{{income.amount.toLocaleString()}}</td>
          <td>{{formattedDate(income.date)}}</td>
          <td class="text-center">
            <button type="button" class="btn btn-danger btn-sm" @click="deleteIncome(index)">
              <i class="fas fa-trash-alt mx-1"></i>
              Delete 
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import moment from 'moment'

export default {
  name: 'App',
  components: {Header},
  data(){
    return{
      showAlert: false,
      description: '',
      amount: null,
      date: '',
      incomes: [
        {
          description: 'Tv',
          amount: -3000,
          date: '2021-11-05'
        },
        {
          description: 'Wage',
          amount: 8000,
          date: '2021-11-12'
        },
        {
          description: 'Iron',
          amount: -250,
          date: '2021-11-15'
        }

      ],
      totalIncomes: 0
    }
  },
  methods:{
    sortedIncomesByDate(data){
      return data.sort((a, b) => new Date(b.date) - new Date(a.date))
    },
    formattedDate(data){
      return moment(data).format('DD/MM/YYYY')
    },
    addIncome(){
      if(this.description == '' || this.amount == null || this.date == ''){
        this.showAlert = true
      }else{
        this.showAlert = false

        let newInput = {
          description: this.description,
          amount: this.amount,
          date: this.date,
        }

        this.incomes.push(newInput)

        const type = '+'
        this.calculateTotalIncome(this.incomes, type)
        
        this.description = ''
        this.amount = null
        this.date = ''
      }
    },
    deleteIncome(index){
      const type = '-'
      this.calculateTotalIncome(this.incomes, type, index)
      this.incomes.splice(index,1)
    },
    calculateTotalIncome(incomes,type,index){
      if(type == '+'){
        let lastItem = incomes[incomes.length - 1].amount
        this.totalIncomes += lastItem
      }else if(type == '-'){
        let item = this.incomes[index].amount
        this.totalIncomes -= item
      }
    }
  },
  created(){
    this.incomes.filter(income => this.totalIncomes += income.amount)
  }
}
</script>

<style>
</style>
