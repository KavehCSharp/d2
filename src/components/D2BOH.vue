<template>
  <v-container>
    <v-row class="pt-5">
      <v-col>
        <v-text-field
          label="decimal #"
          placeholder="25"
          v-model="decimal"
          solo
          @input="enter"
        ></v-text-field>
      </v-col>
      <v-col>
        <v-select @change="enter" solo v-model="b" :items="[2, 8, 16]">
        </v-select>
      </v-col>
      <v-col>
        <v-text-field
          readonly
          :value="
            integralPart
              .slice()
              .reverse()
              .map((e) => e.rem)
              .join('')
              .concat(fractionalPart.length > 0 ? '.' : '')
              .concat(
                fractionalPart.map((e) => e.c).join('')
              )
              
          "
          label="result"
          solo
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row class="pt-2">
      <v-col>
        <h4 class="mb-3">Integral Part</h4>

        <v-row
          v-for="(item, index) in integralPart"
          :key="index"
          class="py-0 my-0"
        >
          <v-col class="my-0 py-0">
            <p class="py-0 my-0">
              <span>{{ item.value }}</span>
              <span>/</span>
              <span>{{ b }}</span>
              <span>=</span>
              <strong>{{ parseInt(item.value / b) }}</strong>
              <span>, reminder = {{ item.rem }}</span>
            </p>
          </v-col>
        </v-row>
      </v-col>
      <v-col>
        <h4 class="mb-3">Fractional Part</h4>

        <v-row
          v-for="(item, index) in fractionalPart"
          :key="index"
          class="py-0 my-0"
        >
          <v-col class="my-0 py-0">
            <p class="py-0 my-0">
              <span>{{ item.value }}</span>
              <span>*</span>
              <span>{{ b }}</span>
              <span>=</span>
              <strong>{{ item.value * b }}</strong>
              <span>, c = {{item.c}}</span>

            </p>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      decimal: "",
      b: 2,
      integralPart: [],
      fractionalPart: [],
    };
  },
  methods: {
    enter() {
      if (!this.decimal) return;
      this.integralPart = [];
      var x = parseInt(this.decimal);
      var r = 0;
      while (x != 0) {
        r = x % this.b;

        switch (this.b) {
          case 16:
            if (r < 10) this.integralPart.push({ value: x, rem: r });
            else
              this.integralPart.push({
                value: x,
                rem: ["F", "E", "D", "C", "B", "A"][15 - r],
              });

            break;
          default:
            this.integralPart.push({ value: x, rem: r });
            break;
        }

        x = parseInt(x / this.b);
      }

      this.fractionalPart = [];

      if (this.decimal.includes(".")) {
        var f = parseFloat(this.decimal.substring(this.decimal.indexOf(".")));
        if (!f) return;

        while (f != 1 && this.fractionalPart.length < 10) {
          var c = parseInt(f * this.b);
          
          switch (this.b) {
          case 16:
            if (c < 10) this.fractionalPart.push({ value: f, c: c });
            else
              this.fractionalPart.push({
                value: f,
                c: ["F", "E", "D", "C", "B", "A"][15 - c],
              });

            break;
          default:
            this.fractionalPart.push({value:f, c: c})
            break;
        }
          
          
          
          //this.fractionalPart.push({value:f, c: c});
          f = f * this.b - c;
        }
      }
    },
  },
};
</script>

<style>
</style>