<template>
    <div class="buttonMenu-container">
        <span class="buttonMenu-container-icon" title="Menu" @click="showList($event)">
            <slot name="icon">
                <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
                viewBox="0 0 60 60" xml:space="preserve">
                    <g id="Layer_1">
                        <rect x="25.792" y="8.432" fill="#58595B" width="8.416" height="8.417"/>
                        <rect x="25.792" y="25.182" fill="#58595B" width="8.416" height="8.416"/>
                        <rect x="25.792" y="42.016" fill="#58595B" width="8.416" height="8.416"/>
                    </g>
                </svg>
            </slot>
            <div class="v-menu-button-cover"></div>
        </span>
        <div class="buttonMenu-dropdown">
            <div v-for="(option, index) in options" :key="index" class="buttonMenu-dropdown__item" :class="[option.class ? option.class : '',{'disabled': option.disabled ? true : false}]" @click="selectOption(option)">
                {{label ? option[label] : option}}
            </div>
        </div>
    </div>
</template>
<script>

export default {

    name: 'buttonMenu',
    props:{

        options:{
            type: Array,
            default: () => [],
        },

        label:{
            type: String,
            default: '',
        },

    },
    methods:{

        selectOption(option){

            if(option.callback){

                this.$emit(option.callback)
                return;

            }
            
            this.$emit('selected', option)

        },

        showList(e){

            let lists = document.querySelectorAll('.buttonMenu-dropdown');

                if(lists){
                    lists.forEach(element => {
                        element.classList.remove('active')
                    });
                }

           let list = e.target.parentElement.parentElement;
           if(list.children[1].classList.contains('buttonMenu-dropdown')){
            list.children[1].classList.add('active');
            console.log(list.children[1].classList)
           }
           

        },

        close(e){

            if(!e.target.classList.contains('buttonMenu') && !e.target.classList.contains('v-menu-button-cover') && !e.target.classList.contains('buttonMenu-dropdown')){

                let lists = document.querySelectorAll('.buttonMenu-dropdown');

                if(lists){
                    lists.forEach(element => {
                        element.classList.remove('active')
                    });
                }

            }

        },

    },
    mounted(){

        document.addEventListener('click', this.close)

    }

}

</script>
<style scoped>

.buttonMenu-container{
    position:relative;
    width:20px;
    height: 20px;
}

.buttonMenu-container-icon{
    position:relative;
    width:20px;
    height:20px;
    cursor:pointer;
}

.v-menu-button-cover{
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    background:transparent;
}

.buttonMenu-container-icon svg{
    width:20px;
    height:20px;
}

.buttonMenu{
    display:flex;
    position:relative;
    width:100%;
    min-height: 32px;
    cursor: default;
}

.buttonMenu-dropdown{
    position:absolute;
    width:auto;
    min-width: 100%;
    max-height: 300px;
    overflow: hidden;
    overflow-y: auto;
    word-break: keep-all;
    right: 0px;
    z-index: 1;
    border:1px rgb(192, 192, 192) solid;
    background:#fff;
    display:none;
    box-shadow: 0px 0px 8px 0px #d1d1d1;
    cursor: default;
    text-align: left;
}

.buttonMenu-dropdown.active{
    display:block !important;
}

.buttonMenu-dropdown__item{
    padding:10px 5px;
}

.buttonMenu-dropdown__item.disabled{
    pointer-events: none;
    opacity:0.5;
    transition: all 0.2s;
}

.buttonMenu-dropdown__item:hover{
    background:rgb(233, 233, 233);
}

</style>