---
layout: default
title: Date and Time
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/date-and-time
nav_order: 9
---

# Date and Time {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
In the chat there are two main time related elements:

- **Date notifications in the chat** - grouping messages and elements from a specific date 

![]({{'/assets/images/chat_datestamp.png' | relative_url}})
{: .image-40}

{: .mt-6}
- **The messages timestamp** - usually appears arround the message bubble

![]({{'/assets/images/chat_timestamp.png' | relative_url}})
{: .image-40}

---

## Datestamp display customization
Refer to the methods to change the default date display:
1. Implement `DatesampFormatFactory`
2. Pass that implementation to the ConversationSettings using `ConversationSettings.datestamp` method.

    ```kotlin
    class MyDatestampFactory : DatesampFormatFactory {
        override fun formatDate(datestamp:Long):String{
            return ...
        }
    }

    val settings = ConversationSttongs()
                    .datestamp(enable, myDatestampFactory)
                    //... set more settings

    ChatController.Builder(context).apply{
            //... do some initiations
            conversationSettings(setting)
        }
    ```

### Datestamp display factories
The SDK provides two predefined datestamp factories: 

- SimpleDatestampFormatFactory
{: .text-blue-300}   
Uses the pattern `"EEE, d MMM, yyyy"` to display dates.

{: .mt-6}
- FriendlyDatestampFormatFactory <sub>_default_</sub>   
{: .text-blue-300}   
Provides a more common display, using `today`, `yesterday` phrases.

---

## Timestamp display customization

Refer to the methods to to change the default time display:

1. Configure by [chat settings]({{'/docs/chat-configuration/chat-settings' | relative_url}})
    {: .strong-sub-title}
  
    Using `ConversationSettings.timestampConfig` method.
    ```kotlin
    val settings = ConversationSttongs()
            .timestampConfig(enable, TimestampStyle(
                time_pattern, text_size, text_color, font_typeface
            ))
            //... set more settings

    ChatController.Builder(context).apply{
    //... do some initiations
    conversationSettings(setting)
    }
    ```

2. Overriding [configure/customize](./how-it-works)
    {: .strong-sub-title}

    Overriding the configure and/or customize methods and set the desired display for the timestamp on `ChatUIProvider.chatElementsUIProvider.`[incomingUIProvider](./incoming-message) and/or `ChatUIProvider.chatElementsUIProvider.`[outgoingUIProvider](./outgoing-message).   
      
    ```kotlin
    val chatUI = ChatUIProvider(context).apply {
            this.chatElementsUIProvider.incomingUIProvider.configure = { adapter ->
                    this.setTimestampStyle(TimestampStyle(time_pattern, text_size, text_color))
                }
                adapter
            }

            this.chatElementsUIProvider.outgoingUIProvider.configure = { adapter ->
                    this.setTimestampStyle(TimestampStyle("hh:mm aa", 10, Color.parseColor("#aeaeae")))
                }
                adapter
            }
        }

    ChatController.Builder(context).apply{
            //... do some initiations
            chatUIProvider(chatUI)
        }
    ```