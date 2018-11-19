---
title: Sharing images, sounds and videos on GBIF
authors: Marie Grosjean
date: '2000-11-15'
slug: gbif-multimedia
categories:
  - GBIF
tags:
  - sound
  - image
  - video
  - multimedia
  - media
  - Dublin Core
  - Darwin Core
  - howto
  - publish
lastmod: '2018-11-15'
keywords: ['sound', 'image', 'video']
description: ''
comment: no
toc: ''
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: yes
contentCopyright: no
reward: no
mathjax: no
mathjaxEnableSingleDollar: no
mathjaxEnableAutoNumber: no
hideHeaderAndFooter: no
flowchartDiagrams:
  enable: no
  options: ''
sequenceDiagrams:
  enable: no
  options: ''
---

This blog post covers the publication of multimedia on GBIF. However, it is not intended to be a documentation. For more information, please check the [references below](#references).

NB: GBIF **does not** host multimedia files and there is no way to upload pictures on the platform. For more information, please read the [how to publish](#how-to-publish-media-in-a-Darwin-Core-Archive) paragraphs.

# Media displayed on the GBIF portal

Let's say that you are looking for [pictures of otters](https://www.gbif.org/occurrence/search?media_type=StillImage&taxon_key=2433753), or perhaps the [call of a sea eagle](https://www.gbif.org/occurrence/search?media_type=Sound&taxon_key=2480444).

You can filter [GBIF occurrences](https://www.gbif.org/occurrence/search?q=) by Media Type. Once you found what you are looking for, you can visualize and/or hear it directly on the GBIF portal.

## Images

GBIF's searching interface has a [gallery](https://www.gbif.org/occurrence/gallery), which displays directly all the images available for the occurrences selected.

<img src="/post/2018-11-19-gbif-multimedia_files/example_gallery.png" alt="drawing" width="400"/>


You can also visualize images for a particular occurrence, by simply clicking on it. See for example, this [occurrence with two images](https://www.gbif.org/occurrence/1822411383) from [the Norwegian Species Observation Service](https://www.gbif.org/dataset/b124e1e0-4755-430f-9eab-894f25a9b59c).

<img src="/post/2018-11-19-gbif-multimedia_files/example_occurrence_with_image.png" alt="drawing" width="200"/>

All the images belonging to a same dataset are visible on [its page](https://www.gbif.org/dataset/b124e1e0-4755-430f-9eab-894f25a9b59c). The same goes for all the images belonging to datasets published under the same organization: see [this example](https://www.gbif.org/publisher/d3978a37-635a-4ae3-bb85-7b4d41bc0b88).

## Audio and Video

Sounds and videos can be directly played from a given occurrence page through a media player.

For example, by clicking on [this occurrence](https://www.gbif.org/occurrence/1934990970) from [Xeno-canto - Bird sounds from around the world](https://www.gbif.org/dataset/b1047888-ae52-4179-9dd5-5448ea342a24), you can listen to a kookaburra call.

You can also take a look at some videos showing the 3D reconstruction of a specimen of sand flea by clicking on [this occurrence](https://www.gbif.org/occurrence/863580710) from [the Museum of Comparative Zoology, Harvard University](https://www.gbif.org/dataset/4bfac3ea-8763-4f4b-a71a-76a6f5f243d3)

# Multimedia supported

What exactly can you make available through GBIF? 

## Media types

In theory, you could share any type of media defined by the [Dublin Core Metadata Initiative](http://dublincore.org/documents/dcmi-type-vocabulary/#H7). However, the recommended terms are:  
* [Collection](http://dublincore.org/documents/dcmi-type-vocabulary/#dcmitype-Collection)
* [StillImage](http://dublincore.org/documents/dcmi-type-vocabulary/#dcmitype-StillImage)
* [Sound](http://dublincore.org/documents/dcmi-type-vocabulary/#dcmitype-Sound)
* [MovingImage](http://dublincore.org/documents/dcmi-type-vocabulary/#dcmitype-MovingImage)
* [InteractiveResource](http://dublincore.org/documents/dcmi-type-vocabulary/#dcmitype-InteractiveResource)
* [Text](http://dublincore.org/documents/dcmi-type-vocabulary/#dcmitype-Text)

In practice,  it is a little bit more complicated.
The system infers the media type from the file format by using [MIME types](https://www.iana.org/assignments/media-types/media-types.xhtml) (these types correspond to the recommended media types cited above). If the type identified is `text/html`, the media is interpreted as a [reference](https://dwc.tdwg.org/terms/#dcterms:references) link instead of an associated media.

In other words, GBIF currently ingrates only images (`StillImage`), sounds (`Sound`), video (`MovingImage`) or collections of the three. For all the other media types, users will have to click on the links given with each occurrence.

For more information about media types, you can check the [references](#references) at the end of this post.

## Formats

I explained in the previous paragraph that GBIF integrates only images, sound and videos for now. But which formats are supported?

In practice, any format that can be interpreted by [Apache Tika](https://github.com/apache/tika) is supported. This should include the formats in this [IANA Media Type list](https://www.iana.org/assignments/media-types/media-types.xhtml) (now called MIME types).

# How to publish media in a Darwin Core Archive

As mentioned at the beginning of this post, GBIF doesn't host any multimedia file for now. This means that you cannot upload pictures or audio files directly on GBIF. They must be hosted on another system. What you should provide, is a URL or URI for each media file you wish to make available.

When sharing your URLs and URIs, keep the following points in mind:
* The URL provided must be a direct link to the file. For example: https://ipt.gbif.org/media/UAIC1008871_X.jpg. 
* Embedded images or preview links such as: http://161.111.171.57/herbarioV/visorVCat.php?img=MA-01-00070296, don't currently work. 
* The file extension doesn't always have to be specified in the URL (see for example the URLs provided with [this occurrence](https://www.gbif.org/occurrence/1019735016) from [The Hemiptera collection (EH) of the Muséum national d'Histoire naturelle](https://www.gbif.org/dataset/d9474ec2-061c-4858-bfdd-e10ba6aca397)).

Note that for the two sub-paragraphs below, I assume that you are somewhat familiar [Darwin Core Archives](https://github.com/gbif/ipt/wiki/DwCAHowToGuide) and [IPTs](https://github.com/gbif/ipt/wiki).

## Simplest method: dwc:associatedMedia

The simplest way to share your media would be to use the [associatedMedia](https://dwc.tdwg.org/terms/#dwc:associatedMedia) field. Since this term belongs to the Darwin Core Occurrence, you don't need to create a second file for multimedia. In other words, you can have both your occurrences and images, sound or video in the same file.

This field can handle one or several URLs separated by a pipe symbol: `|`.

For example:
* `https://ipt.gbif.org/media/UAIC1008871_X.jpg`
* `https://ipt.gbif.org/media/UAIC1008871_X.jpg | https://ipt.gbif.org/media/UAIC1052169_Pheidole_obtusospinosa_65mm_3x_compedit_lg.jpg`

However, this method doesn't allow to attach any metadata to the media (no title, no license, no author, etc.) so it is not ideal.

## Extensions: Simple multimedia and Audubon Media Description

The better way to share your images, sounds or videos, would be to use extension files.

> “Extension” files support the exchange of additional, described classes of data that relate to the core data type (Occurrence or Taxon). An extension record points to a record in the core data file.
(Definition from the [Darwin Core Archive - How-to wiki](https://github.com/gbif/ipt/wiki/DwCAHowToGuide).)

GBIF currently supports two types of extensions:
* [Simple multimedia](http://rs.gbif.org/extension/gbif/1.0/multimedia.xml)
* [Audubon Media Description](http://rs.gbif.org/extension/ac/audubon.xml) (partial support for now)

Both of these extensions will allow you to share detailed information about your media such as `creator`, `description`, `license`, etc. However, the Audubon Media Description is way more exhaustive.

Whether you decide to use one extension or the other, you need to generate a file containing:
* an [occurrenceID](https://dwc.tdwg.org/terms/#occurrenceID) field (referring to the occurrence or specimen concerned),
* unique identifiers (`dcterms:identifier`), links to the media (`dcterms:source` or `accessURI`),
* etc.
This file should be mapped with the proper terms and integrated in the Darwin Core Archive.

For more information about the terms available in each extension, please check the [references](#references).

## Examples

Here are a few datasets using different methods to share their media. Don't hesitate to check out their Darwin Core Archive to see how it looks like.
* [This macroinvertebrate deep-sea dataset](https://www.gbif.org/dataset/4a53a180-f0c8-4dd8-a1fb-18768668edc9) uses the dwc:associatedMedia field.
* [The cnidarians collection (IK) of the Muséum national d'Histoire naturelle](https://www.gbif.org/dataset/b5cdf587-3342-48ec-9130-ba1281d7166f) uses Simple multimedia extension.
* A great example of the use of the Audubon Media Description is this [Xeno-canto dataset](https://www.gbif.org/dataset/b1047888-ae52-4179-9dd5-5448ea342a24). 

# How to publish media outside of Darwin Core Archives

As you might know, you can publish resources on GBIF using alternatives to Darwin Core Archives.

See, for example, the two systems below:
* [BioCaSe](http://www.biocase.org)
* [Symbiota](http://symbiota.org)

I have never set up nor used either of these systems so I am not the best person to advise on this but I can try to give some links.

The only piece of documentation I found concerning the mapping of media fields between ABCD standards (used by BioCaSe) and Darwin Core Terms comes from [this Blog post](https://gbif.blogspot.com/2014/05/multimedia-in-gbif.html) from 2014:

> In ABCD 2.06 we use the unit MultiMediaObject subelements instead. Here there are distinct file and webpage URLs (FileURI, ProductURI), the description (Comment),  the license (License/Text, TermsOfUseStatements) and also an indication of the mime type (Format).

If you have found better documentation, please leave a comment below.

Symbiota documents how to submit and upload images on any Symbiota portal [here](http://symbiota.org/docs/image-submission-2/). To make the images accessible from GBIF, you simply need to follow [these instructions](http://symbiota.org/docs/publishing-to-gbif-from-a-symbiota-portal/). As far as I know, Symbiota doesn't support sounds nor videos at the moment.

Please don't hesitate to mention and link to other systems and their documentations to share multimedia files through GBIF.

# Choose a license

GBIF doesn't give any official recommendation to set a license on your multimedia files. The Licenses fields are essentially free text. However, I would **strongly** encourage you to set up your licenses in a machine readable format.

For example: `https://creativecommons.org/licenses/by-nc/4.0/`

All the occurrences on GBIF have one of the three following licenses:
* [CC0](https://creativecommons.org/publicdomain/zero/1.0/), for data made available for any use without any restrictions
* [CC BY](https://creativecommons.org/licenses/by/4.0/), for data made available for any use with appropriate attribution
* [CC BY-NC](https://creativecommons.org/licenses/by-nc/4.0/), for data made available for any non-commercial use with appropriate attribution

Although your multimedia licenses don't have to match your occurrence licenses, you could consider choosing one of them.

**NB**: This information might change in the near future and I will try to update this post accordingly or make a new post for multimedia licenses specifically.

# Where to host images and other media

Most of the publishers host their own multimedia files but some have used third party platforms such as [flickr.com](https://www.flickr.com).

I would advise against using [iNaturalist.org](https://www.inaturalist.org) as a way to host the images for your dataset. Since the iNaturalist portal makes its [Research-grade Observations](https://www.gbif.org/dataset/50c9509d-22c7-4a22-a47d-8c48425ef4a7) available on GBIF, this would create duplicate occurrences.

If you are publishing dataset through an [IPT](https://github.com/gbif/ipt/wiki), you could consider hosting your mutlimedia files on the same server. You can store your images on a `media` folder and share them with Apache (see [this example](https://ipt.gbif.org/media/)). If your are not publishing with your own IPT, don't hesitate to contact your IPT administrator.

Don't hesitate to leave a comment if you have any question or suggestion.

# References

* [Blog post](https://gbif.blogspot.com/2014/05/multimedia-in-gbif.html) from 2014
* [Presentation](http://labs.gbif.org/%7Emblissett/2018/08/gbif-multimedia-support/#1) from Matt Blissey at TDWG 2018
* [Dublin Core Metadata Initiative - term type](http://dublincore.org/documents/dcmi-terms/#terms-type)
* [TDWC - Dublin Core - term type](https://terms.tdwg.org/wiki/Audubon_Core_Term_List#dc:type)
* [IANA - Media Types - Formerly known as MIME type](https://www.iana.org/assignments/media-types/media-types.xhtml)
* [Apache - tika - MIME types](https://github.com/apache/tika/blob/master/tika-core/src/main/resources/org/apache/tika/mime/tika-mimetypes.xml)
* [GBIF API Media Types](https://gbif.github.io/gbif-api/apidocs/org/gbif/api/vocabulary/MediaType.html)
* [Darwin Core Archives - How-to Guide](https://github.com/gbif/ipt/wiki/DwCAHowToGuide)
* [IPT manual](https://github.com/gbif/ipt/wiki)
* [dwc:associatedMedia](https://dwc.tdwg.org/terms/#dwc:associatedMedia)
* [Simple Multimedia extension](http://rs.gbif.org/extension/gbif/1.0/multimedia.xml)
* [Audubon Media Description extension](http://rs.gbif.org/extension/ac/audubon.xml)
* [Audubon Core](https://www.tdwg.org/standards/ac/)
* [ABCD standard - TDWG](https://www.tdwg.org/standards/abcd/)
* [Publishing to GBIF from a Symbiota portal](http://symbiota.org/docs/publishing-to-gbif-from-a-symbiota-portal/)
* [Quick guide to publishing data through GBIF.org](https://www.gbif.org/publishing-data)
* [Images on an IPT server](https://ipt.gbif.org/media/)