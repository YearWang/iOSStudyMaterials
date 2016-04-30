# What's the difference between all the Selection Segues?

---

> Show
Show Detail
Present Modally
Popover presentation
Custom

###Show 
Pushes the destination view controller onto the navigation stack, moving the source view controller out of the way (destination slides overtop from right to left), providing a back button to navigate back to the source - on all devices
>Example: Navigating inboxes/folders in Mail

###Show Detail 
Replaces the detail/secondary view controller when in a UISplitViewController with no ability to navigate back to the previous view controller
>Example: In Mail on iPad in landscape, tapping an email in the sidebar replaces the view controller on the right to show the new email

##Present Modally 
Presents a view controller in various different ways as defined by the Presentation option, covering up the previous view controller - most commonly used to present a view controller that animates up from the bottom and covers the entire screen on iPhone, but on iPad it's common to present it as a centered box overtop that darkens the underlying view controller and also animates up from the bottom
>Example: Tapping the + button in Calendar on iPhone

##Popover Presentation
When run on iPad, the destination appears in a small popover, and tapping anywhere outside of this popover will dismiss it. On iPhone, popovers are supported as well but by default if it performs a Popover Presentation segue, it will present the destination view controller modally over the full screen.
>Example: Tapping the + button in Calendar on iPad (or iPhone, realizing it is converted to a full screen presentation as opposed to an actual popover)

##Custom
You may implement your own custom segue and have control over its behavior.

The deprecated segues are essentially the non-adaptive equivalent of those described above. These segue types are deprecated in iOS 8: Push, Modal, Popover, Replace.

For more info, you may check out Building Adaptive Apps with UIKit - Session 216, that Apple presented at WWDC 2014. They talked about how you can build adaptive apps using these new Adaptive Segues, and they built a demo project that utilizes these segues.




