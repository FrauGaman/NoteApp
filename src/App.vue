<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">

          <message v-if="message" :message="message" />

          <newNote :note="note" :priorities="priorities" @addNote="addNote" @addPriority="addPriority"/>
          
          <div class="notes-header">
            <h1>{{ title }}</h1>

            <search :value="search" placeholder="Find your note" @search="search = $event"/>

            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>

              <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>

            </div>
          </div>

          <notes 
            :notes="notesFilter"
            :grid="grid"
            @remove="removeNote"/>

        </div>
      </section>

    </div>

  </div>
</template>

<script>

  import notes from '@/components/Notes.vue'
  import newNote from '@/components/NewNote.vue'
  import message from '@/components/Message.vue'
  import search from '@/components/Search.vue'

  export default {
    components: {
      notes, newNote, message, search
    },
    data() {
      return {
        title: 'NotesApp',
        search: '',
        message: null,
        grid: true,
        priorities: [
          {name: 'Standart'},
          {name: 'Important'},
          {name: 'Very Important'}
        ],
        note: {
          title: '',
          descr: '',
          priority: 'Standart'
        },
        notes: [
          {
            title: 'First note',
            descr: 'Description for first note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'Standart'
          },
          {
            title: 'Second note',
            descr: 'Description for second note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'Standart'
          },
          {
            title: 'Third note',
            descr: 'Description for third note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'Standart'
          }
        ]
      }
    },
    computed: {
      notesFilter() {
        let array = this.notes
        let search = this.search
        if (!search) return array

        //small text
        search = search.trim().toLowerCase()

        //Filter
        array = array.filter(function(item) {
          if (item.title.toLowerCase().indexOf(search) !== -1) {
            return item
          }
        })

        //error
        return array
      }
    },
    methods: {
      addPriority(index) {
        this.note.priority = this.priorities[index].name
      },
      addNote() {
        let {title, descr, priority} = this.note

        if (title === "") {
          this.message = 'Title can\'t be blank'
          return false
        }

        this.notes.push({
          title,
          descr, 
          date: new Date(Date.now()).toLocaleString(),
          priority
        })

        this.note.title = ''
        this.note.descr = ''
        this.note.priority = 'Standart'

        this.message = null
      },
      removeNote(index) {
        this.notes.splice(index, 1)
        console.log(index)
      }
    }
  }
</script>

