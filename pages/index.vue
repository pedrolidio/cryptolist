<template>
  <div class="m-10">
      <form>
          <v-select :options="options" v-model="search" placeholder="Search cryptocurrency..."></v-select>
          <button type="submit" v-on:click.prevent.stop="addCrypto()">Add</button>
      </form>
      Watchlist:<br>
      <ul>
          <li v-for="asset in assets" :key="asset.asset_id">
              <br>
              <div v-if="selectedAssets.includes(asset.asset_id)">
                  {{ asset.name }} <br>
                  {{ asset.asset_id }} <br>
                  {{ asset.price_usd }} <br>
              </div>
          </li>
      </ul>
  </div>
</template>

<script>
    import Vue from 'vue';
    import vSelect from 'vue-select';
    import 'vue-select/dist/vue-select.css';

    Vue.component('v-select', vSelect)

    export default {
        name: '',
        data() {
            const search = "";
            const selectedAssets = [ "BTC" ];
            const assets = [
                { 
                    asset_id: "BTC",
		            name: "Bitcoin",
                    type_is_crypto: 1,
                    price_usd: 43379.5815368347054717827221,
                },
                { 
                    asset_id: "LTC",
		            name: "Litecoin",
                    type_is_crypto: 1,
                    price_usd: 112.05786531075521143207474033,
                },
                { 
                    asset_id: "XRP",
		            name: "Ripple",
                    type_is_crypto: 1,
                    price_usd: 0.7702436494715246747879875251,
                },
                { 
                    asset_id: "DOGE",
		            name: "DogeCoin",
                    type_is_crypto: 1,
                    price_usd: 0.1430201111305217207456219165,
                },
                {
                    asset_id: "USD",
		            name: "US Dollar",
		            type_is_crypto: 0,
                    price_usd: 1,
                }
            ];

            const options = [];

            assets.forEach((asset) => {
                if (asset.type_is_crypto === 1)
                    options.push({
                        label: `${asset.name} - ${asset.asset_id}`,
                        asset_id: asset.asset_id,
                    });
            });
            
            return {
                search,
                selectedAssets,
                assets,
                options
            };
        },
        methods: {
            addCrypto() {
                if(this.search == "" || this.search == null)
                    alert('Please select a cryptocurrency to add to your watchlist!');

                else if(this.selectedAssets.includes(this.search.asset_id))
                    alert(`"${this.search.label}" is already being listed!`);

                else {
                    this.selectedAssets.push(this.search.asset_id);
                    alert(`"${this.search.label}" added to watchlist!`);
                    this.search = "";
                }
            }
        },
        //async asyncData({ $axios, $config, $data }) {
            // Pega informações sobre o BTC apenas
            /*const selectedAssets = await $axios.$get('v1/assets/BTC', {
                headers: {
                    "X-CoinAPI-Key": $config.coinApiKey
                }
            });*/
        /*
            const selectedAssets = await $axios.$get('v1/assets', {
                headers: {
                    "X-CoinAPI-Key": $config.coinApiKey
                }
            });
            
            return {
                selectedAssets,
            };
        },*/
        /*async fetch() {
            this.selectedAssets = await this.$axios.$get('v1/assets', {
                headers: {
                    "X-CoinAPI-Key": this.$config.coinApiKey
                }
            });
        },*/
    }
</script>
