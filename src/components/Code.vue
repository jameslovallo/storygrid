<template>
	<div class="code" v-editable="blok">
		<v-style>{{ blok.css }}</v-style>
		<div v-html="blok.html" />
	</div>
</template>

<script>
export default {
	props: ["blok"],
	methods: {
		init() {
			if (this.blok.javascript) {
				const st = document.createElement("script");
				st.type = "module";
				st.innerHTML = this.blok.javascript;
				document.body.appendChild(st);
			}
		},
	},
	mounted() {
		if (!this.blok.external_js_1) this.init();
	},
	metaInfo() {
		const b = this.blok;
		const js = [b.external_js_1, b.external_js_2, b.external_js_3];
		const css = [b.external_css_1, b.external_css_2, b.external_css_3];
		let scripts = [];
		let links = [];
		js.forEach((url) => {
			if (url)
				scripts.push({
					hid: url,
					src: url,
					callback: this.init,
					defer: true,
				});
		});
		css.forEach((url) => {
			if (url)
				links.push({
					hid: url,
					href: url,
					rel: "stylesheet",
					type: "text/css",
				});
		});
		return {
			link: links,
			script: scripts,
		};
	},
};
</script>
