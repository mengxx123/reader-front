<template>
    <my-page :title="title" :page="page">
        <ui-raised-button class="file-btn" label="打开 TXT" v-if="!content">
            <input type="file" class="ui-file-button" @change="fileChange($event, 1)">
        </ui-raised-button>
        <pre class="content" :style="contentStyle">{{ content }}</pre>
        <ui-drawer right :open="open" :docked="false" @close="toggle()">
            <ui-appbar title="设置">
                <ui-icon-button icon="close" @click="toggle" slot="left" />
            </ui-appbar>
            <div class="body">
                <ui-text-field type="number" v-model.number="style.fontSize" />
            </div>
        </ui-drawer>
    </my-page>
</template>

<script>
    const IntentEx = window.IntentEx

    export default {
        data () {
            return {
                title: '文本阅读器',
                content: '',
                open: false,
                style: {
                    fontSize: 20
                },
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
        computed: {
            contentStyle() {
                return {
                    'font-size': this.style.fontSize + 'px'
                }
            }
        },
        mounted() {
            this.init()
        },
        methods: {
            init() {
                // text parameter support
                let text = this.$route.query.text
                if (text) {
                    this.content = text
                }
                // url parameter support
                let url = this.$route.query.url
                if (url) {
                    this.loadTextFromUrl(url)
                }
                // eslint-disable-next-line
                new IntentEx(data => {
                    this.content = data
                }, () => {
                    console.log('不支持啊')
                })
                // intent.sendResponse(data)
            },
            setting() {
                this.toggle()
            },
            toggle () {
                this.open = !this.open
            },
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
            },
            loadTextFromUrl(url) {
                this.$http.get(url).then(
                    response => {
                        let data = response.data
                        if (/\.md$/.test(url)) {
                            console.log('markdown')
                        } else {
                            this.content = marked(data)
                        }
                    },
                    response => {
                        console.log(response)
                    })
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
    margin-bottom: 16px;
}
.content {
    font-size: 16px;
    font-family: Helvetica Neue,Helvetica,PingFang SC,Hiragino Sans GB,Microsoft YaHei,微软雅黑,Arial,sans-serif;
}
.body {
    padding: 16px;
}
</style>
