<template>
  <div class="kelola-pelajaran">
    
		<!-- content -->
		<v-container fluid>
				<v-layout row wrap>
					<v-flex xs12 sm12 md3>
    					<sidebarGuru />
					</v-flex>
					<v-flex xs12 sm12 md9>
              <v-card-text style="padding-top:0">
              <v-toolbar
                card
                color="#353b48" 
                dark
              >
                <v-toolbar-title>Tambah Pertanyaan</v-toolbar-title>
              </v-toolbar>

              <form>
                <v-card-text style="background-color:#fff">
                  <v-text-field box v-model="this.title" label="Judul Video" value=""></v-text-field>

                  <v-text-field v-model="this.link" box label="Link Video" value=""></v-text-field>
                  
                  <v-divider></v-divider>
                  <v-card-actions style="background-color:#fff">
                    <v-spacer></v-spacer>
                    <v-btn
                      color="primary"
                      depressed
                      @click="submit" 
                      :loading="btn_load" 
                    >
                      Simpan
                    </v-btn>
                  </v-card-actions>
                </v-card-text>
              </form>

            </v-card-text>
					</v-flex>
				</v-layout>
		</v-container>
		<!-- end scontent -->
	</div>
</template>

<script>
	import sidebarGuru from '../../components/cerevid-component/sidebarGuru'
	import axios from 'axios'

	export default {
		name:"cerevid_tambah_pertanyaan",
		components:{
			sidebarGuru
		},
    data () {
      return {
        title: '',
        link: '',
        right: null,
        btn_load: false
      }
	},
	methods: {
        submit(){
          // this.btn_load = true;

          // let data = new FormData();
          // data.append('cover', this.dataDetailPelajaran.data.cover);
          // data.set('title', this.dataDetailPelajaran.data.title);
          // data.set('description', this.dataDetailPelajaran.data.description);
          // data.set('curriculum', this.dataDetailPelajaran.data.curriculum);
          // data.set('lesson_id', this.dataDetailPelajaran.data.lesson_id);
          // data.set('user_id', this.dataDetailPelajaran.data.dataUser);

          // axios.defaults.headers = {  
          //   'Content-Type': 'multipart/form-data',  
          //   'Authorization': 'Bearer ' + this.$store.state.token 
          // }
          // axios.put('http://api.ceredinas.id/api/courses/'+this.$route.params.id, data)
          // .then(response => {
          //   this.btn_load = false;
          //   console.log(response.data)
          // })
          // .catch(error => {
          //   this.btn_load = false;
          //   console.log(error)
          // })
        }
    }
  }
</script>
