---
title: .NET MAUI Community Toolkit - Markup
author: bijington
description: C# Markup is a set of fluent helper methods and classes designed to simplify the process of building declarative .NET Multi-platform App UI (.NET MAUI) user interfaces in code.
ms.date: 02/13/2022
---

# C# Markup

## Overview

C# Markup is a set of fluent helper methods and classes designed to simplify the process of building declarative .NET Multi-platform App UI (.NET MAUI) user interfaces in code. The fluent API provided by C# Markup is available in the `CommunityToolkit.Maui.Markup` namespace.

[!INCLUDE [docs under construction](../includes/preview-note.md)]

Just as with XAML, C# Markup enables a clean separation between UI (View) and Business Logic (View Model).

C# Markup is available on all platforms supported by .NET MAUI.

## NuGet package

The C# Markup package can be included in your project(s) as decribed in our [Getting started](../get-started.md#communitytoolkitmauimarkup) guide.

## Examples

Here are some brief examples showing how common tasks can be achieved through the use of the Markup package.

### Bindings

First let's take a look at how a Binding could be defined without the Markup package:

```csharp
var entry = new Entry();
entry.SetBinding(Entry.TextProperty, new Binding(nameof(ViewModel.RegistrationCode));
```

Markup allows us to define the binding fluently and therefore chain multiple methods together to reduce the verbosity of our code:

```csharp
new Entry().Bind(Entry.TextProperty, nameof(ViewModel.RegistrationCode))
```

For further details on the possible options for the `Bind` method refer to the [`BindableObject` extensions documentation](extensions/bindable-object-extensions.md).

### Sizing

First let's take a look at how an `Entry` could be sized without the Markup package:

```csharp
var entry = new Entry();
entry.WidthRequest = 200;
entry.HeightRequest = 40;
```

Markup allows us to define the sizing fluently and therefore chain multiple methods together to reduce the verbosity of our code:

```csharp
new Entry().Size(200, 40);
```

For further details on the possible options for the `Bind` method refer to the [`VisualElement` extensions documentation](extensions/visual-element-extensions.md).

### In-depth example

The following example shows setting the page content to a new `Grid` containing a `Label` and an `Entry`, in C#:

```csharp
class SampleContentPage : ContentPage
{
    public SampleContentPage()
    {
        Grid grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = new GridLength(36, GridUnitType.Absolute) }
            },

            ColumnDefinitions =
            {
                new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star) },
                new ColumnDefinition { Width = new GridLength(2, GridUnitType.Star) }
            }
        }

        Label label = new Label { Text = "Code: " };
        grid.Children.Add(label);
        GridLayout.SetColumn(label, 0);
        GridLayout.SetRow(label, 0);

        Entry entry = new Entry
        {
            Placeholder = "Enter number",
            Keyboard = Keyboard.Numeric,
            BackgroundColor = Colors.AliceBlue,
            TextColor = Colors.Black,
            FontSize = 15,
            HeightRequest = 44,
            Margin = new Thickness(5)
        };
        grid.Children.Add(entry);
        GridLayout.SetColumn(label, 1);
        GridLayout.SetRow(label, 0);
        entry.SetBinding(Entry.TextProperty, new Binding(nameof(ViewModel.RegistrationCode));

        Content = grid;
    }
}
```

This example creates a `Grid` object, with child `Label` and `Entry` objects. The `Label` displays text, and the `Entry` data binds to the `RegistrationCode` property of the viewmodel. Each child view is set to appear in a specific row in the `Grid`, and the `Entry` spans all the columns in the `Grid`. In addition, the height of the `Entry` is set, along with its keyboard, colors, the font size of its text, and its `Margin`. Finally, the `Page.Content` property is set to the `Grid` object.

C# Markup enables this code to be re-written using its fluent API:

```csharp
using static CommunityToolkit.Maui.Markup.GridRowsColumns;

class SampleContentPage : ContentPage
{
    public SampleContentPage()
    {
        Content = new Grid
        {
            RowDefinitions = Rows.Define(
                (Row.TextEntry, 36)),

            ColumnDefinitions = Columns.Define(
                (Column.Description, Star),
                (Column.Input, Stars(2))),

            Children =
            {
                new Label { Text = "Code:" }
                    .Row(Row.TextEntry).Column(Column.Description),

                new Entry
                {
                    Placeholder = "Enter number",
                    Keyboard = Keyboard.Numeric,
                    BackgroundColor = Colors.AliceBlue,
                    TextColor = Colors.Black
                }.Row(Row.TextEntry).Column(Column.Input)
                 .FontSize(15)
                 .Height(44)
                 .Margin(5, 5)
                 .Bind(Entry.TextProperty, nameof(ViewModel.RegistrationCode))
            }
        };
    }

    enum Row { TextEntry }
    enum Column { Description, Input }
}
```

This example is identical to the previous example, but the C# Markup fluent API simplifies the process of building the UI in C#. 

C# Markup extensions also allow developers to use an `enum` to define names for Columns and Rows (e.g. `Column.Input`).

## Extensions

> [!NOTE]
> C# Markup includes extension methods that set specific view properties. They are designed to improve code readability, and can be used in combination with property setters. It's recommended to always use an extension method when one exists for a property, but you can choose your preferred balance.

| Extension | Description |
| --------- | ----------- |
| [`AbsoluteLayout`](extensions/absolute-layout-extensions.md) | The AbsoluteLayout extensions provide a series of extension methods that support positioning `View`s in `AbsoluteLayout`s. |
| [`BindableObject`](extensions/bindable-object-extensions.md) | The `BindableObject` extensions provide a series of extension methods that support configuring `Binding`s on a `BindableObject`. |
| [`VisualElement`](extensions/visual-element-extensions.md) | The `VisualElement` extensions provide a series of extension methods that support configuring the sizing, styling and behaviors of a `VisualElement`. |
