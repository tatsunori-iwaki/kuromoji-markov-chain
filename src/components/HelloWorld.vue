<template>
  <div class="hello">
    <h1>markov chain</h1>
    <textarea
      v-model="message"
      class="form-control"
      placeholder="add multiple lines.."
      @input="tokenize"
      @keyup="tokenize"
    ></textarea>

    <h1>automatically</h1>
    <div style="padding: 20px; border: solid 10px #ccc">
      <blockquote class="blockquote">
        <p>{{ sentence }}</p>
      </blockquote>
    </div>
  </div>
</template>

<script>
import * as Kuromoji from "kuromoji";
import Markov from "./Markov.js";
export default {
  name: "HelloWorld",
  data() {
    return {
      message: "",
      builder: null,
      tokenizer: null,
      tokens: [],
      sentence: "",
    };
  },
  created() {
    this.builder = Kuromoji.builder({
      dicPath: "./kuromoji/dict",
    });
    this.builder.build((err, tokenizer) => {
      if (err) {
        console.log(err);
        return;
      }
      this.tokenizer = tokenizer;
    });
  },
  methods: {
    tokenize: async function () {
      if (!this.message) {
        this.tokens = [];
        return;
      }
      try {
        this.tokens = this.tokenizer.tokenize(this.message);

        const markov = new Markov();
        const words = this.tokens.map(function (token) {
          return token.surface_form;
        });
        markov.add(words);
        this.sentence = markov.make();
      } catch (e) {
        console.log(e);
        this.tokens = [];
      }
    },
  },
};
</script>
