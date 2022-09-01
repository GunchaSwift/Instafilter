# Instafilter
### **Day 62 of #100DaysOfSwiftUI**

Introduction to the project, creation of the app.

***How property wrappers become structs and how they act?***

When we use propert wrappers, such as @State, it actually creates a struct. Inside these structures there is a wrappedValue that we are changing. This value contains 'nonmutating' set method, so writing to it won't change the structure itself. That is why, by using slider to change value, didSet won't be triggered - value changes, but not the struct itself, which didSet is attached to.

***Responding to state changes using onChange().***

Because of the way SwiftUI sends binding updates, we must use .onChange() modifier, which tells when something changes. It can be used anywhere in view hierarchy like this:
```
.onChange(of: changedValue) { newValue in
    print("New value is \(newValue)")
}
```

***Showing multiple options with confirmationDialog().***

This .confirmationDialog() differs from alert(), because it slides from bottom, can contain multiple buttons and can be dismissed by tapping outside the options. The way they are created, however, is almost identical.
```
.confirmationDialog("Change background", isPresented: $showingConfirmation) {
     Button("Red") { backgroundColor = .red }
     Button("Green") { backgroundColor = .green }
     Button("Blue") { backgroundColor = .blue }
     Button("Cancel", role: .cancel) { }
} message: {
     Text("Select a new color")
}
```

### **Day 63 of #100DaysOfSwiftUI**

***Integrating Core Image with SwiftUI***

CoreImage is builtin SwiftUI framework used to manipulate images - apply blurs, filters and so on. To get meaningful results out of CoreImage, we must know 4 types of images:

1. SwiftUI Image - used to represent the image in SwiftUI;
2. UIImage - UIKit image, powerful;
3. CGImage - CoreGraphics, two dimensional array of pixels;
4. CIImage - CoreImage, stores information about the image, like "image recipe".

Interoperability of these image types:

UIImage <-> CGImage

UIImage -> CIImage <-> CGImage

CGImage -> SwiftUI Image <-> UIImage
