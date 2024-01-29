##### 使用方法

```html
<template>
	<div>
		<Editor
			:data="textData"
			:title="titleText"
			:titleMax="31"
			ref="EditorRef"
			@getEditorNode="EditorNode"
			@getEditorHtml="EditorHtml"
			:imgWidth="'350px'"
			:textSize="'16px'"
			:RecycleBinImageData="RecycleBinImageDataA"
			:fontLibrary="fontArr"
		></Editor>
	</div>
</template>
```

```javascript
<script setup>
import Editor from "@/components/Editor/index.vue";

const EditorRef = ref(null);
const Node = ref(null);

{/* 通过方法一获取Node节点 */}
const EditorNode = (EditorNode) => {
	Node.value = EditorNode;
	// console.log(Node);
};
{/* 通过方法一获取Html格式 */}
const EditorHtml = (EditorHtml) => {
	// console.log(EditorHtml);
};
nextTick(() => {
	// console.log(EditorRef.value);
	EditorRef.value.editorContentFun(); // 通过方法二获取Node节点
	EditorRef.value.getEditorHtmlFun(); // 通过方法二获取Html格式
});

const titleText = ref("2024跨年晚会冲上热搜！她公开道歉表示已尽力，没想到网友不买账");

const fontArr = ref([
	{
		fontName: "字体1",
		fontValue: 'Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif',
	},
	{
		fontName: "字体2",
		fontValue:
			'-apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif',
	},
	{
		fontName: "字体3",
		fontValue: "system-ui,-apple-system,BlinkMacSystemFont,Helvetica Neue,PingFang SC,Hiragino Sans GB,Microsoft YaHei UI,Microsoft YaHei,Arial,sans-serif",
	},
]);

const textData = ref([
	{
		content:
			"在新年钟声敲响之际，跨年晚会无疑是人们心中的一场盛宴。在这个充满欢乐的舞台上，各大卫视、演员、歌手齐聚一堂，为观众献上一场视听盛宴。然而，在这华丽的灯光下，也藏着一些我们不愿看到的现象。例如，在江苏卫视的某场演出中，一首歌曲《让我欢喜让我忧》引发了观众的热议。由孟子义、周华健等人合唱的这首歌原本应该是欢乐的曲调，但因为孟子义的跑调，引发了网友们的一致吐槽。",
		type: "text",
	},
	// {
	// 	url: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTKV7EuytRzh6UL0__x2wWKd8hA8OxJ31wkG84QJ4Wt9Q&s",
	// 	type: "image",
	// 	imageKey: "diasbledImage",
	// },
	{
		content: "这个问题，真的只是孟子义一个人的问题吗？还是说，这只是跨年晚会上常出现的问题？",
		type: "text",
	},
]);
const RecycleBinImageDataA = ref([
	{
		url: "https://t7.baidu.com/it/u=1819248061,230866778&fm=193&f=GIF",
	},
	{
		url: "https://t7.baidu.com/it/u=4036010509,3445021118&fm=193&f=GIF",
	},
]);

{/* -------------------------------------------------------------- */}
const props = defineProps({
	data: {
		// 编辑器数据[{content:""},{content:""}]
		type: Array,
		required: true,
	},
	title: {
		// 编辑器标题
		type: String,
		required: true,
	},
	titleMax: {
		// 编辑器标题最多文字数
		type: Number,
		required: true,
	},
	imgWidth: {
		// 图片宽度
		type: String,
		default: "300px",
	},
	textSize: {
		type: String,
		default: "18px",
	},
	RecycleBinImageData: {
		type: Array,
	},
	fontLibrary: {
		type: Array,
		required: true,
	},
});
</script>
```
