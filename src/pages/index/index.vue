<template>
	<view class="content">
		<view class="item head">
			<view class="name">书名</view>
			<view class="author">作者</view>
			<view class="operate">操作</view>
		</view>
		<view
			v-for="(item, index) in list"
			class="item"
			:key="item.id"
		>
			<view class="name" v-if="!item.edit">{{item.text}}</view>
			<input
				confirmtype="text"
				class="name-input" 
				v-if="item.edit"
				v-model="item.newName"
				:class="{'warn': item.nameWarn}"
			/>
			<view class="author" v-if="!item.edit">{{item.authore}}</view>
			<input
				confirmtype="text"
				class="name-input"
				v-if="item.edit"
				v-model="item.newAuthor"
				:class="{'warn': item.authorWarn}"
			/>
			<view class="operate">
				<text class="btn edit" v-if="!item.edit" @tap="handleEdit(item, index)">编辑</text>
				<text class="btn delete" v-if="!item.edit" @tap="handleDelete(item, index)">删除</text>
				<text class="btn edit" v-if="item.edit" @tap="handleConfirm(item, index)">确定</text>
				<text class="btn delete" v-if="item.edit" @tap="handleCancel(item, index)">取消</text>
			</view>
		</view>
		<view @tap="handleAdd">新增</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [
					{
						text: '三国演义',
						id: '1',
						authore: '罗贯中'
					},
					{
						text: '水浒传',
						id: '2',
						authore: '施耐庵'
					},
					{
						text: '红楼梦',
						id: '3',
						authore: '曹雪芹'
					},
					{
						text: '西游记',
						id: '4',
						authore: '吴承恩'
					},
					{
						text: '史记',
						id: '5',
						authore: '司马迁'
					},
					{
						text: '悲惨世界',
						id: '6',
						authore: '雨果'
					},
					{
						text: '呐喊',
						id: '7',
						authore: '鲁迅'
					},
				]
			}
		},
		onLoad() {

		},
		methods: {
			handleEdit(item, index) {
				var list = [...this.list];
				list[index].edit = true;
				list[index].newName = item.text;
				list[index].newAuthor = item.authore;
				this.list = list;
			},
			handleConfirm(item, index) {
				var list = [...this.list];
				var isWarn = false;
				if (!item.newName.trim()) {
					isWarn = true;
					list[index].nameWarn = true;
				}
				if (!item.newAuthor.trim()) {
					isWarn = true;
					list[index].authorWarn = true;
				}
				if (isWarn) {
					this.list = list;
					return;
				}
				list[index].nameWarn = false;
				list[index].authorWarn = false;
				list[index].edit = false;
				list[index].text = item.newName;
				list[index].authore = item.newAuthor;
				delete list[index].isNew;
				this.list = list;
			},
			handleCancel(item, index) {
				if (item.isNew) {
					this.list.splice(index, 1);
					return;
				}
				var list = [...this.list];
				list[index].edit = false;
				this.list = list;
			},
			handleDelete(item, index) {
				this.list.splice(index, 1);
			},
			handleAdd() {
				this.list.push({
					text: '',
					authore: '',
					edit: true,
					newName: '',
					newAuthor: '',
					isNew: true,
				})
			},
		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
	}

	.logo {
		height: 200upx;
		width: 200upx;
		margin-top: 200upx;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}

	.item {
		text-align: left;
		padding: 0 20px;
		height: 30px;
		line-height: 30px;
		display: flex;
		align-items: center;
		font-size: 14rpx;
	}

	.head {
		font-size: 16rpx;
		font-weight: bold;
	}

	.name, .author {
		flex: 1;
	}
	
	.name {
		margin-right: 10px;
	}

	.operate {
		width: 120rpx;
	}

	.delete {
		margin-left: 10px;
	}

	.name-input {
		flex: 1;
		border: 1px solid #000;
		margin-right: 10px;
	}
	
	.warn {
		border-color: red;
	}
</style>
