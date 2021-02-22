<template>
<v-container>
<div>
    <v-row>
        <v-col align="left">
          <v-card style="margin-top: -2%;">
            <form>
                <br>
                <v-text-field
                v-model="form.title"
                label="Title"
                style="width:20%; margin-left:2%;"
                ></v-text-field>

                <v-select
                v-model="form.titleOperation"
                :items="items"
                item-text="name"
                item-value="name"
                label="Operation"
                data-vv-name="select"
                hint="Choose operation"
                persistent-hint
                :menu-props="{ maxHeight: '400' }"
                style="width:20%; margin-left: 30%; margin-top: -6%;"
                ></v-select>

                <v-text-field
                v-model="form.keywords"
                label="Key words"
                style="width:20%; margin-left:2%"
                ></v-text-field>

                <v-select
                v-model="form.keywordsOperation"
                :items="items"
                item-text="name"
                item-value="name"
                label="Operation"
                data-vv-name="select"
                hint="Choose operation"
                persistent-hint
                :menu-props="{ maxHeight: '400' }"
                style="width:20%; margin-left: 30%; margin-top: -6%;"
                ></v-select>

                <v-text-field
                v-model="form.genres"
                label="Genres"
                style="width:20%; margin-left:2%"
                ></v-text-field>

                <v-select
                v-model="form.genresOperation"
                :items="items"
                item-text="name"
                item-value="name"
                label="Operation"
                data-vv-name="select"
                hint="Choose operation"
                persistent-hint
                :menu-props="{ maxHeight: '400' }"
                style="width:20%; margin-left: 30%; margin-top: -6%;"
                ></v-select>

                <br>

                <v-text-field
                v-model="form.writer"
                label="Writer"
                style="width:20%; margin-left:2%"
                ></v-text-field>

                <v-select
                v-model="form.writerOperation"
                :items="items"
                item-text="name"
                item-value="name"
                label="Operation"
                data-vv-name="select"
                hint="Choose operation"
                persistent-hint
                :menu-props="{ maxHeight: '400' }"
                style="width:20%; margin-left: 30%; margin-top: -6%;"
                ></v-select>

                <v-text-field
                v-model="form.content"
                label="Content"
                style="width:20%; margin-left:2%"
                ></v-text-field>

                <v-select
                v-model="form.contentOperation"
                :items="items"
                item-text="name"
                item-value="name"
                label="Operation"
                data-vv-name="select"
                hint="Choose operation"
                persistent-hint
                :menu-props="{ maxHeight: '400' }"
                style="width:20%; margin-left: 30%; margin-top: -6%;"
                ></v-select>

                <br>
                <br>

                <v-btn
                class="mr-4"
                @click="submit()"
                outlined
                style="margin-left:2%; width:20%"
                >
                Search
                </v-btn>

                <v-select
                v-model="form.searchType"
                :items="items2"
                item-text="name"
                item-value="name"
                label="Search type"
                data-vv-name="select"
                hint="Choose search type"
                persistent-hint
                :menu-props="{ maxHeight: '400' }"
                style="width:20%; margin-left: 30%; margin-top: -6%;"
                ></v-select>
                
                <br>
                <br>
                <v-btn
                class="mr-4"
                @click="getAll()"
                outlined
                style="margin-left:2%; width:20%"
                >
                Get all
                </v-btn>

                <v-btn
                class="mr-4"
                @click="refresh()"
                outlined
                style="width:20%; margin-left: 6%;"
                >
                Refresh
                </v-btn>
                <br>
                <br>
            </form>
        </v-card>
    </v-col>
</v-row>
<v-row>
    <v-row>
    <v-col>
      <v-row>
        <v-col>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Title</th>
                  <th class="text-left">Writer</th>
                  <th class="text-left">Genre</th>
                  <th class="text-left">Key words</th>
                  <th class="text-left">Highlights</th>
                  <th class="text-left">Download</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="book in books" :key="book.id">
                  <td>{{ book.title }}</td>
                  <td>{{ book.writer }}</td>
                  <td>{{ book.genres }}</td>
                  <td>{{ book.keywords }}</td>
                  <td></td>
                  <td></td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</v-row>
</div>
</v-container>
</template>

<script>
import axios from "axios";
import { mapGetters, mapMutations } from "vuex";
export default {
data() {
return {
    form: {
      title: null,
      genres: null,
      keywords: null,
      writer: null,
      content: null,
      titleOperation: null,
      genresOperation: null,
      keywordsOperation: null,
      writerOperation: null,
      contentOperation: null,
      searchType: "phrase"
    },
    items: [
        'AND',
        'OR',
        'NOT'
    ],
    items1: [],
    items2: [
        'regular',
        'phrase',
        'fuzzy',
        'prefix',
        'range'
    ],
    select: [],
    genres: [],
    books: [],
}
  },
  methods: {
    submit() {
      let fd = new FormData();
      if(this.form.title != null && this.form.title.trim() == ""){
        this.form.title = null;
        this.form.titleOperation = null;
      }
      fd.append("title", this.form.title);

      if(this.form.keywords != null && this.form.keywords.trim() == ""){
        this.form.keywords = null;
        this.form.keywordsOperation = null;
      }
      fd.append("keywords", this.form.keywords);

      if(this.form.genres != null && this.form.genres.trim() == ""){
        this.form.genres = null;
        this.form.genresOperation = null;
      }
      fd.append("genres", this.form.genres);

      if(this.form.writer != null && this.form.writer.trim() == ""){
        this.form.writer = null;
        this.form.writerOperation = null;
      }
      fd.append("writer", this.form.writer);

      if(this.form.content != null && this.form.content.trim() == ""){
        this.form.content = null;
        this.form.contentOperation = null;
      }
      fd.append("content", this.form.content);

      if(this.form.titleOperation != null && this.form.titleOperation.trim() == ""){
        this.form.titleOperation = null;
      }
      fd.append("titleOperation", this.form.titleOperation);

      if(this.form.keywordsOperation != null && this.form.keywordsOperation.trim() == ""){
        this.form.keywordsOperation = null;
      }
      fd.append("keywordsOperation", this.form.keywordsOperation);

      if(this.form.genresOperation != null && this.form.genresOperation.trim() == ""){
        this.form.genresOperation = null;
      }
      fd.append("genresOperation", this.form.genresOperation);

      if(this.form.writerOperation != null && this.form.writerOperation.trim() == ""){
        this.form.writerOperation = null;
      }
      fd.append("writerOperation", this.form.writerOperation);

      if(this.form.contentOperation != null && this.form.contentOperation.trim() == ""){
        this.form.contentOperation = null;
      }
      fd.append("contentOperation", this.form.contentOperation);
      
      fd.append("searchType", this.form.searchType);
      axios
        .post("api/search", fd)
        .then(response => {
          this.books = response.data
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getAll() {
      axios
        .get("api/search")
        .then(response => {
          this.books = response.data
        })
        .catch((error) => {
          console.log(error);
        });
    },
    refresh() {
      this.form.title = null;
      this.form.keywords = null;
      this.form.writer = null;
      this.form.genres = null;
      this.form.content = null;
      this.form.titleOperation = null;
      this.form.keywordsOperation = null;
      this.form.writerOperation = null;
      this.form.genresOperation = null;
      this.form.contentOperation = null;
      this.books = [];
      this.form.searchType = "phrase"
    },
  }
}
</script>

<style>

</style>