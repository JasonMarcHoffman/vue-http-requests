<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <!-- remove the .prevent modifier to allow the browser to send data to the backend -->
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input type="radio" id="rating-poor" value="poor" name="rating" v-model="chosenRating" />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input type="radio" id="rating-great" value="great" name="rating" v-model="chosenRating" />
          <label for="rating-great">Great</label>
        </div>
        <p
          v-if="invalidInput"
        >One or more input fields are invalid. Please check your provided data.</p>
        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
    </base-card>
  </section>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      invalidInput: false,
    };
  },
  // emits: ['survey-submit'],
  methods: {
    submitSurvey() {
      if (this.enteredName === '' || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;

      // this.$emit('survey-submit', {
      //   userName: this.enteredName,
      //   rating: this.chosenRating,
      // });

      // we added the surveys.json to the url which is firebase specific,
      // the name can be anything but ensure to add the .json

      // fetch('https://vue-http-demo-4a081-default-rtdb.firebaseio.com/surveys.json', {
      //     // send data to firebase
      //     method: 'POST',
      //     // provide the headers and the body
      //     headers: {
      //       'Content-Type': 'application/json'
      //     },
      //     // turns javasript object into a string
      //     body: JSON.stringify({
      //       name: this.enteredName,
      //       rating: this.chosenRating
      //     })
      //   }
      // )

      // Installing and usinf axios over the native fetch
      // 1. npm install axios
      // 2. add import axios from 'axios';
      // 3. the below code is much less code
      axios.post('https://vue-http-demo-4a081-default-rtdb.firebaseio.com/surveys.json', {
        name: this.enteredName,
        rating: this.chosenRating
      }),

      this.enteredName = '';
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>