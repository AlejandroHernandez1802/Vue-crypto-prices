

<template>
  <div class="myapp">
    <div class="container">
      <div class="row">

        <h1 class="text-center text-success mt-2 mb-2">COIN MARKET</h1>

        <input v-model="this.coinToSearch" @keyup="handleKeyUp" placeholder="Filter coin" type="text" class="bg-dark text-light my-4 border-0 rounded-0 form-control"/>

        <table class="table table-dark table-striped">
          <thead>
            <tr>
              <th v-for="(title,index) in this.titles" :key="index">{{title}}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="coin in filteredCoins" :key="coin.id">
              <td>
                <span class="text-muted">{{coin.market_cap_rank}}</span>
              </td>
              <td>
                <img class="coin-img" :src="coin.image" alt="Coin image" />
                <span>{{coin.name}}</span>
                <span class="ms-2 text-uppercase text-muted">{{coin.symbol}}</span>
              </td>
              <td>
                <span>${{coin.current_price}}</span>
              </td>
              <td>
                <span :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">%{{coin.price_change_percentage_24h}}</span>
              </td>
              <td>
                <span>${{coin.total_volume.toLocaleString()}}</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>  
</template>

<script>
  export default{
    name:"App",
    data(){
      return{
        coins:[],
        filteredCoins: [],
        titles:['#', 'Coin', 'Price', 'Price change', '24H volume'],
        coinToSearch:''
      }
    },
    methods:{
      handleKeyUp(){

        console.log(this.coinToSearch);

        console.log(this.filteredCoins = this.coins.filter(coin => 
          coin.name.toLowerCase().includes(this.coinToSearch.toLowerCase()) 
          || coin.symbol.toLowerCase().includes(this.coinToSearch.toLowerCase())));

        this.filteredCoins = this.coins.filter(coin => 
          coin.name.toLowerCase().includes(this.coinToSearch.toLowerCase()) 
          || coin.symbol.toLowerCase().includes(this.coinToSearch.toLowerCase()));
      }
    },
    async mounted() {
      const response = await fetch("https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false");
      const data = await response.json();
      this.coins = data;
      this.filteredCoins = data;
    },
  }
</script>

<style scoped>
  .coin-img{
    width: 2rem;
    margin-right: 2px;
  }
</style>
