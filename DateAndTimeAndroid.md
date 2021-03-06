# Date & Time
## How to customize chat date and time display

In the chat there are 2 main time related elements:
1. The dates notifications in the chat, grouping elements of the same date messages 
2. the messages timestamp (usually appears arround the message bubble)

### Customizing chat datestamp display
<img alt='datestamp' src='images/Android/chat_datestamp.png' width=50%/>


In order to change default datestamp display:
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
In the SDK we provide 2 predefined factories: 
- `SimpleDatestampFormatFactory` using the pattern `"EEE, d MMM, yyyy"` to display dates
- `FriendlyDatestampFormatFactory` (which is the default) - provides more common display, using `today`, `yesterday` phrases.

### Customizing chat messages timestamp display
<img alt='datestamp' src='images/Android/chat_timestamp.png' width=50%/>

There are 2 ways of setting a new look to the timestamp display.
##### 1. Using `ConversationSettings.timestampConfig` method.
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
##### 2. Overriding `ChatUIProvider.chatElementsUIProvider.incomingUIProvider/outgoingUIProvider` [configure/customize](./ChatCustomizationsAndroid) method.
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
