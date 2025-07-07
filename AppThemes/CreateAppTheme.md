# Create an AppTheme

## Introduction

AppThemes can be created for any Rocket system. Each system may include its own specific functions, and Razor templates may vary between them.

For standard Rocket systems, there are two primary types:

- **RocketContent**
- **RocketDirectory**

**RocketContent** is a module base designed to display a single data row or a list of data rows.

**RocketDirectory** is a more advanced module base that can display lists and detailed views of data. It supports additional features such as properties, categories, data management, and search functionality.

Several open-source systems, such as **RocketBlog**, **RocketEvents**, and **RocketNews**, are built on top of RocketDirectory. These systems inherit all of RocketDirectory's capabilities and extend them with their own specialized features.

While other Rocket systems also exist, the process for creating an AppTheme remains consistent. However, specific functionality may vary depending on the system.

## The Editor and IntelliSense

The recommended editor for working on an AppTheme is **Visual Studio**, which provides full IntelliSense support.  
**However, any text editor will work.**

To enable IntelliSense, refer to the `AppThemes-W3-CSS` project:

🔗 [AppThemes-W3-CSS.csproj on GitHub](https://github.com/Rocket-CDS/AppThemes-W3-CSS/blob/main/AppThemes-W3-CSS.csproj)

This project includes references to the `bin` folder of a DNN installation. You should replicate this referencing approach, matching the required assemblies — and adding extras if needed.

> **Note:** Occasionally, IntelliSense may stop working for Razor templates in Visual Studio. If this happens:
> 1. Close Visual Studio.
> 2. Delete the hidden `.vs` folder in your project directory.
> 3. Reopen the solution.

## W3.CSS Examples

[https://github.com/Rocket-CDS/AppThemes-W3-CSS/blob/main/AppThemes-W3-CSS.csproj](https://github.com/Rocket-CDS/AppThemes-W3-CSS/blob/main/AppThemes-W3-CSS.csproj)  


## Making an AppTheme

The easiest way to create a new AppTheme is to **copy an existing one** that closely matches your desired layout or functionality, and then customize it.

Each AppTheme includes:
- **Admin templates**
- **View templates** (used for front-end website display)

AppThemes follow a **standard naming convention and folder structure**, which should always be maintained for compatibility.

> **Note:** These instructions assume you have:
> - A working **DNN installation**
> - **RocketCDS** installed
> - The `AppThemes-W3-CSS` project downloaded

### Steps to Create an AppTheme

1. Navigate to: \DesktopModules\RocketThemes\AppThemes-W3-CSS\rocketcontentapi.About
2. Copy the entire folder.
3. Paste it into the same directory: \DesktopModules\RocketThemes\AppThemes-W3-CSS\
4. Rename the folder to: `rocketcontentapi.IsThatIt`
5. Restart the DNN application pool.

You now have a new AppTheme named `IsThatIt`, which can be selected when adding the RocketContent module to a DNN page.

This same **copy-and-paste** method works for RocketDirectory and other Rocket systems as well.

> **Important:**  
> AppTheme folder names **must not start with a number**. AppThemes with numeric prefixes will not be selectable in the RocketContent module.


>**IsThatIt ?  NO, you need to change it:**  
>See editing an AppTheme under the RocketContent and RocketDirectory documentation for how to edit the AppTheme.

### Base AppTheme Examples

There are two starter AppThemes included in `AppThemes-W3-CSS`:
- `rocketcontentapi.01blankrow`
- `rocketcontentapi.01blankrows`

These can be used as base templates.  
Be sure to **rename them without the numeric prefix** when creating your own versions.

