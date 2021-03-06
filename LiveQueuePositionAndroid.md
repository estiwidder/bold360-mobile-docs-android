# Live Queue position component

On live chat creation, if all agent are occupied to their capacity, the chat will be add to a waiting queue.
Once current active chats will end, the created chat will be promoted in the queue, until position 0, means the chat is no longer in the queue and the chat is waiting for agent acceptance.

> User can end the chat while in the queue with [life cycle state](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_android_adv_chat_lifecycle.html) (`StateEvent.InQueue`) or while waiting for acceptance (`StateEvent.Pending`).    
Chat canceling considered as unavailable, so the user will be presented with unavailability form. (or with an unavailability message, if form is disabled)

### How to enable support for the Queue Component
1. Enable `[automatic distribution]` in bold console.   
Some configurations can be changed, like queue limit, agent concurrent chat limit etc.
2. Enable `[Show queue position]` in bold console, in order to be notified of user position in queue.

### How to customize the Queue Component
Queue position UI display is provided by the SDK, and can be customize either by configuration setting or by view implementation overriding.

#### Customizing by Configure
    Override `ChatUIProvider.queueCmpUIProvider.configure` method:
    ```kotlin
    ChatUIProvider(context).apply {
        queueCmpUIProvider.configure = { adapter:QueueCmpAdapter -> 
            ...
        }
    }
    ```
#### Customizing by override
   Override overrideFactory value with your own `QueueFactory` implementation:
    ```kotlin
    ChatUIProvider(context).apply {
        queueCmpUIProvider.overrideFactory = MyQueueFactory()
    }

    // for exp:
    class MyQueueFactory : QueueFactory{
        // QueueCmpAdapter interface should be 
        // implemented by the overriding component
        return object : QueueCmpAdapter{
            ...
        }
    }

    ```
