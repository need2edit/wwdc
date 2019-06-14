# WWDC 2019
My favorite links, assets, sample code, and thoughts from WWDC 2019.

# Must See Talks

### Creating Swift Packages
https://developer.apple.com/videos/play/wwdc2019/410/

### Adopting Swift Packages in Xcode
https://developer.apple.com/videos/play/wwdc2019/408/

### Modern Swift API Design
https://developer.apple.com/videos/play/wwdc2019/415/

# Being Pragmatic When You Program

There is no silver bullet. You can use delegates, you can use callbacks, you can use functional reactive programming. Anything that solves the problem is fair game. That said, its important to think about a few things when approaching your problem:

#### Will anyone else be using your code?

Deciding on how to organize your project and the design pattern(s) you use to implement your solutions might need to be understood and worked on in a collaborative way. What if a new developer is onboarded? Will they understand your clever and advanced design pattern? Is there a QA team involved? Perhaps a non-engineer that needs to write tests? 

If your code will be used and maintained in a team, it is worth documenting your code well and using widely understood design patterns. Don't be clever and opt for clarity.

#### How long will this project need to last?

Everything has a shelf life. Is this just a hobby project? A proof of concept? Production use in front of customers for years to come? Be careful to balance delivery time, testing needs, and maintence requirements with whatever your building.

#### Simplify. ~Simplify. Simplify.~

Sometimes you just need to get things working. Don't get destracted by the "new and shinny" design pattern that just started generating some buzz. You can always refactor, you can always ask someone to review your code. Don't over think things and let the perfect be the enemy of the good. 

#### Think about Performance

No one likes the loading indicator. You may need to consider different perormance needs depending on your use case. Here are a few tips:

Understand the interaction between a client app and a web service. How often does something need to refresh? If you see slowness, is it how your app is processing the data? Is it a Wifi or Cellular issue? Is it slowness coming from the server? Is it a combo of all three? Seemingly little inefficencies add up in your projects and can cause a poor user experience if not respected. 

Learn tools for measuring performance. I use Xcode to compare the speed of functions when I need to zoom in on performance. I might have 2 different ways to search a library of content. In one recent case, my solution using a `for loop` ended up being 50x faster than using Swift `.filter`.

If you are implementing a search feature, think of how often you are asking a server for results. Use throttling techniques to wait until the user pauses for a moment before running the search. This reduces the load on the network.

# Patterns I Like

## Coordinators
I try to use a coordinator pattern with most projects. This keeps my view controllers small and "dumb". Let's consider a shopping cart example:

1. Add or remove items to an order
2. Review the order
3. Confirm the order

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
