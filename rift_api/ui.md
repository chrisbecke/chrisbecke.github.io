# UI Frames

## UI.Context
Lists all contexts that have been created. Organizes them under [addonIdentifier][contextIdentifier][constructionOrder]. As an example, if TestAddon created four contexts named UIElement, the second would be UI.Context.TestAddon.UIElement[2].

## UI.CreateContext
Creates a new UI context. A UI context must be created in order to create any frames.

    context = UI.CreateContext(name)   -- Context <- string

#### Parameters:
**name** - A descriptive name for this element. Used for error reports and performance information. May be shown to end-users.  

#### Return Values:
**context** - A new context. Contexts are guaranteed to have at least the capabilities of a Frame.  

## UI.CreateFrame
Creates a new frame. Frames are the blocks that all addon UIs are made out of. Since all frames must have a parent, you'll need to create a Context before any frames. See UI.CreateContext.
List of valid frame types:

| Element | Description |
| --- | --- |
| Frame | The base type. No special capabilities. Useful for spacing, organization, input handling, and solid color squares. |
| Mask | Obscures the contents of child frames that do not fall within the mask boundaries. |
| Text | Displays text. |
| Texture | Displays a static texture image. |
| RiftButton | A standard Rift button widget |
| RiftCheckbox | A standard Rift checkbox widget |
| RiftScrollbar | A standard Rift scrollbar widget |
| RiftSlider | A standard Rift slider widget |
| RiftTextfield | A standard Rift textfield widget |
| RiftWindow | A standard Rift window widget |

    frame = UI.CreateFrame(type, name, parent)   -- Frame <- string, string, Element

#### Parameters:
**type** - The type of your new frame.  
**name** - A descriptive name for this element. Used for error reports and performance information. May be shown to end-users.  
**parent** - The new parent for this frame.  

#### Return Values:
**frame** - Your new frame.  

### UI.Frame
Lists all frames that have been created. Organizes them under [addonIdentifier][contextIdentifier][constructionOrder]. As an example, if TestAddon created four frames named UIElement, the second would be UI.Frame.TestAddon.UIElement[2].

## Layout

### Layout:EventAttach
Attaches an event handler to an event.

    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number

#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.  
**priority** - Priority of the event handler. Higher numbers trigger first.  
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.  
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Layout:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.

    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil

#### Parameters:
**owner** - Owner to search for.  
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.  
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.  
**callback** - A callback function to search for.  
**priority** - Priority of the event handler. Higher numbers trigger first.  

### Layout:EventList
Lists the current event handlers for an event.

    result = Layout:EventList(handle)   -- table <- eventFrame

#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.  

#### Return Values:
**result** - A table of event handlers for this event.  

#### Returned Members:
**owner** - Owner to search for.  
**macro** - The macro that will run when the event fires.  
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.  
**handler** - The handler that will be called when the event fires.  
**priority** - Priority of the event handler. Higher numbers trigger first.  

### Layout:EventMacroGet
Gets the macro that will be triggered when this event occurs.

    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame

#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.  

#### Return Values:
**macro** - The macro that will be triggered.  

### Layout:EventMacroSet
Sets the macro that will be triggered when this event occurs.

    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil

#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.  
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.  

#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.

### Layout:GetBottom
Retrieves the Y position of the bottom edge of this element.

    bottom = Layout:GetBottom()   -- number <- void

#### Return Values:
**bottom** - The Y position of the bottom edge of this element.  

### Layout:GetBounds
Retrieves the complete bounds of this element.

    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void

#### Return Values:
**right** - The X position of the right edge of this element.  
**left** - The X position of the left edge of this element.  
**bottom** - The Y position of the bottom edge of this element.  
**top** - The Y position of the top edge of this element.  

### Layout:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".

    eventTable = Layout:GetEventTable()   -- table <- void

#### Return Values:
**eventTable** - The event table of this element.  

### Layout:GetHeight
Retrieves the height of this element.

    height = Layout:GetHeight()   -- number <- void

#### Return Values:
**height** - The height of this element.  

### Layout:GetLeft
Retrieves the X position of the left edge of this element.

    left = Layout:GetLeft()   -- number <- void

#### Return Values:
**left** - The X position of the left edge of this element.  

### Layout:GetName
Retrieves the name of this element.

    name = Layout:GetName()   -- string <- void

#### Return Values:
**name** - The name of this element, as provided by the addon that created it.  

### Layout:GetOwner
Retrieves the owner of this element.

    owner = Layout:GetOwner()   -- string <- void

#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.  

### Layout:GetRight
Retrieves the X position of the right edge of this element.

    right = Layout:GetRight()   -- number <- void

#### Return Values:
**right** - The X position of the right edge of this element.  

### Layout:GetTop
Retrieves the Y position of the top edge of this element.

    top = Layout:GetTop()   -- number <- void

#### Return Values:
**top** - The Y position of the top edge of this element.  

### Layout:GetType
Retrieves the type of this element.

    type = Layout:GetType()   -- string <- void

#### Return Values:
**type** - The type of this element.  

### Layout:GetWidth
Retrieves the width of this element.

    width = Layout:GetWidth()   -- number <- void

#### Return Values:
**width** - The width of this element.  

### Layout:ReadAll
Read all set points and sizes from this frame.

    results = Layout:ReadAll()   -- table <- void

#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.  

### Layout:ReadHeight
Read a set height from this frame.

    height = Layout:ReadHeight()   -- number <- void

#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.  

### Layout:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.

    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil

#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.  
**x** - X coordinate of the point. Either this or Y must be nil.  
**coordinate** - Named coordinate. Must be a one-axis coordinate.  

#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.  
**offset** - The offset in pixels from the source location to the actual location.  
**layout** - The table that this point is pinned to.  
**origin** - The string "origin".  

### Layout:ReadWidth
Read a set width from this frame.

    width = Layout:ReadWidth()   -- number <- void

#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.  

## Layout Events

### Event.UI.Layout.Layer
Signals a change in layer.

    Event.UI.Layout.Layer(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Layout.Move
Signals a change in the top-left corner's position. May be delayed after the actual movement.

    Event.UI.Layout.Move(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Layout.Size
Signals a change in size. May be delayed after the actual change.

    Event.UI.Layout.Size(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Layout.Strata
Signals a change in strata.

    Event.UI.Layout.Strata(handle)

#### Parameters:
**handle** - nil  

## Native
Native -> [Layout]

### Native:GetLayer
Gets the native item's layer order.

    layer = Native:GetLayer()   -- number <- void

#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.  

### Native:GetLoaded
Gets whether or not this native element is loaded and rendering.

    loaded = Native:GetLoaded()   -- boolean <- void

#### Return Values:
**loaded** - true if it is loaded.  

### Native:GetSecureMode
Get the native element's secure mode. See Frame:SetSecureMode() for details.

    secure = Native:GetSecureMode()   -- string <- void

#### Return Values:
**secure** - The current secure mode.  

### Native:GetStrata
Gets the native item's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.

    strata = Native:GetStrata()   -- string <- void

#### Return Values:
**strata** - The item's current strata.  

### Native:GetStrataList
Gets a list of valid stratas for this native element.

    stratas = Native:GetStrataList()   -- table <- void

#### Return Values:
**stratas** - An array of valid stratas, in order.  

### Native:SetLayer
Sets the frame layer for this native element. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.

    Native:SetLayer(layer)   -- number

#### Parameters:
**layer** - The new layer for this frame.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Native:SetStrata
Sets the strata for this native element.

    Native:SetStrata(layer)   -- string

#### Parameters:
**layer** - The new layer for this frame.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

## Native Events

### Event.UI.Native.Loaded
Signals a change in this native frame's loaded state.

    Event.UI.Native.Loaded(handle)

#### Parameters:
**handle** - nil  

## Element
Element -> [Layout]

### Element:GetChildren
Returns a table containing all of this element's children.

    children = Element:GetChildren()   -- table <- void

#### Return Values:
**children** - A table containing this element's children.  

### Element:GetKeyFocus
Gets the key focus status.

    focus = Element:GetKeyFocus()   -- boolean <- void

#### Return Values:
**focus** - Whether this frame is the current key focus.  

### Element:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.

    mask = Element:GetMouseMasking()   -- string <- void

#### Return Values:
**mask** - The current mouse masking mode.  

### Element:GetVisible
Gets the visibility flag for this frame.

    visible = Element:GetVisible()   -- boolean <- void

#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.  

### Element:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.

    Element:SetAlpha(alpha)   -- number

#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.  

### Element:SetBackgroundColor
Sets the background color of this frame.

    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number

#### Parameters:
**b** - Blue.  
**r** - Red.  
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.  
**g** - Green.  

### Element:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.

    Element:SetKeyFocus(focus)   -- boolean

#### Parameters:
**focus** - The new key focus setting.  

### Element:SetMouseMasking
Sets the frame's mouse masking mode.

    Element:SetMouseMasking(mask)   -- string

#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Element:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.

    Element:SetVisible(visible)   -- boolean

#### Parameters:
**visible** - The new visibility flag.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

## Frame

Frame -> [Element] -> [Layout]

### Frame:ClearAll
Clear all set points and sizes from this frame.

    Frame:ClearAll()   -- void

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:ClearHeight
Clear a set height from this frame.

    Frame:ClearHeight()   -- void

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:ClearPoint
Clear a set point from this frame.

    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil

#### Parameters:
**y** - Y coordinate of the point.  
**x** - X coordinate of the point.  
**coordinate** - Named coordinate.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:ClearWidth
Clear a set width from this frame.

    Frame:ClearWidth()   -- void

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:GetLayer
Gets the frame's layer order.

    layer = Frame:GetLayer()   -- number <- void

#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.  

### Frame:GetMouseoverUnit
Gets the unit that is being represented by this frame.

    unit = Frame:GetMouseoverUnit()   -- string <- void

#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.  

### Frame:GetParent
Gets the parent of this frame.

    parent = Frame:GetParent()   -- Element <- void

#### Return Values:
**parent** - The parent element of this frame.  

### Frame:GetSecureMode
Get the current secure mode. See SetSecureMode for details.

    secure = Frame:GetSecureMode()   -- string <- void

#### Return Values:
**secure** - The current secure mode.  

### Frame:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.

    strata = Frame:GetStrata()   -- string <- void

#### Return Values:
**strata** - The item's current strata.  

### Frame:GetStrataList
Gets a list of valid stratas for this frame.

    stratas = Frame:GetStrataList()   -- table <- void

#### Return Values:
**stratas** - An array of valid stratas, in order.  

### Frame:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.

    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout

#### Parameters:
**target** - Target Layout to pin this frame's edges to.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.

    Frame:SetHeight(height)   -- number

#### Parameters:
**height** - The new height of this frame.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.

    Frame:SetLayer(layer)   -- number

#### Parameters:
**layer** - The new layer for this frame.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:SetMouseoverUnit
Sets the unit that will be represented by this frame.

    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil

#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.  

#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.

### Frame:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".

    Frame:SetParent(parent)   -- Element

#### Parameters:
**parent** - The new parent for this frame.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it.
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.

    Frame:SetPoint(...)   -- ...

#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".

    Frame:SetSecureMode(secure)   -- string

#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".  

### Frame:SetStrata
Sets the strata for this frame.

    Frame:SetStrata(strata)   -- string

#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### Frame:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.

    Frame:SetWidth(width)   -- number

#### Parameters:
**width** - The new width of this frame.  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

## Canvas

Canvas -> [Frame] -> [Element] -> [Layout]

### Canvas:GetShape
Returns the current canvas shape.

    shape, fill, stroke = Canvas:GetShape()   -- table, table, table <- void

#### Return Values:
**stroke** - The current stroke style. See Canvas:SetShape() for details.  
**fill** - The current fill style. See Canvas:SetShape() for details.  
**shape** - The current shape. See Canvas:SetShape() for details.  

### Canvas:SetShape
Sets the canvas shape. Must have at least one of "fill" or "stroke".

    Canvas:SetShape(path, fill, stroke)   -- nil, nil, nil
    Canvas:SetShape(path, fill, stroke)   -- table, table, nil
    Canvas:SetShape(path, fill, stroke)   -- table, nil, table
    Canvas:SetShape(path, fill, stroke)   -- table, table, table

#### Parameters:
**path** - A path. Consists of a table containing a series of tables, with consecutive integer keys starting from 1. Each internal table contains coordinates, either as an absolute value relative to the top-left corner of the frame, or Proportional to the frame size, as a number from 0 to 1. Giving both an absolute value and a proportional value for any coordinate is not allowed. Members:  
				x/xProportional: x coordinate of this point. Required.
				y/yProportional: y coordinate of this point. Required.
				xControl/xControlProportional: x coordinate of the curve control handle. If this exists, yControl/yControlProportional must also exist.
				yControl/yControlProportional: y coordinate of the curve control handle. If this exists, xControl/xControlProportional must also exist.
**stroke** - A stroke style. Members:  
				r: Red color value, betwen 0 and 1. Required.
				g: Green color value, betwen 0 and 1. Required.
				b: Blue color value, betwen 0 and 1. Required.
				a: Alpha color value, betwen 0 and 1. Defaults to 1.
				cap: Endcap behavior for non-loops. May be "none", "square", or "round". Defaults to "round".
				miter: Corner behavior. May be "miter", "round", or "bevel". Defaults to "miter".
				miterLimit: Determines how small the corner angle can be until the miter is chopped off. Defaults to 3. Available only if miter behavior is set to "miter".
				thickness: Line thickness. Required.
**fill** - A fill style. By default, "gradientLinear" spreads on the X axis, between 0 and 100. "gradientRadial" is centered around the local origin, with a radius of 100 units. "texture" starts at the origin and extends out to the texture size. Members:  
				type: One of "solid", "gradientLinear", "gradientRadial", "texture". Required.
				r: Red color value, betwen 0 and 1. Available and required for "solid".
				g: Green color value, betwen 0 and 1. Available and required for "solid".
				b: Blue color value, betwen 0 and 1. Available and required for "solid".
				a: Alpha color value, betwen 0 and 1. Defaults to 1. Available for "solid".
				color: Gradient color values. A table containing a series of tables, with consecutive integer keys starting from 1. Each internal table contains color values and a gradient position. Available and required for "gradientLinear" and "gradientRadial". Members:
					r: Red color value, between 0 and 1. Required.
					g: Green color value, between 0 and 1. Required.
					b: Blue color value, between 0 and 1. Required.
					a: Alpha color value, between 0 and 1. Defaults to 1.
					position: Location within the gradient. Must be nondecreasing. Gradient automatically rescales to fit the largest position value seen.
				transform: An array containing the first two rows of a 3x3 transformation matrix. The third row is set to (0,0,1). Defaults to {1, 0, 0, 0, 1, 0}, representing the identity matrix. See Utility.Matrix.Create() for basic functionality. Available for "gradientLinear", "gradientRadial", and "texture".
				focus: Number between 0 and 1 controlling the center bias of radial gradients. Defaults to 0. Available for "gradientRadial".
				wrap: Control the bounds behavior of textures. Must be either "clamp" or "repeat". Defaults to "clamp". Available for "texture".
				smooth: Boolean controlling texture smoothing behavior. Defaults to true. Available for "texture".
				source: Source addon ID of the texture to be rendered. See Texture:SetTexture() for details. Available and required for "texture".
				texture: Texture identifier of the texture to be rendered. See Texture:SetTexture() for details. Available and required for "texture".

## Mask

### Mask:GetShape
Returns the current mask shape.

    shape = Mask:GetShape()   -- table <- void

#### Return Values:
**shape** - The current shape. See Canvas:SetShape() for details.  

### Mask:SetShape
Sets the mask shape. Note that masks with custom shapes will not cull mouse events according to the custom shape.

    Mask:SetShape(path)   -- nil
    Mask:SetShape(path)   -- table

#### Parameters:
**path** - A path. Consists of a table containing a series of tables, with consecutive integer keys starting from 1. Each internal table contains coordinates, either as an absolute value relative to the top-left corner of the frame, or Proportional to the frame size, as a number from 0 to 1. Giving both an absolute value and a proportional value for any coordinate is not allowed. Members:  
				x/xProportional: x coordinate of this point. Required.
				y/yProportional: y coordinate of this point. Required.
				xControl/xControlProportional: x coordinate of the curve control handle. If this exists, yControl/yControlProportional must also exist.
				yControl/yControlProportional: y coordinate of the curve control handle. If this exists, xControl/xControlProportional must also exist.

## RiftButton

### RiftButton:GetEnabled
Gets whether the button is enabled or grayed out.

    enabled = RiftButton:GetEnabled()   -- boolean <- void

#### Return Values:
**enabled** - The current enable state of this item.  

### RiftButton:GetSkin
Gets the current skin of this item.

    skin = RiftButton:GetSkin()   -- string <- void

#### Return Values:
**skin** - The current skin. Possible results include "default" and "close".  

### RiftButton:GetText
Gets this button's text.

    text = RiftButton:GetText()   -- string <- void

#### Return Values:
**text** - The current text of the element.  

### RiftButton:SetEnabled
Sets whether the button is enabled or grayed out.

    RiftButton:SetEnabled(enabled)   -- boolean

#### Parameters:
**enabled** - The new enable state of this item.  

### RiftButton:SetSkin
Sets the current skin of this item.

    RiftButton:SetSkin(skin)   -- string

#### Parameters:
**skin** - The new skin. Possible values are "default" and "close".  

### RiftButton:SetText
Sets this button's text.

    RiftButton:SetText(text)   -- string

#### Parameters:
**text** - The new text for the element.  

## RiftCheckbox

### RiftCheckbox:GetChecked
Gets whether the button is checked or not.

    checked = RiftCheckbox:GetChecked()   -- boolean <- void

#### Return Values:
**checked** - The current checked state of this item.  

### RiftCheckbox:GetEnabled
Gets whether the checkbox is enabled or grayed out.

    enabled = RiftCheckbox:GetEnabled()   -- boolean <- void

#### Return Values:
**enabled** - The current enable state of this item.  

### RiftCheckbox:SetChecked
Sets whether the checkbox is checked or not.

    RiftCheckbox:SetChecked(checked)   -- boolean

#### Parameters:
**checked** - The new checked state of this item.  

### RiftCheckbox:SetEnabled
Sets whether the checkbox is enabled or grayed out.

    RiftCheckbox:SetEnabled(enabled)   -- boolean

#### Parameters:
**enabled** - The new enable state of this item.  

## RiftScrollBar

### RiftScrollbar:GetEnabled
Gets whether the scrollbar is enabled or disabled.

    enabled = RiftScrollbar:GetEnabled()   -- boolean <- void

#### Return Values:
**enabled** - The current enable state of this item.  

### RiftScrollbar:GetOrientation
Gets the current orientation of the scrollbar.

    orientation = RiftScrollbar:GetOrientation()   -- string <- void

#### Return Values:
**orientation** - The current orientation. Valid results include "vertical" and "horizontal".  

### RiftScrollbar:GetPosition
Returns the current position of the scrollbar.

    position = RiftScrollbar:GetPosition()   -- number <- void

#### Return Values:
**position** - The current position of this scrollbar.  

### RiftScrollbar:GetRange
Returns the current range of the scrollbar.

    minimum, maximum = RiftScrollbar:GetRange()   -- number, number <- void

#### Return Values:
**maximum** - The maximum value for the position of this slider.  
**minimum** - The minimum value for the position of this slider.  

### RiftScrollbar:GetThickness
Returns the thickness of the scrollbar handle. Size is relative to the range.

    thickness = RiftScrollbar:GetThickness()   -- number <- void

#### Return Values:
**thickness** - The thickness of the handle.  

### RiftScrollbar:Nudge
Modify the scrollbar position, clamped to the current bounds.

    RiftScrollbar:Nudge(offset)   -- number

#### Parameters:
**offset** - The amount to nudge.  

### RiftScrollbar:NudgeDown
Shift the scrollbar down (i.e. away from zero) by a standard amount.

    RiftScrollbar:NudgeDown()   -- void

### RiftScrollbar:NudgeUp
Shift the scrollbar up (i.e. towards zero) by a standard amount.

    RiftScrollbar:NudgeUp()   -- void

### RiftScrollbar:SetEnabled
Sets whether the scrollbar is enabled or disabled.

    RiftScrollbar:SetEnabled(enabled)   -- boolean

#### Parameters:
**enabled** - The new enable state of this item.  

### RiftScrollbar:SetOrientation
Sets the orientation of the scrollbar.

    RiftScrollbar:SetOrientation(orientation)   -- string

#### Parameters:
**orientation** - The new orientation. Must be either "vertical" or "horizontal".  

### RiftScrollbar:SetPosition
Changes the current position of the scrollbar.

    RiftScrollbar:SetPosition(position)   -- number

#### Parameters:
**position** - The new position of this scrollbar. Must be within the current range.  

### RiftScrollbar:SetRange
Changes the current range of the scrollbar.

    RiftScrollbar:SetRange(minimum, maximum)   -- number, number

#### Parameters:
**maximum** - The new maximum value for the position of this slider. Must be an integer and larger than or equal to "minimum".  
**minimum** - The new minimum value for the position of this slider. Must be an integer and smaller than or equal to "maximum".  

### RiftScrollbar:SetThickness
Sets the current thickness of the scrollbar handle. Size is relative to the range.

    RiftScrollbar:SetThickness(thickness)   -- number

#### Parameters:
**thickness** - The new thickness of the handle.  

## RiftScrollbar Events

### Event.UI.Scrollbar.Change
Signals a change in scrollbar position.

    Event.UI.Scrollbar.Change(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Scrollbar.Grab
Signals that a scrollbar has been grabbed.

    Event.UI.Scrollbar.Grab(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Scrollbar.Release
Signals that a scrollbar has been released.

    Event.UI.Scrollbar.Release(handle)

#### Parameters:
**handle** - nil  

## RiftSlider

### RiftSlider:GetEnabled
Gets whether the slider is enabled or grayed out.

    enabled = RiftSlider:GetEnabled()   -- boolean <- void

#### Return Values:
**enabled** - The current enable state of this item.  

### RiftSlider:GetPosition
Returns the current position of the slider.

    position = RiftSlider:GetPosition()   -- number <- void

#### Return Values:
**position** - The current position of this slider.  

### RiftSlider:GetRange
Returns the current range of the slider.

    minimum, maximum = RiftSlider:GetRange()   -- number, number <- void

#### Return Values:
**maximum** - The maximum value for the position of this slider.  
**minimum** - The minimum value for the position of this slider.  

### RiftSlider:SetEnabled
Sets whether the slider is enabled or grayed out.

    RiftSlider:SetEnabled(enabled)   -- boolean

#### Parameters:
**enabled** - The new enable state of this item.  

### RiftSlider:SetPosition
Changes the current position of the slider.

    RiftSlider:SetPosition(position)   -- number

#### Parameters:
**position** - The new position of this slider. Must be within the current range.  

### RiftSlider:SetRange
Sets the current range of the slider.

    RiftSlider:SetRange(minimum, maximum)   -- number, number

#### Parameters:
**maximum** - The new maximum value for the position of this slider. Must be an integer and larger than or equal to "minimum".  
**minimum** - The new minimum value for the position of this slider. Must be an integer and smaller than or equal to "maximum".  

## RiftSlider Events

### Event.UI.Slider.Change
Signals a change in slider position.

    Event.UI.Slider.Change(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Slider.Grab
Signals that a slider has been grabbed.

    Event.UI.Slider.Grab(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Slider.Release
Signals that a slider has been released.

    Event.UI.Slider.Release(handle)

#### Parameters:
**handle** - nil  

## RiftTextfield

### RiftTextfield:GetCursor
Returns the current position of the cursor.

    cursor = RiftTextfield:GetCursor()   -- number <- void

#### Return Values:
**cursor** - The current position of this cursor. 0 indicates a cursor placed before any text.  

### RiftTextfield:GetSelection
Returns the current bounds of the selected text.

    begin, end = RiftTextfield:GetSelection()   -- number, number <- void

#### Return Values:
**begin** - The beginning of the selected text, in the same format GetCursor uses. nil if there is no text selected.  
**end** - The end of the selected text, in the same format GetCursor uses. nil if there is no text selected.  

### RiftTextfield:GetSelectionText
Get the current selected text for this element. Returns nil if no text has been selected.

    text = RiftTextfield:GetSelectionText()   -- string <- void

#### Return Values:
**text** - The current text of the element.  

### RiftTextfield:GetText
Get the current text for this element.

    text = RiftTextfield:GetText()   -- string <- void

#### Return Values:
**text** - The current text of the element.  

### RiftTextfield:SetCursor
Changes the current position of the cursor.

    RiftTextfield:SetCursor(cursor)   -- number

#### Parameters:
**cursor** - The new position of this cursor. Must be within the valid range.  

### RiftTextfield:SetSelection
Sets the current bounds of the selected text. Call with no arguments to remove the current selection.

    RiftTextfield:SetSelection()   -- void
    RiftTextfield:SetSelection(begin, end)   -- number, number

#### Parameters:
**begin** - The new beginning of the selected text, in the same format SetCursor uses. Must be an integer and smaller than "end".  
**end** - The new end of the selected text, in the same format SetCursor uses. Must be an integer and larger than "begin".  

### RiftTextfield:SetText
Set the current text for this element.

    RiftTextfield:SetText(text)   -- string

#### Parameters:
**text** - The new text for the element.  

## RiftTextFueld Events

### Event.UI.Textfield.Change
Signals a change in textfield contents.

    Event.UI.Textfield.Change(handle)

#### Parameters:
**handle** - nil  

### Event.UI.Textfield.Select
Signals a change in textfield selection.

    Event.UI.Textfield.Select(handle)

#### Parameters:
**handle** - nil  

## RiftWindow

### RiftWindow:GetBorder
Gets the element representing the window border.

    border = RiftWindow:GetBorder()   -- Element <- void

#### Return Values:
**border** - The window border.  

### RiftWindow:GetContent
Gets the element representing the window content area.

    content = RiftWindow:GetContent()   -- Element <- void

#### Return Values:
**content** - The window content area.  

### RiftWindow:GetController
Gets the ID of the current controller for this window.

    controller = RiftWindow:GetController()   -- string <- void

#### Return Values:
**controller** - "border" or "content", whichever dictates the dimensions of the other.  

### RiftWindow:GetTitle
Get the current title for this element.

    title = RiftWindow:GetTitle()   -- string <- void

#### Return Values:
**title** - The current title of the element.  

### RiftWindow:GetTrimDimensions
Gets the thicknesses of the border's visual trim.

    left, top, right, bottom = RiftWindow:GetTrimDimensions()   -- number, number, number, number <- void

#### Return Values:
**right** - The thickness of the right window border.  
**left** - The thickness of the left window border.  
**bottom** - The thickness of the bottom window border.  
**top** - The thickness of the top window border.  

### RiftWindow:SetController
Sets the current controller for this window. The controller will take on the exact dimensions of the RiftWindow object, and the other element will adjust accordingly.

    RiftWindow:SetController(controller)   -- string

#### Parameters:
**controller** - The new controller ID. May be either "border" or "content".  

#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.

### RiftWindow:SetTitle
Set the current title for this element.

    RiftWindow:SetTitle(title)   -- string

#### Parameters:
**title** - The new title for the element.  

## RiftWindowBorder

## RiftWindowContent

## Text

### Text:GetEffectBlur
Gets the current parameters for the Blur effect.

    effect = Text:GetEffectBlur()   -- table <- void
    effect = Text:GetEffectBlur()   -- nil <- void

#### Return Values:
**effect** - The current parameters for this effect. All members are optional. Pass nil to disable the effect.  

#### Returned Members:
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.  
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.  

### Text:GetEffectGlow
Gets the current parameters for the Glow effect.

    effect = Text:GetEffectGlow()   -- table <- void
    effect = Text:GetEffectGlow()   -- nil <- void

#### Return Values:
**effect** - The current parameters for this effect. All members are optional. Pass nil to disable the effect.  

#### Returned Members:
**colorA** - Controls the alpha channel of the glow effect. Defaults to 1.  
**offsetY** - Controls the glow offset along the Y axis. Defaults to 0.  
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.  
**strength** - Controls the strength of the glow. Defaults to 1.  
**offsetX** - Controls the glow offset along the X axis. Defaults to 0.  
**replace** - Hides the original text. Defaults to false.  
**knockout** - Enables the "knockout" effect. Defaults to false.  
**colorG** - Controls the green channel of the glow effect. Defaults to 0.  
**colorR** - Controls the red channel of the glow effect. Defaults to 0.  
**colorB** - Controls the blue channel of the glow effect. Defaults to 0.  
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.  

### Text:GetFont
Gets the current font used for this element.

    source, font = Text:GetFont()   -- string, string <- void

#### Return Values:
**font** - The actual font identifier. Either a resource identifier or a filename.  
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.  

### Text:GetFontColor
Gets the current font color for this element.

    r, g, b, a = Text:GetFontColor()   -- number, number, number, number <- void

#### Return Values:
**b** - Blue.  
**r** - Red.  
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.  
**g** - Green.  

### Text:GetFontSize
Gets the font size of the current element.

    fontsize = Text:GetFontSize()   -- number <- void

#### Return Values:
**fontsize** - The current font size of this element.  

### Text:GetText
Get the current text for this element.

    text, html = Text:GetText()   -- string, boolean <- void

#### Return Values:
**text** - The current text of the element.  
**html** - The current HTML flag.  

### Text:GetWordwrap
Gets the wordwrap flag for this element.

    wordwrap = Text:GetWordwrap()   -- boolean <- void

#### Return Values:
**wordwrap** - The current wordwrap flag for this element. If false, long lines will be truncated. Defaults to false.  

### Text:SetEffectBlur
Sets the parameters for the Blur effect.

    Text:SetEffectBlur(effect)   -- table
    Text:SetEffectBlur(effect)   -- nil

#### Parameters:
**effect** - The parameters for this effect. All members are optional. Pass nil to disable the effect.  

#### Returned Members:
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.  
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.  

### Text:SetEffectGlow
Sets the parameters for the Glow effect.

    Text:SetEffectGlow(effect)   -- table
    Text:SetEffectGlow(effect)   -- nil

#### Parameters:
**effect** - The parameters for this effect. All members are optional. Pass nil to disable the effect.  

#### Returned Members:
**colorA** - Controls the alpha channel of the glow effect. Defaults to 1.  
**offsetY** - Controls the glow offset along the Y axis. Defaults to 0.  
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.  
**strength** - Controls the strength of the glow. Defaults to 1.  
**offsetX** - Controls the glow offset along the X axis. Defaults to 0.  
**replace** - Hides the original text. Defaults to false.  
**knockout** - Enables the "knockout" effect. Defaults to false.  
**colorG** - Controls the green channel of the glow effect. Defaults to 0.  
**colorR** - Controls the red channel of the glow effect. Defaults to 0.  
**colorB** - Controls the blue channel of the glow effect. Defaults to 0.  
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.  

### Text:SetFont
Sets the current font used for this element.

    Text:SetFont(source, font)   -- string, string

#### Parameters:
**font** - The actual font identifier. Either a resource identifier or a filename.  
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.  

### Text:SetFontColor
Sets the current font color for this element.

    Text:SetFontColor(r, g, b)   -- number, number, number
    Text:SetFontColor(r, g, b, a)   -- number, number, number, number

#### Parameters:
**b** - Blue.  
**r** - Red.  
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.  
**g** - Green.  

### Text:SetFontSize
Sets the current font size of this element.

    Text:SetFontSize(fontsize)   -- number

#### Parameters:
**fontsize** - The new font size of this element.  

### Text:SetText
Sets the current text for this element.

    Text:SetText(text)   -- string
    Text:SetText(text, html)   -- string, boolean

#### Parameters:
**text** - The new text for the element.  
**html** - Enables HTML mode. In HTML mode, a limited number of formatting tags are available: &lt;u>, &lt;font color="#rrggbb">, and &lt;a lua="print('This is a lua script.')">.  

### Text:SetWordwrap
Sets the wordwrap flag for this element.

    Text:SetWordwrap(wordwrap)   -- boolean

#### Parameters:
**wordwrap** - The new wordwrap flag for this element. If false, long lines will be truncated. Defaults to false.  

## Texture

### Texture:GetTexture
Gets the current texture used for this element.

    source, texture = Texture:GetTexture()   -- string, string <- void

#### Return Values:
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.  
**texture** - The actual texture identifier. Either a resource identifier or a filename.  

### Texture:GetTextureHeight
Returns the actual pixel height of the current texture.

    height = Texture:GetTextureHeight()   -- number <- void

#### Return Values:
**height** - The height of the current texture in pixels.  

### Texture:GetTextureWidth
Returns the actual pixel width of the current texture.

    width = Texture:GetTextureWidth()   -- number <- void

#### Return Values:
**width** - The width of the current texture in pixels.  

### Texture:SetTexture
Sets the current texture used for this element.

    Texture:SetTexture(source, texture)   -- string, string

#### Parameters:
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.  
**texture** - The actual texture identifier. Either a resource identifier or a filename.  
