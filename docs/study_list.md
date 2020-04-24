---
layout: default
title: Study List
nav_order: 2
permalink: /docs/study_list
---

# Study List
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The study list is the main interface in Kheops. It is used to visualize the contents of both the [inbox](/docs/inbox) and [albums](/docs/albums). Study lists are in fact lists of DICOM series organized by study. Since the inbox and albums contain series, the series that are available within a study can differ between different context. For example, only a selected number of series might be added to an album. In this case study will contain additional series when viewed in the inbox, and fewer when viewed in the album.

![study list](/img/study_list.png)

## Study Actions

To display the study action, hover the mouse over a study.

![Study Actions](/img/study_actions.png)

| Icon | Action |
|:-----|:-------|
| ![Download](/img/download_action.png) | Download the study as a zip file |
| ![OsiriX](/img/osirix_action.png) | Download the study into OsiriX (Only available on macOS and iOS) |
| ![View](/img/view_action.png) | Open the study in a viewer |
| ![Add](/img/add_action.png) | Add an image or movie to the study. Images will be added to a new series within the study |
| ![Comment](/img/comment_action.png) | Add or view comments made to the study |
| ![Favorite](/img/favorite_action.png) | Set the study as a favorite |
| ![Report Providers](/img/report_provider_action.png) | Activate a Report Provider |

## The Toolbar

The toolbar contains buttons that will perform actions on the selected series. 

![Series List Toolbar](/img/series_list_toolbar.png)

| Icon | Action |
|:---|:----|
| ![Send](/img/send.png) | Send the selected series to another user on Kheops. The sent series will become available in the destination user's inbox. | 
| ![Add to an album](/img/add_to_album.png) | Add the selected series to an album. It is also possible to directly create a new album with the selected series. ![Create album item](/img/create_album_item.png) |
| ![Add to inbox](/img/add_to_inbox.png) | Add the selected series to the inbox. |
|![Favorites](/img/favorites.png) | Add the selected Study to the list of favorites. |
|![Delete](/img/delete.png) | Remove the selected series from the study list. This will remove the selected series from the currently viewed study list, but it will not affect other study lists such as other albums. |

## Selecting Series

All series in a study can be selected by clicking on the checkbox next to the patient name. It is also possible to select only specific series by click on the appropriate checkboxes. For example, in the screenshot below, only 3D VR and 3D MIP are selected. When the buttons in the toolbar are clicked, the action will only be applied to these two series.

![Selected Series](/img/select_series.png)

## Import Studies

You can drag and drop directories containing DICOM files to upload them.

![Import video](/img/import_vid.gif)

Another option is to use the import drop-down menu to import files or directories.

![Import dropdown](/img/import_dropdown.png)

## Study Search

Click on the search icon to display the search bar.

 ![Search](/img/search.png)

![Search Bar](/img/search_bar.png)

## List Refresh

Click on the refresh icon to refresh the study list if needed.

![Refresh](/img/refresh.png)

