<template>
<v-row>
    <v-col align="center" justify="center">
        <v-card style="width: 40%; margin-top: 2.5%;">
            <v-card-title justify="center">
                <v-col>
                    <h3>Upload</h3>
                </v-col>
            </v-card-title>
            <form>
                <v-text-field
                v-model="form.title"
                label="Title"
                style="width:60%"
                ></v-text-field>

                <v-text-field
                v-model="form.keyWords"
                label="Key words"
                style="width:60%"
                ></v-text-field>

                <v-select
                v-model="form.genresIds"
                :items="items"
                item-text="name"
                item-value="id"
                label="Genres"
                data-vv-name="select"
                multiple
                hint="Choose genres"
                persistent-hint
                :menu-props="{ maxHeight: '400' }"
                style="width:60%"
                ></v-select>

                <v-file-input
                v-model="form.files"
                accept=".pdf"
                label="Choose file"
                style="width: 65.5%;
                margin-left: -5%;"
                ></v-file-input>

                <br>

                <v-btn
                class="mr-4"
                @click="submit()"
                outlined
                >
                Submit
                </v-btn>
                <v-btn @click="clear()"
                outlined>
                Cancel
                </v-btn>
                
                <br>
                <br>
            </form>
        </v-card>
    </v-col>
</v-row>
</template>

<script>
import axios from "axios";
import { mapGetters, mapMutations } from "vuex";
export default {
data() {
    return {
        form: {
            files: null,
            writerId: null,
            title: '',
            genresIds: '',
            keyWords: ''
        },
        items: []
    };
  },
  computed: {
    ...mapGetters(["user"]),
  },
  methods: {
    submit() {
      let a = "";
      for(let i = 0; i < this.form.genresIds.length; i++) {
        a += this.form.genresIds[i];
        if(i != this.form.genresIds.length - 1) {
          a += ",";
        }
      }
      this.form.genresIds = a;
      this.form.writerId = this.user.userResponse.id;
      let fd = new FormData();
      fd.append("files", this.form.files);
      fd.append("genresIds", this.form.genresIds);
      fd.append("writerId", this.form.writerId);
      fd.append("keyWords", this.form.keyWords);
      fd.append("title", this.form.title);
      console.log(fd)
      axios
        .post("api/books", fd)
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
      console.log(this.user.userResponse)
    axios
      .get("api/genres")
      .then(items => {
        this.items = items.data;
        
      })
      .catch(error => {
        console.log(error);
      });
  }
}
</script>

<style>

</style>