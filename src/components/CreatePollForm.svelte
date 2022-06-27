<script>
   import PollStore from '../stores/PollStore';
   import { createEventDispatcher }  from 'svelte';
   import Button from "../shared/Button.svelte";

   let dispatch = createEventDispatcher();
   let fields = { question: "", answerA: "", answerB: "" };
   let errors = { question: "", answerA: "", answerB: "" };
   let valid = false;

   const handleSubmit = () => {
      valid = true;

      // validate question
      if (fields.question.trim().length < 5) {
         valid = false;
         errors.question = "Question must be at least 5 characters long";
      } else {
         errors.question = "";
      }

      // validate answer a
      if (fields.answerA.trim().length < 1) {
         valid = false;
         errors.answerA = "Answer A can not be empty";
      } else {
         errors.answerA = "";
      }

      // validate answer b
      if (fields.answerB.trim().length < 1) {
         valid = false;
         errors.answerB = "Answer B can not be empty";
      } else {
         errors.answerB = "";
      }

      // add new poll
      if (valid) {
         let poll = {...fields, votesA: 0, votesB: 0, id: Math.floor(Math.random() * 1000)};

         // save poll to the store
         PollStore.update(currentPolls => {
            return [poll, ...currentPolls];
         });

         dispatch('add', poll)
      }
   };
</script>

<form on:submit|preventDefault={handleSubmit} autocomplete="off">
   <div class="form-field">
      <label for="question">Poll Question:</label>
      <input type="text" id="question" bind:value={fields.question} />
      <div class="error">{errors.question}</div>
   </div>
   <div class="form-field">
      <label for="answer-a">Answer A:</label>
      <input type="text" id="answer-a" bind:value={fields.answerA} />
      <div class="error">{errors.answerA}</div>
   </div>
   <div class="form-field">
      <label for="answer-b">Answer B:</label>
      <input type="text" id="answer-b" bind:value={fields.answerB} />
      <div class="error">{errors.answerB}</div>
   </div>

   <div class="button">
      <Button type="secondary" flat={true}>Add Poll</Button>
   </div>
</form>

<style>
   form {
      width: 400px;
      margin: 0 auto;
   }

   .form-field {
      margin: 18px auto;
   }

   input {
      width: 100%;
      border-radius: 6px;
      padding: 10px;
      margin-top: 10px;
      border: 1px solid #ddd;
   }

   input:focus {
      outline: 2px solid #ddd;
   }

   .form-field label {
      text-align-last: left;
   }

   .button {
      display: flex;
      justify-content: center;
      padding: 10px;
   }

   .error {
      font-weight: bold;
      font-size: 12px;
      color: #d91b42;
      margin-top: 5px;
   }
</style>
