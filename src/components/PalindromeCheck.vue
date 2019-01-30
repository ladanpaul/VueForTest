<template>
  <div v-if="word">
    <div v-if="isPalindrome">
      The word {{ word }} is palindrome :)
    </div>
    <div v-else>
      This word {{ word }} is't palindrome :(
    </div>
  </div>
</template>

<script>
export default {
  name: "PalindromeCheck",

  props: {
    word: {
      type: String,
      required: true
    }
  },

  computed: {
    isPalindrome() {
      const checkToPalindrome = this.word
        .split("")
        .reverse()
        .join("");

      return this.word === checkToPalindrome ? true : false;
    }
  }
};
</script>

