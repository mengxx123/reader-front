<template>
    <my-page :title="title" :page="page">
        <ui-raised-button class="file-btn" label="打开 TXT">
            <input type="file" class="ui-file-button" @change="fileChange($event, 1)">
        </ui-raised-button>
        <pre>{{ content }}</pre>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
                title: '文本阅读器',
                content: '',
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'settings',
                            click: this.setting,
                            title: '设置'
                        },
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help',
                            title: '帮助'
                        }
                    ]
                }
            }
        },
        methods: {
            setting() {},
            fileChange(e) {
                let file = e.target.files[0]
                this.title = file.name
                // if (left === 1) {
                //     var f_name = file.name;
                //     var f_type = f_name.substring(f_name.lastIndexOf("."))
                // }
                // _this.result = {}
                let reader = new FileReader()
                reader.onload = e => {
                    let content = e.target.result
                    this.content = content.toString()
                    console.log(content)
                }
                reader.readAsText(file, 'utf-8')
            }
        }
    }
</script>

<style scoped>
.ui-file-button{
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    opacity: 0;
}
</style>
