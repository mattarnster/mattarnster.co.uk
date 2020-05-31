---
title: "RadarrApp - My First UWP Application"
date: 2020-05-28T11:31:24-04:00
summary: A look at the first UWP application I have built and submitted to the Microsoft Store.
draft: false
---

GitHub Repo: [mattarnster/radarr-uwp-app](https://github.com/mattarnster/radarr-uwp-app)

Microsoft Store link: <https://www.microsoft.com/en-us/p/radarrapp/9p89t3x4ml77>

***

A few weeks ago, I came up with the idea to develop my first UWP app for the Windows Store.

I wanted a simple application to view my [Radarr](https://radarr.video) library from my desktop so that I don't have to keep opening up a web-browser to keep tabs on what was in the queue and what has and hasn't been downloaded.

Initially, I was planning to write out the API layer by hand, but I found a library that someone else had made: [RadarrSharp by Hertizch](https://github.com/Hertizch/RadarrSharp).

This is one of the first times I had ever used the XAML templating language and there are a few things during development that I thought would be good to talk about.

## Should I use { x:Bind } or { Binding } to display variables in my views?

This was something I had struggled with understanding until I found [this Stack Overflow post](https://stackoverflow.com/questions/37398038/difference-between-binding-and-xbind).
In a nutshell, { Binding } is an old way of binding properties, doesn't require types to be specified and binds to the property name.
{ x:Bind } binds to the code behind, and needs all types fixed at compile time.

## How should I lay everything out?

I'm a .NET developer by trade. I made bespoke addons for Sage 200 (the accounting software). These are all made in Win32 Forms applications.

During my time developing these Windows Forms applications, I had never used any layout tools such as the FlowLayoutPanel. 

Recently, I worked on a project where I found that using the FlowLayoutPanel was invaluable for just dropping controls onto the form and not having to worry about the layout.

During the early development of RadarrApp, I would place all of the controls I wanted onto the page and then align everything manually, but I thought to myself _"There must be a better way to lay this all out"_.

I remembered some time ago seeing a post on Twitter about the new [XAML Controls Gallery](https://www.microsoft.com/en-us/p/xaml-controls-gallery/9msvh128x2zt?activetab=pivot:overviewtab) which was available on the Microsoft Store.

![XAML Controls Gallery](/images/radarrapp/xaml-controls-gallery.png)

This application is great, and it allows you to preview all of the different controls and modify some of the configuration for them.

Looking through the controls, I found the RelativePanel and it was exactly what I needed for the layout of my application's pages.

You can adjust the postions of every control included within the RelativePanel and it provides all of the flexibility I wanted.

This XAML makes up the view of the Settings page:

```xml
<RelativePanel Margin="40,40,0,0">
    <TextBlock HorizontalAlignment="Left" Text="Settings" FontSize="36" TextWrapping="Wrap" VerticalAlignment="Top"/>
    <StackPanel Margin="0,120,0,0" Orientation="Horizontal">
        <TextBlock x:Name="HostnameDesc" Text="Hostname" Margin="0,4,0,0"></TextBlock>
        <TextBox x:Name="HostnameValue" PlaceholderText="IP or Domain" Margin="40,0,0,0" Width="350"/>
    </StackPanel>
    <StackPanel Margin="0,180,0,0" Orientation="Horizontal">
        <TextBlock x:Name="PortDesc" Text="Port" Margin="0,4,0,0"></TextBlock>
        <TextBox x:Name="PortValue" Text="" Margin="79,0,0,0" Width="350" PlaceholderText="7878"></TextBox>
    </StackPanel>
    <StackPanel Margin="0,240,0,0" Orientation="Horizontal">
        <TextBlock x:Name="UseHTTPSDesc" Text="Use HTTPS" Margin="0,6,0,0"></TextBlock>
        <ToggleSwitch x:Name="UseHTTPS" Margin="38,0,0,0" Toggled="UseHTTPS_Toggled"/>
    </StackPanel>
    <StackPanel Margin="0,300,0,0" Orientation="Horizontal">
        <TextBlock x:Name="APIKeyDesc" Text="API Key" Margin="0,4,0,0"></TextBlock>
        <TextBox x:Name="APIKeyValue" Text="" Margin="59,0,0,0" Width="350" PlaceholderText="" MaxLength="37"></TextBox>
    </StackPanel>
    <Button Content="Save" Click="Button_Click" Margin="0,360,0,0" Height="36" Width="69"/>
    <TextBlock Name="SavedText" HorizontalAlignment="Left" Margin="0,420,0,0" Foreground="Green" Text="Your settings have been saved." TextWrapping="Wrap" VerticalAlignment="Top" Visibility="Collapsed"/>
</RelativePanel>
```

## Submitting the app to the Microsoft Store

This was a relatively painless process, however it took over 3 days for it to fully go live, as I had to provide a privacy policy URL in order and then recertify the app.

You'll need to sign up for a [Microsoft Partner Center](https://partner.microsoft.com/) account to be able to submit your app.

### *How much does it cost to submit an app to the Microsoft Store?*

Signing up to submit apps as an individual developer to the Microsoft Store requires a $19.99 one-off charge. If you want to submit apps as part of a business or team, you'll be charged $99.

For more information about how much this would cost you in your location, [see this article on docs.microsoft.com](https://docs.microsoft.com/en-us/windows/uwp/publish/account-types-locations-and-fees).

The payment process failed the first time for some unknown reason, but the second time I tried a few days later, it worked fine.

I released my app for free as it's built using open-source libraries for communication with Radarr servers - all I did was make a pretty interface for it.








