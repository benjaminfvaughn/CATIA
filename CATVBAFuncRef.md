CATIA automation is powerful and relatively simple if you're already familiar VBA. The challenge is learning which methods are available and how to use them. This list is my running record of methods that I have used in CATIA and their parent object. The notes were written from my experience using the function and what I though was important from the CAA API Documentation.

*This is a list of CATVBA functions organized by object parent. The list also contains terse and basic implementation notes. Search this list using Ctrl+F. Pull requests are welcome.*

# Object
CATIA-specific construct that has properties and methods/functions.
## Method
Methods that are members of the object.

# Body
A body is the solid model output of the .CATPart. It is modified by boolean operations that add, remove, or intersect volumes or other solids.
## AddNewPad()
* Add a pad to an existing body
* Requires sketch

## AddNewPadFromRef()
* Add a pad to an existing body
* Creates pad from non-sketch wireframe geometry

# HybridBody
A wireframe, surface geometry, or organizational (abstract component) of a part.
## Name
* Read of change the name of HybridBody

# HybridBodies
A collection of the features that comprise wireframe, surface geometry, or organizational (abstract component) in a part.
## Add
* Create a new HybridBody as a member of the specified HybridBodies collection.
* Usually to create a geometrical set.

# HybridShapeFactory
The hybrid shape factory is an abstract object that provides access to generate and modify geometry. Think of HybridShapeFactory like a library/toolbox.
## AddNewFill()
* Create a fill

## AddNewCircleCtrRad()
* Create a new circle with center (ctr) and radius (rad).

**Example:**
<code>Set hybridShapeCircleCtrRad = hybridShapeFactory.AddNewCircleCtrRad(centerPoint, planeRefYZ, False, radius)</code>

# Part
## FindObjectByName()
* Locate an item in the feature tree by searching for its name. No need to understand the structure/organization of the feature tree.

**Example:**
<code>Set myPoint = myPart.FindObjectByName("MyPointInFeatureTree")</code>

## InWorkObject
* Change the current in work object.

## Name
* Return the name of a part object.

## Update
* Update the entire part.

## UpdateObject
* Update an individual element in the feature tree.
* Usable on geometrical sets and individual elements.

**Example:**
<code>part.UpdateObject geoSet 'Update all the features in a geometrical set "geoSet".</code>

# Selection
## SelectElement2()
SelectElement2 is the simplest member of the SelectElement family alongside SelectElement3 and SelectElement4. It is a one-stop-shop for picking elements from the CATIA Feature Tree. It allows you to prompt the user to pick an element from the Feature Tree while limiting the users possible selections with a filter and updating the CATIA status bar to inform the user that the application is waiting for their input. The filter must be provided to the function as an array type. The array should contain a string corresponding to the type of tree elements that you wish to be selectable. I have found that it is best to create the array without a specified type (variant.)

* Interactive selection function
* Inputs: filter [array], status bar message [string], ObjectSelectionBeforeCommandUsePossibility [boolean]
* Output: selection

**Example:**
<pre><code>Private Sub btnRefPoint_Click()
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

End Sub</code></pre>
