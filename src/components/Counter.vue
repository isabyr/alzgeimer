<template lang='pug'>
  b-container()
    b-row.justify-content-md-center
      b-col(md='auto')
        b-card(class='my-3')
          b-form(inline)
            b-input.mb-2.mr-2(v-model='dimension' style='width:4em;' min='2' max='7' type='number')
            b-checkbox.mb-2.mr-2(v-model='reverse' @input='initMatrix') Reverse
            b-button.mb-2.mr-2(variant='primary' @click='initMatrix')
              b-icon-arrow-repeat
    b-row
      b-col()
      b-col(md='8')
        table.grid(width='100%')
          tr(v-for='(row, ridx) in matrix.length/dimension')
            td(v-for='(col, cidx) in matrix.length/dimension')
              b-button.w-100.p-4(@click='clickCell(ridx, cidx)' :variant='matrix[ridx*dimension+cidx]>0?"success":"warning"')
                span.cell(v-if='matrix[ridx*dimension+cidx]>0') {{matrix[ridx*dimension+cidx]}}
                span.cell(v-else) -
      b-col()
</template>

<script>
export default {
  name: 'Counter',
  data(){
    return {
      dimension: 3,
      size: 9,
      counter: 0,
      matrix: [],
      reverse: false
    }
  },
  methods: {
    mouseDownCell(e) {
      alert(e.which)
    },
    clickCell(r, c) {
      let idx = r*this.dimension+c
      if(this.reverse) {
        if(this.matrix[idx]==this.counter+this.size) {
          if(this.counter<=0) this.$set(this.matrix, idx, '')
          else this.$set(this.matrix, idx, this.counter)
          this.counter--
        }
      } else {
        if(this.matrix[idx]==this.counter-this.size) {
          if(this.counter>this.limit) this.$set(this.matrix, idx, '')
          else this.$set(this.matrix, idx, this.counter)
          this.counter++
        }
      }
    },
    populateMatrix(){
      this.counter = 1
      if(this.reverse) this.counter = this.limit
      for(let i=0;i<this.size;++i) {
        this.setToRandomCoords(this.counter)
        if(this.reverse) this.counter--
        else this.counter++
      }
    },
    setToRandomCoords(value){
      let i = Math.round(Math.random()*(this.size-1))
      while(this.matrix[i]!=-1) {
        i = (i+1)%this.size
      }
      this.matrix[i] = value
    },
    initMatrix() {
      this.matrix = []
      for(let r=0;r<this.dimension;++r) {
        for(let c=0;c<this.dimension;++c) {
          this.matrix.push(-1)
        }
      }
      this.populateMatrix()
      this.$forceUpdate()
    }
  },
  watch: {
    dimension() {
      this.size = this.dimension*this.dimension
      this.initMatrix()
    },
    limit() {
      this.initMatrix()
    }
  },
  computed: {
    limit() {
      return Math.pow(this.dimension, this.dimension)
    }
  },
  created() {
    this.initMatrix(this.dimension)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cell {
  font-size: 3em;
}

.grid {
  table-layout: fixed;
}
</style>
