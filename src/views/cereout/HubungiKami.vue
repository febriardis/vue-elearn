<template>
    <div class="hubungi_kami">
        <!-- sub content -->
        <v-container fluid grid-list-md>
            <v-layout row wrap>
                <!-- sidebar -->
                <v-flex md3 sm12 xs12>
                    <v-card>
                        <SideBar class="hidden-sm-and-down" style="float:left;"/>
                        <Navbar class="hidden-md-and-up" />
                    </v-card>
                </v-flex>
                <!-- /sidebar -->

                <!-- hubungi kami -->
                <v-flex md6 sm12 xs12 style="min-height:300px">
                    <h5 class="headline" style="color:#424242;margin-bottom:10px">Hubungi Kami</h5>

                    <div v-show="load_data" style="margin:150px auto; width:5%;">
                        <v-layout column justify-center align-center>
                            <hollow-dots-spinner
                            :animation-duration="1000"
                            :dot-size="15"
                            :dots-num="3"
                            color="#ff1d5e"
                            />
                        </v-layout>
                    </div>

                    <div v-show="listData" v-for="item in items" :key="item.id">
                        <v-card style="padding:10px">
                            <div style="float:left">
                                <label style="font-size:12px;color:#757575">Telepon Admin</label><br>
                                <span style="font-size:16px">{{item.phone}}</span>
                            </div>
                            <div style="float:right;margin:10px 10px 0px 0px">
                                <v-icon>phone</v-icon>
                            </div>
                            <div class="clear"></div>
                        </v-card>
                        
                        <a href="https://mauorder.online/diproses" style="cursor:pointer" target="_blank">
                            <v-card style="padding:10px">
                                <div style="float:left">
                                    <label style="font-size:12px;color:#757575">Whatsapp Admin</label><br>
                                    <span style="font-size:16px">Chat sekarang!</span>
                                </div>
                                <div style="float:right;margin:11px 10px 0px 0px">
                                    <v-img :src="require('../../assets/images/whatsapp.png')" width="22px"></v-img>
                                    <!-- <v-icon>phone</v-icon> -->
                                </div>
                                <div class="clear"></div>
                            </v-card>
                        </a>

                        <v-card style="padding:10px">
                            <div style="float:left">
                                <label style="font-size:12px;color:#757575">Email Admin</label><br>
                                <span style="font-size:16px">{{item.email}}</span>
                            </div>
                            <div style="float:right;margin:10px 10px 0px 0px">
                                <v-icon>email</v-icon>
                            </div>
                            <div class="clear"></div>
                        </v-card>
                    </div>
                </v-flex>
                <!-- /hubungi kami -->
            </v-layout>
        </v-container>
        <!-- /sub content -->
    </div>
</template>

<script>
    import SideBar from '../../components/cereout-component/SideBar'
    import Navbar from '../../components/cereout-component/Navbar'
    import axios from 'axios';
    import { HollowDotsSpinner } from 'epic-spinners'

    export default {
        components:{
            SideBar,
            Navbar,
            HollowDotsSpinner
        },

        data(){
            return {
                load_data:true,
                listData:false,
                items: []
            }
        },

        mounted(){
            axios.get('/master/generalInformation')
            .then(response => {
                this.load_data = false
                this.listData  = true
                this.items     = response.data.data
                // console.log(response.data.data)
            })
            .catch(error => {
                console.log(error.response)
            })
        }
    }
</script>
