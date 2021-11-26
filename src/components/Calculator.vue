<template>
  <div class="calculator-wrapper">
    <div class="output">
      <div class="output-upper">
        <span class="history-icon">
          <svg
            focusable="false"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
          >
            <path
              d="M13 3a9 9 0 0 0-9 9H1l3.89 3.89.07.14L9 12H6c0-3.87 3.13-7 7-7s7 3.13 7 7-3.13 7-7 7c-1.93 0-3.68-.79-4.94-2.06l-1.42 1.42A8.954 8.954 0 0 0 13 21a9 9 0 0 0 0-18zm-1 5v5l4.28 2.54.72-1.21-3.5-2.08V8H12z"
            ></path>
          </svg>
        </span>
        <span class="answer">Ans = {{ answer }}</span>
      </div>
      <div class="operations">
        <span>{{ input }}</span>
      </div>
    </div>
    <div class="keys">
      <table cellspacing="0" cellpadding="0">
        <tbody>
          <tr v-for="(row, index) in operations" :key="`row-${index}`">
            <td
              v-for="(col, i) in row"
              :key="`col-${i}`"
              :colspan="i == 0 && index == 0 ? 2 : 1"
            >
              <div
                @click="handleClick(col)"
                v-html="col.value"
                :class="['key', col.type]"
              ></div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      answer: null,
      input: "0",
      result: null,
      operations: [
        [
          {
            type: "function",
            value: '<span>Rad</span><span class="disable">Deg</span>',
          },
          {
            type: "function",
            value: "x!",
          },
          {
            type: "function",
            value: "(",
          },
          {
            type: "function",
            value: ")",
          },
          {
            type: "function",
            value: "%",
          },
          {
            type: "clear",
            value: "CE",
          },
        ],
        [
          {
            type: "function",
            value: "Inv",
          },
          {
            type: "function",
            value: "sin",
          },
          {
            type: "function",
            value: "In",
          },
          {
            type: "number",
            value: "7",
          },
          {
            type: "number",
            value: "8",
          },
          {
            type: "number",
            value: "9",
          },
          {
            type: "function",
            value: "÷",
          },
        ],
        [
          {
            type: "function",
            value: "π",
          },
          {
            type: "function",
            value: "cos",
          },
          {
            type: "function",
            value: "log",
          },
          {
            type: "number",
            value: "4",
          },
          {
            type: "number",
            value: "5",
          },
          {
            type: "number",
            value: "6",
          },
          {
            type: "function",
            value: "×",
          },
        ],
        [
          {
            type: "function",
            value: "e",
          },
          {
            type: "function",
            value: "tan",
          },
          {
            type: "function",
            value: "√ ",
          },
          {
            type: "number",
            value: "1",
          },
          {
            type: "number",
            value: "2",
          },
          {
            type: "number",
            value: "3",
          },
          {
            type: "function",
            value: "−",
          },
        ],
        [
          {
            type: "function",
            value: "Ans",
          },
          {
            type: "function",
            value: "EXP",
          },
          {
            type: "function",
            value: "x<sup>y</sup>",
          },
          {
            type: "number",
            value: "0",
          },
          {
            type: "number",
            value: ".",
          },
          {
            type: "equal",
            value: "=",
          },
          {
            type: "function",
            value: "+ ",
          },
        ],
      ],
    };
  },
  methods: {
    clear() {
      this.answer = null;
      this.input = "0";
      this.result = null;
    },
    handleClick(item) {
      console.log({ item });
      if (item.type == "clear") {
        this.clear();
      } else if (item.type === "equal") {
        try {
          const result = eval(
            this.input
              .replaceAll("×", "*")
              .replaceAll("÷", "/")
              .split(" ")
              .join("")
          );
          this.answer = result;
          this.result = result;
        } catch (e) {
          console.log(e)
        }
      } else {
        if(this.result && item.type === 'number'){
          this.input = '0',
          this.result = null
        }
        const exp =  this.result || this.input;
        this.result = null;
        this.input = `${exp === "0" && item.value !== "." ? "" :exp}${
          item.value
        }`;
      }
    },
  },
};
</script>

<style>
.calculator-wrapper {
  max-width: 650px;
  width: 100%;
  margin: 50px auto;
}
.calculator-wrapper:hover .output {
  border: 1px solid #dadce0;
  box-shadow: inset 0px 1px 2px rgb(0 0 0/10%);
  border-top-color: #70757a;
}
.output {
  height: 60px;
  border: 1px solid #ebebeb;
  border-radius: 8px;
  margin: 0 3px;
  padding: 4px 14px 0 10px;
  box-sizing: border-box;
}
.output-upper {
  display: flex;
  justify-content: space-between;
}
.history-icon svg {
  height: 22px;
  line-height: 22px;
  width: 22px;
  fill: #70757a;
}
.answer {
  width: 100%;
  text-align: right;
  color: #70757a;
  font-size: 13px;
  white-space: nowrap;
  margin-top: 3px;
}
.operations {
  font-size: 24px;
  line-height: 1;
  text-align: right;
}
table {
  width: 100%;
  border-collapse: collapse;
  table-layout: fixed;
  margin-top: 3px;
}
.key {
  height: 27px;
  line-height: 26px;
  margin: 2px;
  cursor: pointer;
  font-family: arial, sans-serif;
  text-align: center;
  box-sizing: border-box;
  position: relative;
  background: #dadce0;
  color: #202124;
  border: 1px solid #dadce0;
  border-radius: 4px;
  font-size: 13px;
}
tr:first-child td:first-child .key {
  display: flex;
  justify-content: space-around;
  position: relative;
}
tr:first-child td:first-child .key::before {
  content: "";
  position: absolute;
  top: 6px;
  bottom: 6px;
  left: 50%;
  border-left: 1px solid #70757a;
  transform: translateX(-50%);
}
.key.number {
  background: #f1f3f4;
  border-color: #f1f3f4;
  font-size: 14px;
}
.key.number:focus {
  border-color: #dadce0;
}
.key:focus {
  border-color: #70757a;
}
.key:active {
  box-shadow: 0 1px 6px rgb(32 33 36 / 28%);
}
.key.equal {
  color: #fff;
  background: #4285f4;
  border-color: #4285f4;
  font-size: 18px;
  font-weight: bold;
}
.key.equal:focus {
  border-color: #1558d6;
}
.key.equal:active {
  background: #4285f4;
  box-shadow: 0 1px 2px 0 rgb(66 133 244 / 45%),
    0 3px 6px 2px rgb(66 133 244 / 30%);
}
span.disable {
  color: #70757a;
}
sup {
  font-size: 8px;
  line-height: 8px;
}
tr:not(:first-child) td:last-child .key {
  font-size: 18px;
}
</style>
