## Introduction

Yesterday, you created a photos app that could display photos using the flow layout. While the flow layout is nice, we want to wow our users by creating a much nicer photo app with a beautiful cover flow layout. 

*NOTE*: You can do this assignment by modifying the app that you wrote yesterday, or by building a standalone app if you didn't finish the assignment yesterday. 

## Cover Flow Layout

[Cover flow](https://www.google.ca/search?q=cover+flow&espv=2&source=lnms&tbm=isch&sa=X&ei=dR1HVK6jFoesyATYtYGYAw&ved=0CAYQ_AUoAQ&biw=1202&bih=932) is an old layout that used to exist in iTunes. You can still find it in Finder app (although the new version in Yosemite is more plain than previously), and you'll also find it at the very top of the featured sections of the iTunes and App Stores on iPad.

Your job is to recreate cover flow layout by implementing your own layout class. Good luck.

## Steps

#### Step 1

##### If Starting From Scratch

a) Create a new project, using the 'Single View Application' template. 

b) Go to the generated Storyboard and delete the view controller that's on it, add a collection view controller, instead. Make sure you set it as the 'initial view controller'. Add a distinct background color to the prototype cell that is on the storyboard so you can recognize it later.

c) Add a custom collection view controller subclass to the project. In the storyboard, change the class of the collection view controller you just added to the subclass you just created.

##### If Converting an Existing Collection View Project

Add a custom collection view controller subclass to the project. In the storyboard, change the class of the collection view controller you just added to the subclass you just created.

#### Step 2

a) Set your project's settings to only run in landscape. 

b) Create a subclass of `UICollectionViewFlowLayout` and name it "CoverFlowLayout". Go to your storyboard, under your collection view controller, select the layout object and set its class to `CoverFlowLayout`. 

#### Step 3 

In `CoverFlowLayout`, implement `prepareLayout`. As you might know, `UICollectionViewFlowLayout` has properties for scroll direction and item size. You need to calculate these in `prepareLayout`. 

Make sure your data source is returning some arbitrary number of cells to be displayed. If you run the app at this point, you'll see a horizontally scrolling list of items. 

#### Step 4

Now, things get interesting. Let's start implementing our cover flow effect by changing the size of each item based on its distance from the center of our collection view. To do this, you'll need to implement `layoutAttributesForElementsInRect:`. Here's a template you can fill out:

      NSArray* attributes = [super layoutAttributesForElementsInRect:rect];

      CGRect visibleRegion;
      visibleRegion.origin = self.collectionView.contentOffset;
      visibleRegion.size   = self.collectionView.bounds.size;

      // Modify the layout attributes as needed here

      return attributes;

To achieve this zooming effect, use the `transform3D` property on `UICollectionViewLayoutAttributes`. `transform3D` is part of the Core Animation framework. We'll talk about that later, but for now, refer to [this documentation](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/CoreAnimation_functions/index.html) for useful functions. To achieve the effect we want, you need to create a 'scaling' transform. 

While you're at it, let's also set the alpha values on our `UICollectionViewLayoutAttributes` objects - such that the further they are from the center, the more faded out they are. 

Once you're done, run the app and see what you get. 

You might notice that the first batch of cells appear correctly, but that as you scroll nothing seems to change. This is because the default behaviour of `UICollectionViewFlowLayout` is to keep its old layout data as its view port changes. You can (and should) override this behaviour in your subclass by returning YES from `shouldInvalidateLayoutForBoundsChange;`.

#### Step 5

The final step would be to add some rotation to our cells as the collection view scrolls. The 3D transform property you used previously can also be used for rotation. We want our cells to rotate around their y-axis. Once again, how much they have rotated depends on their distance from the center. Go through the documentation for Core Animation functions. Figure out how you can create a rotation transform and how you can combine it with the zoom transform from Step 4. 