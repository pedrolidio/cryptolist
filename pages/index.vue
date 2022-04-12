<template>
    <div class="flex flex-col container mx-auto bg-gray-100 p-4 sm:rounded sm:my-10 border shadow-lg">
        <form class="mb-4 flex flex-row">
            <v-select class="w-full bg-white rounded shadow-md" :options="options" v-model="search" placeholder="Search cryptocurrency..."></v-select>
            <button class="rounded bg-blue-400 px-4 py-1 ml-2 text-white shadow-md" type="submit" v-on:click.prevent.stop="addCrypto()">Add</button>
        </form>
        <button class="rounded bg-blue-400 px-4 py-1 text-white shadow-md" @click="$fetch">Force Refetch</button>
        <br>
        <p class="text-xl">
            Watchlist: (Last update: {{lastUpdate}})
        </p>
        <br>
        <ul class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 2xl:grid-cols-6 gap-4">
            <li v-for="asset in selectedAssetsData" :key="asset.asset_id">
                <div class="bg-white border shadow-md p-2 h-20 w-1200 rounded">
                    {{ asset.name }} - {{ asset.asset_id }} <br>
                    {{ asset.asset_id }}/USD: {{ asset.price_usd.toLocaleString('en-US', { style: 'currency', currency: 'USD' }) }} <br>
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
            const counter = 0;
            const search = "";
            const selectedAssets = [ "BTC" ];
            const selectedAssetsData = [];
            const assets = [];
            const dateTime= new Date();
            const lastUpdate = dateTime.toLocaleTimeString();
            const options = [];
            
            return {
                search,
                selectedAssets,
                selectedAssetsData,
                assets,
                options,
                counter,
                lastUpdate
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
                    this.updateData();
                    alert(`"${this.search.label}" added to watchlist!`);
                    this.search = "";
                }
            },
            updateData() {
                this.selectedAssetsData = this.assets.filter((asset)=>{return this.selectedAssets.includes(asset.asset_id)}, this);
            },
            updateOptions() {
                if(this.assets != undefined)
                    for(var i = 0; i < this.assets.length; i++)
                    {
                        if (this.assets[i].type_is_crypto === 1 && !this.options.some(option => option.asset_id === this.assets[i].asset_id))
                            this.options.push({
                                label: `${this.assets[i].name} - ${this.assets[i].asset_id}`,
                                asset_id: this.assets[i].asset_id,
                            });
                    }
            },
            updateTime() {
                const dateTime= new Date();
                this.lastUpdate = dateTime.toLocaleTimeString();
            }
        },
        async mounted() {
            this.updateData();
            this.updateOptions();

            /*setInterval(async (c) => {
                c.$fetch()
            }, 30000, this);*/
        },
        /*async asyncData({ $axios, $config, $data }) {
            const assets = await $axios.$get('v1/assets/BTC', {
                headers: {
                    "X-CoinAPI-Key": $config.coinApiKey
                }
            });

            return {
                assets,
            };
        },*/
        async fetch() {
            this.assets = await this.$axios.$get('v1/assets', {
                headers: {
                    "X-CoinAPI-Key": this.$config.coinApiKey
                }
            });
            this.updateData();
            this.updateOptions();
            this.updateTime();
        },
    }
</script>

<style>
    
</style>