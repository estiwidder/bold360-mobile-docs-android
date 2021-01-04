---
layout: default
title: Chat Settings
parent: Chat Configuration
nav_order: 2
# permalink: /docs/chat-configuration/chat-settings
---

# Chat settings {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
The different chat settings are configured in the AI and Bold administrative consoles, and apply to any chat type by the SDKs pre defined available options.  
Chat configurations can be controlled by the embbeding app as well, by passing a customed `ConversatioSettings` object upon the ChatController creation.
{: .overview}

```kotlin
val settings = ConversationSettings()
    // do some customed change

val chatController = ChatController.Builder(context).apply {
                        conversationSettings(settings)
                        ....
                     }.build(account...)
```
---

## Available configurations
{: .mt-6}
### Voice control
**_voiceSettings_** - Defines the desired voice behavior.   
The voice behavior for a chat,is configured by this setting and by the available voice behavior supported by the ChatHandler.   

Available voice behavior options are defined by <u>`VoiceSupport`</u>.   
##  WHAT IS VOICESUPPORT?

>**None, SpeechRecognition, VoiceToVoice, HandsFreeVoiceToVoice<sub>(currently not supported)</sub>**

{: .mt-8}
### Chat transcript request
Chats with this configuration are "recorded" while in progress, to allow sending the chat transcript, if requested, just before it ends or is escalated.
Exp: Passing AI chat flow to a live agent, when AI chat is being escalated to a live or messaging chat.
To manage which chat scope can request sending the chat transcript, use the following:
```kotlin
val conversationSettings = ConversationSettings()
                           .enableChatLogRequest(Requesting_Scope, isEnabled)
```

> Exp: In order to enable passing preceding botchat records to the live agent:
```kotlin
ConversationSettings().enableChatLogRequest(StatementScope.BoldScope, true)         
```   

- By default, `BoldScope` chat is enabled to request preceding chats. 

{: .mt-8}   
### Features availability configurations
##  COMMENT: WHAT ARE THESE PROPERTIES? aRE THEY DESCRIBED SOMEWHERE ELSE?

Enable/Disable/Inspect support of some of the SDKs provided features.
- Autocomplete - Configure and Inspect support for AI chats

- Feedback - Inspect configured support for AI chats

- Typing indication - configure display for live chats

- Chatbar top header - configure display for live chats

- End chat over chat header - configure display

- Chat scroller display - scroll to bottom floating button

- Datestamp header - configure display and format


