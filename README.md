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

***Wrapping a UIViewController in a SwiftUI view***

Important note about UIKit:

1. UIView - parent class of all views;
2. UIViewController - brings views to life;
3. Delegation - decides where work happens.

To wrap UIViewController in a SwiftUI view, we must create a structure that conforms to UIViewControllerRepresentable protocol. To do so, structure must have two functions - makeUIViewController and updateUIViewcontroller.

### **Day 64 of #100DaysOfSwiftUI**

***Using coordinators to manage SwiftUI view controllers***

On Day 63 we managed to display PHPickerViewController, but we were not able to select photos or cancel the VC. Thus, we must use coordinators, which act like delegates for UIKit ViewControllers. Delegates are objects that respond to events occuring elsewhere.

How to add coordinators:

1. Create Coordinator class and make sure it is capable of acting like a delegate.
```
class Coordinator: NSObject, PHPickerViewControllerDelegate { }
```
2. Make the coordinator.
```
func makeCoordinator() -> Coordinator {
    Coordinator()
```
3. Ask PHPickerViewController to tell our coordinator when something happens.
```
picker.delegate = Context.coordinator
```
4. Now we can finally make sure that our coordinator conforms to PHPickerViewControllerDelegate by giving it didFinishPicking function.
```
func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
    // Tell picker to dismiss
    picker.dismiss(animated: true)
    
    // Exit if no selection was made
    guard let provider = results.first?.itemProvider else { return }
    
    // If this has an image, use it
    if provider.canLoadObject(ofClass: UIImage.self) {
        provider.loadObject(ofClass: UIImage.self) { image, error in
            self.parent.image = image as? UIImage
        }
    }
}
```

***Complete process summed up:***

1. We create a SwiftUI view that conforms to UIViewControllerRepresentable.
2. We give it a makeUIViewController() method that creates some sort of ViewController, for example, PHPickerViewController.
3. Add nested class for Coordinator to act as a bridge between UIKIt VC and SwiftUI view.
4. Give Coordinator didFinishPicking method, which is triggered when image is selected.
5. Add @Binding property to our ImagePicker, so it can send changesback to its parent view.
