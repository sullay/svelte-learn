<style>
h1{
	text-align: center;
}
img{
	width: 100%;
}
.html :global(h1){
	color: chartreuse;
}
</style>

<script>
	import Sullay from '../components/sullay.svelte'
	let name = 'world';
	let src ="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570390818923&di=1b477739d17e14e5d8ee39b19cb40748&imgtype=0&src=http%3A%2F%2Fimg.zcool.cn%2Fcommunity%2F012e3b5954ee3da8012193a387c830.png%401280w_1l_2o_100sh.png"
	let html="<h1>I am sullay!</h1>"
</script>

<h1>Hello {name}!</h1>
<h1>Hello {name.toUpperCase()}!</h1>
<img {src} alt="Hello {name}!">
<Sullay/>
<div class="html">{@html html}</div>