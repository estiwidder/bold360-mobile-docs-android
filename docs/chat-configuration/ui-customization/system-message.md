---
layout: default
title: System Message
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/system-message
nav_order: 5
---

# System Messages {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview 
...

## Constomization
Currently the system messages display can only be overriden by `override`.

- ### Customize by overriding layout resouce   
  
  Create your own layout resources with the following names:   
  - Regular messages: system_message_regular (inner textview should keep SDK known id: message_textview)
  - Removable messages: system_message_removable (inner textview should keep SDK known id: message_textview)

- ### Customizing by override SystemUIProvider factory
  {: .mt-5}
    1. _Implement SystemFactory:_ 
        - Override the `removableSystemInfo` method to change the removable system messages layout (appears for a specific state and then removed from the chat)
        - Override the `info` method to change the regular messages display.

        
    2. _Set `SystemUIProvider.overrideFactory` with your implementation._    

        ```kotlin
        class MySystemMessageFactory: SystemFactory{
            override fun info():ViewInfo{
                return ....
            }
            override fun removableSystemInfo():ViewInfo{
                return ....
            }
        }
        
        val chatUI = ChatUIProvider(context).apply {
            this.chatElementsUIProvider.systemUIProvider.overrideFactory = mySystemMessageFactory
        }

        ChatController.Builder(context).apply{
                //... do some initiations
                chatUIProvider(chatUI)
            }
        ```