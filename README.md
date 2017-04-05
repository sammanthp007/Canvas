# Canvas


### Assets

You can download the Canvas assets [here](https://www.dropbox.com/s/94x5xry9ib3xc3w/Canvas%20Assets.zip).

## Milestones

### Setup:

* Create a new project (disable Auto Layout).
  [Guide](https://courses.codepath.com/courses/ios_university/pages/new_project)
* Add the image assets above.
  [Guide](https://github.com/codepath/ios_guides/wiki/Adding-Image-Assets)
* When working with coordinates, it might be helpful to review the [UIKit
  coordinate
  system](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW5).

## 1. Add and Configure View Elements

* Create a custom ViewControllerSwift file, `CanvasViewController` and associate
  it with the ViewController in Storyboard. [Creating Custom View
  Controllers](https://courses.codepath.com/courses/ios_university/pages/creating_custom_view_controllers)
* Add a UIView to the bottom of the ViewController to serve as a "tray".
* Add 6 smiley images to the tray along with the down arrow icon. [Adding Image
  View from Media Library Add Tray and
  faces](https://guides.codepath.com/ios/Using-UIImageView#adding-imageview-with-image-from-media-library)

<img src="http://i.imgur.com/2a7GnL9.gif" title='Drag a pan gesture recognizer onto the tray view' alt='Drag a pan gesture recognizer onto the tray view'/>

## 2. Make the Tray Draggable

* Create an outlet for the trayView, `trayView`.
* At the top of the file, where you create your outlets, create a "global" variable to store the original center of the trayView: `var trayOriginalCenter: CGPoint!`
* Drag a UIPanGestureRecognizer onto the tray view.
HINT: You can see which view the Gesture Recognizer is attached to by ctrl +
Clicking on the Gesture Recognizer in the Document Outline.

<img src="http://i.imgur.com/Yv8WOcP.gif" title='Drag a pan gesture recognizer
onto the tray view' alt='Drag a pan gesture recognizer onto the tray view'/>

* Ctrl-drag from the Pan Gesture Recognizer to the CanvasViewController swift file to create an action. Be sure to change the type to UIPanGestureRecognizer. You can name the Action, `didPanTray`. 

<img src="http://i.imgur.com/ddu28U5.gif" title='Create action from Pan Gesture
Recognize' alt='Create action from Pan Gesture Recognize'/>

**Within** the `didPanTray(_:)` method, access the translation parameter of the
UIPanGestureRecocognizer and store it in a constant. [Pan Gesture Recognizer](https://guides.codepath.com/ios/Using-Gesture-Recognizers#example-2-pan-gesture-recognizer-1)

```
let translation = sender.translation(in: view)
```

* Within the `didPanTray(_:)` method, after you have accessed the translation,
  **create a conditional statement to check the current gesture state during
  the pan:** `.began`, `.changed` or `.ended` [Pan Gesture
  Recognizer](https://guides.codepath.com/ios/Using-Gesture-Recognizers#example-2-pan-gesture-recognizer-1)
* When the gesture begins (`.began`), store the tray's center into the
  trayOriginalCenter variable:
```
trayOriginalCenter = trayView.center
```
* As the user pans (.changed), change the trayView.center by the translation. Note: we ignore the x translation because we only want the tray to move up and down:
trayView.center = CGPoint(x: trayOriginalCenter.x, y: trayOriginalCenter.y + translation.y)

## 3. Tray Behavior

<img src="http://i.imgur.com/P4v6iHa.gif" title='pull up tray' alt='pull up
tray'/>

Upon release (`.ended`), the tray should either snap to the open or closed
position, depending on the velocity. In other words, if the tray was moving up,
animate it to the open position position. If it was moving down, animate it to
the closed position.

* Get the velocity of the [pan gesture
  recognizer](https://guides.codepath.com/ios/Using-Gesture-Recognizers#example-pan-gesture-recognizer):
```
var velocity = sender.velocity(in: view)
```

* Create two new class variables to store the tray's position when it's "up"
  and "down" as well as a variable for the offset amount that the tray will
  move down.
```
var trayDownOffset: CGFloat!
var trayUp: CGPoint!
var trayDown: CGPoint!
```

* Assign values to the `trayDownOffset`, `trayUp` and `trayDown` variables in
  `viewDidLoad()`. The trayDownOffset will dictate how much the tray moves
  down. 160 worked for my tray, but you will likely have to adjust this value
  to accommodate the specific size of your tray.
```
trayDownOffset = 160
trayUp = trayView.center
trayDown = CGPoint(x: trayView.center.x ,y: trayView.center.y + trayDownOffset)
```
* For the gesture state, `.ended`, create a conditional statement to check the y component of the velocity.
* If the `velocity.y` is greater than 0, it's moving down. Otherwise, it's moving up.
    * In the case that the tray is moving down, animate the tray position to
      the `trayDown` point, otherwise, animate it towards the `trayUp` point.
      [Animating View
      Properties](https://guides.codepath.com/ios/Animating-View-Properties)
      **Optional**: Try animating the ending tray motion with a bounce using
      the damping ratio and initial spring velocity. [Spring
      Animation](https://guides.codepath.com/ios/Animating-View-Properties#spring-animation)

## 4. Creating Faces

Creating Faces gif

In Storyboard, drag a pan gesture recognizer onto each smiley face. (Make sure that User Interaction is enabled in the view properties for each face.)
Adding gestures to faces

Create an action from any of the face pan gesture recognizers. After you create the first action, you can drag the other pan gesture recognizers onto the same method, so they can share it.
Sharing methods

We'll be creating new faces, so we'll need to create a variable to keep track of the new face. Add a variable of type UIImageView to the top of the file:

var newlyCreatedFace: UIImageView!
Add a conditional statement with conditions for the gesture states, .began, .changed, and ended.
In the began state (.began) of the Pan Gesture Recognizer, we will create a new image view that contains the same image as the view that was panned on. Conveniently, Gesture Recognizers know the view they are attached to! Guide: programmatically Creating Views

var imageView = sender.view as! UIImageView. imageView now refers to the face that you panned on.
newlyCreatedFace = UIImageView(image: imageView.image). Create a new image view that has the same image as the one you're currently panning.
view.addSubview(newlyCreatedFace). Add the new face to the main view.
newlyCreatedFace.center = imageView.center. Initialize the position of the new face.
newlyCreatedFace.center.y += trayView.frame.origin.y. Since the original face is in the tray, but the new face is in the main view, you have to offset the coordinates.
Now that the new face has been created, we want to actually pan it's position. As with the tray pan, you will want to create another variable at the top of the file to capture the initial center of the new face.*

var newlyCreatedFaceOriginalCenter: CGPoint!
Get the Translation from the Pan Gesture Recognizer. Pan Gesture Recognizer
Back in the began state .began, set newlyCreatedFaceOriginalCenter = newlyCreatedFace.center.
In the changed state .changed, we want to pan the position of the newlyCreatedFace.
newlyCreatedFace.center = CGPoint(x: newlyCreatedFaceOriginalCenter.x + translation.x, y: newlyCreatedFaceOriginalCenter.y + translation.y)
Optional Simulate picking up and dropping the new face from the tray to the canvas. When the new face is panned from the tray, scale it up a little. When the panning stops, scale it back down.

In the .began state of the Pan Gesture, animate the scale of the face to be a little larger. Animating View Properties, Using View Transforms
When panning ends, .ended, animate the scale back to normal, or slightly larger (Designers choice). This is a good candidate for the spring animation to simulate the "drop"! Animating View Properties, Applying Scale Transform
5. Panning Faces on the Canvas

Panning faces on Canvas gif

After you create the new face, programmatically create and add a UIPanGestureRecognizer to the newly created face. The guide demonstrates adding the gesture recognizer in viewDidLoad, however you will add your Pan Gesture Recognizer where you're creating the new image view.
You will also need to set newlyCreatedFace.userInteractionEnabled = true in order for the Gesture Recognizer to work.
Create a new function to handle panning the faces on the canvas. It should have the same name as the action: when you created the Pan Gesture Recognizer. REMEMBER: Do not create this function within any other function, give it it's own space in the file! Implement the Event Handler Method
Get the translation from the Gesture Recognizer. Pan Gesture Recognizer
Add a conditional statement with conditions for the gesture states, .began, .changed, and .ended.
When panning begins, (.began)...
Set newlyCreatedFace = sender.view as! UIImageView to get the face that we panned on.
Set newlyCreatedFaceOriginalCenter = newlyCreatedFace.center so we can offset by translation later.
While in the process of panning, (.changed)...
Pan the face around,
   newlyCreatedFace.center = CGPoint(x: newlyCreatedFaceOriginalCenter.x + translation.x, y: newlyCreatedFaceOriginalCenter.y + translation.y)
6. Bonus: Scaling Faces

After you create the new face, programmatically create and add a UIPinchGestureRecognizer to the newly created face. Instantiate the Pinch Gesture Recognizer just like you did the pan gesture recognizer.
Create a method to be called when the Pinch Gesture Recognizer is activated.
Use the scale property of the pinch gesture recognizer to set a scale transform on the view.
7. Bonus: Rotating Faces

After you create the new face, programmatically add a UIRotateGestureRecognizer to the newly created face.
Guide: Working with Simultaneous Gesture Recognizers
8. The Tray Arrow

The arrow should rotate as the tray travels between the open position and the closed position. Hint: Set the transform in the pan gesture in the change state. In the end state, animate it to the up or down position. Guide: Using View Transforms
9. Bonus: Dropping a Face on the Tray

If the user drops a newly created face on the tray, the face should animate back to where it came from.
10. Bonus: Deleting Faces

Double tap to delete a face.
Add a tap gesture recognizer and set the number of taps to recognize to 2.
To remove a view from its superview just do something like, faceView.removeFromSuperview()
11. Bonus: Frictional Tray

If the user attempts to drag the tray above the open position, it should began a frictional drag. In other words, only move the tray 1 pixel for every 10 pixels the user's finger moves.
