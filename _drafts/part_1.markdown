---
layout: post
title:  "How did they do that? Dissecting the Groove app part 1"
date:   2016-10-21 07:06:00 +0100
categories: ux uwp template10 c#
---

Welcome to the first part of the dissection of the Microsoft Groove app. In this first iteration I've chosen to create a shell which will contain the hamburger menu as well as the bottom play bar since these element feel global to the application. I'm going to leave the page headers to the actual pages, which is something that might be changed moving forward.

I create a new solution in Visual Studio 2015 and call it "GrooveSandbox". The next step is to add the Template10 framework by using it's Nuget package, by entering the command "Install-package Template10" into the Package Manager Console. The version of Template10 I'm using as I'm writing this is version 1.1.12 and I've stumbled upon some issues with it that requires some additional manual steps to properly function. Right click the solution in the Solution Explorer and choose Manage NuGet Packages for Solution and make sure to upgrade the Microsoft.NETCore.UniversalWindowsPlatform package to version 5.2.2.

Now I need to add the actual page which will be the application shell and contain the other pages while navigating, as well as the important hamburger menu. I feel that this page is as important as the App class and hence I put this Shell.xaml in the root of the project, the views however will be put in a folder called Views (I actually move the MainPage into that folder also, and don't forget to update the namespace accordingly). 

If you have followed along this quick creation you should now have a solution structure that looks like the image below.

<img src="/assets/images/SolutionStructure_part1.png" alt="The solution structure after initial creation" style="width: 500px;"/>

Now I'm going to update the existing files to leverage Template10 and apply som initial styling, lets begin with App.xaml:

```xml
<t10common:BootStrapper x:Class="GrooveSandbox.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:t10common="using:Template10.Common"
    RequestedTheme="Dark">

    <Application.Resources>

        <Color x:Key="GrooveDarkGray">#FF0E0D0D</Color>
        <Color x:Key="GrooveGray">#FF171717</Color>
        <Color x:Key="GrooveLightGray">#FF878380</Color>

        <SolidColorBrush x:Name="GrooveDarkGrayBrush" Color="{StaticResource GrooveDarkGray}"/>
        <SolidColorBrush x:Name="GrooveGrayBrush" Color="{StaticResource GrooveGray}"/>
        <SolidColorBrush x:Name="GrooveLightGrayBrush" Color="{StaticResource GrooveLightGray}"/>

    </Application.Resources>

</t10common:BootStrapper>
```

Examining the xaml above you'll see that I made some changes:

1. I now inherit the Template10.Common.BootStrapper instead of the ordinary Application
2. The RequestedTheme is specfied to Dark
3. There are a couple of color and brush resources which I've found in the screenshots of the Microsoft Groove app, there will be several more, but these will work for now.
4. I cleaned up some xmlns includes

Let's continue with the code behind file App.xaml.cs:

```c#
using System.Threading.Tasks;
using Template10.Common;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace GrooveSandbox
{
    sealed partial class App
    {
        public App()
        {
            this.InitializeComponent();
        }

        // Initialize the shell
            
        public override Task OnStartAsync(BootStrapper.StartKind startKind, IActivatedEventArgs args)
        {
            NavigationService.Navigate(typeof(Views.MainPage));
            return Task.FromResult<object>(null);
        }
    }
}
```

Important changes:

1. I removed the inheritance from Application
2. The BootStrapper requires the OnStartAsync method to be overriden 
3. The placeholder comment will be replaced later
4. Please note that I've already moved the MainPage view to the Views folder and updated the namespace to be GrooveSandbox.Views for that file

So how does the MainPage.xaml look like?

```xml
<Page x:Class="GrooveSandbox.Views.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:t10controls="using:Template10.Controls">

    <Grid>
        <t10controls:PageHeader Text="YOUR GROOVE" Background="{StaticResource GrooveGrayBrush}"/>
    </Grid>
</Page>
```

This page is very simple, I pretty much only cleaned it up and added add [PageHeader][pageheader-doc] control which comes from the Template10 framework. The code behind is even simpler:

```c#
namespace GrooveSandbox.Views
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
        }
    }
}
```

As you can see, there's nothing fancy here. If you where to run the application now you will end up with a very basic view:

<img src="/assets/images/MainPage_thumbnail.png" alt="The MainPage after initial creation"/>

Now we're going to add the code to the Shell.xaml as follows:

```xml
<Page x:Class="GrooveSandbox.Shell"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:t10controls="using:Template10.Controls">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>

        <t10controls:HamburgerMenu x:Name="Menu"
                                   AccentColor="{ThemeResource SystemAccentColor}"
                                   HamburgerBackground="{StaticResource GrooveGrayBrush}"
                                   NavAreaBackground="{StaticResource GrooveGrayBrush}"
                                   NavButtonCheckedForeground="{StaticResource GrooveLightGrayBrush}"
                                   NavButtonCheckedIndicatorBrush="{StaticResource GrooveLightGrayBrush}">

            <!-- Primary Buttons -->
            
            <!-- Secondary Buttons -->
            
        </t10controls:HamburgerMenu>

        <Grid Grid.Row="1" Height="64" Background="{StaticResource GrooveDarkGrayBrush}"/>

    </Grid>
</Page>
```

The page has two sections, the hamburger menu and the play bar in the bottom, but honestly I'm not really comfortable with this solution and it will be changed later since the hamburger menu should be allowed to overlay the playbar in some layouts of the application but after initial investigations I've come to the conclusion that this requires the hamburger menu control to be restyled which might be something for a later post. If you want to learn more about the hamburger menu and how Template10 have implemented it there are some documentation [here][hamburger-menu-doc]

I've tried to keep the page as clean as possible for now and hence I've only included placeholders for where the actual buttons will be placed later. Now let's update the Shell.xaml.cs code behind:

```c#
using Template10.Services.NavigationService;

namespace GrooveSandbox
{
    public sealed partial class Shell
    {
        public Shell(INavigationService navigationService)
        {
            this.InitializeComponent();
            Menu.NavigationService = navigationService;
        }
    }
}
```

The most important piece of this file is that the constructor now requires a navigationService parameter which will be passed to the HamburgerMenu, which also means we will have to instantiate this file differently than the ordinary navigation scheme. We will update the App.xaml.cs with the following snippet (which will replace the earlier placeholder):

```c#
        // Initialize the shell
        public override Task OnInitializeAsync(IActivatedEventArgs args)
        {
            var nav = NavigationServiceFactory(BackButton.Attach, ExistingContent.Include);
            Window.Current.Content = new Shell(nav);
            return Task.FromResult<object>(null);
        }
```

This snippet will override a method which will be called when the app is being initialized. In this method we create the navigationservice, making sure to attach to the BackButton of the current device (either software of hardware) and then pass this service to the instance of the shell created which we in turn add as the action content of the current Window.

Running the applicastion now you'll see the MainPage and also a working hamburger menu and we're closing in on this posts final parts.

<img src="/assets/images/MainPage2_thumbnail.png" alt="The MainPage after the shell has been created"/>

I'm going to add a couple of primary and secondary buttons as well to the hamburger menu, replace the placeholders in the Shell.xaml as follows:

```xml
            <!-- Primary Buttons -->
            <t10controls:HamburgerMenu.PrimaryButtons>

                <t10controls:HamburgerButtonInfo>
                    <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                        <SymbolIcon Symbol="Find" Width="48" Height="48"/>
                        <TextBlock Text="Search" Margin="12,0,0,0" VerticalAlignment="Center"/>
                    </StackPanel>
                </t10controls:HamburgerButtonInfo>

                <t10controls:HamburgerButtonInfo>
                    <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                        <SymbolIcon Symbol="Contact" Width="48" Height="48"/>
                        <TextBlock Text="Artists" Margin="12,0,0,0" VerticalAlignment="Center"/>
                    </StackPanel>
                </t10controls:HamburgerButtonInfo>

            </t10controls:HamburgerMenu.PrimaryButtons>
```

And also:

```xml
            <!-- Secondary Buttons -->
            <t10controls:HamburgerMenu.SecondaryButtons>

                <t10controls:HamburgerButtonInfo>
                    <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                        <SymbolIcon Symbol="AddFriend" Width="48" Height="48"/>
                        <TextBlock Text="Sign In" Margin="12,0,0,0" VerticalAlignment="Center"/>
                    </StackPanel>
                </t10controls:HamburgerButtonInfo>

            </t10controls:HamburgerMenu.SecondaryButtons>
```

Now you should be able to run the application and it will display the primary buttons in top of the hamburger menu and the secondary button will be placed beneath the menu separator. We are far from done, but have made some serious progress.

<img src="/assets/images/MainPage3_thumbnail.png" alt="The menu now has some buttons"/>

If you want to examine the final solution for this chapter it is available on [GitHub][groovesandbox-github]

[hamburger-menu-doc]:   https://github.com/Windows-XAML/Template10/wiki/Controls#hamburgermenu
[pageheader-doc]:       https://github.com/Windows-XAML/Template10/wiki/Controls#pageheader
[groovesandbox-github]: https://github.com/coderox/GrooveSandbox/tree/part_1