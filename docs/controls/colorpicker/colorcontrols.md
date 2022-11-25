# ColorPicker

The `ColorPicker` provides a highly customizable, general-purpose control that users can use to select colors in RGB or HSV color space. This implementation is just as much about providing primitive controls that developers can use to build their own color pickers as it is about providing a ready-to-use picker. 

The ColorPicker includes a family of controls (components):

 * `ColorSpectrum` (primitive) : A two dimensional spectrum for color selection.
 * `ColorSlider` (primitive) : A slider with a background that represents a single color component.
 * `ColorPreviewer` (primitive) : Presents a preview color with optional accent colors.
 * `ColorView` : Presents a color for user editing using a spectrum, palette and component sliders.
 * `ColorPicker` : Presents a color for user editing using a spectrum, palette and component sliders within a drop down. Editing is available when the drop down flyout is opened; otherwise, only the preview color is shown.

Each primitive component can be used on its own and mixed/matched with others. This allows significant composability that isn't possible with other color picker implementations. For example, you can quickly bind together the `ColorSpectrum`, `ColorSlider` and `ColorPreviewer` primitives to create your own color picker with a brand-new design.

## Is this the right control?

This control is intended to be used directly to select colors in a user-friendly, developer customizable way. This can be done using either a canvas-type `ColorView` control or a compact `ColorPicker` drop down.

For apps with even more special-purpose needs, each control and primitive component can be independently customized to create a new color picker without having to re-implement all the advanced rendering and color logic. This is very useful to match a specific app's design and usability requirements.

Developers using this control may:
 1. Use `ColorView` or `ColorPicker` as-is directly in their apps
 2. Customize `ColorView` or `ColorPicker` using the included properties. These properties allow significant changes to the control such as disabling components sliders, showing different palettes or hiding all but the spectrum tab.
 3. Create a new color picker to meet a specific app's design and usability requirements using the existing primitives components.

## Using in Your App

Avalonia is used in several resource-constrained environments such as embedded devices. For this and other reasons, certain larger controls such as the ColorPicker are not included with the main Avalonia Nuget packages. This means a bit of extra work is required to add the ColorPicker to your app:

 1. Add the `Avalonia.Controls.ColorPicker` nuget to your project
 2. Add control themes and styles for all color picker controls in `App.axaml` by adding:
    * `<StyleInclude Source="avares://Avalonia.Controls.ColorPicker/Themes/Fluent/Fluent.xaml" />` for Fluent themes and
	* `<StyleInclude Source="avares://Avalonia.Controls.ColorPicker/Themes/Simple/Simple.xaml" />` for Simple themes
    
	Note that this step is not required from some themes packages such as FluentAvalonia which include all controls by default.

## History of This Control

This control originated as a re-styling of the one in UWP (later WinUI) using the basic designs implemented for the Windows Community Toolkit. The WinUI ColorPicker isn't conducive to smaller screen sizes and the overall design/usability of the control left something to be desired for both users and developers.

However, that control still wasn't as good as it should be. Overall, it couldn't be re-templated and customized without a lot of effort partially because individual components were highly inter-dependent on each other. It also used a lot of template parts and code-behind. The Avalonia version of the control (a complete rewrite) attempts to fix all of these issues and become the predominant color picker design.

Main improvements learning from WinUI were:
 * The `ColorPicker` is implemented as a drop-down unlike in WinUI (as all other "pickers" are). There is also a `ColorView` control for those that want the canvas-type control.
 * The Avalonia controls attempt to do everything possible in XAML control themes keeping code-behind to an absolute minimum. This significantly increases composability and enables app developers to customize every part of these controls (and even the primitives in most cases).
 * Primitives such as the `ColorSlider` and `ColorSpectrum` are fully self-contained and can be used separately enabling app developers to create custom color picker implementations.
 * A new `HsvColor` struct was added to base Avalonia itself (alongside `Color` and `HslColor`) and is now used in all color picker controls. This simplified code-behind and also made binding of color properties between primitives and controls possible.
 * `HsvColor` along with `ColorSlider` together unlock a lot of power compared to WinUI (and enable easy re-templating).
 * Many new properties (more than in WinUI) were added to control all aspects of the `ColorView` visibility. Each tab can be separately hidden along with most individual subsections. This allows a lot of design customization without having to re-template or drill into styles.
 * Color palettes were added using the `IColorPalette` interface (from the Windows Community Toolkit). No color palettes are supported in the WinUI version of this control.
 * New properties such as `SelectedIndex` and `ColorModel` allow customizing the control and putting it into a pre-defined state. For example: the WinUI ColorPicker always defaults to RGB and this cannot be changed in code or XAML. This implementation does not have such limitations.

## Controls & Primitives

| Control          | Link                                         |
|------------------|----------------------------------------------|
| `ColorPicker`    | See the dedicated [`ColorPicker`]() page.    |
| `ColorView`      | See the dedicated [`ColorView`]() page.      |
| `ColorSpectrum`  | See the dedicated [`ColorSpectrum`]() page.  |
| `ColorSlider`    | See the dedicated [`ColorSlider`]() page.    |
| `ColorPreviewer` | See the dedicated [`ColorPreviewer`]() page. |

## Color Palettes

Several pre-defined color palettes implementing the `IColorPalette` interface are provided. Instances of these palettes may be set to the `Palette` property of a `ColorView` or `ColorPicker`.

<table>
  <tr>
    <th>Palette</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>
      <img src="../../../.gitbook/assets/color-palette-fluent.png" alt="Fluent Color Palette" width="300" />
    </td>
    <td>Contains the Fluent color palette found in Windows 10 and later. This is the default color palette.</td>
  </tr>
  <tr>
    <td>
      <img src="../../../.gitbook/assets/color-palette-flat.png" alt="Flat UI Color Palette" width="300" />
    </td>
    <td>Contains the full <a href="https://github.com/designmodo/Flat-UI">Flat UI color palette</a>.</td>
  </tr>
  <tr>
    <td>
      <img src="../../../.gitbook/assets/color-palette-flat-half.png" alt="Flat UI Half Color Palette" width="300" />
    </td>
    <td>Contains half of <a href="https://github.com/designmodo/Flat-UI">Flat UI color palette</a> for improved usability especially on mobile devices.</td>
  </tr>
  <tr>
    <td>
      <img src="../../../.gitbook/assets/color-palette-material.png" alt="Material Color Palette" width="300" />
    </td>
    <td>Contains most of the <a href="https://material.io/design/color/the-color-system.html#tools-for-picking-colors">Material design color palette</a>. In order to make the palette uniform and rectangular the following alterations were made 1. The A100-A700 shades of each color are excluded. These shades do not exist for all colors (Brown/Gray). 2. Black/White are stand-alone colors and are also excluded.</td>
  </tr>
  <tr>
    <td>
      <img src="../../../.gitbook/assets/color-palette-material-half.png" alt="Material Half Color Palette" width="300" />
    </td>
    <td>Contains half of the <a href="https://material.io/design/color/the-color-system.html#tools-for-picking-colors">Material design color palette</a> shown above for improved usability especially on mobile devices.</td>
  </tr>
  <tr>
    <td>
      <img src="../../../.gitbook/assets/color-palette-sixteen.png" alt="Sixteen Color Palette" width="300" />
    </td>
    <td>Contains the standard <a href="https://en.wikipedia.org/wiki/Web_colors#HTML_color_names">sixteen color palette</a> from the HTML 4.01 specification.</td>
  </tr>
</table>
