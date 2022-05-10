<template>
  <div>
    <div
      class="number"
      :id="`number-${number}`"
      v-for="number in getRandomNumbers"
      :key="number"
      @mouseover="addActiveClassnames(number)"
      @mouseout="removeActiveClassnames"
    >
      {{ number }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Numbers",
  data() {
    return {
      limit: this.$parent.limit,
      ///// My first approach ended up being too computationally expensive and overcomplicating destructuring but I left it here for interest sake
      // make an array of objects, { key: numbers, value: [divisor list of number] } so that I can move all logic to the computed property to call the function only once the limit changes or on initial render
      // destructure accordingly
      // on hover only select the divisor elements in a loop and add or remove the active class using a computed property instead of firing a function everytime on mouseover (which turned out to be more computationally expensive)
      numbers: []
    };
  },
  watch: {
    ["$parent.limit"](newLimit) {
      this.limit = newLimit;
    }
  },
  methods: {
    getDivisorList(number) {
      let divisor = [];
      for (let i = 1; i <= number; i++) {
        if (number % i === 0) {
          divisor.push(i);
        }
      }

      console.log(`${number} has got ${divisor.length} Divisors:`, ...divisor);
      return divisor;
    },
    addActiveClassnames(number) {
      this.getDivisorList(number).forEach(number => {
        const divisorElement = document.getElementById(`number-${number}`);
        divisorElement.classList.add("active");
      });

      ///////////////////////////////
      // const allNumbersEl = document.querySelectorAll(".number");

      // for (let i = 0; i < numberEl.length; i++) {
      // let divisor = [];

      // for (let i = 0; i < this.limit; i++) {
      //   const num = allNumbersEl[i].textContent.trim();
      //   if (number % num === 0) {
      //     // divisor.push(num);
      //     allNumbersEl[i].classList.add("active");
      //   }
      // }

      // divisor.sort((a, b) => a - b);
      // console.log("divisors", ...divisor);
    },
    removeActiveClassnames() {
      // this.getDivisorList(number).forEach(number => {
      //   const element = document.getElementById(`number-${number}`);
      //   element.classList.remove("active");
      // });

      // if you forEach the divisorList here it will cost more resources to loop over all the elements on screen (divisor.length) times to select the individual queries than to loop over it once using querySelectorAll to remove the active class on each element.
      const numberEl = document.querySelectorAll(".number");
      numberEl.forEach(num => num.classList.remove("active"));
    }
  },
  computed: {
    getRandomNumbers() {
      let numbers = [];
      for (var i = 1; i <= this.limit; i++) {
        numbers = [...numbers, i];
        // This is way too computationally expensive to move all the methods to the computed property and just select the id based off the divisor list computed at the beginning
        // numbers.push({ [i]: this.getDivisors(i) });
      }

      numbers.sort(() => Math.random() - 0.5);

      // console.log(numbers);
      console.log("RandomNumbers function fired");
      this.numbers = numbers;
      return numbers;
    }
  }
};
</script>

<style scoped>
.number {
  display: inline-block;
  padding: 5px;
  background-color: lightgrey;
  margin: 5px;
}

.active {
  background-color: red;
}
</style>
