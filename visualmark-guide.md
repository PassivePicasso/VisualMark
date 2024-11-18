# VisualMark Getting Started Guide

## Overview
VisualMark is a Unity package that enables rendering Markdown content in Unity's UI system using UI Elements (UIElements/UI Toolkit). This guide will walk you through setting up and using VisualMark in your Unity project.

## Installation

1. Add the package to your project:
   ```json
   {
     "dependencies": {
       "com.passivepicasso.visualmark": "1.0.0"
     }
   }
   ```

## Basic Usage

### 1. Creating a Markdown Element in UXML

VisualMark can be used directly in UXML files. Here's the basic syntax:

```xml
<UXML xmlns:vm="VisualMark.Markdown">
    <vm:MarkdownElement
        data="Assets/Documentation/example.md"
        markdown-data-type="Source"
        empty-line-after-code-block="true"
        empty-line-after-heading="true"
        empty-line-after-thematic-break="true"
        expand-auto-links="true"
        list-item-character="*"
        space-after-quote-block="true"
    />
</UXML>
```

### 2. Creating a Markdown Element in C#

You can also create MarkdownElement instances programmatically:

```csharp
using VisualMark.Markdown;

public class MarkdownView : VisualElement
{
    public MarkdownView()
    {
        var markdownElement = new MarkdownElement
        {
            Data = "Assets/Documentation/example.md",
            MarkdownDataType = MarkdownDataType.Source,
            EmptyLineAfterCodeBlock = true,
            EmptyLineAfterHeading = true,
            ListItemCharacter = "*"
        };
        
        Add(markdownElement);
        markdownElement.RefreshContent();
    }
}
```

## Advanced Features

### 1. Custom Link Types

VisualMark supports several special link types:

#### Asset Links
Links to Unity assets using the `assetlink://` scheme:

```markdown
[Open Prefab](assetlink://Assets/Prefabs/Example.prefab)
[Open Asset by GUID](assetlink://GUID/1234567890abcdef1234567890abcdef)
```

#### Menu Links
Execute Unity menu items using the `menulink://` scheme:

```markdown
[Build Project](menulink://File/Build Settings...)
[Open Scene](menulink://File/Open Scene)
```

### 2. JSON Front Matter

Add metadata to your Markdown files using JSON front matter:

```markdown
---
{
    "pageStylePath": "Assets/UI/Styles/custom.uss",
    "title": "Documentation",
    "headerClasses": ["page-header", "documentation"],
    "titleClasses": ["title", "large"],
    "iconClasses": ["icon", "doc-icon"],
    "contentUrl": "Assets/Documentation/content.md"
}
---

# Main Content
```

### 3. Styling

VisualMark provides extensive styling capabilities through USS (Unity Style Sheets):

```css
.header-1 {
    border-bottom-width: 1px;
    border-bottom-color: rgb(0, 0, 0);
    margin-bottom: 16px;
    margin-top: 16px;
    padding-bottom: 10px;
    flex-direction: row;
    -unity-font-style: bold;
}

.code {
    font-size: 12px;
    background-color: rgba(128, 128, 128, 0.5);
    border-radius: 4px;
    padding: 7px 14px;
    margin: 4px 0;
}
```

### 4. Dynamic Content Updates

VisualMark automatically monitors Markdown files for changes:

```csharp
public class MarkdownViewController : VisualElement
{
    private MarkdownElement markdownElement;

    public MarkdownViewController()
    {
        markdownElement = new MarkdownElement();
        markdownElement.Data = "Assets/Documentation/live-update.md";
        markdownElement.MarkdownDataType = MarkdownDataType.Source;
        Add(markdownElement);
        
        // Content will automatically update when the file changes
        markdownElement.RefreshContent();
    }
}
```

### 5. Working with Images

VisualMark handles both local and remote images with automatic caching:

```markdown
![Local Image](Assets/Images/logo.png)
![Remote Image](https://example.com/image.png)
```

For programmatic image handling:

```csharp
using VisualMark.Markdown.Helpers;

public class ImageHandler : VisualElement
{
    public void AddImage(string url)
    {
        var image = ImageElementFactory.GetImageElement(url, "custom-image-class");
        Add(image);
    }
}
```

## CSS Utility Classes

VisualMark provides numerous utility classes for styling:

```css
/* Width utilities */
.fw100 { width: 100px; }
.fw150 { width: 150px; }
.fw200 { width: 200px; }

/* Margin utilities */
.m0 { margin: 0px; }
.m1 { margin: 1px; }
.m2 { margin: 2px; }
.m4 { margin: 4px; }

/* Padding utilities */
.p0 { padding: 0px; }
.p1 { padding: 1px; }
.p2 { padding: 2px; }
.p4 { padding: 4px; }

/* Border utilities */
.b0 { border-width: 0px; }
.b1 { border-width: 1px; }
.b2 { border-width: 2px; }
.b4 { border-width: 4px; }
```

## Best Practices

1. **File Organization**
   - Keep Markdown files in a dedicated Documentation folder
   - Use relative paths within your project structure
   - Organize complex documentation with front matter

2. **Performance**
   - Use local assets when possible
   - Leverage the image caching system for remote images
   - Avoid unnecessary refreshes of MarkdownElement content

3. **Styling**
   - Create a consistent styling scheme using USS
   - Utilize the provided utility classes
   - Keep custom styles in separate USS files

4. **Content Updates**
   - Let VisualMark handle file watching automatically
   - Use RefreshContent() when programmatically updating content
   - Consider using contentUrl in front matter for modular documentation

## Common Issues and Solutions

1. **Missing Content**
   - Verify file paths are correct and relative to the Assets folder
   - Check MarkdownDataType is set correctly
   - Ensure RefreshContent() is called after setting Data

2. **Styling Issues**
   - Confirm USS files are properly referenced
   - Check class names match exactly
   - Verify style sheet loading order

3. **Image Loading**
   - Verify image paths are correct
   - Check Unity's internet access settings for remote images
   - Ensure image cache directory exists

## Extending VisualMark

### Custom Link Schemes

VisualMark allows you to register custom link schemes for special handling:

```csharp
using VisualMark.Markdown.ObjectRenderers;

public class CustomLinkHandler : MonoBehaviour
{
    void RegisterCustomSchemes()
    {
        // Basic link handler
        LinkInlineRenderer.RegisterScheme("customlink", link => 
        {
            Debug.Log($"Handling custom link: {link}");
            // Handle the link click
        });

        // Advanced link handler with UI preprocessing
        LinkInlineRenderer.RegisterScheme(
            "advancedlink",
            // Link click handler
            link => 
            {
                Debug.Log($"Handling advanced link: {link}");
            },
            // UI preprocessor
            label => 
            {
                var container = new VisualElement();
                
                // Add custom icon
                var icon = new Image();
                icon.AddToClassList("custom-icon");
                
                container.Add(icon);
                container.Add(label);
                
                return container;
            }
        );
    }
}
```

Usage in Markdown:
```markdown
[Custom Link](customlink://some/path)
[Advanced Link](advancedlink://some/path)
```

### Custom Renderers

You can create custom renderers for new Markdown elements or override existing ones:

```csharp
using Markdig.Syntax;
using VisualMark.Markdown.ObjectRenderers;

// Create a custom block type
public class CustomBlock : ContainerBlock
{
    public CustomBlock() : base() { }
}

// Create a renderer for the custom block
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

// Register the custom renderer
public class CustomMarkdownElement : MarkdownElement
{
    protected override void LoadRenderers()
    {
        base.LoadRenderers();
        ObjectRenderers.Add(new CustomBlockRenderer());
    }
}
```

### Custom Front Matter Handlers

Extend the JSON front matter functionality with custom handlers:

```csharp
using VisualMark.Markdown.Extensions.Json;

public class CustomFrontMatterRenderer : JsonFrontMatterRenderer
{
    public struct CustomFrontMatter : FrontMatter
    {
        public string customField;
        public CustomData[] customDataArray;
    }

    protected override void Write(UIElementRenderer renderer, JsonFrontMatterBlock frontMatterBlock)
    {
        try
        {
            var json = frontMatterBlock.Lines.ToString().Trim();
            var frontMatter = JsonUtility.FromJson<CustomFrontMatter>(json);

            // Create custom UI based on front matter
            var container = new VisualElement();
            renderer.Push(container);

            if (!string.IsNullOrEmpty(frontMatter.customField))
            {
                var customElement = new Label(frontMatter.customField);
                renderer.WriteElement(customElement);
            }

            // Handle custom data array
            foreach (var data in frontMatter.customDataArray)
            {
                // Process custom data
            }

            // Continue with base front matter processing
            base.Write(renderer, frontMatterBlock);
        }
        catch (Exception e)
        {
            Debug.LogError(e);
            renderer.WriteElement(new Label(e.Message));
        }
    }
}
```

Usage in Markdown:
```markdown
---
{
    "title": "Custom Front Matter",
    "customField": "Custom Value",
    "customDataArray": [
        { "id": 1, "value": "First" },
        { "id": 2, "value": "Second" }
    ]
}
---
```

### Custom Style Sheet Extensions

Create custom USS extensions for specialized styling:

```csharp
public class CustomMarkdownStyles : MarkdownElement
{
    public CustomMarkdownStyles()
    {
        // Add custom style sheets
        AddSheet("Assets/CustomStyles/custom-markdown.uss");
        
        // Add conditional styles
        if (EditorGUIUtility.isProSkin)
        {
            AddSheet("Assets/CustomStyles/custom-markdown-dark.uss");
        }
        
        // Add version-specific styles
        #if UNITY_2021_1_OR_NEWER
        AddSheet("Assets/CustomStyles/custom-markdown-2021.uss");
        #endif
    }
}
```

### Event Handling System

Implement custom event handling for markdown elements:

```csharp
public class InteractiveMarkdownElement : MarkdownElement
{
    public class MarkdownInteractionEvent : EventBase<MarkdownInteractionEvent>
    {
        public string ElementType { get; set; }
        public string ElementContent { get; set; }
    }

    public InteractiveMarkdownElement()
    {
        // Register for various interaction events
        this.RegisterCallback<MarkdownInteractionEvent>(OnMarkdownInteraction);
        
        // Add custom mouse handling
        this.RegisterCallback<MouseUpEvent>(evt =>
        {
            var target = evt.target as VisualElement;
            if (target?.ClassListContains("custom-interactive") ?? false)
            {
                var interactionEvent = MarkdownInteractionEvent.GetPooled();
                interactionEvent.ElementType = "custom";
                interactionEvent.ElementContent = target.userData as string;
                target.SendEvent(interactionEvent);
            }
        });
    }

    private void OnMarkdownInteraction(MarkdownInteractionEvent evt)
    {
        Debug.Log($"Interaction with {evt.ElementType}: {evt.ElementContent}");
    }
}
```

## API Reference

### MarkdownElement Properties

```csharp
public class MarkdownElement
{
    public string Data { get; set; }                    // Source data or direct markdown content
    public MarkdownDataType MarkdownDataType { get; set; } // Implicit, Source, or Text
    public bool SpaceAfterQuoteBlock { get; set; }      // Add space after quote blocks
    public bool EmptyLineAfterCodeBlock { get; set; }   // Add empty line after code blocks
    public bool EmptyLineAfterHeading { get; set; }     // Add empty line after headings
    public bool EmptyLineAfterThematicBreak { get; set; } // Add empty line after thematic breaks
    public string ListItemCharacter { get; set; }       // Character used for list items
    public bool ExpandAutoLinks { get; set; }           // Automatically expand URLs into links
    
    public void RefreshContent();                       // Refresh markdown content
    public void AddSheet(string templatePath, string modifier = null); // Add USS stylesheet
}
```

### Front Matter Structure

```csharp
public struct FrontMatter
{
    public string pageStylePath;      // Path to USS file
    public string[] headerClasses;    // Classes for header container
    public string title;              // Page title
    public string[] titleClasses;     // Classes for title label
    public string[] iconClasses;      // Classes for header icon
    public string contentUrl;         // Path to additional content
}
```
