---
layout: page
title: Kaltura Thumbnail API
weight: 503
---

In addition to the [thumbAsset](https://developer.kaltura.com/api-docs/#/thumbAsset) service available as part of the Kaltura API v3, Kaltura provides a special Thumbnail API, aimed at simplifying the creation of thumbnails on-the-fly.

The Thumbnail API provides a simple web interface to dynamically generate image snapshots of Kaltura video entries on the fly.

The images are generated upon demand (with caching on disk and via CDN) by calling the following URL - 

```http://YourKalturaServer/p/{partner\_id}/thumbnail/entry\_id/{entry_id}```

The result of the thumbnail API is a JPEG image with one or more of the following features:

*   A re-sized / cropped version of the original thumbnail image.
*   A specific frame from a video clip.
*   An older version of the entry thumbnail.
*   A various compression quality of the thumbnail image.

>Note: The Thumbnail API will work on Video entries as well as on Image entries.

## Using the Thumbnail API  

The parameters are passed in the following form:

```http://YourKalturaServer/p/{partner\_id}/thumbnail/entry\_id/{entry_id}**/paramX\_name/paramX\_value/...*```

1.  Replace *{partner_id}* with your partner ID from the [KMC Integration Settings][1].  
  (![KMC Integration Settings](https://github.com/kaltura/DeveloperPortalDocs/blob/master/documentation/Engage_and_Publish/images/kmc-integration.png). 
Please always ensure that a descriptive Alt text is specified on your image to ensure compatability with screen readers and better search indexing.
2.  Replace *{entry_id}* with the id of the desired entry thumbnail. The id can be found in the list of entries in the [KMC Content Tab](http://www.kaltura.com/index.php/kmc/kmc4#content|manage).  
  (![KMC Content Tab entries](https://github.com/kaltura/DeveloperPortalDocs/blob/master/documentation/Engage_and_Publish/images/entries%20table%20entry%20id.JPG). 
3.  Then append any of the parameters below according to the following format: /paramX\_name/paramX\_value/...

 [1]: http://www.kaltura.com/index.php/kmc/kmc4#account|integration


## Thumbnail API Parameters  

| Parameter name | Type    | Mandatory | Description                                                                                                                                                                                                                                       |
|----------------|---------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| entry_id       | String  | Yes       | The entry ID                                                                                                                                                                                                                                      |
| widget_id      | String  | No        | The widget ID                                                                                                                                                                                                                                     |
| version        | integer | No        | The thumbnail version                                                                                                                                                                                                                             |
| width          | integer | No        | Requested width in pixels                                                                                                                                                                                                                         |
| height         | integer | No        | Requested height in pixels                                                                                                                                                                                                                        |
| type           | integer | No        | Type of crop to be used – see remarks below                                                                                                                                                                                                       |
| bgcolor        | string  | No        | 6 hex digits web colorcode                                                                                                                                                                                                                        |
| crop_provider  | string  | No        | ADVANCED. Indicates a custom thumbnail generatorThis parameter is used by the Kaltura WordPress plugin to generate custom player thumbnails.To add custom thumbnail generators see existing plugins under: /alpha/apps/kaltura/lib/crop_providers |
| quality        | string  | No        | Jpeg quality for output (0-100). The default is 75                                                                                                                                                                                                |
| src_x          | integer | No        | 1st part of a rectangle to take from original picture                                                                                                                                                                                             |
| src_y          | integer | No        | 2nd part of a rectangleto take from original picture                                                                                                                                                                                              |
| src_w          | integer | No        | 3rd part of a rectangleto take from original picture                                                                                                                                                                                              |
| src_h          | integer | No        | 4th part of a rectangleto take from original picture                                                                                                                                                                                              |
| rel_width      | integer | No        | Actual width of the image from which the src_* parameters were taken                                                                                                                                                                              |
| rel_height     | integer | No        | Actual height of the image from which the src_* parameters were taken                                                                                                                                                                             |
| vid_sec        | integer | No        | second to snap from video                                                                                                                                                                                                                         |
| vid_slice      | integer | No        | Number of slice out of number of slices                                                                                                                                                                                                           |
| vid_slices     | integer | No        | Number of slices                                                                                                                                                                                                                                  |


## Parameters Usage 

To use the parameters, append each parameter name followed by the desired value in the following format: /paramX\_name/paramX\_value/...

*   If both width and height were given a zero value the original image will be returned.
*   If neither width nor height were specified, the resulting size will be 120x90 (default thumbnail size).
*   If only width or height were specified, the given dimension combined with the type parameter will control the resulting size of the image.
*   If vid\_slices is provided without vid\_slice the result will be a horizontal strip with the different slices.
*   The 'type' parameter controls the resize/cropping options:
1.  resize according to the given dimensions while maintaining the original aspect ratio.
2.  place the image within the given dimensions and fill the remaining spaces using the given background color.
3.  crop according to the given dimensions while maintaining the original aspect ratio. The resulting image may be cover only part of the original image.
4.  crops the image so that only the upper part of the image remains.

## Examples  

Let's consider this as the original thumbnail we'll play with:

*   http URL - [http://cdn.kaltura.com/p/811441/thumbnail/entry\_id/0\_wf3km7rh][2]
*   secured URL - <a href="https://cdnapisec.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh" target="_blank">https://cdnapisec.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh </a>

 [2]: http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh

 

>Note: If neither width nor height were specified, the resulting size will be 120x90 (default thumbnail size).</span>

### Resizing  

The thumbnail with dimensions of 500 x 400 (with maintain aspect-ratio from original) and defined quality: (X is the quality)

<span>http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/100/type/1/quality/X</span>

<span></span><span class="mce-sub-heading">         10% Quality:                            <strong><span class="mce-sub-heading">50% Quality:                             <strong><span class="mce-sub-heading">100% Quality:</span></strong></span></strong></span><strong class="mce-sub-heading"><br /></strong><img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/100/type/1/quality/10" border="0" alt="Resized thumbnail to 200x100 and Quality of 10%" title="Resized thumbnail to 200x100 and Quality of 10%" width="178" height="100" />     <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/100/type/1/quality/50" border="0" alt="Resized thumbnail to 200x100 and Quality of 50%" title="Resized thumbnail to 200x100 and Quality of 50%" width="178" height="100" style="border-style: initial; border-color: initial;" />     <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/100/type/1/quality/100" border="0" alt="Resized thumbnail to 200x100 and Quality of 100%" title="Resized thumbnail to 200x100 and Quality of 100%" width="178" height="100" style="border-style: initial; border-color: initial;" />

>Note: The Quality parameter affects the compression value of the JPEG algorithm. To learn more about JPEG Compressions, visit the [JPEG entry on Wikipedia](http://en.wikipedia.org/wiki/JPEG#Effects_of_JPEG_compression).

### Using vid_sec parameter  

The vid_sec parameter allows us to select a specific second in a video entry. The following will capture various frames from the 10<sup>th</sup>, 50<sup>th</sup>, 100<sup>th</sup> and 150<sup>th</sup> second in the video: (X is the second)</span>

<span></span>http://cdn.kaltura.com/p/811441/thumbnail/entry\_id/0\_wf3km7rh/width/200/height/100/type/1/vid_sec/X

<p class="mce-sub-heading">
       10<sup>th</sup> Second:            50<sup>th</sup> Second:           100<sup>th</sup> Second:           150<sup>th</sup> Second:<br /><img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/120/type/1/vid_sec/10" border="0" alt="Thumbnail from the 10th second of the video" width="120" height="68" />   <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/120/type/1/vid_sec/50" border="0" alt="Thumbnail from the 50th second of the video" width="120" height="68" style="border-style: initial; border-color: initial;" />   <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/120/type/1/vid_sec/100" border="0" alt="Thumbnail from the 100th second of the video" title="Thumbnail from the 100th second of the video" width="120" height="68" style="border-style: initial; border-color: initial;" />   <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/120/type/1/vid_sec/150" border="0" alt="Thumbnail from the 150th second of the video" title="Thumbnail from the 150th second of the video" width="120" height="68" style="border-style: initial; border-color: initial;" />
</p>

   

<p class="mce-note-graphic">
  NOTE: If the given vid_sec is out of range, meaning, larger than the duration of the video, the returned thumbnail will be an image with the defined dimensions (as indicated by the width and height parameters)
</p>

<p class="mce-heading-3">
  Cropping
</p>

Using the src\_x, src\_y, src\_w and src\_y parameters, it is possible to define a cropping rectangle, indicating the X and Y to position the rectangle at and W and H to indicate the size of the rectangle. The returned thumbnail will then be the pixels of the image that are within the defined rectangle.

<p class="mce-sub-heading">
  Source Thumbnail:
</p>

<img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/vid_sec/10" border="0" width="200" height="113" />

<p class="mce-sub-heading">
  <span>Cropped (300, 100, 500, 400):</span>
</p>

<p class="mce-sub-heading">
  <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/200/vid_sec/10/src_x/300/src_y/100/src_w/500/src_h/400" border="0" alt="Cropped (300, 100, 500, 400)" title="Cropped (300, 100, 500, 400)" width="200" height="160" />
</p>

<p class="mce-note-graphic">
  NOTE: To accurately calculate the dimension of the cropping rectangle, you should know the original video (source flavor) dimensions. In the above example, the dimensions were 1280 x 720. To avoid having to know the source dimensions, use the rel_width and rel_height parameters to define a "new-virtual-source-dimensions", the Thumbnail API will use resize the frame to these dimensions and then perform the cropping.
</p>

<p class="mce-heading-3">
  Slicing
</p>

A more advanced use case of the Thumbnail API is to create "animated-thumbnails" where series of thumbnails from the video are shown in rotation one after another, creating the impression of a slient movie. 

Slicing can also be used in advanced CSS techniques using sprites.

To learn how to put slicing together in a JavaScript based animated thumbnail, read: [How To Create a Video Thumbnail Rotator in JavaScript][3]

 [3]: http://knowledge.kaltura.com/how-create-video-thumbnail-rotator-javascript

To create thumbnail slices, use the <span>vid_slices and vid_slice</span> parameters. <span>vid_slices defines the number of frame-slices the video will be divided to and the vid_slice indicates the specific frame-slice to retrieve in that specific call (think of it as pager size and page number).</span>

<span><span>If vid_slices is provided without vid_slice, a stripe of all slices will be returned, which can be later used as CSS sprites.</span></span>

<p class="mce-sub-heading">
  <span><span>      Slice #1 out of 90                     Slice #30 out of 90                   Slice #60 out of 90<br /></span></span><img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/100/vid_slices/90/vid_slice/0" border="0" alt="Video Slice #1 from a series of 90 slices starting" title="Video Slice #1 from a series of 90 slices starting" width="178" height="100" style="border-style: initial; border-color: initial;" />    <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/100/vid_slices/90/vid_slice/30" border="0" alt="Video Slice #30 from a series of 90 slices starting" title="Video Slice #30 from a series of 90 slices starting" width="178" height="100" style="border-style: initial; border-color: initial;" />    <img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/200/height/100/vid_slices/90/vid_slice/60" border="0" alt="Video Slice #60 from a series of 90 slices starting" title="Video Slice #60 from a series of 90 slices starting" width="178" height="100" style="border-style: initial; border-color: initial;" />
</p>

<p class="mce-sub-heading">
  Without vid_slice (A stripe of the images):
</p>

([http://cdn.kaltura.com/p/811441/thumbnail/entry\_id/0\_wf3km7rh/width/120/vid_slices/3/][4])

 [4]: http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/120/vid_slices/3/

<img src="http://cdn.kaltura.com/p/811441/thumbnail/entry_id/0_wf3km7rh/width/120/vid_slices/3/widget_id/0" border="0" alt="Stripe of 3 slices" title="Stripe of 3 slices" width="360" height="68" />

<p class="mce-note-graphic">
  NOTE: The slices are 0 indexed. The first slice in the series will always be 0.<br /> 
</p>

 

 
