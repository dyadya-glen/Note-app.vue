<template>
    <div class="wrappwe">
        <div class="wrapper-content">
            <section>
                <div class="container">
                    <div class="note-header page-title">
                        <p> {{ title }} </p>
                    </div>
                    <!-- message -->
                    <message v-if="message" :message="message"/>
                    <!-- new note -->
                    <newNote :note="note" @addNote="addNote"/>

                    <div class="note-header">
                        <!-- title -->
                        <h1>{{ title }}</h1>
                        <!-- search -->
                        <search
                            :value="search"
                            placeholder="Find your note"
                            @search="search = $event" style="margin: 30px 0;"/>
                        <!-- icons controls -->
                        <div class="icons">
                            <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
                            <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
                        </div>
                    </div>
                    <!-- notes list -->
                    <notes :notes="notesFilter" :grid="grid" />
                    <!-- <notes :notes="notes" :grid="grid" @remove="removeNote"/> -->
                </div>
            </section>
        </div>
    </div>
</template>

<script>
import message from '@/components/Message.vue'
import newNote from '@/components/NewNote.vue'
import search from '@/components/Search.vue'
import notes from '@/components/Notes.vue'
export default {
    components: { message, newNote, search, notes },
    data () {
        return {
            title: 'Notes App',
            search: '',
            message: null,
            grid: true,
            note: {
                title: '',
                descr: '',
                importance: 'standard',
                show: true
            },
            //importance: 'box-shadow: none',
            notes: [
                {
                    title: 'First note',
                    descr: 'Description from first note',
                    date: new Date(Date.now()).toLocaleString(),
                    importance: 'standard',
                    show: true
                },
                {
                    title: 'Second note',
                    descr: 'Description from second note',
                    date: new Date(Date.now()).toLocaleString(),
                    importance: 'importantly',
                    show: true
                },
                {
                    title: 'Third note',
                    descr: 'Description from third note',
                    date: new Date(Date.now()).toLocaleString(),
                    importance: 'essential',
                    show: true
                }
            ]
        }
    },
    computed: {
        notesFilter () {
            let array = this.notes,
                search = this.search
            if (!search) {
                return array
            }
            // Small
            search = search.trim().toLowerCase()
            // Filter
            array = array.filter(function (item) {
                if (item.title.toLowerCase().indexOf(search) !== -1) {
                    return item
                }
            })
            // Error
            return array
        }
    },
    methods: {
        addNote () {
            let {title, descr, importance, show} = this.note
            if (title === '') {
                this.message = 'title can`t be blank'
                return false
            }
            this.notes.push({
                title,
                descr,
                importance,
                show,
                date: new Date(Date.now()).toLocaleString()
            })
            this.message = null
            this.note.title = ''
            this.note.descr = ''
            this.note.importance = 'standard'
            this.note.show = true
        },
        // removeNote (index) {
        //     this.notes.splice(index, 1)
        // }
    }
}
</script>

<style scoped>
.container {
  max-width: 800px;
}
.page-title {
    margin: 36px 0;
    justify-content: center;
}
</style>
