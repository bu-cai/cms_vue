<template>
    <div>
        <go-back-header title="商品评论"></go-back-header>
        <comment :commentId="commentId"></comment>
    </div>
</template>
<script>
export default {
    data() {
        return {
            commentId: ''
        }
    }, created() {
        //获取参数
        this.commentId = this.$route.params.pid;
    }
}
</script>
<style>
</style>
