<template>

<div id="settings">
    <h1>Hello ! {{this.name}}</h1>
    <h3>{{this.photo}}</h3>
    <div v-if="display">
        <input v-model="temp_name" placeholder="New Name">
        <input v-model="temp_photo" placeholder="New Photo">
        <v-btn v-on:click="saveData(true)"><h4>Save</h4></v-btn>
        <v-btn v-on:click="saveData(false)"><h4>Cancel</h4></v-btn>
    </div>

    <div v-else>
        <v-btn v-on:click="display=true"><h4>Edit Information</h4></v-btn>
    </div>
</div>

</template>

<script>
import generate_query_string from "./generate_query_string";
export default {
    name: "UserInfo",
    data: function() {
        return {
            name: null,
            photo: null,
            temp_name: null,
            temp_photo: null,
            display: false
        };
    },
    created: function() {
        this.getData();
    },
    methods: {
        /**
         * Getting user information for user.
         */
        getData: function() {
            let url = {
                what: "user_info",
                user_id: this.$route.params.user_id
            };
            fetch("get_info.php?" + generate_query_string(url))
                .then(res => res.json())
                .then(data => {
                    this.name = data.DATA[0].name;
                    this.photo = data.DATA[0].photo;
                })
                .catch(err => {
                    this.$emit("error", err.toString());
                });
        },
        /**
         * If save is true, values entered within fields are saved as new
         * name and photo if not null.
         *
         * @param save Set true if data within temporary fields should be saved
         */
        saveData: function(save) {
            if (save) {
                this.name = this.temp_name != null ? this.temp_name : this.name;
                this.photo =
                    this.temp_photo != null ? this.temp_photo : this.photo;
            }
            this.temp_name = null;
            this.temp_photo = null;
            this.display = false;

            let body = {
                user_list: [
                    {
                        user_id: this.$route.params.user_id,
                        name: this.name,
                        photo: this.photo
                    }
                ]
            };

            let url = {
                what: "user_info",
                user_id: this.$route.params.user_id,
                action: "add_or_update"
            };
            fetch("post_info.php?" + generate_query_string(url), {
                method: "POST",
                body: JSON.stringify(body)
            }).catch(err => this.$emit("error", err.toString()));
        }
    }
};
</script>
