<script>
export default {
    data: () => ({
        background: '',
        quote: {
            content: '',
            author: ''
        },
        loading: false
    }),
    methods: {
        async getRandomQuote() {
            const response = await fetch('https://api.quotable.io/random');
            const json = await response.json();
            return {content: json.content, author: json.author};
        },
        async getRandomBackground() {
            const response = await fetch('https://api.pexels.com/v1/curated?per_page=1', {
                headers: {
                    'Authorization': '563492ad6f9170000100000116c8c28b372747eab3379eacf4c6e8ff'
                }
            });
            const json = await response.json();
            return json.photos[0].src.original;
        }
    },
    async mounted() {
        this.loading = true;
        this.quote = await this.getRandomQuote();
        this.background = await this.getRandomBackground();
        this.loading = false;
    }
};
</script>

<template>
    <div class="wrapper" :style="{'background-image': `url(${background})`}" v-if="!loading">
        <div class="content">{{quote.content}}</div>
        <div class="author">- {{quote.author}}</div>
    </div>
</template>

<style>
body {
    margin: 0;
}

div {
    box-sizing: border-box;
}
.wrapper {
    min-height: 100vh;
    background-size: cover;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
    text-shadow: 0 0 4px black;
    font-size: 10vw;
    font-weight: bold;
    text-align: center;
}
.content {
    text-align: center;
    width: 100%;
    padding: 20px;
}
.author {
    text-align: right;
    width: 100%;
    padding: 20px;
}
</style>
