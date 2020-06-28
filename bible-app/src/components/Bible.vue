<template>
  <div>
      
    <h1>Search the Bible</h1>

    <br />
    <br />
    <section>
      <div class="container-selection">
        <div class="row">
          <div class="col">
            <h2>Book</h2>
            <v-select :options="book" v-model="get_book"></v-select>
          </div>
          <div class="col">
            <h2>Chapter</h2>
            <v-select :options="chapter" v-model="get_chapter"></v-select>
          </div>
          <div class="col">
            <h2>Verse</h2>
            <v-select :options="verse" v-model="get_verse"></v-select>
          </div>
        </div>
      </div>
    </section>
    <br />
    <br />
    <section>
      <div class="container">
        <div class="row">
          <div class="col">
            <!-- Button trigger modal -->
            <button
            v-on:click="fetchVerse"
              type="button"
              class="btn btn-outline-dark btn-lg"
              data-toggle="modal"
              data-target="#staticBackdrop"
            >Search</button>

            <!-- Modal -->
            <div
              class="modal fade"
              id="staticBackdrop"
              data-backdrop="static"
              data-keyboard="false"
              tabindex="-1"
              role="dialog"
              aria-labelledby="staticBackdropLabel"
              aria-hidden="true"
            >
              <div class="modal-dialog">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="staticBackdropLabel">Verse</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                      <span aria-hidden="true">&times;</span>
                    </button>
                  </div>
                  <div class="modal-body"><h2>{{info}}</h2></div>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Share</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col">
            <button type="button" class="btn btn-outline-dark btn-lg" @click="selected = undefined">Clear</button>
          </div>
        </div>
      </div>
    </section>
<section>
    
    <footer>
  <p><h3>Developed by </h3><a href="https://tinasheim.netlify.com/" target="_blank"><h3>Tinashe Matembo</h3></a>
</footer>
</section>
  </div>
</template>

<script>
import axios from 'axios'


export default {
    
  name: "Bible",
  data() {
    return {
        
    url_base: 'https://bible-api.com/',
    translation: '?translation=kjv',
    query: '',
      book: [
    'Genesis',         'Exodus',          'Leviticus',     'Numbers',
    'Deuteronomy',     'Joshua',          'Judges',        'Ruth',
    '1 Samuel',        '2 Samuel',        '1 Kings',       '2 Kings',
    '1 Chronicles',    '2 Chronicles',    'Ezra',          'Nehemiah',
    'Esther',          'Job',             'Psalm',         'Proverbs',
    'Ecclesiastes',    'Song of Solomon', 'Isaiah',        'Jeremiah',
    'Lamentations',    'Ezekiel',         'Daniel',        'Hosea',
    'Joel',            'Amos',            'Obadiah',       'Jonah',
    'Micah',           'Nahum',           'Habakkuk',      'Zephaniah',
    'Haggai',          'Zechariah',       'Malachi',       'Matthew',
    'Mark',            'Luke',            'John',          'Acts',
    'Romans',          '1 Corinthians',   '2 Corinthians', 'Galatians',
    'Ephesians',       'Philippians',     'Colossians',    '1 Thessalonians', 
    '2 Thessalonians', '1 Timothy',       '2 Timothy',     'Titus',
    'Philemon',        'Hebrews',         'James',         '1 Peter',
    '2 Peter',         '1 John',          '2 John',        '3 John',
    'Jude',            'Revelation'
],
      chapter: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 149, 150],
      verse: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 149, 150, 151, 152, 153, 154, 155, 156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 174, 175, 176],
      bible_verse: {},
  
      header: null,
      info: null,
      get_book: "",
      get_chapter: "",
      get_verse: ""
    }
  },
    methods:{
        fetchVerse(){
             axios
      .get('https://bible-api.com/' + this.get_book + this.get_chapter + ':' + this.get_verse)
      .then(response => (this.info = response.data.text))
        },
        clear(){

        }
    },
};

</script>

<style scoped>
div.container {
  width: 500px;
  margin: auto;
  border: 3px solid #ffffff;
}

div.container-selection {
  width: 900px;
  margin: auto;
  border: 3px solid #ffffff;
}

body {
  font-family: "Source Sans Pro", "Helvetica Neue", Arial, sans-serif;
  text-rendering: optimizelegibility;
  -moz-osx-font-smoothing: grayscale;
  -moz-text-size-adjust: none;
}

h1,
.muted {
  color: #2c3e5099;
}

h1 {
  font-size: 50px;
  font-weight: 600;
}

h2,
.muted {
  color: #2c3e5099;
}

h2 {
  font-size: 40px;
  font-weight: 600;
}

#app {
  max-width: 30em;
  margin: 1em auto;
}

footer { 
    text-align: center;
position: absolute;
  bottom: 0;
  width: 100%;
  height: 2.5rem;  
  }

footer p {
    color: #888;
    font-size: 13px;
    letter-spacing: .4px;
}

footer a {
    color: #4a89dc;
    text-decoration: none;
    transition: all .2s ease;
}

footer a:hover {
    color: #666;
    text-decoration: underline;
}

footer img {
    width: 80px;
    transition: all .2s ease;
}

footer img:hover { opacity: .83; }

footer img:focus , footer a:focus { outline: none; }


</style>
