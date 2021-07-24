<template>
  <div class="container calculator">
    <div class="row justify-content-center">
      <div class="col-6">
        <div class="row">
          <div class="col-md-6">
            <label for="exampleFormControlInput1" class="form-label"
              >第一個數字</label
            >
            <input
              id="num1"
              type="number"
              class="form-control"
              placeholder="請輸入要計算的第一個數字"
              v-model.number="num1"
            />
          </div>
          <div class="col-md-6">
            <label for="num2" class="form-label">第二個數字</label>
            <input
              id="num2"
              type="number"
              class="form-control"
              placeholder="請輸入要計算的第二個數字"
              v-model.number="num2"
            />
          </div>
          <div class="col-md-12 mt-3">
            <div class="btn-group w-100">
              <button
                type="button"
                class="btn btn-outline-primary w-25"
                :class="{ active: operator === '+'}"
                @click="getOperator"
              >
                +
              </button>
              <button
                type="button"
                class="btn btn-outline-primary w-25"
                :class="{ active: operator === '-'}"
                @click="getOperator"
              >
                -
              </button>
              <button
                type="button"
                class="btn btn-outline-primary w-25"
                :class="{ active: operator === '*'}"
                @click="getOperator"
              >
                *
              </button>
              <button
                type="button"
                class="btn btn-outline-primary w-25"
                :class="{ active: operator === '/'}"
                @click="getOperator"
              >
                /
              </button>
            </div>
          </div>
        </div>
        <button
          type="button"
          class="btn btn-outline-primary d-block mt-3 w-100"
          @click="calculate"
        >
          計算結果
        </button>
        <h4 class="mt-3">結果： {{ result }}</h4>
      </div>
      <div class="col-3">
        <h5>歷史紀錄</h5>
        <ul>
          <li v-for="(record, index) in records" :key="index">
            {{ record }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
      num1: '',
      num2: '',
      operator: '',
      result: '',
      records: [],
    };
  },
  methods: {
    calculate() {
      if (!this.num1 || !this.num2 || !this.operator) {
        return;
      }
      const text = `${this.num1} ${this.operator} ${this.num2}`;
      // eslint-disable-next-line no-eval
      this.result = eval(text);
      if (this.records.length > 9) {
        this.records.unshift(this.result);
        this.records.splice(-1, 1);
      } else {
        this.records.push(`${text} = ${this.result}`);
      }
      localStorage.setItem('records', JSON.stringify(this.records));
    },
    getOperator(e) {
      this.operator = e.target.textContent.trim();
    },
  },
  mounted() {
    this.records = JSON.parse(localStorage.getItem('records')) || [];
  },
};
</script>

<style>
.calculator {
  margin-top: 200px;
}
</style>
