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
                accept=".csv"
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
            console.log(files);
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
            //parseFile(files[0]);
            var reader = new FileReader();
            reader.readAsText(files[0]);
            reader.onerror = this.errorHandler;
            reader.onload = this.loadHandler;

        },
        pickFile:function() {
            this.$refs.image.click ()
        },
        loadHandler:function(event){
            var csv = event.target.result;
            csv = this.processData(csv);
            this.createAssociation(csv);
        },
        errorHandler:function(event){
            this.$emit("error", event.target.error.name);
        },
        processData:function(csv){
            var allTextLines = csv.split(/\r\n|\n/);
            var lines = [];
            for (var i=0; i<allTextLines.length; i++) {
                var data = allTextLines[i].split(',');
                    var tarr = [];
                    for (var j=0; j<data.length; j++) {
                        tarr.push(data[j]);
                    }
                    lines.push(tarr);
            }
            return lines;
        },
        createAssociation:function(csv){
            for (let row of csv){
                let course_code = row[0];
                for (let index=1; index<row.length; index++){
                    let cell=JSON.parse(row[index]);

                    let body = JSON.stringify({
                        association_list: [
                            {
                                course: {
                                    course_code: course_code
                                },
                                section: {
                                    section_id: section_id
                                },
                                user_id: user_id
                            }
                        ]
                    });
                }
            }
        }
    }

};
</script>
