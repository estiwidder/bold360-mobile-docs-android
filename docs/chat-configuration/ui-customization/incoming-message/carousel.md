---
layout: default
title: Carousel 
np_toc: true
nav_exclude: true
---

# Carousel 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Create a carousel response](https://support.bold360.com/bold360/help/how-to-create-an-image-gallery-in-conversational-articles)

---

## Overview 
Displays the response as a carousel of items. 
The carousel, just like a regular incoming message, has a message section with a timestamp and avatar, and the quick options and channels at the bottom. The carousel provides the ability to display multiple related answers to a user query in a horizontal scrollable structure.
{: .overview}
![]({{'/assets/images/carousel.png' | relative_url}})
{: .image-40}


## Carousel Item
Each corousel item consists of three display properties: 
- **Image** <sub>(optional)</sub> - THe image can be configured with a URL link. If not image is set, a default image will be displayed.     
- **Info** <sub>(optional)</sub> - Title <sub>(mandatory)</sub> and a subtitle for the item displayed.
COMMENT: HOW CAN THIS BE OPTIONAL IS THE TITL IS MANDATORY? 
- **Options** <sub>(optional)</sub> - Item options .

---

## Carousel UI configurations
The carousel UI can be customized, by overriding the `CarouselItemsUIProvider.configure` method and/or the `CarouselItemsUIProvider.customize` method for [dynamic data related customizations]({{'/docs/chat-configuration/ui-cutomization/how-it-works#customize' | relative_url}}).   
The available configuration proprties can be fund in the `CarouselItemsUIAdapter`.

### General item configurations
- Use the `CarouselItemsUIAdapter.setCardStyle` method to configure the carousel item display. 
  > The carousel items display configurations are:
  -  Border-  card like or flat
  - Corners - rounded or squre 

  >_**Note:**, In case a card style display is selected, the item's background should not be set to transparent_

- Carousel items sections (info, image and options), display order, can be configured with `CarouselItemsUIAdapter.setInfoTextAlignment`, defaults to `CarouselItemConfiguration.ItemInfoAlignment.AlignBelowThumb` 

  ![]({{'/assets/images/customed-carousel.png' | relative_url}})
  {: .image-40}


### Info section
The info section contains a title and a sub-title. The numbr of lines for each is configurable with the following methods:
-  `setInfoTitleMinLines` - minumum is one line
- `setInfoSubTitleMinLines` - minimum are two lines

The sub-title height is flexible and may stretch to the maximum available item height.

The Minimun height for this section will be calulated according to the definenumber of lines described above.  


![]({{'/assets/images/carousel-info.png' | relative_url}})
{: .image-40}

> Additional info section configurationscan be found in the  `CarouselItemsUIAdapter` prefixed with **setInfo...**.

##COMMENT - CAN WE PROVIDE THE LIST HERE?

### Options section
Item options are contained in a non-scrollable vertical order layout.  
The options, text alignment and display are configurable.

All options are configured to the same line count, defaults to 1 line.   
Use `CarouselItemsContainer.setOptionsLineCount` method to change the line count.
{: .light-title}   
> Carousel view height is being calculated according to the item with the maximum number of options.

### Image section
The image size is constant. 
If a link is provided for the image, it will be passed to the hosting app upon click for further handling, via the  [`ChatEventListener.onUrlLinkSelected` implementation]({{'/docs/chat-configuration/tracking-events/events-and-notifications#listening-to-url-navigation' | relative_url}}).

---

## UI override
The Carousel list section can be overriden by setting the  `CarouselItemsUIProvider.overrideFactory` property.    
A customed implementation of `CarouselItemsAdapter` should be provided by the overriding factory, as shown below.

```kotlin
ChatUIProvider(context).apply {
    this.chatElementsUIProvider.incomingUIProvider.carouselUIProvider.overrideFactory =
            object : CarouselItemsUIProvider.CarouselFactory {
                override fun createItemsAdapter(context: Context): CarouselItemsAdapter {
                    return MyCarouselImplementation();
                }
    }
}
```     
{: .mb-10}       
