<template>
    <div>
        <v-btn @click="dialog = true">Launch</v-btn>
        <v-dialog v-model="dialog" width="500">
            <v-card>
                <v-card-title>Launch Survey</v-card-title>
                <v-card-text>Launch</v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="primary"
                    flat
                    @click="dialog = false"
                  >
                    I accept
                  </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>

<script>
  export default {
    data () {
      return {
          dialog:false
      }
  }
  }
</script>
