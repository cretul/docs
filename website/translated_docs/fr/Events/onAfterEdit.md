---
id: onAfterEdit
title: Sur après modification
---

| Code | Peut être appelé par                                                                                                                                                                                                                                                                                                                                                                                   | Définition                                                                     |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| 45   | [Zone 4D View Pro](FormObjects/viewProArea_overview) - [Zone 4D Write Pro](FormObjects/writeProArea_overview) - [Combo Box](FormObjects/comboBox_overview.md) - Formulaire - [Zone de saisie](FormObjects/input_overview.md) - [Liste hiérarchique](FormObjects/list_overview.md) - [List Box](FormObjects/listbox_overview.md) - [Colonne List Box](FormObjects/listbox_overview.md#list-box-columns) | The contents of the enterable object that has the focus has just been modified |


## Description

### Cas général

Cet événement peut être utilisé pour filtrer la saisie de données dans les objets saisissables au clavier au niveau le plus bas.

When it is used, this event is generated after each change made to the contents of an enterable object, regardless of the action that caused the change, *i.e.*:

- Actions d'édition standard qui modifient le contenu comme les actions coller, couper, supprimer ou annuler;
- Déposer une valeur (action similaire à coller);
- Toute entrée au clavier effectuée par l'utilisateur; dans ce cas, l'événement `On After Edit` est généré après les événements [`On Before Keystroke`](onBeforeKeystroke.md) et [`On After Keystroke`](onAfterKeystroke.md), s'ils sont utilisés.
- Toute modification apportée à l'aide d'une commande de langage qui simule une action de l'utilisateur (c'est-à-dire `POST KEY`).

### 4D View Pro

The object returned by the `FORM Event` command contains:

| Propriété   | Type        | Description                                                                                         |
| ----------- | ----------- | --------------------------------------------------------------------------------------------------- |
| code        | entier long | Sur après modification                                                                              |
| description | Texte       | "On After Edit"                                                                                     |
| objectName  | Texte       | 4D View Pro area name                                                                               |
| sheetName   | Texte       | Name of the sheet of the event                                                                      |
| action      | Texte       | "editChange", "valueChanged", "DragDropBlock", "DragFillBlock", "formulaChanged", "clipboardPasted" |

Depending on the `action` property value, the [event object](overview.md#event-object) will contain additional properties.

#### action = editChange

| Propriété   | Type    | Description                       |
| ----------- | ------- | --------------------------------- |
| range       | object  | Cell range                        |
| editingText | variant | The value from the current editor |

#### action = valueChanged

| Propriété | Type    | Description                 |
| --------- | ------- | --------------------------- |
| range     | object  | Cell range                  |
| oldValue  | variant | Value of cell before change |
| newValue  | variant | Value of cell after change  |


#### action = DragDropBlock

| Propriété | Type    | Description                                         |
| --------- | ------- | --------------------------------------------------- |
| fromRange | object  | Range of source cell range (being dragged)          |
| toRange   | object  | Range of the destination cell range (drop location) |
| copy      | boolean | Specifies if the source range is copied or not      |
| insert    | boolean | Specifies if the source range is inserted or not    |


#### action = DragFillBlock

| Propriété | Type   | Description         |
| --------- | ------ | ------------------- |
| fillRange | object | Range used for fill |
 autoFillType|longint|Value used for the fill.<li>0: Cells are filled with all data (values, formatting, and formulas)<li>1: Cells are filled with automatically sequential data<li>2: Cells are filled with formatting only<li>3: Cells are filled with values but not formatting<li>4: Values are removed from the cells<li>5: Cells are filled automatically| |fillDirection|longint|Direction of the fill.<li>0: The cells to the left are filled<li>1: The cells to the right are filled<li>2: The cells above are filled<li>3: The cells below are filled|


#### action = formulaChanged

| Propriété | Type   | Description         |
| --------- | ------ | ------------------- |
| range     | object | Cell range          |
| formula   | Texte  | The formula entered |

#### action = clipboardPasted

| Propriété   | Type        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| range       | object      | Cell range                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| pasteOption | entier long | Specifies what is pasted from the clipboard:<li>0: Everything is pasted (values, formatting, and formulas)<li>1: Only values are pasted<li>2: Only the formatting is pasted<li>3: Only formulas are pasted<li>4: Values and formatting are pasted (not formulas)<li>5: Formulas and formatting are pasted (not values) |
| pasteData   | object      | The data from the clipboard to be pasted<li>"text" (text): The text from the clipboard<li>"html" (text): The HTML from the clipboard                                                                                                                                                                                                                                                                           |


#### Exemple

Here is an example handling an `On After Edit` event:

```4d
 If(FORM Event.code=On After Edit)
    If(FORM Event.action="valueChanged")
       ALERT("WARNING: You are currently changing the value\  
       from "+String(FORM Event.oldValue)+\  
       " to "+String(FORM Event.newValue)+"!")
    End if
 End if
    End if
 End if
    End if
 End if
```

The above example could generate an event object like this:

```
{

"code":45;
"description":"On After Edit";
"objectName":"ViewProArea"
"sheetname":"Sheet1";
"action":"valueChanged";
"range": {area:ViewProArea,ranges:[{column:1,row:2,sheet:1}]};
"oldValue":"The quick brown fox";
"newValue":"jumped over the lazy dog";
}
```