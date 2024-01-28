<template>
    <div>
        <v-app-bar :elevation="2" color="primary">
            <template v-slot:prepend>

            </template>

            <v-app-bar-title>En väder app</v-app-bar-title>
            <router-link class="link1" to="/"> Hem </router-link>
            <router-link class="link2" to="/weatherInfo"> Alla Väder</router-link>
        </v-app-bar>
    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');

* {
    font-family: Montserrat;
}

.v-app-bar {
    background: #627fb5;
}

.link1 {
    margin-right: 20px;
}

.link2 {
    margin-right: 50px;
}

a {
    color: #ffffff;
    text-decoration: none;
}

a:visited {
    color: #ffffff;
    text-decoration: none;

}
</style>
