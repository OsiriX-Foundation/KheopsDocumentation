---
layout: default
title: Study List
nav_order: 2
permalink: /docs/study_list
---

# Study List

The study list is the main interface in Kheops. It is used to visualize the contents of both the [inbox](/docs/inbox) and [albums](/docs/albums). Study lists are in fact lists of DICOM series organized by study. Since the inbox and albums contain series, the series that are availablewithin a study can differ between different context. For example, only a selected number of series might be added to an album. In this case study will contain additional series when viewed in the inbox, and fewer when viewed in the album.

![study list](/img/study_list.png)

## The Toolbar

The toolbar contains buttons that will perform actions on the selected series. 

![Series List Toolbar](/img/series_list_toolbar.png)

* ![Send](/img/send.png) Send the selected series to another user on Kheops. The sent series will become available in the destination user's inbox.
* ![Add to an album](/img/add_to_album.png) Add the selected series to an album. It is also possible to directly create a new album with the selected series. ![Create album item](/img/create_album_item.png)

## Selecting series

All series in a study can be selected by clicking on the checkbox next to the patient name. It is also possible to select only specific series by click on the appropriate checkboxes. For example, in the screeshot below, only 3D VR and 3D MIP are selected. When button in the toolbar are clicked, the action will only be applied to these two series.

![Selected Series](/img/select_series.png)

