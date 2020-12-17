<template>
    <div class="app">
        <div v-for="cat in categories" :key="cat.id"
             class="droppable"
             @drop="onDrop($event, cat.id)"
             @dragenter.prevent
             @dragover.prevent>
            <h4>{{ cat.title }}</h4>
            <div v-for="item in items.filter(i => i.categoryId === cat.id)"
                 :key="item.id"
                 class="draggable"
                 draggable="true"
                 @dragstart="onDragStart($event, item)">
                <h5>{{item.title}}</h5>
            </div>
        </div>
    </div>
</template>

<script>
    import {ref} from 'vue'

    export default {
        name: 'App',
        setup() {
            const items = ref([
                {id: 0, title: 'Audi', categoryId: 0},
                {id: 1, title: 'Porshe', categoryId: 0},
                {id: 2, title: 'Cat', categoryId: 1},
            ])
            const categories = ref([
                {id: 0, title: 'Cars'},
                {id: 1, title: 'Animals'}
            ])

            function onDragStart(event, item) {
                event.dataTransfer.dropEffect = 'move'
                event.dataTransfer.effectAllowed = 'move'
                event.dataTransfer.setData('itemId', item.id.toString())
            }

            function onDrop(event, categoryId) {
                const itemId = parseInt(event.dataTransfer.getData('itemId'))
                items.value = items.value.map(i => {
                    if (i.id === itemId) {
                        i.categoryId = categoryId
                    }
                    return i
                })
            }

            return {
                items,
                categories,
                onDragStart,
                onDrop
            }
        }
    }
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono&display=swap');

    * {
        font-family: 'JetBrains Mono', monospace;
        text-align: center;
    }

    .droppable {
        margin-top: 1rem;
        padding: 15px;
        border-radius: 5px;
        background: #2c3e50;
    }

    .droppable h4 {
        color: white;
    }

    .draggable {
        background: white;
        padding: 5px;
        border-radius: 5px;
        margin-bottom: 5px;
    }

    .draggable h5 {
        margin: 0;
    }

</style>
