<template>
  <div class="hello">
    <h1>morphological analysis</h1>
    <textarea
      v-model="message"
      class="form-control"
      placeholder="add multiple lines.."
      @input="tokenize"
      @keyup="tokenize"
    ></textarea>
    <table v-show="tokens.length" class="table table-hover">
      <thead>
        <tr>
          <th>表層形</th>
          <th>品詞</th>
          <th>品詞細分類1</th>
          <th>品詞細分類2</th>
          <th>品詞細分類3</th>
          <th>活用型</th>
          <th>活用形</th>
          <th>基本形</th>
          <th>読み</th>
          <th>発音</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="token in tokens" :key="token.surface_form">
          <td>{{ token.surface_form }}</td>
          <td>{{ token.pos }}</td>
          <td>{{ token.pos_detail_1 }}</td>
          <td>{{ token.pos_detail_2 }}</td>
          <td>{{ token.pos_detail_3 }}</td>
          <td>{{ token.conjugated_type }}</td>
          <td>{{ token.conjugated_form }}</td>
          <td>{{ token.basic_form }}</td>
          <td>{{ token.reading }}</td>
          <td>{{ token.pronunciation }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import * as Kuromoji from "kuromoji";
export default {
  name: "HelloWorld",
  data() {
    return {
      message: "",
      builder: null,
      tokenizer: null,
      tokens: [],
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
      } catch (e) {
        console.log(e);
        this.tokens = [];
      }
    },
  },
};
</script>
