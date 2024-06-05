---
layout: post
title: Creating a prompt that uses Python and Dall-E to create animation stills
subtitle: ChatGPT, Prompt design
---

[Small Art Animator](https://chatgpt.com/g/g-QUaGrxjkc-small-art-animator)

I made a ChatGPT prompt that creates a series of stills useful for animation. So far here's what it looks like:

![URL](/img/monkeyframes.png)

As you can see it uses python to make the pixel width and height along with the amount of frames to create a cavnas dimension size. So far, it will often create the correct amount of frames with the right 1024x1024 size--I'm not sure how because the math doesn't make sense. I think this is a limitation of the Dall-E model not being able to create specific sizes of objects based on exact dimensions. Here's that current prompt:


Use python to multiply the specified width and height in pixels times the amount frames mentioned and pass that answer as the dimensions for the canvas size in your Dall-E prompt 
where it will create an image that is a combination of stills, dividing the canvas up into discrete images, that form an animation based on the prompt's canvas size. 
The start of the animation stills should progress logically from one step to another, forming an animation in the number of frames described by the user in the following prompt. 
Take into consideration the following prompt's direction the character or object in the stills should be facing if one is mentioned. 
If there is an image uploaded in one of the next comments by the user, try and recreate that image's character as a series of stills like previously mentioned, and use that instead of creating your own from scratch.
Descriptions to better tune what's being made into animated stills can be considered as well either from the comments if mentioned or the images uploaded.

Do not repeat any of the original prompt information. Try to make sure the image isn't cut off on the edges. Default to 64x64 and 6 frames unless a size and frame amount is mentioned in the following prompt.



Here's an updated prompt with potentially better interpretability for Dall-E:

Use Python to multiply the width and heigh and pass that into Dall-E as the canvas size. Create an image that is a combination of stills based on the amount of frames that will be mentioned at the specified pixel width and height, as discrete images, that form an animation based on the prompt's canvas size. The start of the animation stills should progress logically from one step to another, forming an animation in the number of frames described by the user in the following prompt. Take into consideration the following prompt's direction the character or object in the stills should be facing if one is mentioned. If there is an image uploaded in one of the next comments by the user, try and recreate that image's character as a series of stills like previously mentioned, and use that instead of creating your own from scratch. Descriptions to better tune what's being made into animated stills can be considered as well either from the comments if mentioned or the images uploaded.

Do not repeat any of the original prompt information. Try to make sure the image isn't cut off on the edges. Default to 64x64 unless a size is mentioned in the following prompt.

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/883991a1-7db7-4d2d-ade0-3a0ae54a34f8)

Interestingly, it seems the interpretability is almost better when specified as text to image per frame. However, the text doesn't match the image perfectly.
