<template>
    <div id="footer">
        <span>Power by {{ author }}，欢迎访问作者博客: <a :href="blog" target="_blank">码瓜博客</a></span>
    </div>
</template>

<script>
export default {
    data() {
        return {
            author: 'Qinhaitao',
            blog: 'https://qht97.com',
        }
    },
}
</script>

<style lang="stylus" scoped>
    #footer {
        margin 40px
        text-align: center
        color: #eee
        font-size 15px
        text-shadow 0 3px 0 2px #eee
    }
        a {
            text-decoration: none;
            color: #eee;
        }
</style>
