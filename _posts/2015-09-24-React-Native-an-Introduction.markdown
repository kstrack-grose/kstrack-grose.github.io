---
layout: post
title:  "React Native: an Introduction"
date:   2015-09-24
categories: jekyll update
---
##Introduction
[React Native](https://facebook.github.io/react-native/) is a mobile JavaScript framework, based on React. As a side note, both React and React Native were created by **facebook**. This post is going to be a basic introduction to what React Native is and a few key features and best practies.

Essentially, React Native is a view-only front end framework. You can pair it with other front end frameworks like Flux, Redux, or Reflux in order to flesh out the 'M' and 'C' in MVC, but it's definitely possible to write a React Native-only application if you don't mind putting a fair amount of logic in  your views.

##Key Features and Best Practices
###Virtual Dom
The feature that I've heard about the most is the idea of the *virtual DOM*. There's a lot of complexity behind the scenes, but the essential idea is that each view, as the app switches from one to another, is analyzed for changes in the DOM. Only the changes are rendered. Think GitHub diffing for your mobile app views! The Virtual DOM makes loading each view much more efficient.

###Best Practices
 * ECMAScript 6. React Native works perfectly well with ES 5, but a lot of the features in ES 6 (context binding, strict mode, etc) are awesome and React Native fully supports ES 6.
 * Your HTML will be in your JSX files. It's a bit hard to get used to, but ends up being very useful. You can keep track of everything in the view in a single file (aside from abstracting away modular bits within the view, of course) and it compounds the feeling of each view being its own whole and separate entity.
 * Stylesheet components. These are similiar to CSS but are not straight-up CSS. These, too, are included in each JSX file, which, once more, is more useful than you might expect. The syntax for creating a Stylesheet will be laid out in the example I'll point you to.
 * Alphabetize everything. This includes your stylesheet components and the React Native components you include at the top. This makes searching for specific components quick and efficient.

##Compiling and Running
Xcode compiles your JSX files into Objective-C or Swift. I've primarily used React Native to build iOS applications, so I'm not overly familiar with the complication process to run your JSX as Java (for Android devices), but I know it's possible with React Native 0.11.0.

Xcode can be very finicky. The best thing I can suggest is to always read the README files for every external module you import. The steps for properly importing the module should be included there and following those will minimize Xcode bugs.

Once your code is compiled, you can run your project, and Xcode will open up the iOS simulator. Except for phone-specific modules like react-native-camera, the simulator will be an accurate and helpful representation of what your app would look like on a phone!

You can also get your app on your phone using Xcode, but this is a bit of an involved process. I may write a follow-up post about it, because it's definitely out of the scope of this post.

##Example
To find the official example, go to the [React Native site](https://facebook.github.io/react-native/docs/tutorial.html#content). However, the example doesn't follow the best practices as I've found in other examples and as I've laid out above. I'll write a follow-up post with actual code as my time allows.

##Conclusion
This was a very high-concept introduction to React Native, so keep an eye out as I continue to post about React Native with specific examples. As always, email me with questions/comments/concerns.