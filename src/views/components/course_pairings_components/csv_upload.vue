<template>

<div>
    <h1>CSV Upload</h1>
    <!--UPLOAD-->

    <v-container fluid>
        <v-flex xs12 class="text-xs-center text-sm-center text-md-center text-lg-center">
            <img :src="imageUrl" height="150" v-if="this.imageUrl"/>
            <v-text-field label="Select File" @click='pickFile' v-model='imageName' prepend-icon='attach_file'></v-text-field>
            <input
                type="file"
                style="display: none"
                ref="image"
                accept="*"
                @change="onFilePicked">
        </v-flex>
	</v-container>
</div>

</template>

<script>
export default {
    name: "CurrentSurveys",
    data:function(){
        return{
            title: "Image Upload",
            imageName: '',
            imageUrl: '',
            imageFile: ''
        }
    },
    methods:{
        onFilePicked:function(e){
            const files = e.target.files
			if(files[0] !== undefined) {
				this.imageName = files[0].name
				if(this.imageName.lastIndexOf('.') <= 0) {
					return
				}
				const fr = new FileReader ()
				fr.readAsDataURL(files[0])
				fr.addEventListener('load', () => {
					this.imageUrl = fr.result
					this.imageFile = files[0]
				})
			} else {
				this.imageName = ''
				this.imageFile = ''
				this.imageUrl = ''
			}
        },
        pickFile () {
            this.$refs.image.click ()
        },
    }

};
</script>
