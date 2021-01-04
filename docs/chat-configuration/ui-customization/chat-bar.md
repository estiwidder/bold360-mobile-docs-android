---
layout: default
title: Chat Bar
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/chat-bar
nav_order: 6
---

# Chat Bar {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
The chat bar is a UI component that appears on top of the chat screen, during a chat with a live chat. 
The Chatbar displays the following:
 1. Active agent details - name and avatar.
 2. `End Chat` clickable view to end current live chat.
 3. E-mail clickable view to provide an e-mail addressfor automatically sending the chat transcript upon chat end. <sub>(optional)</sub> 
 ## THE E_MAIL PROPERTIES AND BEHAVIOR NEED TO BE DESCRIBED BELOW IN THE RELEVANT SECTIONS.

{: .overview}

![]({{'/assets/images/chatbar.png' | relative_url}})
{: .image-40}

## How it works
The chat bat component data is updated upon chat acceptance and any operator change indication. 

> By defauly operator change events are not passed to the hosting app.
   To pass operator change events to the app, override the SDKs provided `Chatbar`, and pass update events from your implementation.

## Chat bar UI customizations
- The chat bar UI component's display can be customized by overriding the [`configure`]({{'/docs/chat-configuration/ui-customization/how-it-works#configure' | relative_url}}) method of `ChatBarCmpUiProvider`.   
See `ChatbarCmpAdapter` for available configurations.
    ```kotlin
    ChatUIProvider(this).apply {
        chatBarCmpUiProvider.configure = { adapter ->
            adapter.configAgentCmp(ChatbarCmpConfig().apply { 
                this.textStyle = StyleConfig(...)
            })
            adapter.configEndCmp(ChatbarCmpConfig().apply {...})
            adapter.setBackground(...)
            adapter
        }
    }
    ```

- **Overriding agent avatar placeholder image** - Override the image resource on the hosting App resources (resource id - `R.drawable.agent`)

- **Remove "end chat" view display** from the chatbar - to remove the end chat button from the chat bar use the  `ConversationSettings` as seen below.
    ```kotlin
    conversationSettings.disableEndLiveChatSupport()
    ```


## Custom Chatbar UI
The default `Chatbar` implementation can be replaced with a custom  implementation of the `ChatbarComponent.ChatbarViewProvider`, by overriding [`overrideFactory`]({{'/docs/chat-configuration/ui-customization/how-it-works#override' | relative_url}}) property of the `ChatBarCmpUIProvider`. 
```kotlin
ChatUIProvider(this).apply {
    chatBarCmpUiProvider.overrideFactory = 
        object : ChatBarCmpUIProvider.ChatBarFactory {
                override fun create(context: Context)
                             : ChatbarComponent.ChatbarViewProvider {
                    // return your ChatbarComponent.ChatbarViewProvider implementation
                }
        }
}
```


