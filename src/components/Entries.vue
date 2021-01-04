<template>
  <div id="main">
    <button @click="handleOrder">{{"Newest to oldest"}}</button>
    <ul :style="{ 'flex-direction' : isReversed? 'column-reverse' : 'column'}" id="entries">
        <li class="entry" v-for="entry in sortedEntries" :key="entry.id">
          <h3>{{entry.title}}</h3>
          <small>{{entry.date | dateFormat()}}</small>
          <br>
          <img :src="entry.image" alt="Entry image">
          <p>{{entry.text}}</p>
        </li>
    </ul>
  </div>
</template>

<script>
    import moment from 'moment'
    export default {
        name: 'Entries',
        props: {
            entries: Array,
            isReversed: Boolean
        },

      computed: {
        sortedEntries: function () {
          let entriesCopy = this.entries.slice()
          return entriesCopy.sort( (a,b) => {return new Date(b.date) - new Date(a.date)})
        }
      },

      methods: {
        handleOrder(event) {
          this.isReversed = !this.isReversed
          if (event.target.textContent === "Oldest to Newest") {
            event.target.textContent = "Newest to Oldest";
          } else {
            event.target.textContent = "Oldest to Newest";
          }
        },
      },

      filters: {
        dateFormat: function (value) {
          return moment(value).format("dddd, MMMM D, YYYY hh:mm A");
        }
      }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 10px;
    }

    ul {
        display: flex;
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
