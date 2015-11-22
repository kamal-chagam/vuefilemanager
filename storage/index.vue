<template>
 	<div class="uk-container" v-if="current_dir">
	 	 <div class="">
	 	 	<div class="uk-grid">
	 	 	   <div class="">
	 	 		<button class="uk-button uk-button-small uk-button-danger" @click="backWard"><i class="uk-icon-backward"></i></button>
	 	 		</div>
		 	 	<div>
				    <label for="file" class="uk-button uk-button-success">upload</label>
				    <input id="file" type="file" name="photos[]" @change="uploadFile" multiple>
				 </div>
				 <div>
				 	<button class="uk-button uk-button-primary" @click="createFolder">Create Folder</button>
				 </div>
	 	 	</div>
	 	 	<hr class="uk-article-divider">
	 		<ul class="uk-grid uk-grid-width-large-1-6" data-uk-grid-margin>
				<li class="" v-for="file in current_dir">
					<div class="uk-panel">
	                        <button  v-if="file.type === 'file'" class="uk-button uk-button-link uk-icon-large uk-icon-justify uk-icon-file-o" @click="selectFile(file)"> <i class=""></i></button>
	                        <button v-if="file.type === 'folder'" class="uk-button uk-button-link uk-icon-large uk-icon-justify uk-icon-folder-o" @click="changePath(file.items,file.path)">  </button>
	                        <h6 class="uk-panel-title"> {{ file.name }} </h7>
					</div>
				</li>
			</ul>
		 </div>
 	</div>
</template>
<script>
export default {
	props:['selected_file'],
	data () {
		return {
			files : {},
			current_dir : {},
			parent_dir : [],
			path :'',
			response : "no data"
		}
	},
	ready() {
		this.$nextTick(this.fetchFiles);
	},
	methods : {
		fetchFiles() {
			this.$http.get('/files')
			.success(function(files){
				this.intialize(files);
			}).error(function(errors) {
				alert("errors");
			})
		},
		intialize (files){
			// files is an array
			this.files = files;
			this.current_dir = files[0].items;
			this.path = files[0].path;
		},
		changePath(folder,path) {
			//console.log(path);	
			this.parent_dir.push({ "path":this.path,"items":this.current_dir});
			this.current_dir = folder;
			this.path = path;
		},
		selectFile(details) {
			this.selected_file = details.path;
		},
		backWard () {
			if(this.parent_dir.length){
				var folder = this.parent_dir.pop();
				this.path = folder.path;
				this.current_dir = folder.items;
			}
		},
		uploadFile(e) {
	      var files = e.target.files || e.dataTransfer.files;
	      if (!files.length)
	        return;

	      var formData = new FormData();
	      formData.append('path',this.path);
	      for(var key in files){
          	formData.append('files['+key+']', files[key]);
  		  }

	      this.$http.post('/uploadFiles', formData)
	      	.success(function (data, status, request) {
	      		this.updateListing(data[0]);
            }).error(function (data, status, request) {
                alert("problem while uploading photo");
            });
	    },
	    createFolder () {
	    	var fake = "Folder Name";
	    	var self = this;
	    	UIkit.modal.prompt("Name:", fake,
	    		 function(name){
	    			self.addFolder(name);
	    		}
	    	);
	    },
	    addFolder(name){
	    	var data = {
	    		'name' :  name,
	    		'path' : this.path
	    	};
	    	//console.log(data);
	    	this.$http.post('/files/',data)
	    				.success( function(updatedList){
	    					//console.log(updatedList);
	    					this.updateListing(updatedList[0]);
	    				}).error(function (errors) {
	    					alert("Problme while creating file");
	    				})
	    },
	    updateListing(updatedList){
	    	//updatedList = updatedList[0];
	    	//if folder not created on root folder, then replace old with latest
	    	this.files = this.replaceWithNew(this.files, updatedList.path, updatedList.items);
	    },
	    replaceWithNew(searchableList, path, updatedList){
	    	// if path is found, then update it's items with latest data
	    	for(var index in searchableList) {
	    		var searchable = searchableList[index];
	    		if(searchable['type'] == 'folder') {
			    	if(searchable['path'] == path){
			    		searchable['items'] = updatedList;
			    		// update current_dir if user opned this path
			    		if(path == this.path) {
			    			this.current_dir = updatedList;
			    		}
			    	}else{
			    		searchable['items'] = this.replaceWithNew(searchable.items, path, updatedList);
			    	}
			    		searchableList[index] = searchable;
			    }
		    }
		    return searchableList;
	    }
		
	}
}
</script>
