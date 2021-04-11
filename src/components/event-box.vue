<template>
  <div class="p-5 shadow-lg">
    <div class="flex">
      <img
        class="h-8 w-8"
        v-if="!expanded"
        @click="expanded = !expanded"
        src="arrow-thin-down.svg"
      />
      <img
        class="h-8 w-8"
        v-if="expanded"
        @click="expanded = !expanded"
        src="arrow-thin-up.svg"
      />
      <p class="text-xl ml-3 font-bold">{{ eventName }}</p>
    </div>

    <div class="" v-if="expanded">
      <div class="flex">
        <input
          type="text"
          v-model="newMember"
          class="outline-none w-3/4 text-xl p-3"
          placeholder="add new member"
        />
        <p
          class="text-center text-white p-3 bg-blue m-3 mb-12 hover:bg-blue-dark w-32 font-bold w-1/5"
          @click="calculateSplit"
        >
          Calculate Bill
        </p>
      </div>
      <p
        class="text-center p-3 bg-orange m-3 mb-12 hover:bg-orange-dark w-32 font-bold"
        @click="addPerson"
      >
        Add Person
      </p>
      <div v-for="(person, value) in contributions" :key="person">
        <div class="flex">
          <p @click="showPersonData(value)" class="p-3 text-xl w-5/6">
            {{ person.name }}
          </p>
          <p @click="showPersonData(value)" class="p-3 text-xl w-1/6">
            {{ person.totalAmt }}
          </p>
        </div>

        <div v-if="person.isTrue">
          <div v-for="(value, items) in person.items" :key="value">
            <div v-if="items != `isTrue`" class="flex flex-row p-3">
              <div class="w-3/4">
                <p class="text-xl text-grey-darkest">{{ items }}</p>
              </div>
              <div class="w-1/4">
                <p class="text-xl text-grey-darkest">{{ value }}</p>
              </div>
            </div>
          </div>
          <div class="p-3">
            <input
              type="text"
              v-model="newItem"
              class="outline-none w-3/4 text-xl"
              placeholder="add new member"
            />
            <input
              type="text"
              v-model="amount"
              class="outline-none w-1/4 text-xl"
              placeholder="amount"
            />
          </div>
          <p
            class="text-center p-3 bg-grey m-3 hover:bg-grey-dark w-32 mx-auto font-bold"
            @click="addSpent(value)"
          >
            Add
          </p>
        </div>
      </div>
      <div class="border-t border-grey">
        <div
          class="text-grey-light"
          v-for="(person, value) in contributions"
          :key="person"
        >
          <div
            class="flex"
            v-bind:class="{
              'text-green': person.balance < 0,
              'text-red': person.balance > 0,
            }"
          >
            <p @click="showPersonData(value)" class="p-3 text-xl w-5/6">
              {{ person.name }}
            </p>
            <p @click="showPersonData(value)" class="p-3 text-xl w-1/6">
              {{ person.balance }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "eventBox",
  props: { eventName: String },
  data() {
    return {
      contributions: [],
      newMember: "",
      newItem: "",
      amount: "",
      expanded: true,
    };
  },
  methods: {
    addPerson: function () {
      if (this.newMember !== "" && this.checkDuplicatePerson()) {
        this.contributions.push({
          name: this.newMember,
          isTrue: false,
          totalAmt: 0,
          balance: 0,
          items: {},
        });
      }
      this.newMember = "";
    },

    //prevent duplicate entry of names
    checkDuplicatePerson: function () {
      let valueRtn = true;
      this.contributions.forEach((elem) => {
        if (elem.name === this.newMember) {
          valueRtn = false;
          return valueRtn;
        }
      });
      return valueRtn;
    },

    showPersonData: function (value) {
      this.contributions[value].isTrue = !this.contributions[value].isTrue;
      for (let i = 0; i < this.contributions.length; i++) {
        if (i !== value) {
          this.contributions[i].isTrue = false;
        }
      }
    },

    addSpent: function (value) {
      if (this.isNumeric(this.amount) && this.checkKeyName(value)) {
        this.contributions[value].items[this.newItem] = this.amount;
      } else if (this.isNumeric(this.amount)) {
        //do code for adding the data
        this.contributions[value].items[this.newItem] =
          parseInt(this.contributions[value].items[this.newItem]) +
          parseInt(this.amount);
      }
      this.newItem = "";
      this.amount = "";
      this.totalAmount(value);
    },

    //function to check if the string is valid number
    isNumeric: function (str) {
      if (typeof str != "string") return false; // we only process strings!
      return (
        !isNaN(str) && // use type coercion to parse the _entirety_ of the string (`parseFloat` alone does not do this)...
        !isNaN(parseFloat(str))
      ); // ...and ensure strings of whitespace fail
    },

    checkKeyName: function (value) {
      //console.log(Object.keys(obj).includes(this.newMember));
      let that = this;
      let isTrue = true;
      Object.keys(this.contributions[value].items).forEach((elem) => {
        if (elem.toLowerCase() === that.newItem.toLowerCase()) {
          that.personSelect = elem;
          isTrue = false;
        }
      });
      return isTrue;
    },

    totalAmount: function (value) {
      //console.log(this.contributions[value].totalAmt);
      const values = Object.values(this.contributions[value].items);
      let sum = 0;
      values.forEach((elem) => (sum += parseInt(elem)));
      this.contributions[value].totalAmt = sum;
    },

    calculateSplit: function () {
      let sum = 0;
      this.contributions.forEach(
        (elem) => (sum = sum + parseInt(elem.totalAmt))
      );
      console.log(sum);
      sum = sum / this.contributions.length;
      this.contributions.forEach(
        (elem) => (elem.balance = Math.ceil(sum - parseInt(elem.totalAmt)))
      );
      this.contributions.forEach((elem) => console.log(elem.balance));
    },
  },
};
</script>

