---
layout: default
title: How it works
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/how-it-works
nav_order: 1
---

# Customization of the chat UI {{site.data.vars.need-work}}
{: .no_toc }
## COMMENT: WE NEED TO THINK OF THE CORRECT PLACE IN THE TOC FOR THIS SECTION. IN ALL SPECIFIC SECTION THIS IS ALSO MENTIONED. 

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
All supporting UI components enable the hosting app to apply some UI changes to the SDKs default implementations or replace them completely by custom components.   
{: .overview}   

Upon [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) creation an instance of `ChatUIProvider` can be provided by the hosting app. 
The `ChatUIProvider` contains definitions and configurations for the SDK UI components, and they are created accordingly when needed.   

In order to change and/or override the SDKs provided implementations and customizations, you need to pass your customed `ChatUIProvider` instance on ChatController creation. 

```kotlin
// creating the provider instance:
ChatUIProvider uiProvider = ChatUIProvider(context).apply{

    // change the chat screen background:
    setChatBackground(context.getResources().getDrawable(R.drawable.bkg_bots));

    // changing default properties for the incoming bubble views:
    chatElementsUIProvider.incomingUIProvider.configure = { adapter ->
        adapter.apply {
            setAvatar(...)
            setTextStyle(...)
        }
    }

    // overriding feedback view factory:
    chatElementsUIProvider.incomingUIProvider
            .feedbackUIProvider.overrideFactory = MyFeedbackFactory()


    ... // apply more customizations
}

ChatController.Builder(context)
                    .chatUIProvider(uiProvider)
                    ...
                    .build(...) 
```

---

## Customizing How UI components display: 
The ChatUIProvider provides access to the chat UI components. The components can be customized as described below. These methods are combined. 

### Chat UI settings configuration
The SDK provides the ability to configure UI settings on a configuration adapter. The set values will be applied on all components of the same type.   
To change the default SDK configurations, change the `configure` method implementation.
  
```kotlin
// method signature:
open var configure: ((Adapter) -> Adapter)

// applying custom implementation
chatUIProvider.chatElementsUIProvider.incomingUIProvider.configure = { adapter ->
    adapter.setAvatar(Context.resources.getDrawable(R.drawable.my_avatar))
}
```

### Chat UI settings customization 
The SDK provides the ability to dynamically change UI settings configured by `configure`on the adapter, according to a data model that will be displayed by the component. Those changes will applied only on the specific components.  
```kotlin
// method signature:
open var customize: ((Adapter, DataElement?) -> Adapter)?

// applying custom implementation:
chatUIProvider.chatElementsUIProvider.incomingUIProvider.customize = { adapter, element ->
    element?.takeIf { it.elemScope.isLive() }?.run {
        adapter.setAvatar(Context.resources.getDrawable(R.drawable.agent_avatar))
    }
}
```

### Chat UI settings overriding
The SDK enables to provide a proprietary implementation for a specific component.   
There are two ways to override the UI settings, depending on the specific component support:   

#### Custom implementation
{: .strong-sub-title} 
A component custom implementation should be set in the component's overrideFactory property type.    
    
> Exp:   
    Provide a new implementation for the feedback view by implementing the `FeedbackUIAdapter` interface.   

```kotlin
class MyFeedbackFactory : FeedbackUIProvider.FeedbackFactory{
    override fun create(context: Context, feedbackDisplayType: Int): FeedbackUIAdapter {
        return new FeedbackViewDummy(context);
    }
}

chatUIProvider.chatElementsUIProvider
                .incomingUIProvider.feedbackUIProvider
                    .overrideFactory = MyFeedbackFactory()
```

#### Override view resource
{: .strong-sub-title .mt-6}
Provide a different layout resource file, in the form of `ViewInfo` object.   
    
```kotlin
class MyUserInputProvider : ChatUIProvider.ChatUIFactory{
    override fun userInputInfo(): ViewInfo {
        return ViewInfo(R.layout.layout_name, R.id.custom_input_container)
    }
}

chatUIProvider.overrideFactory = MyUserInputProvider();
```
