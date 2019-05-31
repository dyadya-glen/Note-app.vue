<template>
    <div class="notes">
        <div class="note"
             :class="{
                full: !grid,
                standard: note.importance == 'standard',
                importantly: note.importance == 'importantly',
                essential: note.importance == 'essential'
             }"
             v-for="(note, index) in notes"
             :key="index"
             @click="editContent(index)">

            <div class="note-header" :class="{ full: !grid }">
                <input type="text" v-show="!note.show" v-model="note.title" @click.stop class="note-input note-input--header">
                <p v-show="note.show">{{ note.title }}</p>
                <p @click.stop="removeNote(index)" class="clickRemove">✕</p>
            </div>
            <div class="note-body">
                <input type="text" v-show="!note.show" v-model="note.descr" @click.stop class="note-input note-input--body">
                <p v-show="note.show">{{ note.descr }}</p>
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
            requires: true
        },
        grid: {
            type: Boolean,
            requires: true
        }
    },
    mounted () {
        let notes = this.notes

        document.addEventListener('keyup', e => {
            if (e.keyCode === 13) {
                for (let i = 0; i < notes.length; i++) {
                    if (notes[i].title.length < 1) {
                        //notes[i].title = `${i + 1}. Note`
                        return false
                    }
                    if (!notes[i].show) {
                        notes[i].date = new Date(Date.now()).toLocaleString()
                    }
                    notes[i].show = true
                }
            }
        })
    },
    methods: {
        editContent (index) {
            let notes  = this.notes

            for (let i = 0; i < notes.length; i++) {
                if (!notes[i].show ) {
                    return false
                }
            }
            notes[index].show = false
        },
        removeNote (index) {
            this.notes.splice(index, 1)
        },
    }
}
</script>

<style lang="scss">
.notes {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    padding: 40px 0;
}
.note {
    width: 48%;
    padding: 18px 20px;
    margin-bottom: 20px;
    background-color: #fff;
    transition: all .25s cubic-bezier(.02, .01, .47, 1);
    box-shadow: 0 30px 30px rgba(0, 0, 0, .02);
    &:hover {
        box-shadow: 0 30px 30px rgba(0, 0, 0, .04);
        transform: translate(0, -6px);
        transition-delay: 0s !important;
    }
    &.full {
        width: 100%;
        text-align: center;
    }
    &.standard {
        //box-shadow: inset 0 0 0 5px #ccc;
        background-color: #fff;
    }
    &.importantly {
        //box-shadow: inset 0 0 0 5px #ff9a02;
        background-color: #fdd18e;
    }
    &.essential {
        //box-shadow: inset 0 0 0 5px #ba3838;
        background-color: #ffadad;
    }
}
.note-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    h1 {
        font-size: 32px;
    }
    p, &--input {
        font-size: 22px;
        color: #402caf;
    }
    & .clickRemove {
        cursor: pointer;
        color: brown;
    }
    svg {
        margin-right: 12px;
        color: #999;
        cursor: pointer;
        &.active {
            color: #402caf;
        }
        &:last-child {
            margin-right: 0;
        }
    }
    &.full {
        justify-content: center;
        p {
            margin-right: 16px;
            &:last-child {
                margin-right: 0;
            }
        }
    }
}
.note-input {
    display: block;
    padding: 0 2%;
    width: auto;
    margin: 0;
    border-radius: 6px;
    border-color: #000;
    font-family: 'Montserrat', Helvetica, Arial, sans-serif;
    background-color: transparent;
    &--header {
        font-size: 22px;
        color: #402caf;
    }
    
}
.note-body {
    p, input {
        margin: 20px 0;
    }
    span {
        flex: 14;
        color: #999;
    }
}
</style>
