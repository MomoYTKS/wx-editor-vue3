<template>
	<div class="flexBox" style="display: flex">
		<div class="editor-content">
			<div id="title-container">
				<input style="width: 86%" v-model="dataTitle" placeholder="请输入文章标题（2～{{titleMax}}个字）" @input="currentTextInput" />
				<span
					:style="{
						minWidth: '100px',
						fontSize: '16px',
						fontWeight: Current_title_text > titleMax ? 'bold' : '',
						color: Current_title_text > titleMax ? 'red' : '',
					}"
				>
					{{ Current_title_text }} / {{ titleMax }}
				</span>
				<!-- <div class="Picture-Recycle-Bin">
					<img
						src="data:image/png;base64,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"
						alt=""
						:style="{ cursor: deleteImageRecycleBin.length !== 0 ? '' : 'default' }"
					/>
					<div class="Picture-Recycle-Bin-hover" v-if="deleteImageRecycleBin.length !== 0">
						<template v-for="(item, index) in deleteImageRecycleBin" :key="index">
							<div style="margin-bottom: 10px">
								<div
									data-text
									style="width: 100%; display: block; text-align: center; user-select: none"
									contenteditable="false"
									draggable="true"
									@dragstart="handleDragStartImageRecycleBin($event)"
								>
									<div style="position: relative; display: inline-block" class="EditorImgHover" draggable="false">
										<img
											:src="item"
											alt=""
											:style="{ width: '100px', height: '100px', userSelect: 'none', borderRadius: '6px' }"
											draggable="false"
										/>
									</div>
								</div>
							</div>
						</template>
					</div>
				</div> -->
			</div>
			<div style="position: relative">
				<div
					id="content"
					ref="editorContent"
					contenteditable="true"
					@paste="EditorPasteChange"
					@mouseup="handleSelection"
					:style="{ fontSize: textSize }"
					@dragstart="handleDragStart($event)"
					@dragover="handleDragOver($event)"
					@drop="handleDrop($event)"
				>
					<!-- , letterSpacing: '0.1em' -->
					<template v-for="(item, index) in data" :key="index">
						<p
							v-if="item.type === 'text'"
							:style="{ fontSize: textSize, fontWeight: '400', margin: '15px 0', position: 'relative', letterSpacing: '0.1em' }"
						>
							{{ item.content }}
						</p>
						<div
							data-text
							v-else-if="item.type === 'image'"
							style="width: 100%; display: block; text-align: center; user-select: none; margin: 15px 0"
							contenteditable="false"
							draggable="false"
						>
							<div
								style="position: relative; display: inline-block"
								class="EditorImgHover"
								draggable="false"
								@mouseenter="addBoxShadow"
								@mouseleave="removeBoxShoadow"
							>
								<img :src="item.url" alt="" :style="{ width: imgWidth, userSelect: 'none' }" draggable="true" />
								<span v-if="item.imageKey !== 'diasbledImage'" @click="deleteEditorImage($event)" class="ttcore-remove-blot"></span>
							</div>
						</div>
					</template>
				</div>
				<!-- 编辑器工具栏 -->
				<div
					class="toolBar"
					ref="ToolbarNode"
					:style="{
						position: 'absolute',
						zIndex: '100',
						userSelect: 'none',
						display: toolBarStyle.toolBarShowHide,
						top: toolBarStyle.toolBarPositionTop + 'px',
						left: toolBarStyle.toolBarPositionLeft + 'px',
					}"
				>
					<div class="toolbar-inline">
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<div style="display: flex">
									<select v-model="EditorFontFamilyValue" @change="toolbarTextFamily" :disabled="textTitleFlag === 'H1'">
										<!-- <option :value="1">字体1</option>
									<option :value="2">字体2</option>
									<option :value="3">字体3</option> -->
										<option v-for="item in fontLibrary" :key="item.fontName" :value="item.fontValue">{{ item.fontName }}</option>
									</select>
									<a-tooltip placement="top">
										<template #title>
											<span>字体名称</span>
										</template>
										<button type="button" class="toolbar-tool-button" @click="fmClick" :disabled="textTitleFlag === 'H1'">
											<img src="./images/fontFamily.svg" alt="" />
										</button>
									</a-tooltip>
								</div>
							</div>
						</div>
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<div style="display: flex">
									<select v-model="EditorFontSizeValue" @change="toolbarTextSize" :disabled="textTitleFlag === 'H1'">
										<option :value="1">12px</option>
										<option :value="2">13px</option>
										<option :value="3">16px</option>
										<option :value="4">18px</option>
										<option :value="5">24px</option>
										<option :value="6">32px</option>
										<!-- ------------------------------- -->
										<!-- <option value="12px">12px</option>
										<option value="15px">15px</option>
										<option value="17px">17px</option>
										<option value="18px">18px</option>
										<option value="19px">19px</option>
										<option value="20px">20px</option>
										<option value="24px">24px</option>
										<option value="32px">32px</option> -->
									</select>
									<a-tooltip placement="top">
										<template #title>
											<span>字体大小</span>
										</template>
										<button type="button" class="toolbar-tool-button" @click="fsClick" :disabled="textTitleFlag === 'H1'">
											<img src="./images/fontSize.svg" alt="" />
										</button>
									</a-tooltip>
								</div>
							</div>
						</div>
						<div class="toolbar-tool">
							<div class="toolbar-tool-position">
								<a-tooltip placement="top">
									<template #title>
										<span>标题标记</span>
									</template>
									<button type="button" class="toolbar-tool-button" @click="toolbarTextTitle">
										<img src="./images/H.svg" alt="" />
									</button>
								</a-tooltip>
							</div>
						</div>
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<a-tooltip placement="top">
									<template #title>
										<span>字体加粗</span>
									</template>
									<button type="button" class="toolbar-tool-button" @click="toolbarTextBold" :disabled="textTitleFlag === 'H1'">
										<img src="./images/B.svg" alt="" />
									</button>
								</a-tooltip>
							</div>
						</div>
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<a-tooltip placement="top">
									<template #title>
										<span>文字靠左</span>
									</template>
									<button type="button" class="toolbar-tool-button" @click="toolbarTextLeft" :disabled="textTitleFlag === 'H1'">
										<img src="./images/left.svg" alt="" />
									</button>
								</a-tooltip>
							</div>
						</div>
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<a-tooltip placement="top">
									<template #title>
										<span>文字居中</span>
									</template>
									<button type="button" class="toolbar-tool-button" @click="toolbarTextCenter" :disabled="textTitleFlag === 'H1'">
										<img src="./images/center.svg" alt="" />
									</button>
								</a-tooltip>
							</div>
						</div>
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<a-tooltip placement="top">
									<template #title>
										<span>字体背景</span>
									</template>
									<div style="display: flex">
										<button type="button" class="toolbar-tool-button" @click="bgClick" :disabled="textTitleFlag === 'H1'">
											<img src="./images/bgColor.svg" alt="" />
										</button>
										<input
											type="color"
											v-model="bgColor"
											@input="bgColorInput"
											:disabled="textTitleFlag === 'H1'"
											style="padding: 0; border: 0; width: 14px"
										/>
									</div>
								</a-tooltip>
							</div>
						</div>
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<a-tooltip placement="top">
									<template #title>
										<span>字体颜色</span>
									</template>
									<div style="display: flex">
										<button type="button" class="toolbar-tool-button" @click="fontClick" :disabled="textTitleFlag === 'H1'">
											<img src="./images/fontColor.svg" alt="" />
										</button>
										<input
											type="color"
											v-model="fontColor"
											@input="fontColorInput"
											:disabled="textTitleFlag === 'H1'"
											style="padding: 0; border: 0; width: 14px"
										/>
									</div>
								</a-tooltip>
							</div>
						</div>
						<div :class="[textTitleFlag === 'H1' ? 'toolbar-tool-Disabled' : 'toolbar-tool']">
							<div class="toolbar-tool-position">
								<a-tooltip placement="top">
									<template #title>
										<span>清除格式</span>
									</template>
									<button type="button" class="toolbar-tool-button" @click="toolbarTextClear" :disabled="textTitleFlag === 'H1'">
										<img src="./images/clearFormat.svg" alt="" />
									</button>
								</a-tooltip>
							</div>
						</div>
					</div>
				</div>
				<!-- 拖拽指示线 -->
				<div
					class="syl-drop-cursor"
					:style="{
						width: 'calc(100% - 116px)',
						height: '1px',
						backgroundColor: 'rgb(26, 116, 255)',
						pointerEvents: 'none',
						position: 'absolute',
						top: lineTop + 'px',
						left: '50px',
					}"
					v-if="lineTopFlag"
				></div>
			</div>
		</div>
		<!-- 图片回收站 -->
		<div class="Image-recycling">
			<div v-for="(item, index) in RecycleBinImageData" :key="index" style="max-width: 200px; padding: 10px">
				<img
					:src="item.url"
					alt=""
					style="width: 100%; cursor: move; border-radius: 10px"
					draggable="true"
					@dragover="handleDragOver($event)"
					@dragstart="DragStartImage($event)"
				/>
			</div>
		</div>
	</div>
</template>
<script setup >
import { ref, reactive, onMounted, defineProps, defineEmits, defineExpose, nextTick } from "vue";
import { message } from "ant-design-vue";
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

const emits = defineEmits(["getEditorNode", "getEditorHtml"]);

// 标题文字限制
function calculateLength(title) {
	// 使用正则表达式匹配全角字符
	const fullWidthPattern = /[^\x00-\xff]/g;
	const fullWidthMatches = title.match(fullWidthPattern) || [];

	// 使用正则表达式匹配半角字符
	const halfWidthPattern = /[\x00-\xff]/g;
	const halfWidthMatches = title.match(halfWidthPattern) || [];

	// 计算标题长度
	const fullWidthLength = fullWidthMatches.length;
	const halfWidthLength = halfWidthMatches.length / 2;
	const totalLength = fullWidthLength + halfWidthLength;

	return Math.floor(totalLength);
}
const dataTitle = ref(props.title); // 编辑器标题
const Current_title_text = ref(calculateLength(dataTitle.value)); // 当前标题字数

// 检查标题字数
const currentTextInput = () => {
	Current_title_text.value = calculateLength(dataTitle.value);
};

const selected_text = ref(""); // 当前选中文本

const editorContent = ref(null); // 编辑器内容节点
const ToolbarNode = ref(null); // 工具栏节点

// 获取编辑器的Node节点
const editorContentFun = () => {
	// console.log(editorContent.value);
	emits("getEditorNode", editorContent.value);
};
// 获取编辑器的HTML格式
const getEditorHtmlFun = () => {
	emits("getEditorHtml", editorContent.value.innerHTML);
};

defineExpose({
	// 暴露方法
	editorContentFun,
	getEditorHtmlFun,
});

const toolBarStyle = ref({
	toolBarShowHide: "none",
	toolBarPositionTop: 0,
	toolBarPositionLeft: 0,
});

const eventTarget = ref(null);
const leftAndCenterTarget = ref(null);
const filterArr = ref([]);
const currentNodeArr = ref([]);

// 选中事件
const handleSelection = (event) => {
	// ---------------初始化---------------
	imgNode.value = null;
	textAndImg.value = false;
	currentNodeArr.value = [];
	leftAndCenterTarget.value = null;
	// ---------------初始化---------------
	// console.log(editorContent.value.children);
	// console.log(event, "event");
	textTitleFlag.value = event.target.tagName; // 标记标题 / 取消标记
	if (event.target.tagName === "P" || event.target.tagName === "H1") {
		eventTarget.value = event.target;
		leftAndCenterTarget.value = event.target;
	} else if (event.target.tagName === "SPAN") {
		eventTarget.value = event.target.parentElement;
		leftAndCenterTarget.value = event.target.parentElement;
	}
	// console.log(eventTarget.value);
	toolBarStyle.value.toolBarShowHide = "none";
	const selection = window.getSelection();
	// console.log(selection.toString().split("\n"));
	filterArr.value = selection
		.toString()
		.split("\n")
		.filter((item) => item !== "");
	// const range = selection.getRangeAt(0);
	// console.log(ToolbarNode);
	toolBarStyle.value.toolBarPositionTop = event.clientY - 160;
	toolBarStyle.value.toolBarPositionLeft = event.clientX - 367;
	if (toolBarStyle.value.toolBarPositionLeft + 367 > editorContent.value.offsetWidth) {
		toolBarStyle.value.toolBarPositionLeft =
			toolBarStyle.value.toolBarPositionLeft + (editorContent.value.offsetWidth - toolBarStyle.value.toolBarPositionLeft - 367);
	}
	if (toolBarStyle.value.toolBarPositionLeft <= 0) {
		toolBarStyle.value.toolBarPositionLeft = 0;
	}
	// console.log("Top：", toolBarStyle.value.toolBarPositionTop, "Left：", toolBarStyle.value.toolBarPositionLeft);
	// console.log(filterArr.value, "filterArr");
	if (filterArr.value.length > 1) {
		let nodeList = [];
		let currentNode = eventTarget.value;
		nodeList.push(currentNode);
		for (let i = 0; i < filterArr.value.length; i++) {
			console.log(eventTarget.value, "000", filterArr.value[i]);
			if (eventTarget.value.innerText.includes(filterArr.value[i])) {
				if (i === 0) {
					for (let i = 0; i < filterArr.value.length - 1; i++) {
						currentNode = currentNode.nextElementSibling;
						while (true) {
							if (currentNode.tagName === "DIV") {
								currentNode = currentNode.nextElementSibling;
								continue;
							} else {
								break;
							}
						}
						nodeList.push(currentNode);
						// console.log("111", nodeList);
					}
					continue;
				} else {
					for (let i = 0; i < filterArr.value.length - 1; i++) {
						currentNode = currentNode.previousElementSibling;
						while (true) {
							if (currentNode.tagName === "DIV") {
								currentNode = currentNode.previousElementSibling;
								continue;
							} else {
								break;
							}
						}
						nodeList.push(currentNode);
						// console.log("222", nodeList);
					}
					continue;
				}
			}
		}
		currentNodeArr.value = nodeList;
		console.log(nodeList);
	}
	// 连点4下此时为选中但是不出现工具栏
	if (selection.toString().length > 0 && selection.toString() !== "" && selection.toString() !== "\n") {
		if (selected_text.value !== selection.toString()) {
			// ToolbarNode.value.style.display = "block";
			toolBarStyle.value.toolBarShowHide = "block";
			selected_text.value = selection.toString();
			// 当文本被选中时触发的逻辑
			// console.log("文本被选中了！");
		} else {
			selected_text.value = "";
			// 选择了相同文本
			// console.log("选择了相同文本");
		}
	}

	// 连点两下再取消，此时为不选中，但是工具栏不隐藏
	// if (selection.isCollapsed === false) {
	// 	toolBarStyle.value.toolBarShowHide = "block";
	// 	selected_text.value = selection.toString();
	// } else {
	// 	toolBarStyle.value.toolBarShowHide = "none";
	// 	selected_text.value = "";
	// }
};

const EditorFontFamilyValue = ref(props.fontLibrary[0].fontValue);
const fmClick = () => {
	console.log(currentNodeArr.value);
	// 工具栏字体样式
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("fontName", true, "Arial");
		if (currentNodeArr.value.length === 0) {
			selection.anchorNode.parentElement.style.fontFamily = EditorFontFamilyValue.value;
		} else {
			for (let i = 0; i < currentNodeArr.value.length; i++) {
				// currentNodeArr.value[i].children[0].style.fontFamily = EditorFontFamilyValue.value;
				const children = currentNodeArr.value[i].children;
				for (let j = 0; j < children.length; j++) {
					children[j].style.fontFamily = EditorFontFamilyValue.value;
				}
			}
		}
	}
};

const toolbarTextFamily = () => {
	// 工具栏字体样式
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("fontName", true, "Arial");
		if (currentNodeArr.value.length === 0) {
			selection.anchorNode.parentElement.style.fontFamily = EditorFontFamilyValue.value;
		} else {
			for (let i = 0; i < currentNodeArr.value.length; i++) {
				// currentNodeArr.value[i].children[0].style.fontFamily = EditorFontFamilyValue.value;
				const children = currentNodeArr.value[i].children;
				for (let j = 0; j < children.length; j++) {
					children[j].style.fontFamily = EditorFontFamilyValue.value;
				}
			}
		}
		// console.log(currentNodeArr.value);
		// if (EditorFontFamilyValue.value === 1) {
		// 	selection.anchorNode.parentElement.style.fontFamily =
		// 		'Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif';
		// } else if (EditorFontFamilyValue.value === 2) {
		// 	selection.anchorNode.parentElement.style.fontFamily =
		// 		'-apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif';
		// } else if (EditorFontFamilyValue.value === 3) {
		// 	selection.anchorNode.parentElement.style.fontFamily =
		// 		"system-ui,-apple-system,BlinkMacSystemFont,Helvetica Neue,PingFang SC,Hiragino Sans GB,Microsoft YaHei UI,Microsoft YaHei,Arial,sans-serif";
		// }
	}
};

const fsClick = () => {
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("FontSize", true, EditorFontSizeValue.value);
	}
};

const EditorFontSizeValue = ref(3);
if (props.textSize === "12px") {
	EditorFontSizeValue.value = 1;
} else if (props.textSize === "13px") {
	EditorFontSizeValue.value = 2;
} else if (props.textSize === "16px") {
	EditorFontSizeValue.value = 3;
} else if (props.textSize === "18px") {
	EditorFontSizeValue.value = 4;
} else if (props.textSize === "24px") {
	EditorFontSizeValue.value = 5;
} else if (props.textSize === "32px") {
	EditorFontSizeValue.value = 6;
}
const toolbarTextSize = () => {
	// 工具栏字体大小
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("FontSize", true, EditorFontSizeValue.value);
	}
	// -------------------------------------------------------------------
	// const selection = window.getSelection();
	// const range = selection.getRangeAt(0);
	// const selectedText = range.toString();

	// if (selectedText) {
	// 	document.execCommand("styleWithCSS", false, true);
	// 	document.execCommand("insertHTML", false, `<span style="font-size: ${EditorFontSizeValue.value}">${selectedText}</span>`);
	// }
};

const toolbarTextBold = () => {
	// 工具栏加粗按钮
	if (eventTarget.value.tagName === "H1") {
		message.error("文本标记后不能加粗");
	} else {
		document.execCommand("bold");
	}
};

const textTitleFlag = ref("");
const toolbarTextTitle = () => {
	if (ctrlABeforeP.value.length !== 0) {
		if (ctrlABeforeP.value[0].tagName === "P") {
			for (const node of ctrlABeforeP.value) {
				const h1Nodefor = document.createElement("h1");
				h1Nodefor.style.fontSize = "18px";
				h1Nodefor.style.lineHeight = "30px";
				h1Nodefor.style.marginTop = "20px";
				h1Nodefor.style.marginBottom = "12px";
				h1Nodefor.style.fontWeight = "700";
				h1Nodefor.style.position = "relative";
				h1Nodefor.textContent = node.innerText;
				const divArrowfor = document.createElement("div");
				divArrowfor.style.width = "16px";
				divArrowfor.style.height = "20px";
				divArrowfor.style.backgroundImage =
					"url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iMTZweCIgaGVpZ2h0PSIxNnB4IiB2aWV3Qm94PSIwIDAgMTYgMTYiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+CiAgICA8IS0tIEdlbmVyYXRvcjogU2tldGNoIDYwLjEgKDg4MTMzKSAtIGh0dHBzOi8vc2tldGNoLmNvbSAtLT4KICAgIDx0aXRsZT7kuInop5LlvaI8L3RpdGxlPgogICAgPGRlc2M+Q3JlYXRlZCB3aXRoIFNrZXRjaC48L2Rlc2M+CiAgICA8ZyBpZD0i5LiJ6KeS5b2iIiBzdHJva2U9Im5vbmUiIHN0cm9rZS13aWR0aD0iMSIgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj4KICAgICAgICA8cGF0aCBkPSJNNi4zNTc0OTI5Myw0LjQyOTE1NDg4IEwxMC41OTEzMDI1LDExLjQ4NTUwNDIgQzEwLjg3NTQ1MDcsMTEuOTU5MDg0NSAxMC43MjE4ODU2LDEyLjU3MzM0NDggMTAuMjQ4MzA1NCwxMi44NTc0OTI5IEMxMC4wOTI4OSwxMi45NTA3NDIyIDkuOTE1MDUzNTcsMTMgOS43MzM4MDk2MiwxMyBMMS4yNjYxOTAzOCwxMyBDMC43MTM5MDU2MjksMTMgMC4yNjYxOTAzNzksMTIuNTUyMjg0NyAwLjI2NjE5MDM3OSwxMiBDMC4yNjYxOTAzNzksMTEuODE4NzU2MSAwLjMxNTQ0ODIxMiwxMS42NDA5MTk2IDAuNDA4Njk3NDUzLDExLjQ4NTUwNDIgTDQuNjQyNTA3MDcsNC40MjkxNTQ4OCBDNC45MjY2NTUyMywzLjk1NTU3NDYxIDUuNTQwOTE1NDksMy44MDIwMDk1NSA2LjAxNDQ5NTc2LDQuMDg2MTU3NzEgQzYuMTU1MjQ1MzYsNC4xNzA2MDc0NyA2LjI3MzA0MzE2LDQuMjg4NDA1MjcgNi4zNTc0OTI5Myw0LjQyOTE1NDg4IFoiIGZpbGw9IiNGRjVFNUUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDUuNTAwMDAwLCA4LjAwMDAwMCkgcm90YXRlKC0yNzAuMDAwMDAwKSB0cmFuc2xhdGUoLTUuNTAwMDAwLCAtOC4wMDAwMDApICI+PC9wYXRoPgogICAgPC9nPgo8L3N2Zz4=)";
				divArrowfor.style.backgroundRepeat = "no-repeat";
				divArrowfor.style.backgroundSize = "16px 16px";
				divArrowfor.style.backgroundPositionY = "6px";
				divArrowfor.style.position = "absolute";
				divArrowfor.style.top = "0px";
				divArrowfor.style.left = "-20px";
				divArrowfor.style.cursor = "pointer";
				divArrowfor.addEventListener("click", function (event) {
					const pNodeClickfor = document.createElement("p");
					pNodeClickfor.style.fontSize = props.textSize;
					pNodeClickfor.style.margin = "15px 0";
					pNodeClickfor.style.fontWeight = "400";
					pNodeClickfor.style.position = "relative";
					pNodeClickfor.style.letterSpacing = "0.1em";
					pNodeClickfor.textContent = event.target.parentElement.innerText;
					event.target.parentElement.parentElement.replaceChild(pNodeClickfor, event.target.parentElement);
				});
				node.parentElement.replaceChild(h1Nodefor, node);
				h1Nodefor.appendChild(divArrowfor);
			}
			ctrlABeforeP.value = [];
		} else {
			for (const node of ctrlABeforeP.value) {
				const pNodefor = document.createElement("p");
				pNodefor.style.fontSize = props.textSize;
				pNodefor.style.margin = "15px 0";
				pNodefor.style.fontWeight = "400";
				pNodefor.style.position = "relative";
				pNodefor.style.letterSpacing = "0.1em";
				pNodefor.textContent = node.innerText;
				node.parentElement.replaceChild(pNodefor, node);
			}
			ctrlABeforeP.value = [];
		}
	} else {
		// 工具栏标题按钮
		const selection = window.getSelection();
		// console.log(selection.toString());
		if (!selection.isCollapsed) {
			const range = selection.getRangeAt(0);
			const selectedNode = range.commonAncestorContainer;
			if (textTitleFlag.value === "P" || textTitleFlag.value === "SPAN" || textTitleFlag.value === "B") {
				if (selectedNode.parentElement.tagName === "DIV" && selectedNode.parentElement.id === "content") {
					return;
				}
				// P 替换为 H1
				const h1Node = document.createElement("h1");
				h1Node.style.fontSize = "18px";
				h1Node.style.lineHeight = "30px";
				h1Node.style.marginTop = "20px";
				h1Node.style.marginBottom = "12px";
				h1Node.style.fontWeight = "700";
				h1Node.style.position = "relative";
				const divArrow = document.createElement("div");
				divArrow.style.width = "16px";
				divArrow.style.height = "20px";
				divArrow.style.marginRight = "6px";
				divArrow.style.backgroundImage =
					"url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iMTZweCIgaGVpZ2h0PSIxNnB4IiB2aWV3Qm94PSIwIDAgMTYgMTYiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+CiAgICA8IS0tIEdlbmVyYXRvcjogU2tldGNoIDYwLjEgKDg4MTMzKSAtIGh0dHBzOi8vc2tldGNoLmNvbSAtLT4KICAgIDx0aXRsZT7kuInop5LlvaI8L3RpdGxlPgogICAgPGRlc2M+Q3JlYXRlZCB3aXRoIFNrZXRjaC48L2Rlc2M+CiAgICA8ZyBpZD0i5LiJ6KeS5b2iIiBzdHJva2U9Im5vbmUiIHN0cm9rZS13aWR0aD0iMSIgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj4KICAgICAgICA8cGF0aCBkPSJNNi4zNTc0OTI5Myw0LjQyOTE1NDg4IEwxMC41OTEzMDI1LDExLjQ4NTUwNDIgQzEwLjg3NTQ1MDcsMTEuOTU5MDg0NSAxMC43MjE4ODU2LDEyLjU3MzM0NDggMTAuMjQ4MzA1NCwxMi44NTc0OTI5IEMxMC4wOTI4OSwxMi45NTA3NDIyIDkuOTE1MDUzNTcsMTMgOS43MzM4MDk2MiwxMyBMMS4yNjYxOTAzOCwxMyBDMC43MTM5MDU2MjksMTMgMC4yNjYxOTAzNzksMTIuNTUyMjg0NyAwLjI2NjE5MDM3OSwxMiBDMC4yNjYxOTAzNzksMTEuODE4NzU2MSAwLjMxNTQ0ODIxMiwxMS42NDA5MTk2IDAuNDA4Njk3NDUzLDExLjQ4NTUwNDIgTDQuNjQyNTA3MDcsNC40MjkxNTQ4OCBDNC45MjY2NTUyMywzLjk1NTU3NDYxIDUuNTQwOTE1NDksMy44MDIwMDk1NSA2LjAxNDQ5NTc2LDQuMDg2MTU3NzEgQzYuMTU1MjQ1MzYsNC4xNzA2MDc0NyA2LjI3MzA0MzE2LDQuMjg4NDA1MjcgNi4zNTc0OTI5Myw0LjQyOTE1NDg4IFoiIGZpbGw9IiNGRjVFNUUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDUuNTAwMDAwLCA4LjAwMDAwMCkgcm90YXRlKC0yNzAuMDAwMDAwKSB0cmFuc2xhdGUoLTUuNTAwMDAwLCAtOC4wMDAwMDApICI+PC9wYXRoPgogICAgPC9nPgo8L3N2Zz4=)";
				divArrow.style.backgroundRepeat = "no-repeat";
				divArrow.style.backgroundSize = "16px 16px";
				divArrow.style.position = "absolute";
				divArrow.style.top = "5px";
				divArrow.style.left = "-20px";
				divArrow.style.cursor = "pointer";
				divArrow.addEventListener("click", function (event) {
					const pNodeClick = document.createElement("p");
					pNodeClick.style.fontSize = props.textSize;
					pNodeClick.style.margin = "15px 0";
					pNodeClick.style.fontWeight = "400";
					pNodeClick.style.position = "relative";
					pNodeClick.style.letterSpacing = "0.1em";
					pNodeClick.textContent = event.target.parentElement.innerText;
					event.target.parentElement.parentElement.replaceChild(pNodeClick, event.target.parentElement);
				});
				if (eventTarget.value.tagName === "B" || eventTarget.value.tagName === "SPAN") {
					h1Node.textContent = eventTarget.value.parentElement.innerText;
				} else {
					h1Node.textContent = eventTarget.value.innerText;
				}
				if (filterArr.value.length > 1) {
					let nodeList = [];
					let currentNode = eventTarget.value;
					nodeList.push(currentNode);
					for (let i = 0; i < filterArr.value.length; i++) {
						if (h1Node.textContent.includes(filterArr.value[i])) {
							if (i === 0) {
								for (let i = 0; i < filterArr.value.length - 1; i++) {
									// console.log(currentNode.nextElementSibling, "22222");
									currentNode = currentNode.nextElementSibling;
									while (true) {
										if (currentNode.tagName === "DIV") {
											currentNode = currentNode.nextElementSibling;
											continue;
										} else {
											break;
										}
									}
									nodeList.push(currentNode);
								}
								continue;
							} else {
								for (let i = 0; i < filterArr.value.length - 1; i++) {
									// console.log(currentNode.previousElementSibling.tagName, "3333");
									currentNode = currentNode.previousElementSibling;
									while (true) {
										if (currentNode.tagName === "DIV") {
											currentNode = currentNode.previousElementSibling;
											continue;
										} else {
											break;
										}
									}
									nodeList.push(currentNode);
								}
								continue;
							}
						}
					}
					// console.log(nodeList);
					for (const node of nodeList) {
						const h1Nodefor = document.createElement("h1");
						h1Nodefor.style.fontSize = "18px";
						h1Nodefor.style.lineHeight = "30px";
						h1Nodefor.style.marginTop = "20px";
						h1Nodefor.style.marginBottom = "12px";
						h1Nodefor.style.fontWeight = "700";
						h1Nodefor.style.position = "relative";
						h1Nodefor.textContent = node.innerText;
						const divArrowfor = document.createElement("div");
						divArrowfor.style.width = "16px";
						divArrowfor.style.height = "20px";
						divArrowfor.style.backgroundImage =
							"url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iMTZweCIgaGVpZ2h0PSIxNnB4IiB2aWV3Qm94PSIwIDAgMTYgMTYiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+CiAgICA8IS0tIEdlbmVyYXRvcjogU2tldGNoIDYwLjEgKDg4MTMzKSAtIGh0dHBzOi8vc2tldGNoLmNvbSAtLT4KICAgIDx0aXRsZT7kuInop5LlvaI8L3RpdGxlPgogICAgPGRlc2M+Q3JlYXRlZCB3aXRoIFNrZXRjaC48L2Rlc2M+CiAgICA8ZyBpZD0i5LiJ6KeS5b2iIiBzdHJva2U9Im5vbmUiIHN0cm9rZS13aWR0aD0iMSIgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj4KICAgICAgICA8cGF0aCBkPSJNNi4zNTc0OTI5Myw0LjQyOTE1NDg4IEwxMC41OTEzMDI1LDExLjQ4NTUwNDIgQzEwLjg3NTQ1MDcsMTEuOTU5MDg0NSAxMC43MjE4ODU2LDEyLjU3MzM0NDggMTAuMjQ4MzA1NCwxMi44NTc0OTI5IEMxMC4wOTI4OSwxMi45NTA3NDIyIDkuOTE1MDUzNTcsMTMgOS43MzM4MDk2MiwxMyBMMS4yNjYxOTAzOCwxMyBDMC43MTM5MDU2MjksMTMgMC4yNjYxOTAzNzksMTIuNTUyMjg0NyAwLjI2NjE5MDM3OSwxMiBDMC4yNjYxOTAzNzksMTEuODE4NzU2MSAwLjMxNTQ0ODIxMiwxMS42NDA5MTk2IDAuNDA4Njk3NDUzLDExLjQ4NTUwNDIgTDQuNjQyNTA3MDcsNC40MjkxNTQ4OCBDNC45MjY2NTUyMywzLjk1NTU3NDYxIDUuNTQwOTE1NDksMy44MDIwMDk1NSA2LjAxNDQ5NTc2LDQuMDg2MTU3NzEgQzYuMTU1MjQ1MzYsNC4xNzA2MDc0NyA2LjI3MzA0MzE2LDQuMjg4NDA1MjcgNi4zNTc0OTI5Myw0LjQyOTE1NDg4IFoiIGZpbGw9IiNGRjVFNUUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDUuNTAwMDAwLCA4LjAwMDAwMCkgcm90YXRlKC0yNzAuMDAwMDAwKSB0cmFuc2xhdGUoLTUuNTAwMDAwLCAtOC4wMDAwMDApICI+PC9wYXRoPgogICAgPC9nPgo8L3N2Zz4=)";
						divArrowfor.style.backgroundRepeat = "no-repeat";
						divArrowfor.style.backgroundSize = "16px 16px";
						divArrowfor.style.backgroundPositionY = "6px";
						divArrowfor.style.position = "absolute";
						divArrowfor.style.top = "0px";
						divArrowfor.style.left = "-20px";
						divArrowfor.style.cursor = "pointer";
						divArrowfor.addEventListener("click", function (event) {
							const pNodeClickfor = document.createElement("p");
							pNodeClickfor.style.fontSize = props.textSize;
							pNodeClickfor.style.margin = "15px 0";
							pNodeClickfor.style.fontWeight = "400";
							pNodeClickfor.style.position = "relative";
							pNodeClickfor.style.letterSpacing = "0.1em";
							pNodeClickfor.textContent = event.target.parentElement.innerText;
							event.target.parentElement.parentElement.replaceChild(pNodeClickfor, event.target.parentElement);
						});
						node.parentElement.replaceChild(h1Nodefor, node);
						h1Nodefor.appendChild(divArrowfor);
					}
				} else {
					eventTarget.value.parentElement.replaceChild(h1Node, eventTarget.value);
					h1Node.appendChild(divArrow);
				}
			} else if (textTitleFlag.value === "H1") {
				const pNode = document.createElement("p");
				pNode.style.fontSize = props.textSize;
				pNode.style.margin = "15px 0";
				pNode.style.fontWeight = "400";
				pNode.style.position = "relative";
				pNode.style.letterSpacing = "0.1em";
				pNode.textContent = eventTarget.value.innerText;
				if (filterArr.value.length > 1) {
					let nodeList = [];
					let currentNode = eventTarget.value;
					nodeList.push(currentNode);
					for (let i = 0; i < filterArr.value.length; i++) {
						const pNodefor = document.createElement("p");
						pNodefor.style.fontSize = props.textSize;
						pNodefor.style.margin = "15px 0";
						pNodefor.style.fontWeight = "400";
						pNodefor.style.position = "relative";
						pNodefor.style.letterSpacing = "0.1em";
						pNodefor.textContent = eventTarget.value.nextElementSibling.innerText;
						if (pNode.textContent.includes(filterArr.value[i])) {
							if (i === 0) {
								for (let i = 0; i < filterArr.value.length - 1; i++) {
									currentNode = currentNode.nextElementSibling;
									while (true) {
										if (currentNode.tagName === "DIV") {
											currentNode = currentNode.nextElementSibling;
											continue;
										} else {
											break;
										}
									}
									nodeList.push(currentNode);
								}
								continue;
							} else {
								for (let i = 0; i < filterArr.value.length - 1; i++) {
									currentNode = currentNode.previousElementSibling;
									while (true) {
										if (currentNode.tagName === "DIV") {
											currentNode = currentNode.previousElementSibling;
											continue;
										} else {
											break;
										}
									}
									nodeList.push(currentNode);
								}
								continue;
							}
						}
					}
					for (const node of nodeList) {
						const pNodefor = document.createElement("p");
						pNodefor.style.fontSize = props.textSize;
						pNodefor.style.margin = "15px 0";
						pNodefor.style.fontWeight = "400";
						pNodefor.style.position = "relative";
						pNodefor.style.letterSpacing = "0.1em";
						pNodefor.textContent = node.innerText;
						node.parentElement.replaceChild(pNodefor, node);
					}
				} else {
					// console.log(eventTarget.value);
					eventTarget.value.parentElement.replaceChild(pNode, eventTarget.value);
				}
			}
		}
		toolBarStyle.value.toolBarShowHide = "none";
	}
};

const toolbarTextLeft = () => {
	if (ctrlABeforeP.value.length !== 0) {
		if (ctrlABeforeP.value[0].tagName === "P") {
			for (const node of ctrlABeforeP.value) {
				node.style.textAlign = "left";
			}
		}
	} else {
		if (currentNodeArr.value.length === 0) {
			leftAndCenterTarget.value.style.textAlign = "left";
		} else {
			for (const node of currentNodeArr.value) {
				node.style.textAlign = "left";
			}
		}
	}
	// 工具栏文字靠左
	// eventTarget.value.style.textAlign = "left";
};
const toolbarTextCenter = () => {
	if (ctrlABeforeP.value.length !== 0) {
		if (ctrlABeforeP.value[0].tagName === "P") {
			for (const node of ctrlABeforeP.value) {
				node.style.textAlign = "center";
			}
		}
	} else {
		if (currentNodeArr.value.length === 0) {
			leftAndCenterTarget.value.style.textAlign = "center";
		} else {
			for (const node of currentNodeArr.value) {
				node.style.textAlign = "center";
			}
		}
	}
	// 工具栏文字居中
	// eventTarget.value.style.textAlign = "center";
};

const bgColor = ref("#ffffff");
const bgColorInput = (event) => {
	// 工具栏字体背景颜色
	bgColor.value = event.target.value;
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("backColor", false, bgColor.value); // 这里将字体背景设置为黄色，你可以根据需求更换背景色
	}
};

const bgClick = () => {
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("backColor", false, bgColor.value); // 这里将字体背景设置为黄色，你可以根据需求更换背景色
	}
};

const fontClick = () => {
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("foreColor", false, fontColor.value); // 这里将颜色设置为红色，你可以根据需求更换颜色
	}
};

const fontColor = ref("#000000");
const fontColorInput = (event) => {
	// 工具栏字体颜色
	fontColor.value = event.target.value;
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();

	// 修改选中文本的样式
	if (selectedText) {
		document.execCommand("styleWithCSS", false, true);
		document.execCommand("foreColor", false, fontColor.value); // 这里将颜色设置为红色，你可以根据需求更换颜色
	}
};

const toolbarTextClear = () => {
	// 工具栏清除样式
	const selection = window.getSelection();
	const range = selection.getRangeAt(0);

	// 获取选中的文本内容
	const selectedText = range.toString();
	// console.log(selection, range);
	// console.log(selectedText);

	// 检查选中的节点是否是包含特定格式的 <div> 元素
	const currentNode = range.commonAncestorContainer;
	if (currentNode.nodeType === Node.ELEMENT_NODE && currentNode.matches("div[data-text][v-else-if=\"item.type === 'image'\"]")) {
		return; // 如果是特定格式的 <div> 元素，则跳过清除文本格式的步骤
	}

	// 修改选中文本的样式
	if (selectedText) {
		// 切换到使用 CSS 样式的编辑模式
		document.execCommand("styleWithCSS", false, true);
		// 清除文本格式
		document.execCommand("removeFormat", false, null);
	}
};

// const deleteImageRecycleBin = ref([]);
// const deleteImageNodes = ref([]);
// 删除图片
const deleteEditorImage = (event) => {
	// deleteImageRecycleBin.value.push(event.target.parentElement.children[0].currentSrc);
	let removeNode = event.target.parentElement.parentElement;
	// deleteImageNodes.value.push(removeNode);
	props.RecycleBinImageData.push({ url: event.target.parentElement.children[0].currentSrc });
	removeNode.parentElement.removeChild(removeNode);
};

// 粘贴事件
const EditorPasteChange = (event) => {
	setTimeout(() => {
		// console.log(event);
		// 如果是另起的新段落的话
		if (event.target.tagName === "BR") {
			// 判断粘贴的是否为图片
			if (event.target.parentElement.children[0].tagName === "IMG") {
				// console.log("是新起的段落");
				let parentP = event.target.parentElement;
				// 创建 <div> 元素
				let div = document.createElement("div");
				// 创建 <img> 元素
				let img = document.createElement("img");
				// 创建 <div> 内部元素
				let innerDiv = document.createElement("div");
				img.src = event.target.parentElement.children[0].currentSrc;
				img.style.width = props.imgWidth;
				img.style.userSelect = "none";
				img.setAttribute("draggable", "true");
				div.style.width = "100%";
				div.style.display = "block";
				div.style.textAlign = "center";
				div.style.userSelect = "none";
				div.style.margin = "15px 0";
				div.setAttribute("contenteditable", "false");
				div.setAttribute("data-ImageBox", "");
				div.setAttribute("draggable", "true");
				innerDiv.style.position = "relative";
				innerDiv.style.display = "inline-block";
				// innerDiv.style.userSelect = "none";
				innerDiv.style.cursor = "move";
				innerDiv.addEventListener("mouseenter", () => {
					innerDiv.style.boxShadow = "0 0 0 3px #1a74ff";
				});
				innerDiv.addEventListener("mouseleave", () => {
					innerDiv.style.boxShadow = "";
				});
				img.addEventListener("mouseenter", () => {
					spanElement.style.display = "block";
				});
				img.addEventListener("mouseleave", () => {
					spanElement.style.display = "none";
				});
				let spanElement = document.createElement("span");
				spanElement.addEventListener("mouseenter", () => {
					spanElement.style.display = "block";
				});
				spanElement.addEventListener("mouseleave", () => {
					spanElement.style.display = "none";
				});
				spanElement.style.width = "20px";
				spanElement.style.height = "20px";
				spanElement.style.cursor = "pointer";
				spanElement.style.borderRadius = "50%";
				spanElement.style.display = "none";
				spanElement.style.position = "absolute";
				spanElement.style.top = "-10px";
				spanElement.style.right = "-10px";
				spanElement.style.background = 'url("/src/views/Emotional/images/ImageClose.png") no-repeat #999';
				spanElement.style.backgroundSize = "contain";
				spanElement.addEventListener("click", (event) => {
					let removeNode = event.target.parentElement.parentElement;
					removeNode.parentElement.removeChild(removeNode);
				});
				innerDiv.appendChild(img);
				innerDiv.appendChild(spanElement);

				div.appendChild(innerDiv);
				parentP.parentElement.insertBefore(div, parentP);
				parentP.parentElement.removeChild(parentP);
			} else {
				// 是文字
				return;
			}
		} else {
			// 反之是从开头插入图片或者在中间插入图片
			let targetP = event.target;
			// 创建 <div> 元素
			let div = document.createElement("div");
			// 创建 <img> 元素
			let img = document.createElement("img");
			// 创建 <div> 内部元素
			let innerDiv = document.createElement("div");
			let p = document.createElement("p");
			let Base64ImgSrc = targetP.children[0]?.currentSrc; // 暂存的base64
			if (targetP.children[0].tagName === "IMG") {
				if (targetP.children[1] === undefined) {
					// 首部或尾部插入图片
					let selection = window.getSelection();
					let range = selection.getRangeAt(0);
					if (range.startOffset === 1) {
						// 自然段首部插入图片
						// console.log("首部插入");
						targetP.removeChild(targetP.children[0]);

						img.src = Base64ImgSrc;
						img.style.width = props.imgWidth;
						img.style.userSelect = "none";
						img.setAttribute("draggable", "true");
						div.style.width = "100%";
						div.style.display = "block";
						div.style.textAlign = "center";
						div.style.userSelect = "none";
						div.style.margin = "15px 0";
						div.setAttribute("contenteditable", "false");
						div.setAttribute("data-ImageBox", "");
						div.setAttribute("draggable", "false");
						innerDiv.style.position = "relative";
						innerDiv.style.display = "inline-block";
						// innerDiv.style.userSelect = "none";
						innerDiv.style.cursor = "move";
						innerDiv.addEventListener("mouseenter", () => {
							innerDiv.style.boxShadow = "0 0 0 3px #1a74ff";
						});
						innerDiv.addEventListener("mouseleave", () => {
							innerDiv.style.boxShadow = "";
						});
						img.addEventListener("mouseenter", () => {
							// imageMenuDivElement.style.display = "block";
							spanElement.style.display = "block";
						});
						img.addEventListener("mouseleave", () => {
							// imageMenuDivElement.style.display = "none";
							spanElement.style.display = "none";
						});
						let spanElement = document.createElement("span");
						spanElement.addEventListener("mouseenter", () => {
							spanElement.style.display = "block";
						});
						spanElement.addEventListener("mouseleave", () => {
							spanElement.style.display = "none";
						});
						spanElement.style.width = "20px";
						spanElement.style.height = "20px";
						spanElement.style.cursor = "pointer";
						spanElement.style.borderRadius = "50%";
						spanElement.style.display = "none";
						spanElement.style.position = "absolute";
						spanElement.style.top = "-10px";
						spanElement.style.right = "-10px";
						spanElement.style.background = 'url("/src/views/Emotional/images/ImageClose.png") no-repeat #999';
						spanElement.style.backgroundSize = "contain";
						spanElement.addEventListener("click", (event) => {
							let removeNode = event.target.parentElement.parentElement;
							removeNode.parentElement.removeChild(removeNode);
						});
						innerDiv.appendChild(img);
						innerDiv.appendChild(spanElement);
						div.appendChild(innerDiv);
						if (targetP.previousElementSibling === null) {
							message.warning("头部图片请手动添加自然段并进行插入图片");
						} else {
							targetP.previousElementSibling.parentElement.insertBefore(div, targetP);
						}
					} else if (range.startOffset === 2) {
						// 自然段尾部插入图片
						// console.log("尾部插入");
						targetP.removeChild(targetP.children[0]);

						img.src = Base64ImgSrc;
						img.style.width = props.imgWidth;
						img.style.userSelect = "none";
						img.setAttribute("draggable", "true");
						div.style.width = "100%";
						div.style.display = "block";
						div.style.textAlign = "center";
						div.style.userSelect = "none";
						div.style.margin = "15px 0";
						div.setAttribute("contenteditable", "false");
						div.setAttribute("data-ImageBox", "");
						div.setAttribute("draggable", "false");
						innerDiv.style.position = "relative";
						innerDiv.style.display = "inline-block";
						// innerDiv.style.userSelect = "none";
						innerDiv.style.cursor = "move";
						innerDiv.addEventListener("mouseenter", () => {
							innerDiv.style.boxShadow = "0 0 0 3px #1a74ff";
						});
						innerDiv.addEventListener("mouseleave", () => {
							innerDiv.style.boxShadow = "";
						});
						img.addEventListener("mouseenter", () => {
							// imageMenuDivElement.style.display = "block";
							spanElement.style.display = "block";
						});
						img.addEventListener("mouseleave", () => {
							// imageMenuDivElement.style.display = "none";
							spanElement.style.display = "none";
						});
						let spanElement = document.createElement("span");
						spanElement.addEventListener("mouseenter", () => {
							// imageMenuDivElement.style.display = "block";
							spanElement.style.display = "block";
						});
						spanElement.addEventListener("mouseleave", () => {
							// imageMenuDivElement.style.display = "none";
							spanElement.style.display = "none";
						});
						// spanElement.classList.add("ttcore-remove-blot");
						spanElement.style.width = "20px";
						spanElement.style.height = "20px";
						spanElement.style.cursor = "pointer";
						spanElement.style.borderRadius = "50%";
						spanElement.style.display = "none";
						spanElement.style.position = "absolute";
						spanElement.style.top = "-10px";
						spanElement.style.right = "-10px";
						spanElement.style.background = 'url("/src/views/Emotional/images/ImageClose.png") no-repeat #999';
						spanElement.style.backgroundSize = "contain";
						spanElement.addEventListener("click", (event) => {
							let removeNode = event.target.parentElement.parentElement;
							removeNode.parentElement.removeChild(removeNode);
						});
						// imageMenuEventPreventDivElement.appendChild(editorImageMenuItemDivElement);
						// imageMenuDivElement.appendChild(imageMenuEventPreventDivElement);
						innerDiv.appendChild(img);
						// innerDiv.appendChild(imageMenuDivElement);
						innerDiv.appendChild(spanElement);
						div.appendChild(innerDiv);
						targetP.parentElement.insertBefore(div, targetP.nextElementSibling);
					}
				} else {
					// 自然段中间插入图片
					// console.log("中间插入");
					console.log(event.target);
					let TextP = targetP.children[1].innerText;
					targetP.removeChild(targetP.children[1]);
					targetP.removeChild(targetP.firstElementChild);

					p.style.fontSize = props.textSize;
					p.style.fontWeight = "400";
					p.style.margin = "15px 0";
					p.style.position = "relative";
					p.style.letterSpacing = "0.1em";
					p.innerText = TextP;
					img.src = Base64ImgSrc;
					img.style.width = props.imgWidth;
					img.style.userSelect = "none";
					img.setAttribute("draggable", "true");
					div.style.width = "100%";
					div.style.display = "block";
					div.style.textAlign = "center";
					div.style.userSelect = "none";
					div.style.margin = "15px 0";
					div.setAttribute("contenteditable", "false");
					div.setAttribute("data-ImageBox", "");
					div.setAttribute("draggable", "false");
					innerDiv.style.position = "relative";
					innerDiv.style.display = "inline-block";
					// innerDiv.style.userSelect = "none";
					innerDiv.style.cursor = "move";
					innerDiv.addEventListener("mouseenter", () => {
						innerDiv.style.boxShadow = "0 0 0 3px #1a74ff";
					});
					innerDiv.addEventListener("mouseleave", () => {
						innerDiv.style.boxShadow = "";
					});
					img.addEventListener("mouseenter", () => {
						spanElement.style.display = "block";
					});
					img.addEventListener("mouseleave", () => {
						spanElement.style.display = "none";
					});
					let spanElement = document.createElement("span");
					spanElement.addEventListener("mouseenter", () => {
						spanElement.style.display = "block";
					});
					spanElement.addEventListener("mouseleave", () => {
						spanElement.style.display = "none";
					});
					spanElement.style.width = "20px";
					spanElement.style.height = "20px";
					spanElement.style.cursor = "pointer";
					spanElement.style.borderRadius = "50%";
					spanElement.style.display = "none";
					spanElement.style.position = "absolute";
					spanElement.style.top = "-10px";
					spanElement.style.right = "-10px";
					spanElement.style.background = 'url("/src/views/Emotional/images/ImageClose.png") no-repeat #999';
					spanElement.style.backgroundSize = "contain";
					spanElement.addEventListener("click", (event) => {
						let removeNode = event.target.parentElement.parentElement;
						removeNode.parentElement.removeChild(removeNode);
					});
					innerDiv.appendChild(img);
					innerDiv.appendChild(spanElement);
					div.appendChild(innerDiv);
					targetP.parentElement.insertBefore(p, targetP.nextElementSibling);
					targetP.parentElement.insertBefore(div, targetP.nextElementSibling);
				}
			} else {
				return;
			}
		}
	}, 100);
};
// 处理拖拽开始事件
const imgNode = ref(null);
const textDragIng = ref(false); // 拖拽的是个文字
const handleDragStart = (event) => {
	console.log(event, "111");
	if (event.target.tagName === "IMG") {
		// console.log("111", event);
		// console.log(event.target);
		imgNode.value = event.target.parentElement.parentElement;
		textAndImg.value = true;
	} else if (event.target.nodeName === "#text") {
		console.log("是文字");
		textDragIng.value = true;
	}
};
// 处理拖拽进入元素事件
const lineTop = ref(0); // 拖拽指示线距离顶部的位置
const lineTopFlag = ref(false); // 拖拽指示线是否显示
const textAndImg = ref(false); // 拖拽的是个图片
const TopAndBottom = ref(false); // 应该将图片插入节点的上方还是下方
const handleDragOver = (event) => {
	if (textDragIng.value) {
		return;
	} else {
		event.preventDefault();
		// console.log("222", event);
		// console.log(editorContent);
		if ((event.target.tagName === "P" && textAndImg.value) || (event.target.tagName === "H1" && textAndImg.value)) {
			// console.log(event.target.getBoundingClientRect());
			const pRect = event.target.getBoundingClientRect();
			const topDiff = event.clientY - pRect.top;
			const bottomDiff = pRect.bottom - event.clientY;
			lineTopFlag.value = true;
			if (topDiff < bottomDiff) {
				// 鼠标位置更接近p标签的顶部
				// console.log("顶部", Math.floor(pRect.top));
				lineTop.value = Math.floor(pRect.top) - 91; // 86
				TopAndBottom.value = true;
			} else {
				// 鼠标位置更接近p标签的底部
				// console.log("底部", Math.floor(pRect.bottom));
				lineTop.value = Math.floor(pRect.bottom) - 76; // 86
				TopAndBottom.value = false;
			}
			// console.log(event.clientY, "-", event.target.clientHeight, "=", lineTop.value);
		} else if (event.target.tagName === "SPAN") {
			// console.log(event,"spansss");
			const pRect = event.target.parentElement.getBoundingClientRect();
			const topDiff = event.clientY - pRect.top;
			const bottomDiff = pRect.bottom - event.clientY;
			lineTopFlag.value = true;
			if (topDiff < bottomDiff) {
				lineTop.value = Math.floor(pRect.top) - 86;
				TopAndBottom.value = true;
			} else {
				lineTop.value = Math.floor(pRect.bottom) - 86;
				TopAndBottom.value = false;
			}
		} else {
			lineTopFlag.value = false;
		}
	}
};
// 处理放置元素事件
const handleDrop = (event) => {
	console.log(event, "333");
	if (textDragIng.value) {
		textDragIng.value = false;
	} else {
		if (event.target.tagName === "P" || event.target.tagName === "H1") {
			// let width = event.target.clientWidth / 2;
			event.preventDefault();
			if (TopAndBottom.value) {
				event.target.insertAdjacentElement("beforebegin", imgNode.value);
			} else {
				event.target.insertAdjacentElement("afterend", imgNode.value);
			}
			// deleteImageNodes.value.splice(imageIndex.value, 1);
			// deleteImageRecycleBin.value.splice(imageIndex.value, 1);
			if (RecyleBinImageFlag.value) {
				props.RecycleBinImageData.splice(imageIndex.value, 1);
				RecyleBinImageFlag.value = false;
			}
			imageIndex.value = undefined;
			imgNode.value = null;
			lineTopFlag.value = false;
			textAndImg.value = false;
		} else if (event.target.tagName === "SPAN") {
			event.preventDefault();
			if (TopAndBottom.value) {
				event.target.parentElement.insertAdjacentElement("beforebegin", imgNode.value);
			} else {
				event.target.parentElement.insertAdjacentElement("afterend", imgNode.value);
			}
			// deleteImageNodes.value.splice(imageIndex.value, 1);
			// deleteImageRecycleBin.value.splice(imageIndex.value, 1);
			if (RecyleBinImageFlag.value) {
				props.RecycleBinImageData.splice(imageIndex.value, 1);
				RecyleBinImageFlag.value = false;
			}
			imageIndex.value = undefined;
			imgNode.value = null;
			lineTopFlag.value = false;
			textAndImg.value = false;
		}
	}
};
// 回收站图片拖拽
const imageIndex = ref(undefined);
// const handleDragStartImageRecycleBin = (event) => {
// 	// console.log(event);
// 	deleteImageNodes.value.map((item, index) => {
// 		if (item.children[0].children[0].currentSrc === event.target.children[0].children[0].currentSrc) {
// 			imageIndex.value = index;
// 		}
// 	});
// 	if (event.target.tagName === "DIV") {
// 		// imgNode.value = event.target;
// 		imgNode.value = deleteImageNodes.value[imageIndex.value];
// 	}
// };

// 右侧回收站图片拖拽
const RecyleBinImageFlag = ref(false);
const DragStartImage = (event) => {
	// console.log(event);
	if (event.target.tagName === "IMG") {
		textDragIng.value = false;
		// 创建 <div> 元素
		let div = document.createElement("div");
		// 创建 <img> 元素
		let img = document.createElement("img");
		// 创建 <div> 内部元素
		let innerDiv = document.createElement("div");
		img.src = event.target.currentSrc;
		img.style.width = props.imgWidth;
		img.style.userSelect = "none";
		img.setAttribute("draggable", "true");
		div.style.width = "100%";
		div.style.display = "block";
		div.style.textAlign = "center";
		div.style.userSelect = "none";
		div.style.margin = "15px 0";
		div.setAttribute("contenteditable", "false");
		div.setAttribute("data-ImageBox", "");
		div.setAttribute("draggable", "false");
		innerDiv.style.position = "relative";
		innerDiv.style.display = "inline-block";
		// innerDiv.style.userSelect = "none";
		innerDiv.style.cursor = "move";
		innerDiv.addEventListener("mouseenter", () => {
			innerDiv.style.boxShadow = "0 0 0 3px #1a74ff";
		});
		innerDiv.addEventListener("mouseleave", () => {
			innerDiv.style.boxShadow = "";
		});
		img.addEventListener("mouseenter", () => {
			spanElement.style.display = "block";
		});
		img.addEventListener("mouseleave", () => {
			spanElement.style.display = "none";
		});
		let spanElement = document.createElement("span");
		spanElement.addEventListener("mouseenter", () => {
			spanElement.style.display = "block";
		});
		spanElement.addEventListener("mouseleave", () => {
			spanElement.style.display = "none";
		});
		spanElement.style.width = "20px";
		spanElement.style.height = "20px";
		spanElement.style.cursor = "pointer";
		spanElement.style.borderRadius = "50%";
		spanElement.style.display = "none";
		spanElement.style.position = "absolute";
		spanElement.style.top = "-10px";
		spanElement.style.right = "-10px";
		spanElement.style.background = 'url("/src/views/Emotional/images/ImageClose.png") no-repeat #999';
		spanElement.style.backgroundSize = "contain";
		spanElement.addEventListener("click", (event) => {
			props.RecycleBinImageData.push({ url: event.target.parentElement.children[0].currentSrc });
			let removeNode = event.target.parentElement.parentElement;
			removeNode.parentElement.removeChild(removeNode);
		});
		innerDiv.appendChild(img);
		innerDiv.appendChild(spanElement);

		div.appendChild(innerDiv);
		imgNode.value = div;
		props.RecycleBinImageData.map((item, index) => {
			if (item.url === event.target.currentSrc) {
				imageIndex.value = index;
			}
		});
		textAndImg.value = true;
		RecyleBinImageFlag.value = true;
	}
};

// 图片悬浮效果
const addBoxShadow = (event) => {
	event.target.style.boxShadow = "0 0 0 3px #1a74ff";
};
const removeBoxShoadow = (event) => {
	event.target.style.boxShadow = "";
};

// 监听全选事件
const ctrlABeforeP = ref([]);
document.addEventListener("keydown", function (event) {
	if (event.ctrlKey || event.metaKey) {
		// 检查 Ctrl 键或 Command 键是否按下
		if (event.key === "a" || event.key === "A") {
			// 检查按下的键是否是 A
			console.log("Ctrl+A 被按下");
			// 在这里执行你想要的操作
			// console.log(event);

			toolBarStyle.value.toolBarShowHide = "block";
			let pArr = [...event.target.children];
			let filterP = pArr.filter((item) => item.tagName === "P" || item.tagName === "H1");
			ctrlABeforeP.value = filterP;
			console.log(ctrlABeforeP.value);
		}
	}
});
</script>
<style scoped lang='scss'>
.flexBox {
	height: 97vh;
	overflow: hidden;
	.editor-content {
		background-color: #fff;
	}
	.Image-recycling {
		min-width: 200px;
		background-color: #fff;
		overflow-y: auto;
		border-left: 1px solid #e8e8e8;
	}
}
#title-container {
	padding: 20px 50px 20px;
	border-bottom: 1px solid #e8e8e8;
	position: relative;
	display: flex;
	justify-content: space-between;
}
// #title-container .Picture-Recycle-Bin {
// 	position: relative;
// }
// #title-container .Picture-Recycle-Bin img {
// 	width: 24px;
// 	height: 24px;
// }
// #title-container .Picture-Recycle-Bin .Picture-Recycle-Bin-hover {
// 	display: none;
// 	position: absolute;
// 	left: -50px;
// 	top: 24px;
// 	z-index: 1;
// 	box-shadow: 0 6px 16px 0 rgba(0, 0, 0, 0.08), 0 3px 6px -4px rgba(0, 0, 0, 0.12), 0 9px 28px 8px rgba(0, 0, 0, 0.05);
// 	padding: 10px;
// 	border-radius: 8px;
// 	max-height: 430px;
// 	overflow: hidden;
// 	overflow-y: auto;
// 	div:last-child {
// 		margin-bottom: 0px !important;
// 	}
// 	.EditorImgHover {
// 		cursor: move;
// 	}
// }
// #title-container .Picture-Recycle-Bin:hover .Picture-Recycle-Bin-hover {
// 	display: block;
// }

#title-container input {
	font-size: 20px;
	border: 0;
	outline: none;
	width: 100%;
	line-height: 1;
	font-weight: bold;
}

#content {
	padding: 0 50px;
	overflow-y: auto;
	height: 90vh;
	outline: none;
	.EditorImgHover {
		cursor: move;
	}
}
/* 图片 ↓↓↓↓↓ */
#content .EditorImgHover:hover .ttcore-remove-blot {
	display: block;
}

#content .ttcore-remove-blot {
	width: 20px;
	height: 20px;
	background: url("/src/views/Emotional/images/ImageClose.png") no-repeat #999;
	background-size: contain;
	cursor: pointer;
	border-radius: 50%;
	display: none;
	position: absolute;
	top: -10px;
	right: -10px;
}

.toolBar {
	.toolbar-inline {
		margin: 0px;
		width: fit-content;
		white-space: nowrap;
		user-select: none;
		padding: 0px 8px;
		background: rgb(255, 255, 255);
		box-shadow: rgba(0, 0, 0, 0.2) 0px 2px 8px;
		border-radius: 4px;
		transition: transform 0.2s linear 0s;
		transform: scale(1);
		visibility: visible;
		.toolbar-tool {
			margin: 0px;
			display: inline-block;
			position: relative;
			user-select: none;
			cursor: pointer;
			vertical-align: middle;
			margin-right: 12px;
			padding: 8px 0px;
			&:hover .toolbar-tool-position .toolbar-tool-button {
				border-color: #d8d8d8;
				background-color: #f5f5f5;
			}
			.toolbar-tool-position {
				position: relative;
				.toolbar-tool-button {
					padding: 0px;
					width: auto;
					overflow: visible;
					background: transparent;
					vertical-align: middle;
					font-size: 0px;
					line-height: 26px;
					cursor: pointer;
					min-width: 26px;
					min-height: 26px;
					border: 1px solid transparent;
					border-radius: 4px;
					margin: 0px !important;
					img {
						padding: 0px;
						margin: auto;
						display: block;
						font-size: medium;
					}
				}
			}
		}
		.toolbar-tool-Disabled {
			margin: 0px;
			display: inline-block;
			position: relative;
			user-select: none;
			cursor: default;
			opacity: 0.4;
			vertical-align: middle;
			margin-right: 12px;
			padding: 8px 0px;
			.toolbar-tool-position {
				position: relative;
				.toolbar-tool-button {
					padding: 0px;
					width: auto;
					overflow: visible;
					background: transparent;
					vertical-align: middle;
					font-size: 0px;
					line-height: 26px;
					cursor: default;
					min-width: 26px;
					min-height: 26px;
					border: 1px solid transparent;
					border-radius: 4px;
					margin: 0px !important;
					img {
						padding: 0px;
						margin: auto;
						display: block;
						font-size: medium;
					}
				}
			}
		}
	}
}
</style>