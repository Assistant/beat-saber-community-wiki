<!-- TITLE: Modding Intro -->
<!-- SUBTITLE: Learn how to setup the plugin template -->

# Intro
Beat Saber is made in Unity 2018 using C# with .NET framework 4.6.
For convenience, RQ has created a plugin template for Visual studio. This guide will show you how to setup that template.

Download the latest version of [Visual Studio Community.](https://visualstudio.microsoft.com/)
Download the [Beat Saber Plugin Template](/uploads/modding/beat-saber-plugin-template.zip "Beat Saber Plugin Template") by RQ.
# Template setup
Install Instructions:
1. Open C:\Users\\<Username\>\Documents\Visual Studio 2017\Templates\ProjectTemplates.
2. Drop in `beat-saber-plugin-template.zip`

After you have place the zip file in the correct directory, open Visual Studio 2017 and create a new project.
You should see the Beat Saber Plugin Template in the Visual C# section.
Create a new project using the template.
![Modding Plugin Template](/uploads/modding/modding-plugin-template.png "Modding Plugin Template")

The default assembly name is `UnnamedAssembly`
To rename the assembly:
1. In the Solution Explorer panel, double click on Properties.

![Modding Plugin Prop Selected](/uploads/modding/modding-plugin-prop-selected.png "Modding Plugin Prop Selected")

2. Change the text in the textbox Assembly name.

![Modding Plugin Properties](/uploads/modding/modding-plugin-properties.png "Modding Plugin Properties")

# Check out the code

In the solution explorer, double click on `Plugin.cs` to open it up.
You should see something like this.

![Plugin Cs Example](/uploads/modding/plugin-cs-example.png "Plugin Cs Example")

Note the red squiggly underlines. This is means Visual Studio can't find our references.

# Fixing references

To do this, right click on `References` in the Solution Explorer, and select `Add Reference...`

![Add Reference](/uploads/modding/add-a-ref.png "Add Reference")

This will open the Reference Manager window, and you can browse to find the DLL files that are missing.
Most of these files will be located within `\<Beat Saber directory>\Beat Saber_Data\Managed`

**Can't find your Beat Saber directory?** See [install folder](/faq/install-folder).

# Compiling the plugin
If you have fixed all of the references in the project, on the top menu bar press `Build -> Build Solution` or <kbd>CTLR + SHIFT + B</kbd>

Your compiled DLL should appear in the `\Bin\Debug` folder of your project.

You can then copy this DLL into the `Plugins` folder within your Beat Saber directory.

# Using dnSpy

Download the [latest release of dnSpy](https://github.com/0xd4d/dnSpy/releases/latest)

Open dnSpy and drop in the file `\<Beat Saber Directory>\Beat Saber_Data\Managed\Assembly-CSharp.dll`
This will allow you to see you nearly all of the games' code, translated back into C#.

![Dnspy Example](/uploads/modding/dnspy-example.png "Dnspy Example")

# Moving forward
Check out the [example mod](example-mod) tutorial.