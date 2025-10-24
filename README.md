# VisualMark

[![Unity Version](https://img.shields.io/badge/Unity-2018.4%2B-blue.svg)](https://unity.com/)
[![License](https://img.shields.io/badge/License-BSD--2--Clause-green.svg)](https://opensource.org/licenses/BSD-2-Clause)
[![Package Version](https://img.shields.io/badge/version-1.0.0-orange.svg)](package.json)

**VisualMark** is a powerful Unity package that brings Markdown rendering to Unity's UI Toolkit (UIElements). Create rich, styled documentation, help systems, and text-based UIs directly within Unity using standard Markdown syntax.

## ✨ Features

- 📝 **Full Markdown Support** - Render standard Markdown with headings, lists, code blocks, links, images, and more
- 🎨 **Unity Style Sheets (USS)** - Style your Markdown content with Unity's powerful styling system
- 🔗 **Custom Link Schemes** - Built-in support for Unity asset links and menu commands, plus extensible custom schemes
- 📋 **JSON Front Matter** - Add metadata and configuration to your Markdown files
- 🖼️ **Image Support** - Display local Unity assets and remote images with automatic caching
- 🔄 **Live Updates** - Automatically refreshes content when Markdown files change
- 🎯 **Editor Integration** - Seamlessly integrates with Unity's Editor UI
- 🌙 **Theme Support** - Built-in light and dark theme support
- 🔌 **Extensible Architecture** - Create custom renderers, link handlers, and extensions

## 📋 Requirements

- **Unity 2018.4** or newer
- **UI Toolkit (UIElements)** - Built into Unity 2019.1+ (Experimental in 2018.4)

## 📦 Installation

### Via Unity Package Manager (Recommended)

1. Open Unity's Package Manager (`Window > Package Manager`)
2. Click the `+` button and select **"Add package from git URL..."**
3. Enter: `https://github.com/PassivePicasso/VisualMark.git`

### Via manifest.json

Add the following to your project's `Packages/manifest.json`:

```json
{
  "dependencies": {
    "com.passivepicasso.visualmark": "https://github.com/PassivePicasso/VisualMark.git",
    ...
  }
}
```

### Manual Installation

1. Download or clone this repository
2. Copy the entire folder into your Unity project's `Packages` directory

## 🚀 Quick Start

### Creating a Markdown Element in UXML

```xml
<UXML xmlns:vm="VisualMark.Markdown">
    <vm:MarkdownElement
        data="Assets/Documentation/example.md"
        markdown-data-type="Source"
        empty-line-after-heading="true"
    />
</UXML>
```

### Creating a Markdown Element in C#

```csharp
using VisualMark.Markdown;
using UnityEngine.UIElements;

public class MyEditorWindow : EditorWindow
{
    private void CreateGUI()
    {
        var markdownElement = new MarkdownElement
        {
            Data = "Assets/Documentation/readme.md",
            MarkdownDataType = MarkdownDataType.Source
        };
        
        rootVisualElement.Add(markdownElement);
        markdownElement.RefreshContent();
    }
}
```

### Inline Markdown

```csharp
var markdownElement = new MarkdownElement
{
    Data = "# Hello World\nThis is **bold** and this is *italic*.",
    MarkdownDataType = MarkdownDataType.Text
};
```

## 🎯 Key Features

### Special Link Types

VisualMark extends standard Markdown with Unity-specific link schemes:

**Asset Links** - Link to Unity assets:
```markdown
[Open Prefab](assetlink://Assets/Prefabs/Example.prefab)
[Open by GUID](assetlink://GUID/1234567890abcdef)
```

**Menu Links** - Execute Unity menu commands:
```markdown
[Build Settings](menulink://File/Build Settings...)
[Preferences](menulink://Edit/Preferences...)
```

### JSON Front Matter

Add metadata to your Markdown files:

```markdown
---
{
    "pageStylePath": "Assets/Styles/custom.uss",
    "title": "Documentation",
    "headerClasses": ["page-header"],
    "titleClasses": ["title"],
    "contentUrl": "Assets/Documentation/content.md"
}
---

# Your Content Here
```

### Styling with USS

VisualMark provides extensive styling through Unity Style Sheets:

```css
.header-1 {
    -unity-font-style: bold;
    font-size: 24px;
    margin-bottom: 16px;
    border-bottom-width: 2px;
    border-bottom-color: rgb(128, 128, 128);
}

.code {
    background-color: rgba(128, 128, 128, 0.2);
    border-radius: 4px;
    padding: 4px 8px;
    font-size: 12px;
}
```

## 📚 Documentation

For detailed documentation, including advanced features and API reference:

- **[Getting Started Guide](visualmark-guide.md)** - Comprehensive guide with examples
- **[Third Party Notices](Third%20Party%20Notices.md)** - Open source licenses

### Topics Covered in the Guide

- Basic and advanced usage
- Custom link schemes
- JSON front matter
- Styling and theming
- Dynamic content updates
- Image handling
- Extending VisualMark
- Custom renderers
- Event handling
- API reference

## 🛠️ Extending VisualMark

### Register Custom Link Schemes

```csharp
using VisualMark.Markdown.ObjectRenderers;

LinkInlineRenderer.RegisterScheme("myscheme", link => 
{
    Debug.Log($"Custom link clicked: {link}");
});
```

### Create Custom Renderers

```csharp
using Markdig.Syntax;
using VisualMark.Markdown.ObjectRenderers;

public class CustomBlockRenderer : UIElementObjectRenderer<CustomBlock>
{
    protected override void Write(UIElementRenderer renderer, CustomBlock obj)
    {
        var element = new VisualElement();
        element.AddToClassList("custom-block");
        renderer.Push(element);
        renderer.WriteChildren(obj);
        renderer.Pop();
    }
}
```

## 🤝 Contributing

Contributions are welcome! Whether it's bug reports, feature requests, or code contributions:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project uses components governed by various open source licenses:

- **Markdig** (v18.3) - BSD 2-Clause License
- **Cascadia Code** - SIL Open Font License 1.1
- **Markdown Test File** - MIT License

See [Third Party Notices](Third%20Party%20Notices.md) for complete license information.

## 🙏 Acknowledgments

- Built on the excellent [Markdig](https://github.com/xoofx/markdig) Markdown processor by Alexandre Mutel
- Uses [Cascadia Code](https://github.com/microsoft/cascadia-code) font by Microsoft
- Test files from [markdown-test-file](https://github.com/mxstbr/markdown-test-file) by Max Stoiber

## 📧 Contact

- **Author**: PassivePicasso
- **Repository**: [https://github.com/PassivePicasso/VisualMark](https://github.com/PassivePicasso/VisualMark)

## 🌟 Support

If you find VisualMark useful, please consider:
- ⭐ Starring the repository
- 🐛 Reporting issues
- 💡 Suggesting features
- 🔧 Contributing code

---

**Happy Markdown rendering in Unity!** 🎮📝
