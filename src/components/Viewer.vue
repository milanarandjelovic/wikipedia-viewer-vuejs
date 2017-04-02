<template>
  <div class="Viewer">
    <div class="Viewer__random text-center">
      <a href="https://en.wikipedia.org/wiki/Special:Random" target="_blank">Click here for random article</a>
    </div>

    <div class="Viewer__search">
      <form @submit.prevent="search">
        <input type="text" name="search"
          autocomplete="off"
          class="center-block"
          v-model="searchInput"
        >
      </form>
    </div>

    <div class="Viewer__results">
      <ul v-for="result in results" class="animated fadeInUp">
        <a :href="page + result.pageid" target="_blank">
          <li>
            <h1>{{ result.title }}</h1>
            <p class="result-description">
              {{ result.extract }}
            </p>
          </li>
        </a>
      </ul>
    </div>

  </div>
</template>

<script>
export default {

  name: 'viewer',

  data () {
    return {
      searchInput: '',
      results: [],
      page: 'https://en.wikipedia.org/?curid='
    }
  }, // data()

  methods: {
    search () {
      this.$http.jsonp('https://en.wikipedia.org/w/api.php?format=json&action=query&generator=search&gsrnamespace=0&gsrlimit=10&prop=pageimages|extracts&pilimit=max&exintro&explaintext&exsentences=1&exlimit=max&gsrsearch=' + this.searchInput + '&callback=JSON_CALLBACK')
        .then((response) => {
          console.log(response)
          this.results = response.data.query.pages
        })
        .catch((error) => {
          console.log(error)
        })
      this.searchInput = ''
    } // search()
  }
}
</script>

<style lang="scss" scoped>
.Viewer {
  width: 600px;
  margin: 0 auto;

  &__random {
    a {
     text-decoration: none;
     color: #FFFFFF;
    }
  }

  &__search {
    margin: 30px 0;

    form {
      input {
        outline: none;
      }

      input::-webkit-search-decoration,
      input::-webkit-search-cancel-button {
        display: none;
      }

      input[type='text'] {
        -webkit-appearance: textfield;
        -webkit-box-sizing: content-box;
        font-family: inherit;
        font-size: 100%;
        background: #ededed url(http://static.tumblr.com/ftv85bp/MIXmud4tx/search-icon.png) no-repeat 9px center;
        border: solid 1px #ccc;
        padding: 9px 10px 9px 32px;
        width: 100px;
        -webkit-border-radius: 10em;
        -moz-border-radius: 10em;
        border-radius: 10em;
        -webkit-transition: all .5s;
        -moz-transition: all .5s;
        transition: all .5s;
      }

      input[type='text']:focus {
        width: 330px;
        background-color: #fff;
        border-color: #66CC75;
        -webkit-box-shadow: 0 0 5px rgba(109,207,246,.5);
        -moz-box-shadow: 0 0 5px rgba(109,207,246,.5);
        box-shadow: 0 0 5px rgba(109,207,246,.5);
      }

      input:-moz-placeholder {
        color: #999;
      }

      input::-webkit-input-placeholder {
        color: #999;
      }
    }
  }

  &__results {
    ul {
      a {
        text-decoration: none;
        color: #000000;

        li {
          list-style: none;
          display: block;
          margin-bottom: 10px;
          padding: 15px 30px 5px 30px;
          background-color: #E7E7E8;
          animation: new-item 1s ease-in-out forwards;
          transition-timing-function: cubic-bezier(0.6, 0, 0.4, 1);

          &:hover {
            border-left: 5px solid #DE6E4B;
            margin-left: -5px;
          }
        }
      }
    }
  }
}
</style>
