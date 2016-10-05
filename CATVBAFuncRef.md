This is a list of CATVBA functions organized by the features that they modify. The list also contains terse, useful implementation notes.

# HybridShapeFactory
The hybrid shape factory is an abstract object that provides access to generate and modify geometry. Think of HybridShapeFactory like a library/toolbox.
## AddNewFill()
* Create a fill

# Pad, creation
## AddNewPad()
* Requires sketch

## AddNewPadFromRef()
* Creates pad from non-sketch wireframe geometry

# Part
## InWorkObject
* Change the current in work object.

## Name
* Return the name of a part object.

## Update
* Update the entire part.

## UpdateObject
* Update an individual element in the feature tree.

# Selection
## SelectElement2
SelectElement2 is the simplest member of the SelectElement family alongside SelectElement3 and SelectElement4. It is a one-stop-shop for picking elements from the CATIA Feature Tree. It allows you to promt the user to pick an element from the Feature Tree while limiting the users possible selections with a filter and updating the CATIA status bar to inform the user that the application is waiting for their input. The filter must be provided to the function as an array type. The array should contain a string corresponding to the type of tree elements that you wish to be selectable. I have found that it is best to create the array without a specified type (variant.)
* Interactive selection function
* Inputs: filter [array], status bar message [string], ObjectSelectionBeforeCommandUsePossibility [boolean]
* Output: selection

**Example:**
<pre><code>
Private Sub btnRefPoint_Click()
'user select the reference point
Dim sel
Set sel = CATIA.ActiveDocument.selection

Dim FilterType(0)
FilterType(0) = "HybridShape"

Me.Hide
Call sel.SelectElement2(FilterType, "Select the reference point", False)
Set centerPoint = sel.Item2(1).Value
Me.Show
RefPointTextBox.Text = centerPoint.Name

Call checkForOk

End Sub
</code></pre>
