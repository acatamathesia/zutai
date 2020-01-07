<script>
export default {
    name: 'View',
    props: {
        editMode: {
            type: Boolean,
            default: false
        },
        detail: {
            type: Object,
            default: {}
        }
    },
    computed: {
        textAlign: function(){
            if(this.detail.style.textAlign == undefined) {
                return "this.detail.style.textAlign";
            } else {
                return this.detail.style.textAlign;
            }
        },
        lineHeight: function() {
            if(this.detail.style.lineHeight == undefined) {
                return this.detail.style.position.h;
            }
            return this.detail.style.lineHeight;
        }
    },
    data() {
        return {
            
        }
    },
    methods: {
        refreshData(val,sceneName) {

        },
        onResize() {

        },        
    },
    mounted() {
        console.log(`[${new Date().toLocaleTimeString()}] : View is used`)
    }
}
</script>
