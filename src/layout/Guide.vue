<template>
	<div class="guide">
		<div class="left">
			<h4
				v-for="(guideNav, index) in guideNavList"
				:key="'guideNav' + index"
				@click.stop="handleGuideNav(guideNav)"
			>
				<router-link
					:to="{ name: guideNav.router }"
					:class="{
						active: guideNav.isActive,
					}"
					>{{ guideNav.title }}</router-link
				>
			</h4>
		</div>
		<div class="content">
			<router-view></router-view>
		</div>
	</div>
</template>
<script lang="ts">
import { defineComponent, reactive, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { NavProps } from '../hooks/TypeProps'
import useClickNav from '../hooks/useClickNav'

export default defineComponent({
	name: 'App',
	components: {},
	setup() {
		const route = useRoute()
		const guideNavList = reactive([
			{
				title: '设计原则',
				router: 'design',
				fullPath: '/guide/design',
				isActive: true,
			},
			{
				title: '导航',
				router: 'nav',
				fullPath: '/guide/nav',
				isActive: false,
			},
		])

		onMounted(() => {
			const curNav = useClickNav(route, guideNavList)
			if (curNav) {
				handleGuideNav(curNav)
			}
		})
		const handleGuideNav = (tab: NavProps) => {
			guideNavList.forEach((ele) => {
				if (ele.title === tab.title) {
					ele.isActive = true
				} else {
					ele.isActive = false
				}
			})
		}

		return {
			handleGuideNav,
			guideNavList,
		}
	},
})
</script>

<style lang="scss" scoped>
.guide {
	display: flex;

	.left {
		padding: 30px 0 0 40px;
		width: 200px;
		height: 100vh;
		overflow: scroll;
		position: fixed;

		.active {
			color: #1989fa;
		}
	}

	.content {
		margin-left: 240px;
		padding: 30px 20px;
		flex-grow: 1;
	}

	a {
		text-decoration: none;
		color: #333;
	}
}
</style>