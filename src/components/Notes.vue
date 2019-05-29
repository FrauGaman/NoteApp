<template>
  <div class="notes">
    <div class="note" v-for="(note, index) in notes" :key="index" :class="{ full: !grid }">
      <div class="note-header" :class="{ full: !grid }">
        <p>{{ note.title }}</p>
        <p class="close" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p>{{ note.descr }}</p>
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
			}

		},
		methods: {
			removeNote(index) {
				this.$emit('remove', index)
			}
		}
	}

</script>

<style lang="scss">
	.close {
		cursor: pointer
	}
	.note-body {
		span {
			font-size: 14px;
			color: #999999
		}
	}
</style>