<script setup>
    const {data: beers, error: beerError} = await useFetch('https://api.nuxtjs.dev/beers', { 
        key: "beer",
        transform: (beers) => {
            return beers.map( beer => ({name: beer.name, type: beer.type}))
        }
    });

    const { pending: pendingPosts, data: posts } = await useLazyFetch('https://api.nuxtjs.dev//posts',
        { key: "posts" }   
        // watch(posts, (newPosts) => {
        // // Because posts might start out null, you won't have access
        // // to its contents immediately, but you can watch it.
        // })
    )

    const {data: mountains, pending} = useAsyncData('mountains', 
        () => $fetch('https://api.nuxtjs.dev/mountains'),
        {server: false, lazy: true})

</script>

<template>
    <div>
        <h1 class="text-xl font-medium tracking-wide text-center p-4" >Testing fetching data</h1>  
        <div class="p-4">
            <div class="text-lg font-medium">Beer</div>
            <p class="px-4 py-1" v-if="beerError">{{ beerError }}</p>
            <ul v-else>
                <li class="px-4 py-1" v-for="beer in beers" :key="beer.name">
                {{ beer.name }} {{ beer.type }}
                </li>
            </ul>
        </div>
        <div class="p-4">
            <div class="text-lg font-medium">Posts</div>
            <div v-if="pendingPosts">
                Loading ...
            </div>
            <ul v-else>
                <li class="px-4 py-1" v-for="post in posts" :key="post.title">
                    {{ post.title }}
                </li>
            </ul>
        </div>
        <div class="p-4">
            <div class="text-lg font-medium">Mountains</div>
            <h1 v-if="pending">Pending...</h1>
            <ul v-else>
                <li class="px-4 py-1" v-for="mountain in mountains" :key="mountain.name">
                {{ mountain.title }} - {{ mountain.height }}
                </li>
            </ul>
        </div> 
    </div>
</template>

<style lang="scss" scoped>

</style>