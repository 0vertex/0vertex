---
title: "PropertyWrapper"
description: ""
date: 2021-11-07T10:25:06+05:30
draft: true
tags: ["Array", "Property"]
categories: ["Swift"]
author: "Manish"
---

### What is a property wrapper?
Swift 5.1 added a new feature called `PropertyWrapper`, a clean and easy way of abstracting core logic behind any stored property.
You might have already seen a few of these like `@State` `@Binding` `@Pubished` etc.

> Property Wrapper provides the layer of separation for the business logic of the property and its definition.

### Example
Let's say we need to build a solution for a theme based color system, where every property will have a different color for light and 
dark mode.

- **Step 1** - *Create an enum naming `Appearance` with light and dark caseses.*

```swift
enum Appearance {
    case light
    case dark
}
```

- **Step 2** - *Create a global property naming `currentMode`, which will define the current appearance mode.*
```swift
var currentMode = Appearance.dark
```

- **Step 3** - *Create a property wrapper naming `ThemedColor`, which will store and return currect color based on current appearance mode*
```swift
@propertyWrapper
struct ThemedColor {
    
    private var light: UIColor
    private var dark: UIColor
    
    var wrappedValue: UIColor {
        get { ((currentMode == .light) ? light : dark) }
    }
    
    init(light: UIColor = .white, dark: UIColor = .black) {
        self.light = light
        self.dark = dark
    }
    
}
```

- **Step 4** - *Create a `Color` struct, which will contain all the color properties*
```swift
struct Color {
    @ThemedColor(light: .yellow, dark: .white)
    public static var titleColor: UIColor

    @ThemedColor(light: .blue, dark: .black)
    public static var backgroundColor: UIColor
}
```

- **Step 5** - *Use*
```swift
Color.titleColor

Color.backgroundColor
```
`titleColor` will return *yellow* for light mode and *white* for dark mode, similarly `backgroundColor` will return *blue* for light mode and *black* for dark mode. 

Try this by changing the value for `currentMode` property to `.dark` or `.light`.