<template>
    <v-navigation-drawer permanent fixed style="margin-left:30px; top:113px; z-index:0;" height="260" width="250">
        <v-toolbar height="50" color="red" flat>
          <h3 style="margin:0px 20px;color:white">Dashboard Guru</h3>
        </v-toolbar>
        <v-divider></v-divider>
        <v-list dense class="pt-0">
            <v-list-tile to="/guru">
                <v-list-tile-action>
                <v-icon>dashboard</v-icon>
                </v-list-tile-action>

                <v-list-tile-content>
                <v-list-tile-title>Dashboard</v-list-tile-title>
                </v-list-tile-content>
            </v-list-tile>

            <!-- <v-list-tile to="/guru/account">
                <v-list-tile-action>
                <v-icon>trending_up</v-icon>
                </v-list-tile-action>

                <v-list-tile-content>
                <v-list-tile-title>Edit Akun</v-list-tile-title>
                </v-list-tile-content>
            </v-list-tile> -->

            <v-list-tile to="/guru/profile">
                <v-list-tile-action>
                <v-icon>create</v-icon>
                </v-list-tile-action>

                <v-list-tile-content>
                <v-list-tile-title>Edit Profile</v-list-tile-title>
                </v-list-tile-content>
            </v-list-tile>

            <!-- <v-list-tile to="/pusat bantuan">
                <v-list-tile-action>
                <v-icon>assessment</v-icon>
                </v-list-tile-action>

                <v-list-tile-content>
                <v-list-tile-title>Pusat Bantuan</v-list-tile-title>
                </v-list-tile-content>
            </v-list-tile> -->

        </v-list>
    </v-navigation-drawer>
</template>

<script>
export default {

}
</script>
