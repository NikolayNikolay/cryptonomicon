<template>
  <div class="container mx-auto flex flex-col items-center bg-gray-100 p-4">
    <div class="container">
      <section>
        <div class="flex">
          <div class="max-w-xs">
            <label for="wallet" class="block text-sm font-medium text-gray-700"
              >Тикер</label
            >
            <div class="mt-1 relative rounded-md shadow-md">
              <input
                @input="inp(ticer)"
                @keyup.enter="add"
                v-model="ticer"
                type="text"
                name="wallet"
                id="wallet"
                class="
                  block
                  w-full
                  pr-10
                  border-gray-300
                  text-gray-900
                  focus:outline-none focus:ring-gray-500 focus:border-gray-500
                  sm:text-sm
                  rounded-md
                "
                placeholder="Например DOGE"
              />
            </div>
            <div
              class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap"
            >
              <span
                v-for="(many, ind) in nameMany"
                :key="ind"
                @click="ticer = many"
                class="
                  inline-flex
                  items-center
                  px-2
                  m-1
                  rounded-md
                  text-xs
                  font-medium
                  bg-gray-300
                  text-gray-800
                  cursor-pointer
                "
              >
                {{ many }}
              </span>
            </div>
            <div v-if="arrSerchIndex" class="text-sm text-red-600">
              Такой тикер уже добавлен
            </div>
          </div>
        </div>
        <button
          @click="add"
          type="button"
          class="
            my-4
            inline-flex
            items-center
            py-2
            px-4
            border border-transparent
            shadow-sm
            text-sm
            leading-4
            font-medium
            rounded-full
            text-white
            bg-gray-600
            hover:bg-gray-700
            transition-colors
            duration-300
            focus:outline-none
            focus:ring-2
            focus:ring-offset-2
            focus:ring-gray-500
          "
        >
          <!-- Heroicon name: solid/mail -->
          <svg
            class="-ml-0.5 mr-2 h-6 w-6"
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            viewBox="0 0 24 24"
            fill="#ffffff"
          >
            <path
              d="M13 7h-2v4H7v2h4v4h2v-4h4v-2h-4V7zm-1-5C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"
            ></path>
          </svg>
          Добавить
        </button>
      </section>
      <template v-if="crypto.length">
        <hr class="w-full border-t border-gray-600 my-4" />
        <div>
          <button
            class="
              my-4
              inline-flex
              items-center
              py-2
              px-4
              border border-transparent
              shadow-sm
              text-sm
              leading-4
              font-medium
              rounded-full
              text-white
              bg-gray-600
              hover:bg-gray-700
              transition-colors
              duration-300
              focus:outline-none
              focus:ring-2
              focus:ring-offset-2
              focus:ring-gray-500
              mx-2
            "
            :disabled="page <= 1"
            @click="page = +page - 1"
          >
            Назад</button
          ><button
            class="
              my-4
              inline-flex
              items-center
              py-2
              px-4
              border border-transparent
              shadow-sm
              text-sm
              leading-4
              font-medium
              rounded-full
              text-white
              bg-gray-600
              hover:bg-gray-700
              transition-colors
              duration-300
              focus:outline-none
              focus:ring-2
              focus:ring-offset-2
              focus:ring-gray-500
              mx-2
            "
            @click="page = +page + 1"
            :disabled="hasNexpPage"
          >
            Вперед
          </button>
        </div>
        <div>
          Фильтр:
          <input
            type="text"
            class="
              border-gray-300
              text-gray-900
              focus:outline-none focus:ring-gray-500 focus:border-gray-500
              sm:text-sm
              rounded-md
            "
            v-model="filter"
          />
        </div>
        <hr class="w-full border-t border-gray-600 my-4" />
        <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
          <div
            v-for="(item, index) in pagineitedCrypto"
            :key="index"
            :class="{ 'border-4': flag == item }"
            @click="(flag = item), (graph = [])"
            class="
              bg-white
              overflow-hidden
              shadow
              rounded-lg
              border-purple-800 border-solid
              cursor-pointer
            "
          >
            <div class="px-4 py-5 sm:p-6 text-center">
              <dt class="text-sm font-medium text-gray-500 truncate">
                {{ item.name }}- USD
              </dt>
              <dd class="mt-1 text-3xl font-semibold text-gray-900">
                {{ item.prise }}
              </dd>
            </div>
            <div class="w-full border-t border-gray-200"></div>
            <button
              @click.stop="deleteCoin(item), (flag = null)"
              class="
                flex
                items-center
                justify-center
                font-medium
                w-full
                bg-gray-100
                px-4
                py-4
                sm:px-6
                text-md text-gray-500
                hover:text-gray-600 hover:bg-gray-200 hover:opacity-20
                transition-all
                focus:outline-none
              "
            >
              <svg
                class="h-5 w-5"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="#718096"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                  clip-rule="evenodd"
                ></path></svg
              >Удалить
            </button>
          </div>
        </dl>
        <hr class="w-full border-t border-gray-600 my-4" />
      </template>
      <section v-if="flag" class="relative">
        <h3 class="text-lg leading-6 font-medium text-gray-900 my-8">
          {{ flag.name }} - USD
        </h3>
        <div class="flex items-end border-gray-600 border-b border-l h-64">
          <div
            v-for="(gr, ind) in normalizeGraph"
            :key="ind"
            :style="{ height: `${gr}%` }"
            class="bg-purple-800 border w-10"
          ></div>
        </div>
        <button
          @click="flag = null"
          type="button"
          class="absolute top-0 right-0"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            xmlns:svgjs="http://svgjs.com/svgjs"
            version="1.1"
            width="30"
            height="30"
            x="0"
            y="0"
            viewBox="0 0 511.76 511.76"
            style="enable-background: new 0 0 512 512"
            xml:space="preserve"
          >
            <g>
              <path
                d="M436.896,74.869c-99.84-99.819-262.208-99.819-362.048,0c-99.797,99.819-99.797,262.229,0,362.048    c49.92,49.899,115.477,74.837,181.035,74.837s131.093-24.939,181.013-74.837C536.715,337.099,536.715,174.688,436.896,74.869z     M361.461,331.317c8.341,8.341,8.341,21.824,0,30.165c-4.16,4.16-9.621,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    l-75.413-75.435l-75.392,75.413c-4.181,4.16-9.643,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    c-8.341-8.341-8.341-21.845,0-30.165l75.392-75.413l-75.413-75.413c-8.341-8.341-8.341-21.845,0-30.165    c8.32-8.341,21.824-8.341,30.165,0l75.413,75.413l75.413-75.413c8.341-8.341,21.824-8.341,30.165,0    c8.341,8.32,8.341,21.824,0,30.165l-75.413,75.413L361.461,331.317z"
                fill="#718096"
                data-original="#000000"
              ></path>
            </g>
          </svg>
        </button>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      ticer: "",
      crypto: [],
      flag: "",
      nameMany: [],
      graph: [],
      page: 1,
      filter: "",
    };
  },
  created() {
    const windowData = Object.fromEntries(
      new URL(window.location).searchParams.entries()
    );
    if (windowData.filter) {
      this.filter = windowData.filter;
    }
    if (windowData.page) {
      this.page = windowData.page;
    }

    const ticersData = localStorage.getItem("cryptonomicon-list");
    if (ticersData) {
      this.crypto = JSON.parse(ticersData);
      this.crypto.forEach((elem) => {
        this.subscribeToUpdates(elem.name);
      });
    }
  },
  computed: {
    startIndex() {
      return (this.page - 1) * 6;
    },
    endIndex() {
      return this.page * 6;
    },
    filteredCrypto() {
      return this.crypto.filter((elem) =>
        elem.name.includes(this.filter.toUpperCase())
      );
    },
    pagineitedCrypto() {
      return this.filteredCrypto.slice(this.startIndex, this.endIndex);
    },
    hasNexpPage() {
      return this.filteredCrypto.length - 1 < this.endIndex;
    },
    normalizeGraph() {
      const maxValue = Math.max(...this.graph);
      const minValue = Math.min(...this.graph);
      if (maxValue === minValue) {
        return this.graph.map(() => 50);
      }
      return this.graph.map(
        (item) => 5 + ((item - minValue) * 95) / (maxValue - minValue)
      );
    },
    arrSerchIndex() {
      return this.crypto.find((item) => item.name == this.nameMany);
    },
  },
  methods: {
    subscribeToUpdates(coinName) {
      // ====вывод крипты на экран==============================
      setInterval(async () => {
        const f = await fetch(
          `https://min-api.cryptocompare.com/data/pricemulti?fsyms=${coinName}&tsyms=USD&   api_key=383e355e889992feeb3166a6d655a0890fcfb0191bd469c8f66bdfaeef1a3b1d`
        );
        const data = await f.json();
        this.crypto.find((item) => item.name == coinName).prise =
          data[coinName].USD;

        if (this.flag?.name == coinName) {
          this.graph.push(data[coinName].USD);
        }
      }, 5000);
      // ==================================
    },
    add() {
      const coin = {
        name: this.ticer.toUpperCase(),
        prise: "-",
      };
      if (this.ticer !== "") {
        // this.crypto.push(coin);
        this.crypto = [...this.crypto, coin];
      }
      this.subscribeToUpdates(coin.name);
      this.nameMany = [];
      this.ticer = "";
      this.filter = "";
    },

    deleteCoin(item) {
      this.crypto = this.crypto.filter((elem) => elem !== item);
      // let ticersData = JSON.parse(localStorage.getItem("cryptonomicon-list"));
      // ticersData = ticersData.filter((elem) => elem.name !== item.name);
      // localStorage.setItem("cryptonomicon-list", JSON.stringify(ticersData));
    },
    async inp(str) {
      const items = await fetch(
        `https://min-api.cryptocompare.com/data/all/coinlist?summary=true`
      );
      const data = await items.json();
      // let cashName = Object.keys(data.Data);
      // this.nameMany.push(cashName.filter(item=> item.toLowerCase() == str.toLowerCase()))
      this.nameMany = Object.keys(data.Data);
      this.nameMany = this.nameMany.filter(
        (item) => item.toLowerCase() == str.toLowerCase()
      );
    },
  },
  watch: {
    crypto() {
      localStorage.setItem("cryptonomicon-list", JSON.stringify(this.crypto));
    },
    pagineitedCrypto() {
      if (this.pagineitedCrypto.length === 0 && this.page > 1) {
        this.page -= 1;
      }
    },
    filter() {
      this.page = 1;
      window.history.pushState(
        null,
        document.title,
        `${window.location.pathname}?filter=${this.filter}&page=${this.page}`
      );
    },
    page() {
      window.history.pushState(
        null,
        document.title,
        `${window.location.pathname}?filter=${this.filter}&page=${this.page}`
      );
    },
  },
};
</script>
