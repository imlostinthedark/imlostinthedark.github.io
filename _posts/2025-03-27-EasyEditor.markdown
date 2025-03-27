---
layout: post
title:  "Easy Custom Inspector Tools"
date:   2025-03-27 11:02:10 +0100
categories: tools
image: "/assets/images/easyEditor.png"
---
As we were developing our games, we came up with  tools to simplify and fasten our work. As we found them really useful, we wanted to share.
This package contains some tools that we thought could be useful. Right now, this is mostly some attributes to add missing things in the Inspector.
We plan to add many more soon.

# Attributes

### Colored Header

As we add more and more element to some of our Monobehaviour, we needed some way to categorize and retrieve useful information. So we wanted to add some way to add colored headers to identify headers.

To add this colored header, you just have to add a ColoredHeader attribute where you want it to appears.
This attribute takes two parameters :

- a label text
- the color of the background of the header (as a hexadecimal string)

```c#
[ColoredHeader("SubFields", "#00FF0055")]
public EditorTesterSubLevel EditorTesterSubLevel;
```

![pic1](/assets/images/pic1.png)

### Subview

We sometimes wanted to link the current component to an other Monobehaviour and be able to modify the data of the two components at the same place.

For example, if you have two monobehaviour classes EditorTester and EditorTesterSubLevel and you want to be able to chose an EditorTesterSubLevel in the EditorTester class and be able to change the EditorTesterSubLevel object data when seeing the EditorTester component.

This attribute uses one attribute: a string containing a list of the displayed properties of the "sub" class. If the parameters is null or an empty string, all the properties are displayed.

```c#
 public class EditorTesterSubLevel : MonoBehaviour
 {
     public string Param1;
     public List<string> Param2 = new List<string>();
     public bool Param3 = false;
 }

 public class EditorTester : Updatable
 {
     public string test;
     [SubField("Param1;Param2")]
     public EditorTesterSubLevel EditorTesterSubLevel;
 }

```

![pic2](/assets/images/pic2.png)

### List Manager

In some case, you want to have a list of assets with many parameters and you want to be able to select this assets using an asset selector as used everywhere in the Inspector.

For example, if you want to have a list of sprites with many parameters associated, you create a class for the Sprites and their info, add a List of this class in your Monobehaviour object and add ListManager attribute

The ListManager attribute has four parameters:

- a string defining the name of the property used to store the info of the linked asset
- the type of the asset (ie: Sprite, SceneAsset, etc.)
- a string defining the name of the property used as a name for the asset. This parameter is optional and if not set, it tries to use a property named Name (if found). When the asset is selected, the defined property is filled with the name of the asset.
- a string defining the label of the "path" field. It is optional and if it's not set the name of the property is used.

```c#
 [Serializable]
 public class SpriteTester
 {
     public string Name;
     public string Path;
     public string Description;
     public bool IsUI = false;
 }

 public class EditorTester : Updatable
 {
    [ListManager("Path", typeof(Sprite), "Name", "Sprite")]
    public List<SpriteTester> Testers = new List<SpriteTester>();
 }

```

![pic3](/assets/images/pic3.png)

### Linked dropdown

In some cases, we had a list of elements and we wanted to be able to select one of them for another use. So we put in place a Linked Dropdown attribute to this.

For example, you have a list of sprites as set in the previous attribute, and you want to have a dropdown to select one of this sprite as the main one. So you set a string property and add a LinkedDropdown attribute.

This attribute has three parameters :

- a string defining the property of the source list
- a string defining the list property used to populate the dropdown. If the source list is a list of strings, this parameter has to be null or empty
- a string used as a label for the dropdown. If this parameter is omitted, the label use the name of the field.

```c#
 [LinkedDropdownAttribute("Testers", "Name", "Selected Sprite")]
 public string DropDown = "";
```

![pic4](/assets/images/pic4.png)
