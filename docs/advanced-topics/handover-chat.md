---
layout: default
title: Handover Chat
parent: Advanced Topics
nav_order: 10
---

# Integrating a 3rd party chat provider - Handover Chat {{site.data.vars.need-review}}
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

--- 

## Overview
This section describes how to integarte an third party chat provider to the all - **Handing** the control of the chat to a provided custom `HandoverHandler` extension.   
 
{: .overview}

---

## HandoverAccount
This account defines the third party live chat provider to handle the chats in the application. Usually used for creating `Handover chats`. 

A HandoverAccount is created automatically by the SDK, when a chat with an AI chatbot is redirected toa  `Handover` chat, by a chat channel selection.   
The Handover configuration in the chat channel, will be set in the account session.
  
```kotlin
val chatConfig = "provider defined configuration string"
val account = HandoverAccount(chatConfig)
```    

---

## Setting the Handover chat escalation
Handover chat is automatically being activated, by the SDK, when chat channel configured with `custom provider` was selected on chat with an AI chatbot.   
A [`HandoverAccount`]({{'/docs/chat-configuration/chat-account/handover-account' | relative_url}}) is created according to the channel data.

To create a successful Handover chat escalation, follow the steps below:

- ### Define Handover escalation channel
    For using a 3rd party chat provider you need to have a 
    [Create a chat channel](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_use_ww_integration.html) in the Bold360ai admin console, configured with `custom provider`.   
    <sup>For more information see: [How do I define a channeling policy?](https://support.bold360.com/bold360/help/how-do-i-define-channeling-policy)</sup>


- ### Create HandoverHandler
    To integrate the third party chat implementation, with the bold chat SDK, an extension of `HandoverHandler` needs to be provided to the ChatController. This handler connects the user, the chat SDK and the third party  chat provider.

    ```kotlin
    // Custom Handover handler:
    class MyHandover(context:Context) : HandoverHandler(context) {
        override fun startChat(accountInfo:AccountInfo?) {
            // create and start your custom chat session.
            ...
            // pass Started event to the ChatController, and to the App.
            passStateEvent(StateEvent(StateEvent.Started, getScope())
        }

        override fun endChat(forceClose: Boolean) {
            // end your custom chat session.
            ...

            // pass Ended event.    
            passStateEvent(StateEvent(StateEvent.Ended, getScope()))
        }

        override fun post(message: ChatStatement){
            // handle user message - pass it to your third party chat
            ...
            // Add the user message to the chat
            chatDelegate.injectElement(message);
        }
    }

    // Set custon Handover handler to the ChatContoller:
    // on ChatController creation:
    val chatController = ChatController.Builder(context)
                                    .chatHandoverHandler(myHandoverHandler)
                                    ...
                                    .build(account,...)
    // or later:
    chatController.handoverHandler = myHandoverHandler                      
    ```
<!-- Handover chat initiation flow Diagram -->

---

## How to

- ### Inject and update chat elements   
    HandoverHandler base class provides various methods, including: `injectElement`, `updateElement`, `storeElement`, which can be used when the handover chat is in progress.   
    Base class implementations also make sure element changes are passed to the `ChatElementListener` (Historical updates)
    ## COMMENT: WHAT ARE BASE CLASS IMPLEMENTATIONS?

- ### Display and enable the chat input field
    On chat start or/and on state `StateEvent.Resumed`, the custom handler needs to enable the chat input field, for the user to type messages. This can be done by activating the method `enableChatInput`. Override this method, in case a different behavior is required for the chat input field than the default behavior.

- ### Control chat UI components
    The HandoverHandler has access to a `ChatDelegate` implementation, which provides access to the chat fragments UI components, the chat elements and other abilities.   

    **Exp: Controlling AgentTyping UI component visibility state:**
    ```kotlin
    // show AgentTyping:
    chatDelegate?.updateCmp(ComponentType.LiveTypingCmp, data = null)

    // hide AgentTyping:
    chatDelegate?.removeCmp(ComponentType.LiveTypingCmp)
    ```

- ### Provide additional details and configurations upon chat creation
    Handover chat is created by the hosting app. It is provided by a `HandoverAccount` that contains  configurations required for the chat.   
    Before the chat starts, the app will be triggered to [provide]({{'/docs/chat-configuration/extra/account-info-provider#account-provide' | relative_url}}) the account needed for the chat. At this point, details can be added to the the account [SessionInfo]({{'/docs/chat-configuration/extra/account-info-provider#session-info' | relative_url}}) property.   
    If no extra details are needed, the account should be passed as is.

