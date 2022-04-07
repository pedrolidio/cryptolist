<template>
  <div>
      <h1>
          Listagem de Criptoativos
      </h1>
      <ul>
          <li v-for="asset in assets" :key="asset.id">
              <div>
                  {{ asset.name }} é cripto?
                  <div v-if="asset.type_is_crypto == 1">
                      SIM
                  </div>
                  <div v-else>
                      NÃO
                  </div>
              </div>
          </li>
      </ul>
  </div>
</template>

<script>
    export default {
        name: '',
        data() {
            return {};
        },
        async asyncData({ $axios }) {
            //const posts = await $axios.$get('https://jsonplaceholder.typicode.com/posts');
            const assets = await $axios.$get('v1/assets', {
                headers: {
                    "X-CoinAPI-Key": "059B5AF1-B2E3-40B8-A106-379253931E05"
                }
            });
            
            return {
                assets,
            };
        }
    }
</script>