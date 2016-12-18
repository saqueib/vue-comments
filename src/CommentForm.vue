<template>
  <div class="comment-box" id="comment">
        <div class="loader" v-show="loading" >
          <span class="spinner"></span>  
        </div>
        
        <form action="" method="post" @submit.prevent="submit">
          <textarea v-model="data.message" class="input-message" name="message" id="message" rows="3" placeholder="Your comment..." required></textarea>
          <input v-model="data.name" class="input-name" type="text" name="name" placeholder="Your Name" required>
          <input :disabled="loading" type="submit" value="Comment">
        </form>
    </div>
</template>

<script>
export default {

    data() {
      return {
        loading: false,
        data: {}
      }
    },

    methods: {
      submit() {
        this.loading = true;

        // Save Comment
        this.$http.post('http://localhost:3434', this.data).then((response) => {
          // success callback

          // fire event for comment
          this.$emit('commented', response.data);  

          // Clear the message
          this.data.message = "";

          this.loading = false;
        }, (response) => {
          // error callback
          this.loading = false;
        });
      }
    }

}
</script>

<style>
  .comment-box {
    background-color: #FFF; 
    width: 100%;
    margin: auto;
    box-shadow: 0 4px 8px rgba(0,0,0, 0.1);
  }

  .comment-box form {
    padding: 1rem;
  }

  .comment-box input, .comment-box textarea {
    font-size: .8em;
    width: 97.3%;
    padding: .6em;
    border: 1px solid #EEE;
    background-color: #f7f7f7;
    display: block;
    margin-bottom: 1em;
    font-family: 'Raleway', sans-serif;
    transition: all ease 0.3s;
  }

  .comment-box input:focus, .comment-box textarea:focus {
    border-color: #47b784;
    outline: none;
    box-shadow: inset 0 0 3px #DDD;
  }

  .comment-box input[type="submit"] {
    margin-bottom: 0;
    width: 30%;
    background-color: #47b784;
    color: #FFF;
    border-color: #47b784;
    font-weight: bold;
    text-transform: uppercase;
  }

  .comment-box input[type="submit"]:focus {
    background-color: #36495d;
  }

  .comment-box input[type="submit"]:disabled {
    opacity: 0.6
  }

  #comment {
    position: relative;
  }

  #comment .spinner {
    margin-top: 5em;
    z-index: 999;
  }

  #comment .loader {
    content: '';
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background-color: rgba(255, 255, 255, 0.8);
    z-index: 2;
  }
</style>