<template>
<div id="app">
  <b-navbar type="dark" variant="dark" class="mb-3">
    <b-navbar-brand>SnackShack</b-navbar-brand>
    <b-navbar-nav class="nav-text ml-auto text-light">
      ${{this.total.toFixed(2)}}
    </b-navbar-nav>
  </b-navbar>
  <b-container id="app">
    <b-row>
      <b-col v-for="item in items" :key="item.name" cols="6">
        <b-button @click="clickItem(item)" variant="dark" class="w-100 mb-3">
          {{item.name}}
          <br/>
          ${{item.value.toFixed(2)}}
          <br/>
          {{item.count}}
        </b-button>
      </b-col>
    </b-row>
    <div class="footer">
      <b-row>
        <b-col>
          <b-button variant="dark" @click="adjustAmount(0.25)" class="w-100 mb-3">+$0.25</b-button>
        </b-col>
        <b-col>
          <b-button variant="dark" @click="adjustAmount(1)" class="w-100 mb-3">+$1.00</b-button>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <b-button variant="danger" @click="clear()" class="w-100">Clear</b-button>
        </b-col>
        <b-col>
          <b-button variant="primary" @click="undo()" class="w-100">Undo</b-button>
        </b-col>
      </b-row>
    </div>
  </b-container>
</div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

type Item = {
  name: string;
  value: number;
  count: number;
};

@Component({
  components: {
  },
})
export default class App extends Vue {
  get total(): number {
    let total = 0;
    this.items.forEach((i) => total += (i.value * i.count));
    total += this.adjustment;
    return total;
  }

  private items: Item[] = [
    {
      name: 'Beer',
      value: 4.5,
      count: 0,
    },
    {
      name: 'Hot Dog',
      value: 4.5,
      count: 0,
    },
    {
      name: 'Soda',
      value: 2.5,
      count: 0,
    },
    {
      name: 'Sports Drink',
      value: 2.75,
      count: 0,
    },
    {
      name: 'Nuts',
      value: 2,
      count: 0,
    },
    {
      name: 'Chips/Candy',
      value: 1.5,
      count: 0,
    },
    {
      name: 'Tall Beer',
      value: 8,
      count: 0,
    },
    {
      name: 'Sandwich',
      value: 7,
      count: 0,
    },
  ];
  private history: string[] = [];
  private adjustment: number = 0;

  private clickItem(item: Item): void {
    const itemFound = this.items.find((i) => i === item);
    if (itemFound === undefined) {
      return;
    }
    itemFound.count++;
    this.history.push(itemFound.name);
  }

  private adjustAmount(amount: number): void {
    this.adjustment += amount;
    this.history.push(String(amount));
  }

  private undo(): void {
    if (this.history.length === 0) {
      return;
    }
    const last = this.history.pop();
    const itemFound = this.items.find((i) => i.name === last);
    if (itemFound === undefined) {
      this.adjustment -= Number(last);
    } else {
      itemFound.count--;
    }
  }

  private clear(): void {
    this.items.forEach((i) => i.count = 0);
    this.adjustment = 0;
    this.history = [];
  }
}
</script>

<style>
html body {
  background-color: black;
}

.nav-text {
  font-size: 20px;
}

#app {
  touch-action: manipulation;
}

</style>
