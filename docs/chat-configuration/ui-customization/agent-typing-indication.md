---
layout: default
title: Agent Typing Indication
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/agent_typing_indication/
nav_order: 8
---

# Agent Typing Indication {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
The typing indication can be shown to the user once the live agent is typing.  
The typing indication can be displayed as text and/or as a  drawable.   
Typing drawable can be an [AnimatedVector](https://developer.android.com/reference/android/graphics/drawable/AnimatedVectorDrawable).
{: .overview}

## How to enable the live agent typing indication
By default the typing indication is `enabled`.   
This indication can  be disabled in the  `ConversationSettings` as seen below:
```kotlin 
val settings =  ConversationSettings()...
        .disableTypingIndication()

ChatController.Builder(context).apply {
    conversationSettings(settings)
    ...
}
```

## Listening to typing notifications

To receive ntifications on typing state changes, subscribe to the `OperatorEvent.OperatorTyping' notifications, with the chatController.
```kotlin
// 1. Implement Notifiable interface
// 2. Pass the implementation and the list of notifications to be received.  
chatController.subscribeNotifications(NotifiableImpl, OperatorEvent.OperatorTyping,...)
```

> Refer to [notifications subscription]({{'/docs/chat-configuration/tracking-events/events-and-notifications' | relative_url}}) for more information.

---

## How to customize

Typing indication UI component is provided by the SDK, and can be customize either in the configuration setting or by view implementation overriding.

### Typing indication in the configuration settings: 
    Override `ChatUIProvider.typingUIProvider.configure` method:
    ```kotlin
    ChatUIProvider(context).apply {
        typingUIProvider.configure = { adapter:TypingUIAdapter -> 
            ...
        }
    }
    ```
    
### Typing indication Customizing by overriding the view implementation:
   Override overrideFactory value with your own `TypingFactory` implementation:
    ```kotlin
    ChatUIProvider(context).apply {
        typingUIProvider.overrideFactory = MyTypingFactory()
    }

    // for exp:
    class MyTypingFactory : TypingFactory{
        // TypingUIAdapter interface should be 
        // implemented by the overriding component
        return object : TypingUIAdapter{
            ...
        }
    }

    ```
