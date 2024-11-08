<template>
    <div class="col-md-4">
        <div class="character-card" @click="switchCharacter()">
            <div class="card-block">
                <h4 class="card-title">{{ character.name }}</h4>
                <p class="card-text">Height: {{ character.height }}cm</p>
                <p class="card-text">Mass: {{ character.mass }}kg</p>
                <p class="card-text">Hair color: {{ character.hair_color }}</p>
                <p class="card-text">Eye color: {{ character.eye_color }}</p>
            </div>
        </div>
        <button class="btn btn-dark my-2" v-if="id_history.length > 1" @click="goBack()">Go back</button>
    </div>
</template>

<script>
    export default {
        props: ['id'],
        data() {
            return {
                character: {},
                id_history: [],
                current_index: 0 
            }
        },
        methods: {
            fetchCharacter(id) {
                fetch(`https://swapi.dev/api/people/${id}`, {
                    method: 'GET'
                })
                .then(response => response.json())
                .then(json => this.character = json)
            },
            switchCharacter() {
                let random_id
                do
                    random_id = Math.floor(Math.random() * 82) + 1
                while(random_id == 17) // the id 17 returns a error. probably's the API fault
                this.fetchCharacter(random_id)
                this.id_history.push(random_id)
                this.current_index++
            },
            goBack() {
                this.fetchCharacter(this.id_history[--this.current_index])
                this.id_history.pop()
            }
        },
        created() {
            this.fetchCharacter(this.id)
            this.id_history.push(this.id)
        }
    }
</script>