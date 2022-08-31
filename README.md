# Instafilter
### **Day 62 of #100DaysOfSwiftUI**

Introduction to the project, creation of the app.

***How property wrappers become structs and how they act?***

When we use propert wrappers, such as @State, it actually creates a struct. Inside these structures there is a wrappedValue that we are changing. This value contains 'nonmutating' set method, so writing to it won't change the structure itself. That is why, by using slider to change value, didSet won't be triggered - value changes, but not the struct itself, which didSet is attached to.
