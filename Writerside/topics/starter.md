# 新的开始

<!--Writerside adds this topic when you create a new documentation project.
You can use it as a sandbox to play with Writerside features, and remove it from the TOC when you don't need it anymore.-->

## 添加新主题
您可以创建空主题，或为不同类型的内容选择一个模板，其中包含一些样板结构以帮助您入门:

![Create new topic options](new_topic_options.png){ width=290 }{border-effect=line}

## 书写内容
%product% 支持两种类型的标记：Markdown 和 XML.
创建新的帮助文章时，您可以在两种主题类型之间进行选择，但这并不意味着您必须坚持单一格式.
您可以在 Markdown 中创作内容并使用语义属性扩展它或注入整个 XML 元素.

## 插入 XML
例如，这就是注入过程的方式:

<procedure title="注入一个过程" id="inject-a-procedure">
    <step>
        <p>开始输入并从完成建议中选择程序类型:</p>
        <img src="completion_procedure.png" alt="completion suggestions for procedure" border-effect="line"/>
    </step>
    <step>
        <p>按下 <shortcut>Tab</shortcut> 或者 <shortcut>Enter</shortcut> 插入标记.</p>
    </step>
</procedure>

## 添加互动元素

### 选项卡
要添加可切换内容，您可以使用选项卡 (通过在新行上输入`tab`来注入它们):

<tabs>
    <tab title="Markdown">
        <code-block lang="plain text">![Alt Text](new_topic_options.png){ width=450 }</code-block>
    </tab>
    <tab title="Semantic markup">
        <code-block lang="xml">
            <![CDATA[<img src="new_topic_options.png" alt="Alt text" width="450px"/>]]></code-block>
    </tab>
</tabs>

### 可折叠块
除了注入整个 XML 元素之外，您还可以使用属性来配置某些元素的行为.
例如，您可以折叠包含非必要信息的章节:

#### 补充信息 {collapsible="true"}
默认情况下，可折叠标题下的内容将折叠,
但您可以通过添加以下属性来修改行为:
`default-state="expanded"`

### 将选择转换为 XML-1
如果需要为某个元素扩展更多功能，可以将选定的内容从 Markdown 转换为语义标记.
例如，如果您想合并表格中的单元格，将其转换为 XML 比在 Markdown 中执行此操作要容易得多.
将插入符号放置在表格中的任意位置并按 <shortcut>Alt+Enter</shortcut>:

<img src="convert_table_to_xml.png" alt="Convert table to XML" width="706" border-effect="line"/>

## 反馈和支持
请向我们报告任何问题、可用性改进或功能请求
<a href="https://youtrack.jetbrains.com/newIssue?project=WRS">YouTrack project</a>
(你需要注册).

欢迎您加入我们
<a href="https://jb.gg/WRS_Slack">public Slack workspace</a>.
在此之前，请阅读我们的 [Code of conduct](https://plugins.jetbrains.com/plugin/20158-writerside/docs/writerside-code-of-conduct.html).
我们假设您在加入之前已阅读并确认.

您也可以随时给我们发电子邮件： [writerside@jetbrains.com](mailto:writerside@jetbrains.com).

<seealso>
    <category ref="wrs">
        <a href="https://plugins.jetbrains.com/plugin/20158-writerside/docs/markup-reference.html">Markup reference</a>
        <a href="https://plugins.jetbrains.com/plugin/20158-writerside/docs/manage-table-of-contents.html">Reorder topics in the TOC</a>
        <a href="https://plugins.jetbrains.com/plugin/20158-writerside/docs/local-build.html">Build and publish</a>
        <a href="https://plugins.jetbrains.com/plugin/20158-writerside/docs/configure-search.html">Configure Search</a>
    </category>
</seealso>