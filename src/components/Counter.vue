<template lang='pug'>
  b-container()
    b-row
      b-col
        b-card(class='my-3')
          b-row
            b-col
              b-input(v-model='dimension' min='2' max='7' type='number')
            b-col
              b-input(v-model='limit' type='number' :min='2*dimension*dimension')
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
      limit: 18,
      matrix: []
    }
  },
  methods: {
    clickCell(r, c) {
      let idx = r*this.dimension+c
      if(this.matrix[idx]==this.counter) {
        if(this.counter+this.size>this.limit) {
          this.$set(this.matrix, idx, '')
        } else {
          this.$set(this.matrix, idx, this.counter + this.size)
        }
        this.counter++
      }
    },
    populateMatrix(){
      this.counter = 1
      for(let i=0;i<this.size;++i) {
        this.setToRandomCoords(i+1)
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
      this.size = this.dimension*this.dimension
      this.limit = 2*this.size
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
      this.initMatrix()
    },
    limit() {
      this.initMatrix()
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
  font-size: 5em;
}

.grid {
  table-layout: fixed;
}
</style>
