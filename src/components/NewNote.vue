<template>
	<div class="new-note">

		<div class="importance">
			<div class="importance_header">Importance</div>

			<label class="importance_item" :class="status.name" v-for="(status, index) in priorities" :key="index"> {{ status.name }}<input type="radio" name="radio" :checked='status.check' @click="addPriority(index)">

				</label>


		</div>

		<label>Title</label>
		<input v-model="note.title" type="text">
		<label>Description</label>
		<textarea v-model="note.descr"></textarea>
		<button class="btn btnPrimary" @click="addNote">New Note</button>
	</div>
</template>

<script>
	export default {
		props: {
			note: {
				type: Object,
				required: true
			},
			priorities: {
				type: Array,
				required: true
			}
		},
		methods: {
			addNote() {
				this.$emit('addNote', this.note)
			},
			addPriority(index) {
				this.$emit('addPriority', index)
			}
		}
	}
</script>