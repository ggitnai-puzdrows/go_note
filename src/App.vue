<template>
	<div class="container-fluid p-0 m-0 vh-100">
		<div class="row p-0 m-0 h-100">
			<div class="col-12 col-md-5 col-lg-4 bg-light p-0 h-100">
				<h3 class="w-100 text-center p-4 custom-heading">
					<i class="bi-flower1"></i>
					GoNote
				</h3>
				<button type="button" class="btn btn-dark w-100 py-2 rounded-0" v-on:click="showAddForm()">
					<i class="bi bi-pen-fill"></i>
					Stwórz nową notatkę
				</button>
				<div v-if="notes.length == 0" class="alert alert-secondary m-3 text-center">
					<i class="bi bi-emoji-frown"></i>
					Jeszcze nie ma żadnych notatek do wyświetlenia
				</div>
				<notes-list v-else v-bind:notes="notes" v-on:onShowNote="showNote($event)" v-on:note-delete="onDeleteNote($event)"></notes-list>
			</div>
			<div class="col-12 col-md-7 col-lg-8 bg-white p-0">
				<div class="cover d-none d-sm-block"></div>
				<note-add-form v-if="currentNote == null" v-on:onAddNote="addNote($event)"></note-add-form>
				<note v-else v-bind:note="currentNote"></note>
			</div>
		</div>
	</div>
	<nav class="navbar fixed-bottom navbar-dark custom-nav">
		<div class="container-fluid justify-content-end p-2">
			<span class="text-muted mx-5 d-none d-md-block">
				©2021 Polityka prywatności | Warunki korzystania z usługi
			</span>
			<span class="text-light">
				<i class="bi bi-heart-fill"></i>
				Moje konto
			</span>
		</div>
	</nav>
</template>

<script>
	import NoteAddForm from "./components/NoteAddForm.vue";
	import NotesList from "./components/NotesList.vue";
	import Note from "./components/Note.vue";
	
	export default {
	components: {NoteAddForm, NotesList, Note},
	data: function (){
	return {
	notes: [],
	currentNote: null,
	}
	},
	methods: {
	onAddNote: function (note){
	var newNote = {
	name: note.name,
	content: note.content,
	};
	this.$http.post('notes', newNote).then((response) => {
	this.notes.push(response.data);
	this.showAddForm = false;
	this.currentNote = response.data;
	});
	},
	onShowAddForm: function (){
	this.currentNote = null;
	this.showAddForm = true;
	},
	onDeleteNote: function (note){
	this.showAddForm = false;
	if (note === this.currentNote){
	this.currentNote = null;
	}
	this.notes = this.notes.filter((n) => { return n !== note; });
	},
	onShowNote: function (note){
	this.showAddForm = false;
	this.currentNote = note;
	}
	
	},
	mounted() {
	this.$http.get('notes').then(response => {
	this.notes = response.data;
	});
	}
	}
</script>

<style>
	.cover {
	background-image: url("./assets/background2.jpeg");
	background-position: center;
	background-size: cover;
	min-height: 150px;
	}
	
	.custom-nav {
	background-color:#D97979;
	}
	
	.custom-heading {
	color: #7BA79C;
	}
	
</style>
