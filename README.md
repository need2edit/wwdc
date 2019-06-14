# WWDC 2019
My favorite links, assets, sample code, and thoughts from WWDC 2019.

# Must See Talks

### Creating Swift Packages
https://developer.apple.com/videos/play/wwdc2019/410/

### Adopting Swift Packages in Xcode
https://developer.apple.com/videos/play/wwdc2019/408/

### Modern Swift API Design
https://developer.apple.com/videos/play/wwdc2019/415/

# Patterns I Like


## Coordinators
I try to use a coordinator pattern with most projects. This keeps my view controllers small and "dumb". Let's consider a shopping cart example:

- Add or remove items to an order
- Review the order
- Confirm the order

This process could be represented by 3 distinct screens. Each screen should not know about the other if we respect the single responsibility guideline. 

```swift

// This will manage our 3 step process, keeping each view controller small and 'dumb'
class OrderCoordinator {
    
    var order: OrderRequest!
    
    let screen: UINavigationController!
    
    init() {
        let createOrderVC = CreateOrderViewController(order: order, delegate: self)
        self.screen = UINavigationController(rootViewController: createOrderVC)
    }
}
```

I create a higher level object that in charge of managing the process. It has a reference to the "order" and each time a screen performs an action, it asks the coordinator what to do next.

```swift

func controllerDidRequestCheckout(_ controller: CreateOrderViewController) {
    // now we need to checkout
}

func controllerDidConfirmCheckout(_ controller: ReviewOrderViewController) {
    // now we need to checkout since the user confirmed
}

func controllerDidCompleteCheckout(_ controller: ReviewOrderViewController) {
    // we complete the order process, dismissing the 
}

```
