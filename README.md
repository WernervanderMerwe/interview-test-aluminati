# Spec

Using Vue, display all numbers from 1 to 100 in a random order on the screen. This number should be configurable via a provided input box.
If the user places their pointer over a given number, highlight that numbers' divisors.
For example, if the user hovers over 21, the numbers 1, 3, 7 would be highlighted. 22 would highlight 1, 2 and 11.

# Interview Task

The provided code is functional, but it's got some issues that need to be resolved. These issues may or may not be logical in nature - just because the code is working doesn't necessarily mean it is the best way of doing something.

Improve the code how you see fit - please leave comments to justify your decisions.

# GitHub Pull Requests

This is an interview task sent to prospective candidates to work at Aluminati. As such, all pull requests will be rejected. This code is, by its very nature, purposely designed with issues that candiates are asked to review!

If you have been invited to perform this test your submission should be through your point of contact with us, e.g. your recruitment agency.

## My Changes Methodology and Comments

1. Declarative programming is much easier to read so I decided to rename variables and function names to declare what the functions do instead of a developer looking at your code and have to look at the logic to figure out what the code is doing.

   ```javascript
   n() -> getRandomNumbers()

   hov(number) -> addActiveClassnames(number) ->

   getDivisorList(number)

   reset() -> removeActiveClassnames()

   const nums = document.querySelectorAll('.number') ->
   const divisorEl // because it is an HTML element and divisor because of refactored code
   ```

2. I noticed that the 'for loop' populating the `[1, ..., 100]` array did not include the last integer so I changed the for loop logic to `<=` to include it.

3. Noticing the `data.numbers` value never getting populated I returned the result of `getRandomNumbers() {}` to the `data()` property to persist the data and to move the `getRandomNumbers() {}` function from the `methods` property to the `computed` property so that it gets called only on the `data.limit` property change when inputting a different value in the text box.

4. I noticed the `addActiveClassnames()` aka. `hover()` function had logic to get the divisibles of the number based on all the elements and looping over all of them on `@mouseover` so it would loop over all 100 elements (or `data.limit` value if changed) even if you hover over 6, where you only need to check (1 <= 6) for divisibles, I refactored the logic and created a new `method` called `getDivisorList(number)` that returns an array of divisibles, I replaced the `console.log('divisor', num)` with this list to make it much more legible and understandable upon a glance.

5. Because I had the `getDivisorList()` function I could use the `:id` attribute meaningfully, moving away from `document.queryselectorAll()` and instead looping through the returned `getDivisorList()` array and selecting each number element using the `:id` attribute to add the 'active' class while iterating through the array.
   I have decided to stay with the `document.queryselectorAll()` on `@mouseout` aka. `removeActiveClassname()` function because then you just iterate through all elements and remove the 'active' class once and you don't need to pass the (number) value to the function eliminating some potential bugs on refactoring.

6. I left some code behind commented out to highlight thought processes and comments on it.

7. I tested the limit number till 100 000 in powers of 10, 100 000 took about 10 seconds to render, but I have a particularly powerful PC. I'm thinking of doing some unit testing to test my theories of saving computational cost, especially between using `queryselectorAll()` and `array.forEach({...getElementbyId()})`.
