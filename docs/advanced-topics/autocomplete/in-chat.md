---
layout: default
title: In chat
parent: Autocomplete
grand_parent: Advanced Topics
nav_order: 1
# permalink: /docs/advanced-topics/autocomplete/in-chat
---

# Autocomplete in AI chat
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .nb-0}
- [UI configurations]({{'/docs/chat-configuration/ui-customization/user-input-field#ui-configurations' | relative_url}})

---

## Overview
Autocomplete refers to displaying comptetion suggestions predited by the system, while the user is typing a message.  The user can select from the suggested text or continue typing his message.   
Selected suggestion will be sent as the user's message and the response will then be displayed.  
{: .overview}
 
![]({{'/assets/images/autocomplete-in-chat.png' | relative_url}})   
{: .image-40}

---

## Enabling Autocomplete in a chat 
{: .mb-4}
- ### [Admin console configuration]({{'/docs/advanced-topics/autocomplete#control-availability' | relative_url}})
{: mb-10}

- ### Chat ConversationSettings configuration   
  The hosting app can control the  features to be enabled by passing [`ConversationSettings`]({{'/docs/chat-configuration/chat-settings' | relative_url}}) instance. The autocomplete configuration is set as part of the   [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) creation. 
  This configuration will apply to  all chats created with the same ChatController instance. 
  
    ```kotlin
    val settings = ConversationSettings().apply {
        // ...
        enableAutocompleteSupport(true/false)
    }

    val chatController = ChatController.Builder(context)  
                            .conversationSettings(settings)                                                   
                            .build(account, ...)                    
    ``` 
{: .mb-8}


> **Client side settings will override console settings,**   with the exception of the case were autocomplete is disabled on the admin console, on the account settings.   
{: .mb-10}