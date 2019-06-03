<template>
  <div class="notes">
    <div class="note" v-for="(note, index) in notes" :key="index" :class="{ full: !grid, importantNote: note.priority == 'Important', veryImportantNote: note.priority == 'Very Important' }" >
      <div class="note-header" :class="{ full: !grid }" >
        <p @click="showTitleInput" >{{ note.title }}</p>
        <input type="text" v-model="note.title" :class="{ showInput: editing.title }" @keyup.enter="saveTitleChanges" @keyup.esc="resetTitleChanges">
        <p class="close" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p @click="showBodyInput">{{ note.descr }}</p>
        <input v-model="note.descr" type="text" :class="{ showBodyInput: editing.descr }" @keyup.enter="saveBodyChanges" @keyup.esc="resetBodyChanges">
        <span>{{ note.date }}</span>
      </div>
    </div>
  </div>
</template>

<script>
	export default {
		props: {
			notes: {
				type: Array,
				required: true
			},
			grid: {
				type: Boolean,
				required: true
			},
			editing: {
				type: Object,
				required: true
			},

		},
		methods: {
			removeNote(index) {
				this.$emit('remove', index)
			},
			showTitleInput() {
				this.editing.title = true
				for (let i = 0; i < this.notes.length; i++) {
					this.editing.oldTitle[i] = this.notes[i].title
				} 
			},
			saveTitleChanges(){
				this.$emit('saveTitleChanges')
			},
			resetTitleChanges() {
				for (let i = 0; i < this.notes.length; i++) {
					if (this.notes[i].title != this.editing.oldTitle[i]) {
						this.notes[i].title = this.editing.oldTitle[i]
					}
				}
				this.editing.oldTitle = []
				this.editing.title = false
			},
			showBodyInput() {
				this.editing.descr = true
				for (let i = 0; i < this.notes.length; i++) {
					this.editing.oldDescr[i] = this.notes[i].descr
				} 
			},
			saveBodyChanges(){
				this.$emit('saveBodyChanges')
			},
			resetBodyChanges() {
				for (let i = 0; i < this.notes.length; i++) {
					if (this.notes[i].descr != this.editing.oldDescr[i]) {
						this.notes[i].descr = this.editing.oldDescr[i]
					}
				}
				this.editing.oldDescr = []
				this.editing.descr = false
			}
		}
	}

</script>

<style lang="sass">
	.close 
		cursor: pointer
	
	.note-body 
		span 
			font-size: 14px
			color: #999999
</style>