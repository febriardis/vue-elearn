<template>
    <div class="exam_result">
        <v-snackbar
            v-model="snackbar"
            :multi-line="'multi-line'"
            :right="'right'"
            :timeout="6000"
            :top="'top'"
            color="rgba(0,0,0,0.5)"
        >
            File soal tidak tersedia !
            <v-btn :color="'col'" flat @click="snackbar = false">
            Close
            </v-btn>
        </v-snackbar>

        <!-- sub content -->
        <v-container fluid grid-list-md>
            <v-layout row wrap>
                <!-- leader board -->
                <v-flex md12 sm12 xs12>
                    <v-card>
                        <div style="float:left">
                            <v-card-text class="px-0"><h6 class="title" style="margin:4px 20px; text-transform:capitalize">Hasil Tryout</h6></v-card-text>
                        </div>
                        <v-btn style="float:right;margin:13px 20px;" color="warning" @click="close">Tutup</v-btn>
                        <div class="clear"></div>
                    </v-card>
                    <hr>
                    <!-- <v-card> -->
                        <div class="nav-result">
                            <ul>
                                <li><a @click="showDetail">DETAIL</a></li>
                                <li><a @click="alert" v-show="discussMenu1">PEMBAHASAN</a></li>
                                <li><a @click="showDiscuss" v-show="discussMenu2">PEMBAHASAN</a></li>
                                <li><a @click="downloadSoal">DOWNLOAD SOAL</a></li>
                            </ul>
                        </div>

                        <v-card v-show="detailV" flat style="height:400px;padding-top:50px">
                            <v-card style="width:600px;margin:0px auto; padding:20px">
                                <v-layout row wrap style="text-transform:capitalize;font-size:16px;">
                                    <v-flex md6 sm12 xs12>
                                        <table>
                                            <tr><td width="150"><b>Jawaban Benar</b></td><td>{{data.correct_answered}}</td></tr>
                                            <tr><td><b>Jawaban Salah</b></td><td>{{data.incorrect_answered}}</td></tr>
                                            <tr><td><b>Tidak Dijawab</b></td><td>{{data.left_answered}}</td></tr>
                                        </table>
                                    </v-flex>
                                    
                                    <v-flex md6 sm12 xs12>
                                        <table>
                                            <tr><td width="100"><b>Nilai</b></td><td>{{data.score}}</td></tr>
                                            <tr><td><b>Status</b></td>
                                                <td>
                                                    <label v-if="data.result_status=='Lulus'" style="color:#0091EA">{{data.result_status}}</label>
                                                    <label v-else style="color:red">{{data.result_status}}</label>
                                                </td>
                                            </tr>
                                        </table>
                                    </v-flex>
                                </v-layout>
                            </v-card>
                        </v-card>
                        
                        <!-- discuss -->
                        <v-card v-show="discussV">
                            <v-container grid-list-xl>
                                <v-layout row wrap>
                                    <v-flex md12 sm12 xs12>
                                        <v-card>
                                            <v-layout row wrap>
                                                <v-flex md9 style="padding-top:14px;padding-left:35px">                        
                                                <h6 class="title">Pembahasan Soal </h6>
                                                </v-flex>
                                            </v-layout>
                                        </v-card>
                                    </v-flex>

                                    <v-flex sm12 xs12 class="hidden-md-and-up">
                                        <v-card style="padding:15px; height:280px;"> 
                                            <v-card style="padding:2px 5px">
                                                <b>Navigasi Soal</b>
                                            </v-card><br>
                                                <div style="margin:-10px 0px 10px 0px;width:100%;height:35%;overflow:auto">
                                                    <a
                                                        class="btn-num"
                                                        v-for="(item, key, index) in detail" :key="item.id" 
                                                        @click="viewQuestion(key)"
                                                    >  
                                                        <span v-if="key+1 < 10 && item.mark=='1'" style="background:orange;padding:10px 14.6px">{{key+1}} </span>       
                                                        <span v-else-if="key+1 >= 10 && item.mark=='1'" style="background:orange;padding:10px 10.6px">{{key+1}}</span>
                                                        
                                                        <span v-else-if="key+1 < 10 && item.answer!=item.discussion.correct_answer" style="background:red;padding:10px 14.6px">{{key+1}}</span>
                                                        <span v-else-if="key+1 >= 10 && item.answer!=item.discussion.correct_answer" style="background:red;padding:10px 10.6px">{{key+1}}</span>
                                                    
                                                        <span v-else-if="key+1 < 10" style="background:#64DD17;padding:10px 14.6px">{{key+1}}</span>
                                                        <span v-else-if="key+1 >= 10" style="background:#64DD17;padding:10px 10.6px">{{key+1}}</span>
                                                        
                                                    </a>
                                                    <div class="clear"></div>
                                                </div>
                                            <div class="clear"></div>
                                            <v-card style="padding:2px 5px">
                                                <b>Keterangan</b>
                                            </v-card>
                                            <v-layout>
                                                <v-flex md12>
                                                    <div><span style="width:15px;height:15px;background:red; margin:2.6px; float:left"></span><span>Jawaban Salah</span></div>
                                                    <div><span style="width:15px;height:15px;background:#64DD17; margin:2.6px; float:left"></span><span>Jawaban Benar</span></div>
                                                    <div><span style="width:15px;height:15px;background:orange; margin:2.6px; float:left"></span><span>Ditandai</span></div>
                                                </v-flex>
                                            </v-layout> 
                                        </v-card>
                                    </v-flex>
                                    
                                    <v-flex md9 sm12 xs12>
                                        <v-card style="padding:5px;">
                                            <span style="margin:18px;font-size:18px"><b>Soal No. {{hal+1}}</b>
                                                <span v-if="discuss.answer == discussion.correct_answer" >&nbsp;<v-icon color="green">done</v-icon></span>
                                                <span v-else>&nbsp;<v-icon color="red">clear</v-icon></span> 
                                            </span>

                                            <div style="float:right; padding:2px 10px 0px 0px;">
                                                <span style="color:#757575">Jawaban Benar: <b style="color:#0091EA">{{discussion.correct_answer}}</b></span>
                                                <span style="margin:10px">|</span>
                                                <span style="color:#757575">Jawaban Anda: <b style="color:#0091EA">{{discuss.answer}}<span v-if="discuss.answer==null">-</span></b></span>
                                                <span style="margin:10px">|</span>
                                                <span style="color:#757575">Nilai Soal: <b style="color:#0091EA">{{discuss.score}}</b></span>
                                            </div>
                                        </v-card>
                                        
                                        <v-card style="min-height:347px">
                                            <div v-show="load_data" style="margin:0px auto; padding-top:40px; width:5%;">
                                                <v-progress-circular
                                                :size="40"
                                                color="primary"
                                                indeterminate
                                                ></v-progress-circular>
                                            </div>

                                            <v-container style="padding-left:25px">
                                                <!-- pertanyaan -->
                                                <b>Pertanyaan:</b>
                                                <p style="font-size:16px" v-html="discussion.question"></p>        
                                                <div v-if="discussion.option_a!=null">
                                                    <b style="float:left">A.&nbsp;</b>
                                                    <span style="float:left" v-html="discussion.option_a"></span>
                                                    <span v-if="discuss.answer!= 'A' && discussion.correct_answer== 'A'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Benar</span>
                                                    <span v-if="discuss.answer== 'A' && discussion.correct_answer== 'A'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Anda</span>
                                                    <span v-if="discuss.answer== 'A' && discussion.correct_answer!= 'A'" style="color:red">&nbsp;<v-icon color="red" size="20">clear</v-icon>Jawaban Anda</span>
                                                    <div class="clear"></div>
                                                </div>

                                                <div v-if="discussion.option_b!=null">
                                                    <b style="float:left">B.&nbsp;</b>
                                                    <span style="float:left" v-html="discussion.option_b"></span>
                                                    <span v-if="discuss.answer!= 'B' && discussion.correct_answer== 'B'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Benar</span>
                                                    <span v-if="discuss.answer== 'B' && discussion.correct_answer== 'B'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Anda</span>
                                                    <span v-if="discuss.answer== 'B' && discussion.correct_answer!= 'B'" style="color:red">&nbsp;<v-icon color="red" size="20">clear</v-icon>Jawaban Anda</span>
                                                    <div class="clear"></div>
                                                </div>

                                                <div v-if="discussion.option_c!=null">
                                                    <b style="float:left">C.&nbsp;</b>
                                                    <span style="float:left" v-html="discussion.option_c"></span>
                                                    <span v-if="discuss.answer!= 'C' && discussion.correct_answer== 'C'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Benar</span>
                                                    <span v-if="discuss.answer== 'C' && discussion.correct_answer== 'C'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Anda</span>
                                                    <span v-if="discuss.answer== 'C' && discussion.correct_answer!= 'C'" style="color:red">&nbsp;<v-icon color="red" size="20">clear</v-icon>Jawaban Anda</span>
                                                    <div class="clear"></div>
                                                </div>

                                                <div v-if="discussion.option_c!=null">
                                                    <b style="float:left">D.&nbsp;</b>
                                                    <span style="float:left" v-html="discussion.option_c"></span>
                                                    <span v-if="discuss.answer!= 'D' && discussion.correct_answer== 'D'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Benar</span>
                                                    <span v-if="discuss.answer== 'D' && discussion.correct_answer== 'D'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Anda</span>
                                                    <span v-if="discuss.answer== 'D' && discussion.correct_answer!= 'D'" style="color:red">&nbsp;<v-icon color="red" size="20">clear</v-icon>Jawaban Anda</span>
                                                    <div class="clear"></div>
                                                </div>

                                                <div v-if="discussion.option_e!=null">
                                                    <b style="float:left">E.&nbsp;</b>
                                                    <span style="float:left" v-html="discussion.option_e"></span>
                                                    <span v-if="discuss.answer!= 'E' && discussion.correct_answer== 'E'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Benar</span>
                                                    <span v-if="discuss.answer== 'E' && discussion.correct_answer== 'E'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Anda</span>
                                                    <span v-if="discuss.answer== 'E' && discussion.correct_answer!= 'E'" style="color:red">&nbsp;<v-icon color="red" size="20">clear</v-icon>Jawaban Anda</span>
                                                    <div class="clear"></div>
                                                </div>

                                                <div v-if="discussion.option_f!=null">
                                                    <b style="float:left">F.&nbsp;</b>
                                                    <span style="float:left" v-html="discussion.option_f"></span>
                                                    <span v-if="discuss.answer!= 'F' && discussion.correct_answer== 'F'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Benar</span>
                                                    <span v-if="discuss.answer== 'F' && discussion.correct_answer== 'F'" style="color:green">&nbsp;<v-icon color="green" size="20">done</v-icon>Jawaban Anda</span>
                                                    <span v-if="discuss.answer== 'F' && discussion.correct_answer!= 'F'" style="color:red">&nbsp;<v-icon color="red" size="20">clear</v-icon>Jawaban Anda</span>
                                                    <div class="clear"></div>
                                                </div>
                                                <!-- /pertanyaan -->
                                                <hr>
                                                <!-- penjelasan -->
                                                <div>
                                                    <b>Penjelasan:</b>
                                                    <span v-html="discussion.explanation"></span>
                                                </div>
                                                <hr>
                                                <div>
                                                    <b>Video Penjelasan:</b>
                                                    <div style="width:100%;height:380px">
                                                        <!-- <iframe :src="'https://www.youtube.com/embed/'+url_video" style="width: 100%;height: 100%;" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->
                                                        <iframe :src="'https://www.youtube.com/embed/'+discussion.url_explanation" style="width: 100%;height: 100%;" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                                                    </div>
                                                </div>
                                                <!-- /penjelasan -->
                                                <br>
                                                
                                                <v-card color="#B71C1C" dark>
                                                    <v-card-text>
                                                    <v-layout style="font-size:15px">
                                                        <v-flex md6>
                                                            <b>Siswa yang menjawab benar</b>&nbsp;:&nbsp;<b>{{discuss.user_right}}</b>
                                                        </v-flex>
                                                        <v-flex md6>
                                                            <b>Siswa yang menjawab salah</b>&nbsp;:&nbsp;<b>{{discuss.user_wrong}}</b>
                                                        </v-flex>
                                                        <!-- <v-flex md4>
                                                            <b>Nilai</b>&nbsp;:&nbsp;<b>{{discuss.score}}</b>
                                                        </v-flex> -->
                                                    </v-layout>
                                                    </v-card-text>
                                                </v-card>

                                                <!-- {{discuss}} -->

                                            </v-container>                    
                                        </v-card>
                                        <hr>
                                        
                                        <v-card style="padding:0px 18px">
                                            <v-btn v-if="hal!=0" @click="previous(hal)" small> <v-icon left dark>keyboard_arrow_left</v-icon>Sebelumnya</v-btn>
                                            <v-btn v-if="hal+1!=detail.length" @click="next(hal)" small>Berikutnya <v-icon right dark>keyboard_arrow_right</v-icon></v-btn>  
                                        </v-card>

                                    </v-flex>
                        
                                    <v-flex md3 class="hidden-sm-and-down">
                                        <v-card style="padding:15px; height:480px"> 
                                            <v-card style="padding:2px 5px">
                                                <b>Navigasi Soal</b>
                                            </v-card><br>
                                            <div style="margin:-10px 0px 10px 0px;width:100%;height:65%;overflow:auto">
                                                <a
                                                    class="btn-num"
                                                    v-for="(item, key, index) in detail" :key="item.id" 
                                                    @click="viewQuestion(key)"
                                                >  
                                                    <span v-if="key+1 < 10 && item.mark=='1'" style="background:orange;padding:10px 14.6px">{{key+1}} </span>       
                                                    <span v-else-if="key+1 >= 10 && item.mark=='1'" style="background:orange;padding:10px 10.6px">{{key+1}}</span>
                                                    
                                                    <span v-else-if="key+1 < 10 && item.answer!=item.discussion.correct_answer" style="background:red;padding:10px 14.6px">{{key+1}}</span>
                                                    <span v-else-if="key+1 >= 10 && item.answer!=item.discussion.correct_answer" style="background:red;padding:10px 10.6px">{{key+1}}</span>
                                                
                                                    <span v-else-if="key+1 < 10" style="background:#64DD17;padding:10px 14.6px">{{key+1}}</span>
                                                    <span v-else-if="key+1 >= 10" style="background:#64DD17;padding:10px 10.6px">{{key+1}}</span>
                                                    
                                                </a>
                                                <div class="clear"></div>
                                            </div>
                                            <div class="clear"></div>
                                            <v-card style="padding:2px 5px">
                                                <b>Keterangan</b>
                                            </v-card>
                                            <v-layout>
                                                <v-flex md12>
                                                    <div><span style="width:15px;height:15px;background:red; margin:2.6px; float:left"></span><span>Jawaban Salah</span></div>
                                                    <div><span style="width:15px;height:15px;background:#64DD17; margin:2.6px; float:left"></span><span>Jawaban Benar</span></div>
                                                    <div><span style="width:15px;height:15px;background:orange; margin:2.6px; float:left"></span><span>Ditandai</span></div>
                                                    <!-- <div><span style="width:15px;height:15px;background:#03A9F4; margin:2.6px; float:left"></span><span>Aktif</span></div> -->
                                                </v-flex>
                                            </v-layout> 
                                        </v-card>
                                    </v-flex>
                                </v-layout>
                            </v-container>
                            
                        </v-card>
                        <!-- /discuss -->

                    <!-- </v-card> -->
                    
                </v-flex>
                <!-- /leader board -->
            </v-layout>
        </v-container>
        <!-- /sub content -->
        <!-- {{IdCereout}} -->
    </div>
</template>

<script>
    import axios from 'axios';
    import getVideoId from 'get-video-id'
    import urlParser from "js-video-url-parser"

    export default {
        name: 'exam_result',
        props: ["id", "data", "IdCereout"],

        data (){
            return {
                snackbar: false,
                latex: '',
                detailV: true,
                discussV: false,
                discussMenu1: true,
                discussMenu2: false,

                // url_video: '',
                detail: [],
                load_data: true,
                hal: 0,
                discuss: [],
                discussion: [],
            }
        },

        watch: {
            latex: function() {
                console.log('data changed')
                // this.reRender();
                this.$nextTick().then(()=>{
                    this.reRender();
                });
            }
        },

        methods: {
            //download soal
            downloadSoal(){
                axios.get('/cereouts/file/'+this.IdCereout)
                .then(res=>{
                    this.forceFileDownload(res)
                    // console.log(res)
                })
                .catch(err=>{
                    console.log(err.res)
                    this.snackbar = true
                })
            },

            //discuss
            viewQuestion(index) {   
                this.hal        = index 
                this.discuss    = this.detail[index]
                this.discussion = this.detail[index].discussion
                this.latex      = this.discussion.question.replace(/(<span[^>]+>|<span>|<\/span>)/g, '$')
                // let url         = urlParser.parse(this.detail[index].discussion.url_explanation).id
                // this.url_video  = url
            },

            previous(hal){
                if(hal > 0){
                    hal--
                    this.hal        = hal
                    this.discuss    = this.detail[hal]
                    this.discussion = this.detail[hal].discussion
                    this.latex      = this.discussion.question.replace(/(<span[^>]+>|<span>|<\/span>)/g, '$')
                    // let url         = urlParser.parse(this.detail[hal].discussion.url_explanation).id
                    // this.url_video  = url
                }
            },

            next(hal){
                if(hal < this.detail.length-1){
                    hal++
                    this.hal        = hal
                    this.discuss    = this.detail[hal]
                    this.discussion = this.detail[hal].discussion
                    this.latex      = this.discussion.question.replace(/(<span[^>]+>|<span>|<\/span>)/g, '$')
                    // let url         = urlParser.parse(this.detail[hal].discussion.url_explanation).id
                    // this.url_video  = url
                }
            },  
            // disucss

            alert(){
                return this.$swal({
                    title: 'Apakah anda yakin?',
                    text: 'Setelah anda melihat pembahasan, percobaan tryout anda akan habis',
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonText: 'Ya',
                    cancelButtonText: 'Tidak',
                    showCloseButton: true,
                    showLoaderOnConfirm: true
                }).then((result) => {
                    if(result.value) { 
                        this.showDiscuss()
                    }
                })
            },

            showDetail(){
                // this.discussMenu1 = false
                // this.discussMenu2 = true
                this.discussV = false
                this.detailV  = true
            },

            reRender() {
                if(window.MathJax) {
                    console.log('rendering mathjax');
                    window.MathJax.Hub.Queue(["Typeset", window.MathJax.Hub], () => console.log('done'));
                }
            },

            showDiscuss(){
                this.reRender();
                this.discussMenu1 = false
                this.discussMenu2 = true
                this.detailV  = false
                this.discussV = true

                axios.get('/cereouts/result/detail/'+this.id)
                .then(response => {
                    this.detail     = response.data.data            
                    this.load_data  = false
                    this.discuss    = this.detail[0]
                    this.discussion = this.detail[0].discussion
                    this.latex      = this.discussion.question.replace(/(<span[^>]+>|<span>|<\/span>)/g, '$')
                    // let url         = urlParser.parse(this.detail[0].discussion.url_explanation).id
                    // this.url_video  = url
                    // console.log(response.data)
                })
                .catch(error => {
                    console.log(error.response)
                })
            },

            close(){
                window.close()
            }
        },
    }
</script>


<style>
    .nav-result{
        background: #B71C1C;
        padding:10px;
    }
    .nav-result ul li{
        display: inline;
    }
    .nav-result ul li a{
        background:#D32F2F;
        color: white;
        padding: 12px 30px;
        margin: 0px 2px;
        text-decoration: none;
    }
    .nav-result ul li a:hover{
        background:#E53935;
    }
</style>

