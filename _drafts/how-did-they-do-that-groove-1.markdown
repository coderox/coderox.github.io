---
layout: post
title:  "How did they do that? Dissecting the Groove app part 0"
date:   2016-10-21 07:06:00 +0100
categories: ux uwp c#
---

In a series of posts I'm going to try and mimic the user experience of one of my favourite apps when it comes to responsive and adaptive layout in Universal Windows Apps, the Microsoft Groove app. The app has a very fluid interface, leverages high resolution graphics as backgrounds and feels very native on my Windows 10 machine.

There are several aspects of the Microsoft Groove app which I find is interesting but I'm going to start it off by simply creating the overall base architecture and structure, including some of the key visual elements such as the hamburger menu and shell. In later posts I hope to be able to further customize the look and feel of some elements that doesn't fully use the default behaviors or visuals of the UWP platforms such as it's presented and packaged for the overall developer community.

Here are a couple of screenshots which displays the overall layout of the Microsoft Groove app in three states, narrow, windowed and fullscreen.

![The full screen version of Groove](/assets/images/GrooveLarge_thumbnail.png)

This full screened version of the app displays the full hamburger menu opened (it can be closed) and also hides the page headers in the pages, replaces them with a cleaner title. It also adds an extra menu option to the hamburger menu to search (which otherwise is available in the page header of the pages). The hamburger menu also impacts the page layout, meaning that it always shows the full page content independet of the menu state.

![The windowed version of Groove](/assets/images/GrooveMedium_thumbnail.png)

The windowed version, well honestly the actual state more depends on some specific dimensions of the window, shows the hamburger menu in it's closed state, when opening the hamburger menu it overlays and hides the left side of the actual content of the page.

![The narrow version of Groove](/assets/images/GrooveSmall_thumbnail.png)

The most narrow version, when the width of the windows is smaller than 768, hides the entire hamburger menu except the actual hamburger button. Tt also shows a smaller page header which includes a button to trigger search in the app, also hiding the search menu effectively from the hamburger menu. When the app is narrower than 468 pixels it also hides some of the more secondary controls in the play bar in the bottom of the window.

This is what I'm going to try and mimic in this first version which will not work with any real or live data, instead use very simple and view specific models if needed. I want to focus on the UX and layout of native Windows 10 UWP apps. I have hence decided to not even bother (at least at this stage) to examine or discuss the information architecture of the app, I merely want to try to create an app that feels native and follows behaviors like other native apps implement, in my opinion this is good for the overall platform and still leaves room for the possibility to shine and be consistent with ones own brand, colors and look and feel.

I'm going to leverage a framework called [Template10][template10-github] which I've used while developing real life client solutions and it has proven to be both solid and efficient in my opinion. It has some occassional glitches left but the team working on it are constantly doing improvements and are open-minded about their work and roadmap. 

I'm also going to keep all of the source code online on [GitHub][groovesandbox-github] if you want to examine the result and possibly even contribute.

Does this sound interesting, want to follow along? Let's get started with the first part!

[template10-github]: http://github.com/Windows-XAML/Template10
[groovesandbox-github]: https://github.com/coderox/GrooveSandbox