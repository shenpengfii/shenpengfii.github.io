<%*
let list = {
  "mark 文字高亮": "mark",
  "quot 引用": "quot",
  "note 备注块": "note",
  "link 链接": "link",
  "copy 复制行": "copy",
  "checkbox 复选": "checkbox",
  "folding 折叠块": "folding",
  "folders 折叠列表": "folders",
  "timeline 时间轴": "timeline",
  "image 图片": "image",
  "tabs 分页": "tabs"
};

let keys = Object.keys(list);
key = await tp.system.suggester(keys, keys);

let templateOutput = "";
switch(list[key]) {
    case "mark":
        templateOutput = "\n{% mark 高亮文本 %}\n";
        break;
    case "quot":
        templateOutput = "\n{% quot 引用1 %}\n{% quot 引用2 icon:hashtag %}\n{% quot 引用3 icon:default %}\n";
        break;
    case "note":
        templateOutput = "\n{% note [title] content [color:color] %}\n";
        break;
    case "link":
	    templateOutput = "\n{% link link_url 链接标题 %}\n";
	    break;
    case "copy":
        templateOutput = "\n{% copy curl -s https://sh.xaox.cc/install | sh prefix:$ %}\n";
        break;
    case "checkbox":
        templateOutput = "\n{% checkbox checked:true 普通的已勾选的复选框 %}\n";
        break;
    case "folding":
	    templateOutput = "\n{% folding color:green 折叠块标题 %}\n\n折叠块内容\n\n{% endfolding %}\n";
        break;
    case "folders":
	    templateOutput = "\n{% folders %}\n\n<!-- folder 折叠表项标题 -->\n\n折叠表项内容\n\n{% endfolders %}\n";
        break;
	case "timeline":
		templateOutput = "\n{% timeline color:white %}\n\n<!-- node 时间轴节点标题 -->\n\n时间轴节点内容\n\n{% endtimeline %}\n";
        break;
    case "image":
	    templateOutput = "\n{% image 图片url 图片标题 download:下载链接url width:200px padding:16px bg:white %}\n"
	case "tabs":
		templateOutput = "\n{% tabs active:1 %}\n\n<!-- tab 标题 -->\n\n{% endtabs %}\n"
}
if (key)
	return templateOutput;
%>