<template>
  <div>
    <v-form ref="form" @submit.prevent="submit">
      <v-container>
        <v-row>
          <v-col sm="6" cols="12">
            <v-text-field label="Nombre" color="blue"></v-text-field>
          </v-col>
          <v-col sm="6" cols="12">
            <v-text-field label="Email" color="blue"></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-textarea label="Mensaje" color="blue"></v-textarea>
          </v-col>
          <v-col align="center">
            <v-btn large outlined color="blue">Enviar</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </div>
</template>
