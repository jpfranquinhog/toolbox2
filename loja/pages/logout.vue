<script>
    import axios from "axios";

    export default {
        methods:{
            logout(){
                sessionStorage.clear();
                window.location.href = '/login';
            }
        },
        created(){
            this.logout()
        },
    }
</script>