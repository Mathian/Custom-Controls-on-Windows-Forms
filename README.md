[![Latest Version on Packagist](https://img.shields.io/badge/Last%20edited%20by-29.12.2024-green)](https://t.me/algor1x)
# <p align="center">Custom Controls on C# .Net Windows Forms </p>

There is a popular misconception that [Windows Forms](https://learn.microsoft.com/en-us/dotnet/desktop/winforms/overview/?view=netdesktop-9.0) is an obsolete technology. Many developers abandon it in favor of [WPF](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/introduction-to-wpf?view=netframeworkdesktop-4.8) and [WinUI](https://learn.microsoft.com/en-us/windows/apps/winui/), but how much is it justified? Let's understand it in order.

Windows Forms was created to provide ease of development for standard desktop applications that do not require complex graphics. WinForms has a high-level wrapper, the [Graphics class](https://learn.microsoft.com/en-en/dotnet/api/system.drawing.graphics?view=windowsdesktop-7.0), to handle GDI+ ( [Graphics Device Interface](https://learn.microsoft.com/en-us/dotnet/desktop/winforms/advanced/about-gdi-managed-code?view=netframeworkdesktop-4.8) ), which renders at the CPU level and does not use hardware acceleration. Whereas WPF has DirectX integrated, which allows you to use the GPU's rendering capabilities ([hardware acceleration](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/advanced/optimizing-performance-taking-advantage-of-hardware?view=netframeworkdesktop-4.8)).

~~Windows Forms also has the ability to render using GPU (e.g. via SharpDX). But that's not what we're talking about now~~

Complex graphics refers to the creation and display of objects in 3D space, texture mapping, object animation, lighting, shadows, and other resource-intensive rendering processes. DirectX works at a lower level, giving developers more control over the graphics subsystem and hardware resources, but GDI+ is more than enough for developing graphical elements of GDI+ user controls.

C# .Net Windows Forms allows you to develop both native and cross-platform applications with an adaptive interface that can have an animated design. This can all be done without [XAML](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/xaml/?view=netdesktop-9.0) layout skills, with an easy to use designer mode and with less hardware requirements.

Many people have fallen in love with WPF because of availability of ready-made solutions on Guna, Avalonia and similar frameworks, without thinking about the fact that it is enough to create only 2-3, well maximum 5 customizable control sets for WinForms for all occasions.

I don't position myself as a professional, but I can provide you with examples of beautiful (IMHO) ready-made control designs. In this repository I will provide basic information on creating and customizing controls. And in my other repositories I will publish my works, which will be useful for you to develop your own designs. 

# <p align="center">Fundamentals</p>
Each control in Windows Forms is an object that is displayed on the screen and that the user can interact with. These controls are controlled by an event system.

The Graphics class is the main tool for drawing on the control. With Graphics you can draw lines, rectangles, circles, text, images and perform other graphical operations. It encapsulates the GDI+ drawing surface.

### Creating a new Control
Open Visual Studio and create a Windows Forms (.Net Framework) project. After that, create a folder in the Solution Browser and name it Controls, UI, GUI or whatever you like. In this folder we will create our controls. For convenience, each control will be in its own file. Right click on the folder and create a new element Class. Name it whatever you want, for example MyCustomControl. Remove all the code from there. That's it, now you're ready to go.

Creating a custom control usually involves two steps:
1. Inheritance from the control base class (e.g., [Control](https://learn.microsoft.com/ru-ru/dotnet/api/system.windows.forms.control?view=windowsdesktop-8.0), [UserControl](https://learn.microsoft.com/ru-ru/dotnet/api/system.windows.forms.usercontrol?view=windowsdesktop-8.0), [Button](https://learn.microsoft.com/ru-ru/dotnet/api/system.windows.forms.button?view=windowsdesktop-8.0), [Label](https://learn.microsoft.com/ru-ru/dotnet/api/system.windows.forms.label?view=windowsdesktop-8.0), [Panel](https://learn.microsoft.com/ru-ru/dotnet/api/system.windows.forms.panel?view=windowsdesktop-8.0) etc);
2. Overriding its methods for drawing or event handling.


Initially, we will need to create a class and add the `System.Windows.Forms` namespace to make our class a child class of the `Control` base class:
```csharp
using System.Windows.Forms;

public class MyCustomControl : Control
{

}
```
Since the `MyCustomControl class` inherits `Control`, it can override or extend its methods, properties and events. If we compile our project, we will find that we have a new element `MyCustomControl` in the element panel.

Now we need to create a constructor of this class inside this class:
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;

public class MyCustomControl : Control
{
    // Constructor
    public MyCustomControl()
    {
    }
}

```
The constructor is used to initialize objects of this class when they are created. In this case, it is public, which means that it can be called externally (for example, when creating an instance of `MyCustomControl` elsewhere in the program). The constructor does not return values.

Next, we can initialize the properties of the control, such as assigning a value to the Size property, i.e., setting the size. For this we already need the `using System.Drawing`
```csharp
using System.Drawing;
using System.Windows.Forms;

public class MyCustomControl : Control
{
    // Constructor
    public MyCustomControl()
    {
        // Set the size of the control
        this.Size = new Size(200, 100);
        // this — keyword that refers to the current instance of the object (i.e. the MyCustomControl instance that is created when the constructor is called)
        // Size — property of the Control base class, which defines the size of the control (control). It represents an object of type Size, which contains two values: width and height
        // new Size(200, 100) — a new object of Size type with a width of 200 pixels and a height of 100 pixels is created
    }
}
```
As a result of running this constructor, every time a new instance of `MyCustomControl` is created, its size will be set to 200 pixels wide by 100 pixels tall. This is useful when you need to set the default dimensions for your control when you create it.

We have learned about inheritance, now let's override the standard drawing behavior of the control. To do this, we need to override the `OnPaint` method:
```csharp
using System.Drawing;
using System.Windows.Forms;

public class MyCustomControl : Control
{
    // Constructor
    public MyCustomControl()
    {
        // Set the size of the control
        this.Size = new Size(200, 100);
    }

    // Override OnPaint method for drawing
    protected override void OnPaint(PaintEventArgs e)
    {
        base.OnPaint(e);
    }
}
```
If I were to add one line each time, this lesson would get too big. So, I'm going to create a small example control with a detailed explanation of each line, and then we'll move on:
```csharp
using System.Drawing;
using System.Windows.Forms;

public class MyCustomControl : Control
{
    // Constructor
    public MyCustomControl()
    {
        // Set the size of the control
        this.Size = new Size(200, 100);
    }

    // Override OnPaint method for drawing
    // This method overrides the standard drawing behavior of the control.
    // In Windows Forms, the OnPaint method is called whenever a control needs to be redrawn,
    // such as when the system redraws it after resizing it or when it becomes visible.
    // PaintEventArgs e — is an object that is passed to the OnPaint method and contains information about the drawing context, such as the Graphics object that is used to draw on the screen.
    protected override void OnPaint(PaintEventArgs e)
    {
        // Calling the basic implementation of the OnPaint method from the Control class allows you to perform the standard drawing provided by Windows Forms before you start adding your own drawing.
        // This is important so you don't forget to draw things like backgrounds, borders, etc.
        base.OnPaint(e);

        // Get the Graphics object. The Graphics object is used for drawing on the screen. 
        // It provides methods for drawing various graphical objects such as lines, rectangles, texts and images.
        // In this case, e.Graphics passes a Graphics object provided by the system to draw on the control.
        Graphics g = e.Graphics;

        // The Clear method clears the drawing area by filling it with the specified color.
        // In this case, the background will be white. This allows you to “reset” the entire previous drawing if the control is redrawn.
        g.Clear(Color.White);
        
        // Drawing a rectangle
        // Create a Pen object that will be used for drawing. In this case it is a blue line, the line thickness is 2 pixels.
        Pen pen = new Pen(Color.Blue, 2);
        // This object will draw the outline of a rectangle. The Brush is used for the fill.

        // The DrawRectangle method draws a rectangle. The parameters passed as parameters are:
        // pen — the drawing pencil used
        // 10, 10 — coordinates of the upper left corner of the rectangle
        // this.Width - 20, this.Height - 20 — the width and height of the rectangle, which are calculated relative to the width and height of the control (with an indent of 10 pixels on each side)
        g.DrawRectangle(pen, 10, 10, this.Width - 20, this.Height - 20);

        // Drawing text
        // Create a Font object that specifies the font and size of the text. The font used here is Arial font size 12
        Font font = new Font("Arial", 12);
        // Create a brush (Brush object) that defines the color of the text, in this case black.
        Brush brush = Brushes.Black;
        // The DrawString method draws a string of text on the screen.
        // The “Custom Control” text will be drawn using a font and brush at a point with coordinates (30, 30) relative to the upper left corner of the control.
        g.DrawString("Custom Control", font, brush, new PointF(30, 30));
    }
}
```
<details>
  <summary>Explanation</summary>
  Thus we have created a primitive control, which will be a blue outline of a rectangle, 2 pixels thick, on a white background, with indents from the edges of 10 pixels, inside which is located the text “Custom Control”, drawn in a point with black color, font Arial, size 12. The text is located in the point with coordinates [30;30].
</details>

# <p align="center">Drawing tools and methods</p>
### <p align="center">Drawing Tools</p>
#### Pen (Drawing tool)
`Pen` - is a tool for drawing lines, rectangles, ellipses and other contour shapes. Its main properties are color, line thickness and line style.
<details>
  <summary>Main properties</summary>
  
1. `Color` - sets the color of the line;<br>
2. `Width` - sets the line thickness;<br>
3. `DashStyle` - sets the line style (e.g., solid, dashed);<br>
4. `StartCap` и `EndCap` - set the start and end style of the line (e.g. round, square, etc.).
</details>
<details>
  <summary>Dash Styles</summary>
  
1. `DashStyle.Solid` - solid line (default);<br>
2. `DashStyle.Dash` - dashed line;<br>
3. `DashStyle.Dot` - dotted line;<br>
4. `DashStyle.DashDot` - alternating dashes and dots;<br>
5. `DashStyle.DashDotDot` - alternating dashes and two dots.
</details>
<details>
  <summary>StartCap & EndCap</summary>
  
1. `Flat` - straight, flat end;<br>
2. `Round` - rounded end (round);<br>
3. `Square` - a square end (protrudes beyond the line);<br>
4. `Triangle` - triangular end (protruding like an arrow);<br>
5. `NoAnchor` - no change at the ends of the line (regular line).
</details>

#
#### Brush (Drawing tool)
`Brush` - is an abstract class that is used to fill shapes (rectangles, ellipses, etc.) of contoured figures. There are several types of brushes, which have their own sets of basic parameters:
<details>
  <summary>Types of Brushes</summary>
  
1. `SolidBrush` - fills shapes with a solid color;<br>
2. `LinearGradientBrush` - fills shapes with a linear gradient;<br>
3. `HatchBrush` - fills shapes with texture (hatching);<br>
4. `TextureBrush` - fills shapes with an image.
</details>

### <p align="center">Drawing Methods</p>
#### Line Drawing:
`DrawLine(Pen pen, float x1, float y1, float x2, float y2)` - рисует линию между двумя точками (`x1`, `y1`) и (`x2`, `y2`)
#### Drawing rectangles and squares:
`DrawRectangle(Pen pen, float x, float y, float width, float height)` - outlines a rectangle<br>
`FillRectangle(Brush brush, float x, float y, float width, float height)` - fills the rectangle with color
#### Drawing elipses and circles:
`DrawEllipse(Pen pen, float x, float y, float width, float height)` - draws an ellipse in a rectangular area (contour)<br>
`FillEllipse(Brush brush, float x, float y, float width, float height)` - fills the ellipse with color<br>
To draw a circle, just set the same width and height for an ellipse.
#### Text drawing:
`DrawString(string text, Font font, Brush brush, float x, float y)` - draws a line of text<br>
`DrawString(string text, Font font, Brush brush, RectangleF layoutRectangle)` - draws text given a given rectangle
#### Polygonal shapes:
`DrawPolygon(Pen pen, Point[] points)` - draws a polygon by connecting the passed points<br>
`FillPolygon(Brush brush, Point[] points)` - fills the polygon with color
#### Additional drawing methods
`DrawArc(Pen pen, float x, float y, float width, float height, float startAngle, float sweepAngle)` - draws an arc (part of an ellipse)<br>
`FillRegion(Brush brush, Region region)` - fills a region (for example, after creating a complex shape using Region).

#### I think that's enough.
# <p align="center">Handling redraw (Invalidate)</p>
Redrawing is the process where a form or control is cleared and redrawn. When an application window is hidden and then reappears, the system redraws its contents. This also happens when the window size changes, or when you want to update part of the screen (for example, when data or state changes). 

However, Windows Forms does not automatically redraw a form or control at every point in time. If you want to update something on the screen, for example, after a state change, you need to request a redraw using the [Invalidate()](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.control.invalidate?view=windowsdesktop-8.0) method.

The Invalidate method triggers the Paint event on the form or control, which causes it to redraw. When you call Invalidate, it tells the system, “Redraw this area of the screen.”

You can call the Invalidate method any time you want to update the appearance of a control or form. For example, if the user has changed settings and those changes need to be displayed on the screen.

#### IMPORTANT: 
The Invalidate method does not cause an immediate redraw. It simply marks the object as needing to be redrawn. The actual repainting will occur in the next step of event handling.

The Paint event will be called automatically when the repaint is done.

```csharp
using System.Drawing;
using System.Windows.Forms;

public class Clicker : Control
{
    private Color currentColor;

    public Clicker()
    {
        // Set the initial color for the rectangle
        currentColor = Color.Red;

        // Set the event handler for mouse clicks
        this.MouseClick += new MouseEventHandler(this.OnMouseClick);

        // Set the dimensions of the form
        this.Size = new Size(400, 400);
    }

    // Mouse click event handler
    private void OnMouseClick(object sender, MouseEventArgs e)
    {
        // Change color with every click
        currentColor = (currentColor == Color.Red) ? Color.Blue : Color.Red;

        // Call form redraw
        // If you don't do this, the color on the form will remain the same
        this.Invalidate();
    }

    // Override the OnPaint method for drawing
    protected override void OnPaint(PaintEventArgs e)
    {
        base.OnPaint(e);

        // Get Graphics object
        Graphics g = e.Graphics;

        // Create a fill brush
        Brush brush = new SolidBrush(currentColor);

        // Draw a rectangle with the current color
        g.FillRectangle(brush, 100, 100, 200, 100);
    }
}
```
`currentColor` - is a variable that stores the current color to fill the rectangle. We start with red color and change it to blue every time we click the mouse.<br>
`MouseClick` event handler - when the user clicks the mouse, the color changes (red to blue or vice versa) and the Invalidate method is called, which marks the form as requiring redrawing.<br>
`OnPaint` method - this method redraws the shape. In it, we create a Graphics object and draw a rectangle with the current color using the FillRectangle method.<br>
`Invalidate` method - every time the color changes, Invalidate is called. This triggers the Paint event, which redraws the entire control, namely the rectangle on the shape.

### Partial redraw (Invalidate(Rectangle)):
Instead of redrawing the entire form, you can specify which specific area should be redrawn. The [Invalidate(Rectangle)](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.control.invalidate?view=windowsdesktop-8.0) method allows you to specify the rectangle to be redrawn, which can improve performance when updating only part of the interface.

`this.Invalidate(new Rectangle(50, 50, 200, 100));` - redraws only rectangle 50,50,200,100

We can restrict redrawing to only the area where the rectangle is drawn:
```csharp
private void OnMouseClick(object sender, MouseEventArgs e)
{
    // Change the color
    currentColor = (currentColor == Color.Red) ? Color.Blue : Color.Red;

    // Redraw only the area occupied by the rectangle
    this.Invalidate(new Rectangle(100, 100, 200, 100));
}
```
### Conclusion
Invalidate is a method that tells the system that a form or control needs to be redrawn. It marks the area as needing to be updated, and the system will automatically call the Paint event, where we draw.

You can redraw the entire form or specific areas using Invalidate(Rectangle).

It is important to remember that the Invalidate method does not cause an immediate redraw, but only marks the area for a later update, which will occur at the time of the next event loop.

#### When to use Invalidate?
**Change of state:** if the program changes the state to be reflected on the screen while the program is running.<br>
**Redrawing after user interaction:** for example, after changing parameters or moving items on the screen.<br>
**Updating the UI based on data:** For example, if the data on the screen changes (such as graphs or statistics).<br>

Thus, the Invalidate method is the main tool for manually redrawing forms and controls in Windows Forms.

# <p align="center">Custom properties of Controls</p>
In Windows Forms, you can add properties to custom controls that allow you to change the behavior or appearance of the control through the properties window in Visual Studio. These properties can be simple types such as `int`, `string`, `Color`, or more complex data types. [Read on the official website](https://learn.microsoft.com/en-us/dotnet/desktop/winforms/controls/defining-a-property-in-windows-forms-controls?view=netframeworkdesktop-4.8)

In order for a property to be displayed in the Properties window in the designer, you must use attributes such as `Browsable`, `Category`, `Description`, and others.

Let's take the code from the previous lesson as an example. We will add another rectangle to our control and two properties: `FirstColor` and `SecondColor`, which will define the two colors of the rectangle. These properties can be changed in designer mode:
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using System.ComponentModel;

public class Clicker : Control
{
    private Color _firstColor; // The first property is color
    private Color _secondColor; // The second property is color

    // Constructor
    public Clicker()
    {
        // Set initial colors
        _firstColor = Color.Red;
        _secondColor = Color.Blue;

        // Set the event handler for mouse clicks
        this.MouseClick += new MouseEventHandler(this.OnMouseClick);

        // Set the dimensions of the control
        this.Size = new Size(400, 400);
    }

    // Mouse click event handler
    private void OnMouseClick(object sender, MouseEventArgs e)
    {
        // Swap colors with each click
        var temp = _firstColor;
        _firstColor = _secondColor;
        _secondColor = temp;

        // Call control redraw
        this.Invalidate();
    }

    // Override the OnPaint method for drawing
    protected override void OnPaint(PaintEventArgs e)
    {
        base.OnPaint(e);

        // Get Graphics object
        Graphics g = e.Graphics;

        // Create brushes for the fill
        Brush brush1 = new SolidBrush(_firstColor);
        Brush brush2 = new SolidBrush(_secondColor);

        // Draw two rectangles with current colors
        g.FillRectangle(brush1, 50, 50, 150, 100); // The first rectangle
        g.FillRectangle(brush2, 200, 50, 150, 100); // The second rectangle
    }

    // Property for the first color
    [Browsable(true)]
    [Category("Appearance")]
    [Description("Color for the first rectangle")]
    public Color FirstColor
    {
        get { return _firstColor; }
        set
        {
            if (_firstColor != value)
            {
                _firstColor = value;
                this.Invalidate(); // Redrawing
            }
        }
    }

    // Property for the second color
    [Browsable(true)]
    [Category("Appearance")]
    [Description("Color for the second rectangle")]
    public Color SecondColor
    {
        get { return _secondColor; }
        set
        {
            if (_secondColor != value)
            {
                _secondColor = value;
                this.Invalidate();
            }
        }
    }
}
```
### Properties
`FirstColor` — color for the first rectangle<br>
`SecondColor` — color for the second rectangle
### Attributes
`[Browsable(true)]` - specifies that the property will be displayed in the Properties window in Visual Studio<br>
`[Category("Appearance")]` - specifies the category in which this property will be displayed<br>
`[Description("Color for the first rectangle")]` - adds a description for the property that will be displayed in the properties window<br>
#### These attributes make properties available for customization through the designer in Visual Studio.
