<template>
    <div class="p-6 w-1/3 mx-auto">
       <div class="mb-4">
            {{ user.name }}
        </div>

        <div class="mb-4">
            <a @click.prevent="sendLikes" href="#" class="rounded-lg block w-48 bg-sky-400 text-white text-center py-2">Send Like</a>
        </div>
  
        <div v-if="like_str" class="mb-4">
            <h3>{{ like_str }}</h3>
        </div>
    </div>
</template>

<script>
export default {
    name: "Show",

    props: [
        'user'
    ],
    data() {
        return {
            like_str: ''
        }
    },
    created() {

        // window.Echo.channel(`send_like_${this.$page.props.auth.user.id}`)
        window.Echo.private(`send_like_${this.$page.props.auth.user.id}`)
        .listen('.send-like', res => {
            this.like_str = res.like_str;
        })

    },
    methods: {
        sendLikes() {
            try {
                axios.post(`/users/${this.user.id}`, {from_id: this.$page.props.auth.user.id})
                .then((response) => { 

                    this.like_str = response.data.like_str 
                })
            } catch (error) {
            console.log(error);
            }
        }
    }
}
</script>