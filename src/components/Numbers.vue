<!-- added some CSS to make the list look a bit better and to use the unneccessary div, otherwise I would have taken it out -->

<template>
  <div class="container">
    <div
      class="number text-center"
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
  data() {
    return {
      limit: this.$parent.limit,
      // numbers never got populated, I populated this.numbers in getRandomNumbers() and moved the method to the computed property to get called only on limit change
      numbers: []
      ///// My first approach ended up being too computationally expensive and overcomplicating destructuring but I left it here for interest sake: /////
      // make an array of objects, { key: numbers, value: [divisor list of number] } so that I can move all logic to the computed property to call the function only once the limit changes or on initial render
    };
  },
  computed: {
    getRandomNumbers() {
      // unneccessary to declare another variable here
      // let numbers = [];
      // included the last integer by changing < to <=
      for (var i = 1; i <= this.limit; i++) {
        this.numbers = [...this.numbers, i];
        // This is way too computationally expensive to move all the methods to the computed property and just select the 'id' based off the divisor list computed at the start of the lifecycle
        // numbers.push({ [i]: this.getDivisors(i) }); ///// or by using array.reduce(fn)
      }
      ////// The above also overcomplicated sorting the array to randomize it /////
      this.numbers.sort(() => Math.random() - 0.5);

      return this.numbers;
    }
  },
  watch: {
    ["$parent.limit"](newLimit) {
      this.limit = newLimit;
      // I directly populated the numbers state with the getRandomNumbers() function, thus on limit change I need to reset the numbers state.
      this.numbers = [];
    }
  },
  methods: {
    //// removed this part of addActiveClassnames() and put it in its own function, returning a much more legible console.log() on mouseover
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
    //// redid the logic to getElementById using the unused :id attribute, I believe this should be less expensive computationally
    //// renamed from hov() to:
    addActiveClassnames(number) {
      this.getDivisorList(number).forEach(divisor => {
        const divisorEl = document.getElementById(`number-${divisor}`);
        divisorEl.classList.add("active");
      });

      ///// Old logic before the getDivisorList(number) function refactor /////
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
    //// renamed the reset() to:
    removeActiveClassnames() {
      ///// If you forEach the divisorList here it will cost more resources to loop over all the elements on screen (divisor.length) times to select the individual queries than to loop over it once using querySelectorAll to remove the active class on each element. /////
      const numberEl = document.querySelectorAll(".number");
      numberEl.forEach(num => num.classList.remove("active"));
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
/* added the following to make the list look nicer */
.container {
  display: grid;
  grid-template-columns: repeat(50, minmax(0, 1fr));
  justify-content: center;
}
.text-center {
  text-align: center;
}
</style>
