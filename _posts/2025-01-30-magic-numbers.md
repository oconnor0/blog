---
layout: post
title: "Magic numbers"
categories: tech
---

Magic numbers are my newest favorite thing in software.
I mean, they aren't new and I was vaguely aware of them prior but I've had recent opportunities to implement them and use them.

Basically certain binary file formats always start with a sequence of bytes that specify what kind of file it is.
This allows reading the first 2-6 bytes or so and determining - with reasonable certainty - the kind of file.
For example, gzip archives start with `1F`, and `8B`; PNGs start with `89`, `50`, `40`, `47`, `0D`, `0A`, `1A`, and `0A` - bytes 1-3 are "PNG"; and PDFs start with `25`, `50`, `44`, `46`, and `2D` - or "%PDF-".

Knowing some of the common ones in your field can be a simple and useful way to figure out if some file or stream of data or web response is in the format you expect.

1. https://en.wikipedia.org/wiki/List_of_file_signatures
