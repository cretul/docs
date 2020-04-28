---
id: version-18R2_BETA-onWindowOpeningDenied
title: On Window Opening Denied
original_id: onWindowOpeningDenied
---

| Code | Peut être appelé par                        | Définition                       |
| ---- | ------------------------------------------- | -------------------------------- |
| 53   | [Zone Web](FormObjects/webArea_overview.md) | A pop-up window has been blocked |


## Description

This event is generated when the opening of a pop-up window is blocked by the Web area. 4D Web areas do not allow the opening of pop-up windows.

You can find out the blocked URL using the `WA Get last filtered URL` command.

### See also

[`On Open External Link`](onOpenExternalLink.md)