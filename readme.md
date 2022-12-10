<!DOCTYPE html>
<html>
<head><meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Titanic assoc rules mining</title><script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>




<style type="text/css">
    pre { line-height: 125%; }
td.linenos .normal { color: inherit; background-color: transparent; padding-left: 5px; padding-right: 5px; }
span.linenos { color: inherit; background-color: transparent; padding-left: 5px; padding-right: 5px; }
td.linenos .special { color: #000000; background-color: #ffffc0; padding-left: 5px; padding-right: 5px; }
span.linenos.special { color: #000000; background-color: #ffffc0; padding-left: 5px; padding-right: 5px; }
.highlight .hll { background-color: var(--jp-cell-editor-active-background) }
.highlight { background: var(--jp-cell-editor-background); color: var(--jp-mirror-editor-variable-color) }
.highlight .c { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment */
.highlight .err { color: var(--jp-mirror-editor-error-color) } /* Error */
.highlight .k { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword */
.highlight .o { color: var(--jp-mirror-editor-operator-color); font-weight: bold } /* Operator */
.highlight .p { color: var(--jp-mirror-editor-punctuation-color) } /* Punctuation */
.highlight .ch { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Multiline */
.highlight .cp { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Preproc */
.highlight .cpf { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.PreprocFile */
.highlight .c1 { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Single */
.highlight .cs { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Special */
.highlight .kc { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Pseudo */
.highlight .kr { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Type */
.highlight .m { color: var(--jp-mirror-editor-number-color) } /* Literal.Number */
.highlight .s { color: var(--jp-mirror-editor-string-color) } /* Literal.String */
.highlight .ow { color: var(--jp-mirror-editor-operator-color); font-weight: bold } /* Operator.Word */
.highlight .w { color: var(--jp-mirror-editor-variable-color) } /* Text.Whitespace */
.highlight .mb { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Bin */
.highlight .mf { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Float */
.highlight .mh { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Hex */
.highlight .mi { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Integer */
.highlight .mo { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Oct */
.highlight .sa { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Affix */
.highlight .sb { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Backtick */
.highlight .sc { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Char */
.highlight .dl { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Delimiter */
.highlight .sd { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Doc */
.highlight .s2 { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Double */
.highlight .se { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Escape */
.highlight .sh { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Heredoc */
.highlight .si { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Interpol */
.highlight .sx { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Other */
.highlight .sr { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Regex */
.highlight .s1 { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Single */
.highlight .ss { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Symbol */
.highlight .il { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Integer.Long */
  </style>



<style type="text/css">
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*
 * Mozilla scrollbar styling
 */

/* use standard opaque scrollbars for most nodes */
[data-jp-theme-scrollbars='true'] {
  scrollbar-color: rgb(var(--jp-scrollbar-thumb-color))
    var(--jp-scrollbar-background-color);
}

/* for code nodes, use a transparent style of scrollbar. These selectors
 * will match lower in the tree, and so will override the above */
[data-jp-theme-scrollbars='true'] .CodeMirror-hscrollbar,
[data-jp-theme-scrollbars='true'] .CodeMirror-vscrollbar {
  scrollbar-color: rgba(var(--jp-scrollbar-thumb-color), 0.5) transparent;
}

/* tiny scrollbar */

.jp-scrollbar-tiny {
  scrollbar-color: rgba(var(--jp-scrollbar-thumb-color), 0.5) transparent;
  scrollbar-width: thin;
}

/*
 * Webkit scrollbar styling
 */

/* use standard opaque scrollbars for most nodes */

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar,
[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-corner {
  background: var(--jp-scrollbar-background-color);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-thumb {
  background: rgb(var(--jp-scrollbar-thumb-color));
  border: var(--jp-scrollbar-thumb-margin) solid transparent;
  background-clip: content-box;
  border-radius: var(--jp-scrollbar-thumb-radius);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-track:horizontal {
  border-left: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
  border-right: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-track:vertical {
  border-top: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
  border-bottom: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
}

/* for code nodes, use a transparent style of scrollbar */

[data-jp-theme-scrollbars='true'] .CodeMirror-hscrollbar::-webkit-scrollbar,
[data-jp-theme-scrollbars='true'] .CodeMirror-vscrollbar::-webkit-scrollbar,
[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-corner,
[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-corner {
  background-color: transparent;
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-thumb,
[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-thumb {
  background: rgba(var(--jp-scrollbar-thumb-color), 0.5);
  border: var(--jp-scrollbar-thumb-margin) solid transparent;
  background-clip: content-box;
  border-radius: var(--jp-scrollbar-thumb-radius);
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-track:horizontal {
  border-left: var(--jp-scrollbar-endpad) solid transparent;
  border-right: var(--jp-scrollbar-endpad) solid transparent;
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-track:vertical {
  border-top: var(--jp-scrollbar-endpad) solid transparent;
  border-bottom: var(--jp-scrollbar-endpad) solid transparent;
}

/* tiny scrollbar */

.jp-scrollbar-tiny::-webkit-scrollbar,
.jp-scrollbar-tiny::-webkit-scrollbar-corner {
  background-color: transparent;
  height: 4px;
  width: 4px;
}

.jp-scrollbar-tiny::-webkit-scrollbar-thumb {
  background: rgba(var(--jp-scrollbar-thumb-color), 0.5);
}

.jp-scrollbar-tiny::-webkit-scrollbar-track:horizontal {
  border-left: 0px solid transparent;
  border-right: 0px solid transparent;
}

.jp-scrollbar-tiny::-webkit-scrollbar-track:vertical {
  border-top: 0px solid transparent;
  border-bottom: 0px solid transparent;
}

/*
 * Phosphor
 */

.lm-ScrollBar[data-orientation='horizontal'] {
  min-height: 16px;
  max-height: 16px;
  min-width: 45px;
  border-top: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='vertical'] {
  min-width: 16px;
  max-width: 16px;
  min-height: 45px;
  border-left: 1px solid #a0a0a0;
}

.lm-ScrollBar-button {
  background-color: #f0f0f0;
  background-position: center center;
  min-height: 15px;
  max-height: 15px;
  min-width: 15px;
  max-width: 15px;
}

.lm-ScrollBar-button:hover {
  background-color: #dadada;
}

.lm-ScrollBar-button.lm-mod-active {
  background-color: #cdcdcd;
}

.lm-ScrollBar-track {
  background: #f0f0f0;
}

.lm-ScrollBar-thumb {
  background: #cdcdcd;
}

.lm-ScrollBar-thumb:hover {
  background: #bababa;
}

.lm-ScrollBar-thumb.lm-mod-active {
  background: #a0a0a0;
}

.lm-ScrollBar[data-orientation='horizontal'] .lm-ScrollBar-thumb {
  height: 100%;
  min-width: 15px;
  border-left: 1px solid #a0a0a0;
  border-right: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='vertical'] .lm-ScrollBar-thumb {
  width: 100%;
  min-height: 15px;
  border-top: 1px solid #a0a0a0;
  border-bottom: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='horizontal']
  .lm-ScrollBar-button[data-action='decrement'] {
  background-image: var(--jp-icon-caret-left);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='horizontal']
  .lm-ScrollBar-button[data-action='increment'] {
  background-image: var(--jp-icon-caret-right);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='vertical']
  .lm-ScrollBar-button[data-action='decrement'] {
  background-image: var(--jp-icon-caret-up);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='vertical']
  .lm-ScrollBar-button[data-action='increment'] {
  background-image: var(--jp-icon-caret-down);
  background-size: 17px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-Widget, /* </DEPRECATED> */
.lm-Widget {
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  cursor: default;
}


/* <DEPRECATED> */ .p-Widget.p-mod-hidden, /* </DEPRECATED> */
.lm-Widget.lm-mod-hidden {
  display: none !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-CommandPalette, /* </DEPRECATED> */
.lm-CommandPalette {
  display: flex;
  flex-direction: column;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-CommandPalette-search, /* </DEPRECATED> */
.lm-CommandPalette-search {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-content, /* </DEPRECATED> */
.lm-CommandPalette-content {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  min-height: 0;
  overflow: auto;
  list-style-type: none;
}


/* <DEPRECATED> */ .p-CommandPalette-header, /* </DEPRECATED> */
.lm-CommandPalette-header {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}


/* <DEPRECATED> */ .p-CommandPalette-item, /* </DEPRECATED> */
.lm-CommandPalette-item {
  display: flex;
  flex-direction: row;
}


/* <DEPRECATED> */ .p-CommandPalette-itemIcon, /* </DEPRECATED> */
.lm-CommandPalette-itemIcon {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-itemContent, /* </DEPRECATED> */
.lm-CommandPalette-itemContent {
  flex: 1 1 auto;
  overflow: hidden;
}


/* <DEPRECATED> */ .p-CommandPalette-itemShortcut, /* </DEPRECATED> */
.lm-CommandPalette-itemShortcut {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-itemLabel, /* </DEPRECATED> */
.lm-CommandPalette-itemLabel {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.lm-close-icon {
	border:1px solid transparent;
  background-color: transparent;
  position: absolute;
	z-index:1;
	right:3%;
	top: 0;
	bottom: 0;
	margin: auto;
	padding: 7px 0;
	display: none;
	vertical-align: middle;
  outline: 0;
  cursor: pointer;
}
.lm-close-icon:after {
	content: "X";
	display: block;
	width: 15px;
	height: 15px;
	text-align: center;
	color:#000;
	font-weight: normal;
	font-size: 12px;
	cursor: pointer;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-DockPanel, /* </DEPRECATED> */
.lm-DockPanel {
  z-index: 0;
}


/* <DEPRECATED> */ .p-DockPanel-widget, /* </DEPRECATED> */
.lm-DockPanel-widget {
  z-index: 0;
}


/* <DEPRECATED> */ .p-DockPanel-tabBar, /* </DEPRECATED> */
.lm-DockPanel-tabBar {
  z-index: 1;
}


/* <DEPRECATED> */ .p-DockPanel-handle, /* </DEPRECATED> */
.lm-DockPanel-handle {
  z-index: 2;
}


/* <DEPRECATED> */ .p-DockPanel-handle.p-mod-hidden, /* </DEPRECATED> */
.lm-DockPanel-handle.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-DockPanel-handle:after, /* </DEPRECATED> */
.lm-DockPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='horizontal'],
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='horizontal'] {
  cursor: ew-resize;
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='vertical'],
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='vertical'] {
  cursor: ns-resize;
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='horizontal']:after,
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='horizontal']:after {
  left: 50%;
  min-width: 8px;
  transform: translateX(-50%);
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='vertical']:after,
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='vertical']:after {
  top: 50%;
  min-height: 8px;
  transform: translateY(-50%);
}


/* <DEPRECATED> */ .p-DockPanel-overlay, /* </DEPRECATED> */
.lm-DockPanel-overlay {
  z-index: 3;
  box-sizing: border-box;
  pointer-events: none;
}


/* <DEPRECATED> */ .p-DockPanel-overlay.p-mod-hidden, /* </DEPRECATED> */
.lm-DockPanel-overlay.lm-mod-hidden {
  display: none !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-Menu, /* </DEPRECATED> */
.lm-Menu {
  z-index: 10000;
  position: absolute;
  white-space: nowrap;
  overflow-x: hidden;
  overflow-y: auto;
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-Menu-content, /* </DEPRECATED> */
.lm-Menu-content {
  margin: 0;
  padding: 0;
  display: table;
  list-style-type: none;
}


/* <DEPRECATED> */ .p-Menu-item, /* </DEPRECATED> */
.lm-Menu-item {
  display: table-row;
}


/* <DEPRECATED> */
.p-Menu-item.p-mod-hidden,
.p-Menu-item.p-mod-collapsed,
/* </DEPRECATED> */
.lm-Menu-item.lm-mod-hidden,
.lm-Menu-item.lm-mod-collapsed {
  display: none !important;
}


/* <DEPRECATED> */
.p-Menu-itemIcon,
.p-Menu-itemSubmenuIcon,
/* </DEPRECATED> */
.lm-Menu-itemIcon,
.lm-Menu-itemSubmenuIcon {
  display: table-cell;
  text-align: center;
}


/* <DEPRECATED> */ .p-Menu-itemLabel, /* </DEPRECATED> */
.lm-Menu-itemLabel {
  display: table-cell;
  text-align: left;
}


/* <DEPRECATED> */ .p-Menu-itemShortcut, /* </DEPRECATED> */
.lm-Menu-itemShortcut {
  display: table-cell;
  text-align: right;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-MenuBar, /* </DEPRECATED> */
.lm-MenuBar {
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-MenuBar-content, /* </DEPRECATED> */
.lm-MenuBar-content {
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: row;
  list-style-type: none;
}


/* <DEPRECATED> */ .p--MenuBar-item, /* </DEPRECATED> */
.lm-MenuBar-item {
  box-sizing: border-box;
}


/* <DEPRECATED> */
.p-MenuBar-itemIcon,
.p-MenuBar-itemLabel,
/* </DEPRECATED> */
.lm-MenuBar-itemIcon,
.lm-MenuBar-itemLabel {
  display: inline-block;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-ScrollBar, /* </DEPRECATED> */
.lm-ScrollBar {
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */
.p-ScrollBar[data-orientation='horizontal'],
/* </DEPRECATED> */
.lm-ScrollBar[data-orientation='horizontal'] {
  flex-direction: row;
}


/* <DEPRECATED> */
.p-ScrollBar[data-orientation='vertical'],
/* </DEPRECATED> */
.lm-ScrollBar[data-orientation='vertical'] {
  flex-direction: column;
}


/* <DEPRECATED> */ .p-ScrollBar-button, /* </DEPRECATED> */
.lm-ScrollBar-button {
  box-sizing: border-box;
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-ScrollBar-track, /* </DEPRECATED> */
.lm-ScrollBar-track {
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  flex: 1 1 auto;
}


/* <DEPRECATED> */ .p-ScrollBar-thumb, /* </DEPRECATED> */
.lm-ScrollBar-thumb {
  box-sizing: border-box;
  position: absolute;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-SplitPanel-child, /* </DEPRECATED> */
.lm-SplitPanel-child {
  z-index: 0;
}


/* <DEPRECATED> */ .p-SplitPanel-handle, /* </DEPRECATED> */
.lm-SplitPanel-handle {
  z-index: 1;
}


/* <DEPRECATED> */ .p-SplitPanel-handle.p-mod-hidden, /* </DEPRECATED> */
.lm-SplitPanel-handle.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-SplitPanel-handle:after, /* </DEPRECATED> */
.lm-SplitPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='horizontal'] > .lm-SplitPanel-handle {
  cursor: ew-resize;
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='vertical'] > .lm-SplitPanel-handle {
  cursor: ns-resize;
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle:after,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='horizontal'] > .lm-SplitPanel-handle:after {
  left: 50%;
  min-width: 8px;
  transform: translateX(-50%);
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle:after,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='vertical'] > .lm-SplitPanel-handle:after {
  top: 50%;
  min-height: 8px;
  transform: translateY(-50%);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-TabBar, /* </DEPRECATED> */
.lm-TabBar {
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-TabBar[data-orientation='horizontal'], /* </DEPRECATED> */
.lm-TabBar[data-orientation='horizontal'] {
  flex-direction: row;
  align-items: flex-end;
}


/* <DEPRECATED> */ .p-TabBar[data-orientation='vertical'], /* </DEPRECATED> */
.lm-TabBar[data-orientation='vertical'] {
  flex-direction: column;
  align-items: flex-end;
}


/* <DEPRECATED> */ .p-TabBar-content, /* </DEPRECATED> */
.lm-TabBar-content {
  margin: 0;
  padding: 0;
  display: flex;
  flex: 1 1 auto;
  list-style-type: none;
}


/* <DEPRECATED> */
.p-TabBar[data-orientation='horizontal'] > .p-TabBar-content,
/* </DEPRECATED> */
.lm-TabBar[data-orientation='horizontal'] > .lm-TabBar-content {
  flex-direction: row;
}


/* <DEPRECATED> */
.p-TabBar[data-orientation='vertical'] > .p-TabBar-content,
/* </DEPRECATED> */
.lm-TabBar[data-orientation='vertical'] > .lm-TabBar-content {
  flex-direction: column;
}


/* <DEPRECATED> */ .p-TabBar-tab, /* </DEPRECATED> */
.lm-TabBar-tab {
  display: flex;
  flex-direction: row;
  box-sizing: border-box;
  overflow: hidden;
}


/* <DEPRECATED> */
.p-TabBar-tabIcon,
.p-TabBar-tabCloseIcon,
/* </DEPRECATED> */
.lm-TabBar-tabIcon,
.lm-TabBar-tabCloseIcon {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-TabBar-tabLabel, /* </DEPRECATED> */
.lm-TabBar-tabLabel {
  flex: 1 1 auto;
  overflow: hidden;
  white-space: nowrap;
}


.lm-TabBar-tabInput {
  user-select: all;
  width: 100%;
  box-sizing : border-box;
}


/* <DEPRECATED> */ .p-TabBar-tab.p-mod-hidden, /* </DEPRECATED> */
.lm-TabBar-tab.lm-mod-hidden {
  display: none !important;
}


.lm-TabBar-addButton.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-TabBar.p-mod-dragging .p-TabBar-tab, /* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging .lm-TabBar-tab {
  position: relative;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging[data-orientation='horizontal'] .p-TabBar-tab,
/* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging[data-orientation='horizontal'] .lm-TabBar-tab {
  left: 0;
  transition: left 150ms ease;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging[data-orientation='vertical'] .p-TabBar-tab,
/* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging[data-orientation='vertical'] .lm-TabBar-tab {
  top: 0;
  transition: top 150ms ease;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging .p-TabBar-tab.p-mod-dragging,
/* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging .lm-TabBar-tab.lm-mod-dragging {
  transition: none;
}

.lm-TabBar-tabLabel .lm-TabBar-tabInput {
  user-select: all;
  width: 100%;
  box-sizing : border-box;
  background: inherit;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-TabPanel-tabBar, /* </DEPRECATED> */
.lm-TabPanel-tabBar {
  z-index: 1;
}


/* <DEPRECATED> */ .p-TabPanel-stackedPanel, /* </DEPRECATED> */
.lm-TabPanel-stackedPanel {
  z-index: 0;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/

@charset "UTF-8";
html{
  -webkit-box-sizing:border-box;
          box-sizing:border-box; }

*,
*::before,
*::after{
  -webkit-box-sizing:inherit;
          box-sizing:inherit; }

body{
  font-size:14px;
  font-weight:400;
  letter-spacing:0;
  line-height:1.28581;
  text-transform:none;
  color:#182026;
  font-family:-apple-system, "BlinkMacSystemFont", "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Open Sans", "Helvetica Neue", "Icons16", sans-serif; }

p{
  margin-bottom:10px;
  margin-top:0; }

small{
  font-size:12px; }

strong{
  font-weight:600; }

::-moz-selection{
  background:rgba(125, 188, 255, 0.6); }

::selection{
  background:rgba(125, 188, 255, 0.6); }
.bp3-heading{
  color:#182026;
  font-weight:600;
  margin:0 0 10px;
  padding:0; }
  .bp3-dark .bp3-heading{
    color:#f5f8fa; }

h1.bp3-heading, .bp3-running-text h1{
  font-size:36px;
  line-height:40px; }

h2.bp3-heading, .bp3-running-text h2{
  font-size:28px;
  line-height:32px; }

h3.bp3-heading, .bp3-running-text h3{
  font-size:22px;
  line-height:25px; }

h4.bp3-heading, .bp3-running-text h4{
  font-size:18px;
  line-height:21px; }

h5.bp3-heading, .bp3-running-text h5{
  font-size:16px;
  line-height:19px; }

h6.bp3-heading, .bp3-running-text h6{
  font-size:14px;
  line-height:16px; }
.bp3-ui-text{
  font-size:14px;
  font-weight:400;
  letter-spacing:0;
  line-height:1.28581;
  text-transform:none; }

.bp3-monospace-text{
  font-family:monospace;
  text-transform:none; }

.bp3-text-muted{
  color:#5c7080; }
  .bp3-dark .bp3-text-muted{
    color:#a7b6c2; }

.bp3-text-disabled{
  color:rgba(92, 112, 128, 0.6); }
  .bp3-dark .bp3-text-disabled{
    color:rgba(167, 182, 194, 0.6); }

.bp3-text-overflow-ellipsis{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal; }
.bp3-running-text{
  font-size:14px;
  line-height:1.5; }
  .bp3-running-text h1{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h1{
      color:#f5f8fa; }
  .bp3-running-text h2{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h2{
      color:#f5f8fa; }
  .bp3-running-text h3{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h3{
      color:#f5f8fa; }
  .bp3-running-text h4{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h4{
      color:#f5f8fa; }
  .bp3-running-text h5{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h5{
      color:#f5f8fa; }
  .bp3-running-text h6{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h6{
      color:#f5f8fa; }
  .bp3-running-text hr{
    border:none;
    border-bottom:1px solid rgba(16, 22, 26, 0.15);
    margin:20px 0; }
    .bp3-dark .bp3-running-text hr{
      border-color:rgba(255, 255, 255, 0.15); }
  .bp3-running-text p{
    margin:0 0 10px;
    padding:0; }

.bp3-text-large{
  font-size:16px; }

.bp3-text-small{
  font-size:12px; }
a{
  color:#106ba3;
  text-decoration:none; }
  a:hover{
    color:#106ba3;
    cursor:pointer;
    text-decoration:underline; }
  a .bp3-icon, a .bp3-icon-standard, a .bp3-icon-large{
    color:inherit; }
  a code,
  .bp3-dark a code{
    color:inherit; }
  .bp3-dark a,
  .bp3-dark a:hover{
    color:#48aff0; }
    .bp3-dark a .bp3-icon, .bp3-dark a .bp3-icon-standard, .bp3-dark a .bp3-icon-large,
    .bp3-dark a:hover .bp3-icon,
    .bp3-dark a:hover .bp3-icon-standard,
    .bp3-dark a:hover .bp3-icon-large{
      color:inherit; }
.bp3-running-text code, .bp3-code{
  font-family:monospace;
  text-transform:none;
  background:rgba(255, 255, 255, 0.7);
  border-radius:3px;
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2);
  color:#5c7080;
  font-size:smaller;
  padding:2px 5px; }
  .bp3-dark .bp3-running-text code, .bp3-running-text .bp3-dark code, .bp3-dark .bp3-code{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#a7b6c2; }
  .bp3-running-text a > code, a > .bp3-code{
    color:#137cbd; }
    .bp3-dark .bp3-running-text a > code, .bp3-running-text .bp3-dark a > code, .bp3-dark a > .bp3-code{
      color:inherit; }

.bp3-running-text pre, .bp3-code-block{
  font-family:monospace;
  text-transform:none;
  background:rgba(255, 255, 255, 0.7);
  border-radius:3px;
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
  color:#182026;
  display:block;
  font-size:13px;
  line-height:1.4;
  margin:10px 0;
  padding:13px 15px 12px;
  word-break:break-all;
  word-wrap:break-word; }
  .bp3-dark .bp3-running-text pre, .bp3-running-text .bp3-dark pre, .bp3-dark .bp3-code-block{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
  .bp3-running-text pre > code, .bp3-code-block > code{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:inherit;
    font-size:inherit;
    padding:0; }

.bp3-running-text kbd, .bp3-key{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#5c7080;
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  font-family:inherit;
  font-size:12px;
  height:24px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  line-height:24px;
  min-width:24px;
  padding:3px 6px;
  vertical-align:middle; }
  .bp3-running-text kbd .bp3-icon, .bp3-key .bp3-icon, .bp3-running-text kbd .bp3-icon-standard, .bp3-key .bp3-icon-standard, .bp3-running-text kbd .bp3-icon-large, .bp3-key .bp3-icon-large{
    margin-right:5px; }
  .bp3-dark .bp3-running-text kbd, .bp3-running-text .bp3-dark kbd, .bp3-dark .bp3-key{
    background:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#a7b6c2; }
.bp3-running-text blockquote, .bp3-blockquote{
  border-left:solid 4px rgba(167, 182, 194, 0.5);
  margin:0 0 10px;
  padding:0 20px; }
  .bp3-dark .bp3-running-text blockquote, .bp3-running-text .bp3-dark blockquote, .bp3-dark .bp3-blockquote{
    border-color:rgba(115, 134, 148, 0.5); }
.bp3-running-text ul,
.bp3-running-text ol, .bp3-list{
  margin:10px 0;
  padding-left:30px; }
  .bp3-running-text ul li:not(:last-child), .bp3-running-text ol li:not(:last-child), .bp3-list li:not(:last-child){
    margin-bottom:5px; }
  .bp3-running-text ul ol, .bp3-running-text ol ol, .bp3-list ol,
  .bp3-running-text ul ul,
  .bp3-running-text ol ul,
  .bp3-list ul{
    margin-top:5px; }

.bp3-list-unstyled{
  list-style:none;
  margin:0;
  padding:0; }
  .bp3-list-unstyled li{
    padding:0; }
.bp3-rtl{
  text-align:right; }

.bp3-dark{
  color:#f5f8fa; }

:focus{
  outline:rgba(19, 124, 189, 0.6) auto 2px;
  outline-offset:2px;
  -moz-outline-radius:6px; }

.bp3-focus-disabled :focus{
  outline:none !important; }
  .bp3-focus-disabled :focus ~ .bp3-control-indicator{
    outline:none !important; }

.bp3-alert{
  max-width:400px;
  padding:20px; }

.bp3-alert-body{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }
  .bp3-alert-body .bp3-icon{
    font-size:40px;
    margin-right:20px;
    margin-top:0; }

.bp3-alert-contents{
  word-break:break-word; }

.bp3-alert-footer{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:reverse;
      -ms-flex-direction:row-reverse;
          flex-direction:row-reverse;
  margin-top:10px; }
  .bp3-alert-footer .bp3-button{
    margin-left:10px; }
.bp3-breadcrumbs{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  cursor:default;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-wrap:wrap;
      flex-wrap:wrap;
  height:30px;
  list-style:none;
  margin:0;
  padding:0; }
  .bp3-breadcrumbs > li{
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex; }
    .bp3-breadcrumbs > li::after{
      background:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M10.71 7.29l-4-4a1.003 1.003 0 00-1.42 1.42L8.59 8 5.3 11.29c-.19.18-.3.43-.3.71a1.003 1.003 0 001.71.71l4-4c.18-.18.29-.43.29-.71 0-.28-.11-.53-.29-.71z' fill='%235C7080'/%3e%3c/svg%3e");
      content:"";
      display:block;
      height:16px;
      margin:0 5px;
      width:16px; }
    .bp3-breadcrumbs > li:last-of-type::after{
      display:none; }

.bp3-breadcrumb,
.bp3-breadcrumb-current,
.bp3-breadcrumbs-collapsed{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  font-size:16px; }

.bp3-breadcrumb,
.bp3-breadcrumbs-collapsed{
  color:#5c7080; }

.bp3-breadcrumb:hover{
  text-decoration:none; }

.bp3-breadcrumb.bp3-disabled{
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-breadcrumb .bp3-icon{
  margin-right:5px; }

.bp3-breadcrumb-current{
  color:inherit;
  font-weight:600; }
  .bp3-breadcrumb-current .bp3-input{
    font-size:inherit;
    font-weight:inherit;
    vertical-align:baseline; }

.bp3-breadcrumbs-collapsed{
  background:#ced9e0;
  border:none;
  border-radius:3px;
  cursor:pointer;
  margin-right:2px;
  padding:1px 5px;
  vertical-align:text-bottom; }
  .bp3-breadcrumbs-collapsed::before{
    background:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cg fill='%235C7080'%3e%3ccircle cx='2' cy='8.03' r='2'/%3e%3ccircle cx='14' cy='8.03' r='2'/%3e%3ccircle cx='8' cy='8.03' r='2'/%3e%3c/g%3e%3c/svg%3e") center no-repeat;
    content:"";
    display:block;
    height:16px;
    width:16px; }
  .bp3-breadcrumbs-collapsed:hover{
    background:#bfccd6;
    color:#182026;
    text-decoration:none; }

.bp3-dark .bp3-breadcrumb,
.bp3-dark .bp3-breadcrumbs-collapsed{
  color:#a7b6c2; }

.bp3-dark .bp3-breadcrumbs > li::after{
  color:#a7b6c2; }

.bp3-dark .bp3-breadcrumb.bp3-disabled{
  color:rgba(167, 182, 194, 0.6); }

.bp3-dark .bp3-breadcrumb-current{
  color:#f5f8fa; }

.bp3-dark .bp3-breadcrumbs-collapsed{
  background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-breadcrumbs-collapsed:hover{
    background:rgba(16, 22, 26, 0.6);
    color:#f5f8fa; }
.bp3-button{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border:none;
  border-radius:3px;
  cursor:pointer;
  font-size:14px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  padding:5px 10px;
  text-align:left;
  vertical-align:middle;
  min-height:30px;
  min-width:30px; }
  .bp3-button > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-button > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-button::before,
  .bp3-button > *{
    margin-right:7px; }
  .bp3-button:empty::before,
  .bp3-button > :last-child{
    margin-right:0; }
  .bp3-button:empty{
    padding:0 !important; }
  .bp3-button:disabled, .bp3-button.bp3-disabled{
    cursor:not-allowed; }
  .bp3-button.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-button.bp3-align-right,
  .bp3-align-right .bp3-button{
    text-align:right; }
  .bp3-button.bp3-align-left,
  .bp3-align-left .bp3-button{
    text-align:left; }
  .bp3-button:not([class*="bp3-intent-"]){
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    color:#182026; }
    .bp3-button:not([class*="bp3-intent-"]):hover{
      background-clip:padding-box;
      background-color:#ebf1f5;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
    .bp3-button:not([class*="bp3-intent-"]):active, .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      background-color:#d8e1e8;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button:not([class*="bp3-intent-"]):disabled, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled{
      background-color:rgba(206, 217, 224, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      outline:none; }
      .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active, .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active:hover, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active:hover{
        background:rgba(206, 217, 224, 0.7); }
  .bp3-button.bp3-intent-primary{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-primary:hover, .bp3-button.bp3-intent-primary:active, .bp3-button.bp3-intent-primary.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-primary:hover{
      background-color:#106ba3;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-primary:active, .bp3-button.bp3-intent-primary.bp3-active{
      background-color:#0e5a8a;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-primary:disabled, .bp3-button.bp3-intent-primary.bp3-disabled{
      background-color:rgba(19, 124, 189, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-success{
    background-color:#0f9960;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-success:hover, .bp3-button.bp3-intent-success:active, .bp3-button.bp3-intent-success.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-success:hover{
      background-color:#0d8050;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-success:active, .bp3-button.bp3-intent-success.bp3-active{
      background-color:#0a6640;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-success:disabled, .bp3-button.bp3-intent-success.bp3-disabled{
      background-color:rgba(15, 153, 96, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-warning{
    background-color:#d9822b;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-warning:hover, .bp3-button.bp3-intent-warning:active, .bp3-button.bp3-intent-warning.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-warning:hover{
      background-color:#bf7326;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-warning:active, .bp3-button.bp3-intent-warning.bp3-active{
      background-color:#a66321;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-warning:disabled, .bp3-button.bp3-intent-warning.bp3-disabled{
      background-color:rgba(217, 130, 43, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-danger{
    background-color:#db3737;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-danger:hover, .bp3-button.bp3-intent-danger:active, .bp3-button.bp3-intent-danger.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-danger:hover{
      background-color:#c23030;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-danger:active, .bp3-button.bp3-intent-danger.bp3-active{
      background-color:#a82a2a;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-danger:disabled, .bp3-button.bp3-intent-danger.bp3-disabled{
      background-color:rgba(219, 55, 55, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button[class*="bp3-intent-"] .bp3-button-spinner .bp3-spinner-head{
    stroke:#ffffff; }
  .bp3-button.bp3-large,
  .bp3-large .bp3-button{
    min-height:40px;
    min-width:40px;
    font-size:16px;
    padding:5px 15px; }
    .bp3-button.bp3-large::before,
    .bp3-button.bp3-large > *,
    .bp3-large .bp3-button::before,
    .bp3-large .bp3-button > *{
      margin-right:10px; }
    .bp3-button.bp3-large:empty::before,
    .bp3-button.bp3-large > :last-child,
    .bp3-large .bp3-button:empty::before,
    .bp3-large .bp3-button > :last-child{
      margin-right:0; }
  .bp3-button.bp3-small,
  .bp3-small .bp3-button{
    min-height:24px;
    min-width:24px;
    padding:0 7px; }
  .bp3-button.bp3-loading{
    position:relative; }
    .bp3-button.bp3-loading[class*="bp3-icon-"]::before{
      visibility:hidden; }
    .bp3-button.bp3-loading .bp3-button-spinner{
      margin:0;
      position:absolute; }
    .bp3-button.bp3-loading > :not(.bp3-button-spinner){
      visibility:hidden; }
  .bp3-button[class*="bp3-icon-"]::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    color:#5c7080; }
  .bp3-button .bp3-icon, .bp3-button .bp3-icon-standard, .bp3-button .bp3-icon-large{
    color:#5c7080; }
    .bp3-button .bp3-icon.bp3-align-right, .bp3-button .bp3-icon-standard.bp3-align-right, .bp3-button .bp3-icon-large.bp3-align-right{
      margin-left:7px; }
  .bp3-button .bp3-icon:first-child:last-child,
  .bp3-button .bp3-spinner + .bp3-icon:last-child{
    margin:0 -7px; }
  .bp3-dark .bp3-button:not([class*="bp3-intent-"]){
    background-color:#394b59;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):hover, .bp3-dark .bp3-button:not([class*="bp3-intent-"]):active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      color:#f5f8fa; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):hover{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):disabled, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-disabled{
      background-color:rgba(57, 75, 89, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
      .bp3-dark .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active{
        background:rgba(57, 75, 89, 0.7); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-button-spinner .bp3-spinner-head{
      background:rgba(16, 22, 26, 0.5);
      stroke:#8a9ba8; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"])[class*="bp3-icon-"]::before{
      color:#a7b6c2; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon, .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon-standard, .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon-large{
      color:#a7b6c2; }
  .bp3-dark .bp3-button[class*="bp3-intent-"]{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:hover{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:active, .bp3-dark .bp3-button[class*="bp3-intent-"].bp3-active{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:disabled, .bp3-dark .bp3-button[class*="bp3-intent-"].bp3-disabled{
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.3); }
    .bp3-dark .bp3-button[class*="bp3-intent-"] .bp3-button-spinner .bp3-spinner-head{
      stroke:#8a9ba8; }
  .bp3-button:disabled::before,
  .bp3-button:disabled .bp3-icon, .bp3-button:disabled .bp3-icon-standard, .bp3-button:disabled .bp3-icon-large, .bp3-button.bp3-disabled::before,
  .bp3-button.bp3-disabled .bp3-icon, .bp3-button.bp3-disabled .bp3-icon-standard, .bp3-button.bp3-disabled .bp3-icon-large, .bp3-button[class*="bp3-intent-"]::before,
  .bp3-button[class*="bp3-intent-"] .bp3-icon, .bp3-button[class*="bp3-intent-"] .bp3-icon-standard, .bp3-button[class*="bp3-intent-"] .bp3-icon-large{
    color:inherit !important; }
  .bp3-button.bp3-minimal{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-button.bp3-minimal:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button.bp3-minimal:active, .bp3-button.bp3-minimal.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button.bp3-minimal:disabled, .bp3-button.bp3-minimal:disabled:hover, .bp3-button.bp3-minimal.bp3-disabled, .bp3-button.bp3-minimal.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button.bp3-minimal:disabled.bp3-active, .bp3-button.bp3-minimal:disabled:hover.bp3-active, .bp3-button.bp3-minimal.bp3-disabled.bp3-active, .bp3-button.bp3-minimal.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button.bp3-minimal{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button.bp3-minimal:hover, .bp3-dark .bp3-button.bp3-minimal:active, .bp3-dark .bp3-button.bp3-minimal.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button.bp3-minimal:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button.bp3-minimal:active, .bp3-dark .bp3-button.bp3-minimal.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button.bp3-minimal:disabled, .bp3-dark .bp3-button.bp3-minimal:disabled:hover, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button.bp3-minimal:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal:disabled:hover.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:hover, .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:disabled, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-primary:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-success{
      color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:hover, .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:disabled, .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-success:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:hover, .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:disabled, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-warning:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-danger{
      color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:hover, .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:disabled, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-danger:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
  .bp3-button.bp3-outlined{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    border:1px solid rgba(24, 32, 38, 0.2);
    -webkit-box-sizing:border-box;
            box-sizing:border-box; }
    .bp3-button.bp3-outlined:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button.bp3-outlined:active, .bp3-button.bp3-outlined.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button.bp3-outlined:disabled, .bp3-button.bp3-outlined:disabled:hover, .bp3-button.bp3-outlined.bp3-disabled, .bp3-button.bp3-outlined.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button.bp3-outlined:disabled.bp3-active, .bp3-button.bp3-outlined:disabled:hover.bp3-active, .bp3-button.bp3-outlined.bp3-disabled.bp3-active, .bp3-button.bp3-outlined.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button.bp3-outlined{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button.bp3-outlined:hover, .bp3-dark .bp3-button.bp3-outlined:active, .bp3-dark .bp3-button.bp3-outlined.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button.bp3-outlined:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button.bp3-outlined:active, .bp3-dark .bp3-button.bp3-outlined.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button.bp3-outlined:disabled, .bp3-dark .bp3-button.bp3-outlined:disabled:hover, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button.bp3-outlined:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined:disabled:hover.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:hover, .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-primary:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-success{
      color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:hover, .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-success:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:hover, .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-warning:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-danger{
      color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:hover, .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-danger:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
    .bp3-button.bp3-outlined:disabled, .bp3-button.bp3-outlined.bp3-disabled, .bp3-button.bp3-outlined:disabled:hover, .bp3-button.bp3-outlined.bp3-disabled:hover{
      border-color:rgba(92, 112, 128, 0.1); }
    .bp3-dark .bp3-button.bp3-outlined{
      border-color:rgba(255, 255, 255, 0.4); }
      .bp3-dark .bp3-button.bp3-outlined:disabled, .bp3-dark .bp3-button.bp3-outlined:disabled:hover, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover{
        border-color:rgba(255, 255, 255, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-primary{
      border-color:rgba(16, 107, 163, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
        border-color:rgba(16, 107, 163, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary{
        border-color:rgba(72, 175, 240, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
          border-color:rgba(72, 175, 240, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-success{
      border-color:rgba(13, 128, 80, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
        border-color:rgba(13, 128, 80, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success{
        border-color:rgba(61, 204, 145, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
          border-color:rgba(61, 204, 145, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-warning{
      border-color:rgba(191, 115, 38, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
        border-color:rgba(191, 115, 38, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning{
        border-color:rgba(255, 179, 102, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
          border-color:rgba(255, 179, 102, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-danger{
      border-color:rgba(194, 48, 48, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
        border-color:rgba(194, 48, 48, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger{
        border-color:rgba(255, 115, 115, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
          border-color:rgba(255, 115, 115, 0.2); }

a.bp3-button{
  text-align:center;
  text-decoration:none;
  -webkit-transition:none;
  transition:none; }
  a.bp3-button, a.bp3-button:hover, a.bp3-button:active{
    color:#182026; }
  a.bp3-button.bp3-disabled{
    color:rgba(92, 112, 128, 0.6); }

.bp3-button-text{
  -webkit-box-flex:0;
      -ms-flex:0 1 auto;
          flex:0 1 auto; }

.bp3-button.bp3-align-left .bp3-button-text, .bp3-button.bp3-align-right .bp3-button-text,
.bp3-button-group.bp3-align-left .bp3-button-text,
.bp3-button-group.bp3-align-right .bp3-button-text{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto; }
.bp3-button-group{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex; }
  .bp3-button-group .bp3-button{
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    position:relative;
    z-index:4; }
    .bp3-button-group .bp3-button:focus{
      z-index:5; }
    .bp3-button-group .bp3-button:hover{
      z-index:6; }
    .bp3-button-group .bp3-button:active, .bp3-button-group .bp3-button.bp3-active{
      z-index:7; }
    .bp3-button-group .bp3-button:disabled, .bp3-button-group .bp3-button.bp3-disabled{
      z-index:3; }
    .bp3-button-group .bp3-button[class*="bp3-intent-"]{
      z-index:9; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:focus{
        z-index:10; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:hover{
        z-index:11; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:active, .bp3-button-group .bp3-button[class*="bp3-intent-"].bp3-active{
        z-index:12; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:disabled, .bp3-button-group .bp3-button[class*="bp3-intent-"].bp3-disabled{
        z-index:8; }
  .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:first-child) .bp3-button,
  .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:first-child){
    border-bottom-left-radius:0;
    border-top-left-radius:0; }
  .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:last-child){
    border-bottom-right-radius:0;
    border-top-right-radius:0;
    margin-right:-1px; }
  .bp3-button-group.bp3-minimal .bp3-button{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-button-group.bp3-minimal .bp3-button:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button-group.bp3-minimal .bp3-button:disabled, .bp3-button-group.bp3-minimal .bp3-button:disabled:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button-group.bp3-minimal .bp3-button:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button:disabled:hover.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button-group.bp3-minimal .bp3-button{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:hover, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled:hover, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled:hover.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success{
      color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger{
      color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
  .bp3-button-group .bp3-popover-wrapper,
  .bp3-button-group .bp3-popover-target{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-button-group.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-button-group .bp3-button.bp3-fill,
  .bp3-button-group.bp3-fill .bp3-button:not(.bp3-fixed){
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-button-group.bp3-vertical{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column;
    vertical-align:top; }
    .bp3-button-group.bp3-vertical.bp3-fill{
      height:100%;
      width:unset; }
    .bp3-button-group.bp3-vertical .bp3-button{
      margin-right:0 !important;
      width:100%; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:first-child .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:first-child{
      border-radius:3px 3px 0 0; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:last-child .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:last-child{
      border-radius:0 0 3px 3px; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:not(:last-child){
      margin-bottom:-1px; }
  .bp3-button-group.bp3-align-left .bp3-button{
    text-align:left; }
  .bp3-dark .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-dark .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:last-child){
    margin-right:1px; }
  .bp3-dark .bp3-button-group.bp3-vertical > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-dark .bp3-button-group.bp3-vertical > .bp3-button:not(:last-child){
    margin-bottom:1px; }
.bp3-callout{
  font-size:14px;
  line-height:1.5;
  background-color:rgba(138, 155, 168, 0.15);
  border-radius:3px;
  padding:10px 12px 9px;
  position:relative;
  width:100%; }
  .bp3-callout[class*="bp3-icon-"]{
    padding-left:40px; }
    .bp3-callout[class*="bp3-icon-"]::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased;
      color:#5c7080;
      left:10px;
      position:absolute;
      top:10px; }
  .bp3-callout.bp3-callout-icon{
    padding-left:40px; }
    .bp3-callout.bp3-callout-icon > .bp3-icon:first-child{
      color:#5c7080;
      left:10px;
      position:absolute;
      top:10px; }
  .bp3-callout .bp3-heading{
    line-height:20px;
    margin-bottom:5px;
    margin-top:0; }
    .bp3-callout .bp3-heading:last-child{
      margin-bottom:0; }
  .bp3-dark .bp3-callout{
    background-color:rgba(138, 155, 168, 0.2); }
    .bp3-dark .bp3-callout[class*="bp3-icon-"]::before{
      color:#a7b6c2; }
  .bp3-callout.bp3-intent-primary{
    background-color:rgba(19, 124, 189, 0.15); }
    .bp3-callout.bp3-intent-primary[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-primary > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-primary .bp3-heading{
      color:#106ba3; }
    .bp3-dark .bp3-callout.bp3-intent-primary{
      background-color:rgba(19, 124, 189, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-primary[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-primary > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-primary .bp3-heading{
        color:#48aff0; }
  .bp3-callout.bp3-intent-success{
    background-color:rgba(15, 153, 96, 0.15); }
    .bp3-callout.bp3-intent-success[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-success > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-success .bp3-heading{
      color:#0d8050; }
    .bp3-dark .bp3-callout.bp3-intent-success{
      background-color:rgba(15, 153, 96, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-success[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-success > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-success .bp3-heading{
        color:#3dcc91; }
  .bp3-callout.bp3-intent-warning{
    background-color:rgba(217, 130, 43, 0.15); }
    .bp3-callout.bp3-intent-warning[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-warning > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-warning .bp3-heading{
      color:#bf7326; }
    .bp3-dark .bp3-callout.bp3-intent-warning{
      background-color:rgba(217, 130, 43, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-warning[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-warning > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-warning .bp3-heading{
        color:#ffb366; }
  .bp3-callout.bp3-intent-danger{
    background-color:rgba(219, 55, 55, 0.15); }
    .bp3-callout.bp3-intent-danger[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-danger > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-danger .bp3-heading{
      color:#c23030; }
    .bp3-dark .bp3-callout.bp3-intent-danger{
      background-color:rgba(219, 55, 55, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-danger[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-danger > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-danger .bp3-heading{
        color:#ff7373; }
  .bp3-running-text .bp3-callout{
    margin:20px 0; }
.bp3-card{
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
  padding:20px;
  -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-card.bp3-dark,
  .bp3-dark .bp3-card{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }

.bp3-elevation-0{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }
  .bp3-elevation-0.bp3-dark,
  .bp3-dark .bp3-elevation-0{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }

.bp3-elevation-1{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-1.bp3-dark,
  .bp3-dark .bp3-elevation-1{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-elevation-2{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 1px 1px rgba(16, 22, 26, 0.2), 0 2px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 1px 1px rgba(16, 22, 26, 0.2), 0 2px 6px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-2.bp3-dark,
  .bp3-dark .bp3-elevation-2{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.4), 0 2px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.4), 0 2px 6px rgba(16, 22, 26, 0.4); }

.bp3-elevation-3{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-3.bp3-dark,
  .bp3-dark .bp3-elevation-3{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }

.bp3-elevation-4{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-4.bp3-dark,
  .bp3-dark .bp3-elevation-4{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4); }

.bp3-card.bp3-interactive:hover{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  cursor:pointer; }
  .bp3-card.bp3-interactive:hover.bp3-dark,
  .bp3-dark .bp3-card.bp3-interactive:hover{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }

.bp3-card.bp3-interactive:active{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  opacity:0.9;
  -webkit-transition-duration:0;
          transition-duration:0; }
  .bp3-card.bp3-interactive:active.bp3-dark,
  .bp3-dark .bp3-card.bp3-interactive:active{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-collapse{
  height:0;
  overflow-y:hidden;
  -webkit-transition:height 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:height 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-collapse .bp3-collapse-body{
    -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-collapse .bp3-collapse-body[aria-hidden="true"]{
      display:none; }

.bp3-context-menu .bp3-popover-target{
  display:block; }

.bp3-context-menu-popover-target{
  position:fixed; }

.bp3-divider{
  border-bottom:1px solid rgba(16, 22, 26, 0.15);
  border-right:1px solid rgba(16, 22, 26, 0.15);
  margin:5px; }
  .bp3-dark .bp3-divider{
    border-color:rgba(16, 22, 26, 0.4); }
.bp3-dialog-container{
  opacity:1;
  -webkit-transform:scale(1);
          transform:scale(1);
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  min-height:100%;
  pointer-events:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none;
  width:100%; }
  .bp3-dialog-container.bp3-overlay-enter > .bp3-dialog, .bp3-dialog-container.bp3-overlay-appear > .bp3-dialog{
    opacity:0;
    -webkit-transform:scale(0.5);
            transform:scale(0.5); }
  .bp3-dialog-container.bp3-overlay-enter-active > .bp3-dialog, .bp3-dialog-container.bp3-overlay-appear-active > .bp3-dialog{
    opacity:1;
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-transform;
    transition-property:opacity, -webkit-transform;
    transition-property:opacity, transform;
    transition-property:opacity, transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-dialog-container.bp3-overlay-exit > .bp3-dialog{
    opacity:1;
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-dialog-container.bp3-overlay-exit-active > .bp3-dialog{
    opacity:0;
    -webkit-transform:scale(0.5);
            transform:scale(0.5);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-transform;
    transition-property:opacity, -webkit-transform;
    transition-property:opacity, transform;
    transition-property:opacity, transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }

.bp3-dialog{
  background:#ebf1f5;
  border-radius:6px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:30px 0;
  padding-bottom:20px;
  pointer-events:all;
  -webkit-user-select:text;
     -moz-user-select:text;
      -ms-user-select:text;
          user-select:text;
  width:500px; }
  .bp3-dialog:focus{
    outline:0; }
  .bp3-dialog.bp3-dark,
  .bp3-dark .bp3-dialog{
    background:#293742;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }

.bp3-dialog-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background:#ffffff;
  border-radius:6px 6px 0 0;
  -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  min-height:40px;
  padding-left:20px;
  padding-right:5px;
  z-index:30; }
  .bp3-dialog-header .bp3-icon-large,
  .bp3-dialog-header .bp3-icon{
    color:#5c7080;
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    margin-right:10px; }
  .bp3-dialog-header .bp3-heading{
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:inherit;
    margin:0; }
    .bp3-dialog-header .bp3-heading:last-child{
      margin-right:20px; }
  .bp3-dark .bp3-dialog-header{
    background:#30404d;
    -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:0 1px 0 rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-dialog-header .bp3-icon-large,
    .bp3-dark .bp3-dialog-header .bp3-icon{
      color:#a7b6c2; }

.bp3-dialog-body{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  line-height:18px;
  margin:20px; }

.bp3-dialog-footer{
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  margin:0 20px; }

.bp3-dialog-footer-actions{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:end;
      -ms-flex-pack:end;
          justify-content:flex-end; }
  .bp3-dialog-footer-actions .bp3-button{
    margin-left:10px; }
.bp3-multistep-dialog-panels{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }

.bp3-multistep-dialog-left-panel{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:1;
      -ms-flex:1;
          flex:1;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column; }
  .bp3-dark .bp3-multistep-dialog-left-panel{
    background:#202b33; }

.bp3-multistep-dialog-right-panel{
  background-color:#f5f8fa;
  border-left:1px solid rgba(16, 22, 26, 0.15);
  border-radius:0 0 6px 0;
  -webkit-box-flex:3;
      -ms-flex:3;
          flex:3;
  min-width:0; }
  .bp3-dark .bp3-multistep-dialog-right-panel{
    background-color:#293742;
    border-left:1px solid rgba(16, 22, 26, 0.4); }

.bp3-multistep-dialog-footer{
  background-color:#ffffff;
  border-radius:0 0 6px 0;
  border-top:1px solid rgba(16, 22, 26, 0.15);
  padding:10px; }
  .bp3-dark .bp3-multistep-dialog-footer{
    background:#30404d;
    border-top:1px solid rgba(16, 22, 26, 0.4); }

.bp3-dialog-step-container{
  background-color:#f5f8fa;
  border-bottom:1px solid rgba(16, 22, 26, 0.15); }
  .bp3-dark .bp3-dialog-step-container{
    background:#293742;
    border-bottom:1px solid rgba(16, 22, 26, 0.4); }
  .bp3-dialog-step-container.bp3-dialog-step-viewed{
    background-color:#ffffff; }
    .bp3-dark .bp3-dialog-step-container.bp3-dialog-step-viewed{
      background:#30404d; }

.bp3-dialog-step{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:#f5f8fa;
  border-radius:6px;
  cursor:not-allowed;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin:4px;
  padding:6px 14px; }
  .bp3-dark .bp3-dialog-step{
    background:#293742; }
  .bp3-dialog-step-viewed .bp3-dialog-step{
    background-color:#ffffff;
    cursor:pointer; }
    .bp3-dark .bp3-dialog-step-viewed .bp3-dialog-step{
      background:#30404d; }
  .bp3-dialog-step:hover{
    background-color:#f5f8fa; }
    .bp3-dark .bp3-dialog-step:hover{
      background:#293742; }

.bp3-dialog-step-icon{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:rgba(92, 112, 128, 0.6);
  border-radius:50%;
  color:#ffffff;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:25px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  width:25px; }
  .bp3-dark .bp3-dialog-step-icon{
    background-color:rgba(167, 182, 194, 0.6); }
  .bp3-active.bp3-dialog-step-viewed .bp3-dialog-step-icon{
    background-color:#2b95d6; }
  .bp3-dialog-step-viewed .bp3-dialog-step-icon{
    background-color:#8a9ba8; }

.bp3-dialog-step-title{
  color:rgba(92, 112, 128, 0.6);
  -webkit-box-flex:1;
      -ms-flex:1;
          flex:1;
  padding-left:10px; }
  .bp3-dark .bp3-dialog-step-title{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-active.bp3-dialog-step-viewed .bp3-dialog-step-title{
    color:#2b95d6; }
  .bp3-dialog-step-viewed:not(.bp3-active) .bp3-dialog-step-title{
    color:#182026; }
    .bp3-dark .bp3-dialog-step-viewed:not(.bp3-active) .bp3-dialog-step-title{
      color:#f5f8fa; }
.bp3-drawer{
  background:#ffffff;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:0;
  padding:0; }
  .bp3-drawer:focus{
    outline:0; }
  .bp3-drawer.bp3-position-top{
    height:50%;
    left:0;
    right:0;
    top:0; }
    .bp3-drawer.bp3-position-top.bp3-overlay-enter, .bp3-drawer.bp3-position-top.bp3-overlay-appear{
      -webkit-transform:translateY(-100%);
              transform:translateY(-100%); }
    .bp3-drawer.bp3-position-top.bp3-overlay-enter-active, .bp3-drawer.bp3-position-top.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-top.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer.bp3-position-top.bp3-overlay-exit-active{
      -webkit-transform:translateY(-100%);
              transform:translateY(-100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-bottom{
    bottom:0;
    height:50%;
    left:0;
    right:0; }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-enter, .bp3-drawer.bp3-position-bottom.bp3-overlay-appear{
      -webkit-transform:translateY(100%);
              transform:translateY(100%); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-enter-active, .bp3-drawer.bp3-position-bottom.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-exit-active{
      -webkit-transform:translateY(100%);
              transform:translateY(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-left{
    bottom:0;
    left:0;
    top:0;
    width:50%; }
    .bp3-drawer.bp3-position-left.bp3-overlay-enter, .bp3-drawer.bp3-position-left.bp3-overlay-appear{
      -webkit-transform:translateX(-100%);
              transform:translateX(-100%); }
    .bp3-drawer.bp3-position-left.bp3-overlay-enter-active, .bp3-drawer.bp3-position-left.bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-left.bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer.bp3-position-left.bp3-overlay-exit-active{
      -webkit-transform:translateX(-100%);
              transform:translateX(-100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-right{
    bottom:0;
    right:0;
    top:0;
    width:50%; }
    .bp3-drawer.bp3-position-right.bp3-overlay-enter, .bp3-drawer.bp3-position-right.bp3-overlay-appear{
      -webkit-transform:translateX(100%);
              transform:translateX(100%); }
    .bp3-drawer.bp3-position-right.bp3-overlay-enter-active, .bp3-drawer.bp3-position-right.bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-right.bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer.bp3-position-right.bp3-overlay-exit-active{
      -webkit-transform:translateX(100%);
              transform:translateX(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
  .bp3-position-right):not(.bp3-vertical){
    bottom:0;
    right:0;
    top:0;
    width:50%; }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-enter, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-appear{
      -webkit-transform:translateX(100%);
              transform:translateX(100%); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-enter-active, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-exit-active{
      -webkit-transform:translateX(100%);
              transform:translateX(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
  .bp3-position-right).bp3-vertical{
    bottom:0;
    height:50%;
    left:0;
    right:0; }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-enter, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-appear{
      -webkit-transform:translateY(100%);
              transform:translateY(100%); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-enter-active, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-exit-active{
      -webkit-transform:translateY(100%);
              transform:translateY(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-dark,
  .bp3-dark .bp3-drawer{
    background:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }

.bp3-drawer-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border-radius:0;
  -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  min-height:40px;
  padding:5px;
  padding-left:20px;
  position:relative; }
  .bp3-drawer-header .bp3-icon-large,
  .bp3-drawer-header .bp3-icon{
    color:#5c7080;
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    margin-right:10px; }
  .bp3-drawer-header .bp3-heading{
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:inherit;
    margin:0; }
    .bp3-drawer-header .bp3-heading:last-child{
      margin-right:20px; }
  .bp3-dark .bp3-drawer-header{
    -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:0 1px 0 rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-drawer-header .bp3-icon-large,
    .bp3-dark .bp3-drawer-header .bp3-icon{
      color:#a7b6c2; }

.bp3-drawer-body{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  line-height:18px;
  overflow:auto; }

.bp3-drawer-footer{
  -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  padding:10px 20px;
  position:relative; }
  .bp3-dark .bp3-drawer-footer{
    -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.4); }
.bp3-editable-text{
  cursor:text;
  display:inline-block;
  max-width:100%;
  position:relative;
  vertical-align:top;
  white-space:nowrap; }
  .bp3-editable-text::before{
    bottom:-3px;
    left:-3px;
    position:absolute;
    right:-3px;
    top:-3px;
    border-radius:3px;
    content:"";
    -webkit-transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-editable-text:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-editable-text.bp3-editable-text-editing::before{
    background-color:#ffffff;
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-disabled::before{
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-editable-text.bp3-intent-primary .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-primary .bp3-editable-text-content{
    color:#137cbd; }
  .bp3-editable-text.bp3-intent-primary:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(19, 124, 189, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(19, 124, 189, 0.4); }
  .bp3-editable-text.bp3-intent-primary.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-success .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-success .bp3-editable-text-content{
    color:#0f9960; }
  .bp3-editable-text.bp3-intent-success:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px rgba(15, 153, 96, 0.4);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px rgba(15, 153, 96, 0.4); }
  .bp3-editable-text.bp3-intent-success.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-warning .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-warning .bp3-editable-text-content{
    color:#d9822b; }
  .bp3-editable-text.bp3-intent-warning:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px rgba(217, 130, 43, 0.4);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px rgba(217, 130, 43, 0.4); }
  .bp3-editable-text.bp3-intent-warning.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-danger .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-danger .bp3-editable-text-content{
    color:#db3737; }
  .bp3-editable-text.bp3-intent-danger:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px rgba(219, 55, 55, 0.4);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px rgba(219, 55, 55, 0.4); }
  .bp3-editable-text.bp3-intent-danger.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-editable-text:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(255, 255, 255, 0.15); }
  .bp3-dark .bp3-editable-text.bp3-editable-text-editing::before{
    background-color:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-disabled::before{
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-editable-text.bp3-intent-primary .bp3-editable-text-content{
    color:#48aff0; }
  .bp3-dark .bp3-editable-text.bp3-intent-primary:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(72, 175, 240, 0), 0 0 0 0 rgba(72, 175, 240, 0), inset 0 0 0 1px rgba(72, 175, 240, 0.4);
            box-shadow:0 0 0 0 rgba(72, 175, 240, 0), 0 0 0 0 rgba(72, 175, 240, 0), inset 0 0 0 1px rgba(72, 175, 240, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-primary.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #48aff0, 0 0 0 3px rgba(72, 175, 240, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #48aff0, 0 0 0 3px rgba(72, 175, 240, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-success .bp3-editable-text-content{
    color:#3dcc91; }
  .bp3-dark .bp3-editable-text.bp3-intent-success:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(61, 204, 145, 0), 0 0 0 0 rgba(61, 204, 145, 0), inset 0 0 0 1px rgba(61, 204, 145, 0.4);
            box-shadow:0 0 0 0 rgba(61, 204, 145, 0), 0 0 0 0 rgba(61, 204, 145, 0), inset 0 0 0 1px rgba(61, 204, 145, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-success.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #3dcc91, 0 0 0 3px rgba(61, 204, 145, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #3dcc91, 0 0 0 3px rgba(61, 204, 145, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-warning .bp3-editable-text-content{
    color:#ffb366; }
  .bp3-dark .bp3-editable-text.bp3-intent-warning:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(255, 179, 102, 0), 0 0 0 0 rgba(255, 179, 102, 0), inset 0 0 0 1px rgba(255, 179, 102, 0.4);
            box-shadow:0 0 0 0 rgba(255, 179, 102, 0), 0 0 0 0 rgba(255, 179, 102, 0), inset 0 0 0 1px rgba(255, 179, 102, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-warning.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #ffb366, 0 0 0 3px rgba(255, 179, 102, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #ffb366, 0 0 0 3px rgba(255, 179, 102, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-danger .bp3-editable-text-content{
    color:#ff7373; }
  .bp3-dark .bp3-editable-text.bp3-intent-danger:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(255, 115, 115, 0), 0 0 0 0 rgba(255, 115, 115, 0), inset 0 0 0 1px rgba(255, 115, 115, 0.4);
            box-shadow:0 0 0 0 rgba(255, 115, 115, 0), 0 0 0 0 rgba(255, 115, 115, 0), inset 0 0 0 1px rgba(255, 115, 115, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-danger.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #ff7373, 0 0 0 3px rgba(255, 115, 115, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #ff7373, 0 0 0 3px rgba(255, 115, 115, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-editable-text-input,
.bp3-editable-text-content{
  color:inherit;
  display:inherit;
  font:inherit;
  letter-spacing:inherit;
  max-width:inherit;
  min-width:inherit;
  position:relative;
  resize:none;
  text-transform:inherit;
  vertical-align:top; }

.bp3-editable-text-input{
  background:none;
  border:none;
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0;
  white-space:pre-wrap;
  width:100%; }
  .bp3-editable-text-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input:focus{
    outline:none; }
  .bp3-editable-text-input::-ms-clear{
    display:none; }

.bp3-editable-text-content{
  overflow:hidden;
  padding-right:2px;
  text-overflow:ellipsis;
  white-space:pre; }
  .bp3-editable-text-editing > .bp3-editable-text-content{
    left:0;
    position:absolute;
    visibility:hidden; }
  .bp3-editable-text-placeholder > .bp3-editable-text-content{
    color:rgba(92, 112, 128, 0.6); }
    .bp3-dark .bp3-editable-text-placeholder > .bp3-editable-text-content{
      color:rgba(167, 182, 194, 0.6); }

.bp3-editable-text.bp3-multiline{
  display:block; }
  .bp3-editable-text.bp3-multiline .bp3-editable-text-content{
    overflow:auto;
    white-space:pre-wrap;
    word-wrap:break-word; }
.bp3-divider{
  border-bottom:1px solid rgba(16, 22, 26, 0.15);
  border-right:1px solid rgba(16, 22, 26, 0.15);
  margin:5px; }
  .bp3-dark .bp3-divider{
    border-color:rgba(16, 22, 26, 0.4); }
.bp3-control-group{
  -webkit-transform:translateZ(0);
          transform:translateZ(0);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:stretch;
      -ms-flex-align:stretch;
          align-items:stretch; }
  .bp3-control-group > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-control-group > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-control-group .bp3-button,
  .bp3-control-group .bp3-html-select,
  .bp3-control-group .bp3-input,
  .bp3-control-group .bp3-select{
    position:relative; }
  .bp3-control-group .bp3-input{
    border-radius:inherit;
    z-index:2; }
    .bp3-control-group .bp3-input:focus{
      border-radius:3px;
      z-index:14; }
    .bp3-control-group .bp3-input[class*="bp3-intent"]{
      z-index:13; }
      .bp3-control-group .bp3-input[class*="bp3-intent"]:focus{
        z-index:15; }
    .bp3-control-group .bp3-input[readonly], .bp3-control-group .bp3-input:disabled, .bp3-control-group .bp3-input.bp3-disabled{
      z-index:1; }
  .bp3-control-group .bp3-input-group[class*="bp3-intent"] .bp3-input{
    z-index:13; }
    .bp3-control-group .bp3-input-group[class*="bp3-intent"] .bp3-input:focus{
      z-index:15; }
  .bp3-control-group .bp3-button,
  .bp3-control-group .bp3-html-select select,
  .bp3-control-group .bp3-select select{
    -webkit-transform:translateZ(0);
            transform:translateZ(0);
    border-radius:inherit;
    z-index:4; }
    .bp3-control-group .bp3-button:focus,
    .bp3-control-group .bp3-html-select select:focus,
    .bp3-control-group .bp3-select select:focus{
      z-index:5; }
    .bp3-control-group .bp3-button:hover,
    .bp3-control-group .bp3-html-select select:hover,
    .bp3-control-group .bp3-select select:hover{
      z-index:6; }
    .bp3-control-group .bp3-button:active,
    .bp3-control-group .bp3-html-select select:active,
    .bp3-control-group .bp3-select select:active{
      z-index:7; }
    .bp3-control-group .bp3-button[readonly], .bp3-control-group .bp3-button:disabled, .bp3-control-group .bp3-button.bp3-disabled,
    .bp3-control-group .bp3-html-select select[readonly],
    .bp3-control-group .bp3-html-select select:disabled,
    .bp3-control-group .bp3-html-select select.bp3-disabled,
    .bp3-control-group .bp3-select select[readonly],
    .bp3-control-group .bp3-select select:disabled,
    .bp3-control-group .bp3-select select.bp3-disabled{
      z-index:3; }
    .bp3-control-group .bp3-button[class*="bp3-intent"],
    .bp3-control-group .bp3-html-select select[class*="bp3-intent"],
    .bp3-control-group .bp3-select select[class*="bp3-intent"]{
      z-index:9; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:focus,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:focus,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:focus{
        z-index:10; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:hover,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:hover,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:hover{
        z-index:11; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:active,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:active,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:active{
        z-index:12; }
      .bp3-control-group .bp3-button[class*="bp3-intent"][readonly], .bp3-control-group .bp3-button[class*="bp3-intent"]:disabled, .bp3-control-group .bp3-button[class*="bp3-intent"].bp3-disabled,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"][readonly],
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:disabled,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"].bp3-disabled,
      .bp3-control-group .bp3-select select[class*="bp3-intent"][readonly],
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:disabled,
      .bp3-control-group .bp3-select select[class*="bp3-intent"].bp3-disabled{
        z-index:8; }
  .bp3-control-group .bp3-input-group > .bp3-icon,
  .bp3-control-group .bp3-input-group > .bp3-button,
  .bp3-control-group .bp3-input-group > .bp3-input-left-container,
  .bp3-control-group .bp3-input-group > .bp3-input-action{
    z-index:16; }
  .bp3-control-group .bp3-select::after,
  .bp3-control-group .bp3-html-select::after,
  .bp3-control-group .bp3-select > .bp3-icon,
  .bp3-control-group .bp3-html-select > .bp3-icon{
    z-index:17; }
  .bp3-control-group .bp3-select:focus-within{
    z-index:5; }
  .bp3-control-group:not(.bp3-vertical) > *:not(.bp3-divider){
    margin-right:-1px; }
  .bp3-control-group:not(.bp3-vertical) > .bp3-divider:not(:first-child){
    margin-left:6px; }
  .bp3-dark .bp3-control-group:not(.bp3-vertical) > *:not(.bp3-divider){
    margin-right:0; }
  .bp3-dark .bp3-control-group:not(.bp3-vertical) > .bp3-button + .bp3-button{
    margin-left:1px; }
  .bp3-control-group .bp3-popover-wrapper,
  .bp3-control-group .bp3-popover-target{
    border-radius:inherit; }
  .bp3-control-group > :first-child{
    border-radius:3px 0 0 3px; }
  .bp3-control-group > :last-child{
    border-radius:0 3px 3px 0;
    margin-right:0; }
  .bp3-control-group > :only-child{
    border-radius:3px;
    margin-right:0; }
  .bp3-control-group .bp3-input-group .bp3-button{
    border-radius:3px; }
  .bp3-control-group .bp3-numeric-input:not(:first-child) .bp3-input-group{
    border-bottom-left-radius:0;
    border-top-left-radius:0; }
  .bp3-control-group.bp3-fill{
    width:100%; }
  .bp3-control-group > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-control-group.bp3-fill > *:not(.bp3-fixed){
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-control-group.bp3-vertical{
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column; }
    .bp3-control-group.bp3-vertical > *{
      margin-top:-1px; }
    .bp3-control-group.bp3-vertical > :first-child{
      border-radius:3px 3px 0 0;
      margin-top:0; }
    .bp3-control-group.bp3-vertical > :last-child{
      border-radius:0 0 3px 3px; }
.bp3-control{
  cursor:pointer;
  display:block;
  margin-bottom:10px;
  position:relative;
  text-transform:none; }
  .bp3-control input:checked ~ .bp3-control-indicator{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
  .bp3-control:hover input:checked ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
  .bp3-control input:not(:disabled):active:checked ~ .bp3-control-indicator{
    background:#0e5a8a;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control input:disabled:checked ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-control input:checked ~ .bp3-control-indicator{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control:hover input:checked ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control input:not(:disabled):active:checked ~ .bp3-control-indicator{
    background-color:#0e5a8a;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-control input:disabled:checked ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-control:not(.bp3-align-right){
    padding-left:26px; }
    .bp3-control:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-26px; }
  .bp3-control.bp3-align-right{
    padding-right:26px; }
    .bp3-control.bp3-align-right .bp3-control-indicator{
      margin-right:-26px; }
  .bp3-control.bp3-disabled{
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-control.bp3-inline{
    display:inline-block;
    margin-right:20px; }
  .bp3-control input{
    left:0;
    opacity:0;
    position:absolute;
    top:0;
    z-index:-1; }
  .bp3-control .bp3-control-indicator{
    background-clip:padding-box;
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    border:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    cursor:pointer;
    display:inline-block;
    font-size:16px;
    height:1em;
    margin-right:10px;
    margin-top:-3px;
    position:relative;
    -webkit-user-select:none;
       -moz-user-select:none;
        -ms-user-select:none;
            user-select:none;
    vertical-align:middle;
    width:1em; }
    .bp3-control .bp3-control-indicator::before{
      content:"";
      display:block;
      height:1em;
      width:1em; }
  .bp3-control:hover .bp3-control-indicator{
    background-color:#ebf1f5; }
  .bp3-control input:not(:disabled):active ~ .bp3-control-indicator{
    background:#d8e1e8;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control input:disabled ~ .bp3-control-indicator{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    cursor:not-allowed; }
  .bp3-control input:focus ~ .bp3-control-indicator{
    outline:rgba(19, 124, 189, 0.6) auto 2px;
    outline-offset:2px;
    -moz-outline-radius:6px; }
  .bp3-control.bp3-align-right .bp3-control-indicator{
    float:right;
    margin-left:10px;
    margin-top:1px; }
  .bp3-control.bp3-large{
    font-size:16px; }
    .bp3-control.bp3-large:not(.bp3-align-right){
      padding-left:30px; }
      .bp3-control.bp3-large:not(.bp3-align-right) .bp3-control-indicator{
        margin-left:-30px; }
    .bp3-control.bp3-large.bp3-align-right{
      padding-right:30px; }
      .bp3-control.bp3-large.bp3-align-right .bp3-control-indicator{
        margin-right:-30px; }
    .bp3-control.bp3-large .bp3-control-indicator{
      font-size:20px; }
    .bp3-control.bp3-large.bp3-align-right .bp3-control-indicator{
      margin-top:0; }
  .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
  .bp3-control.bp3-checkbox:hover input:indeterminate ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
  .bp3-control.bp3-checkbox input:not(:disabled):active:indeterminate ~ .bp3-control-indicator{
    background:#0e5a8a;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-checkbox:hover input:indeterminate ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-checkbox input:not(:disabled):active:indeterminate ~ .bp3-control-indicator{
    background-color:#0e5a8a;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-control.bp3-checkbox .bp3-control-indicator{
    border-radius:3px; }
  .bp3-control.bp3-checkbox input:checked ~ .bp3-control-indicator::before{
    background-image:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M12 5c-.28 0-.53.11-.71.29L7 9.59l-2.29-2.3a1.003 1.003 0 00-1.42 1.42l3 3c.18.18.43.29.71.29s.53-.11.71-.29l5-5A1.003 1.003 0 0012 5z' fill='white'/%3e%3c/svg%3e"); }
  .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator::before{
    background-image:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M11 7H5c-.55 0-1 .45-1 1s.45 1 1 1h6c.55 0 1-.45 1-1s-.45-1-1-1z' fill='white'/%3e%3c/svg%3e"); }
  .bp3-control.bp3-radio .bp3-control-indicator{
    border-radius:50%; }
  .bp3-control.bp3-radio input:checked ~ .bp3-control-indicator::before{
    background-image:radial-gradient(#ffffff, #ffffff 28%, transparent 32%); }
  .bp3-control.bp3-radio input:checked:disabled ~ .bp3-control-indicator::before{
    opacity:0.5; }
  .bp3-control.bp3-radio input:focus ~ .bp3-control-indicator{
    -moz-outline-radius:16px; }
  .bp3-control.bp3-switch input ~ .bp3-control-indicator{
    background:rgba(167, 182, 194, 0.5); }
  .bp3-control.bp3-switch:hover input ~ .bp3-control-indicator{
    background:rgba(115, 134, 148, 0.5); }
  .bp3-control.bp3-switch input:not(:disabled):active ~ .bp3-control-indicator{
    background:rgba(92, 112, 128, 0.5); }
  .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator{
    background:rgba(206, 217, 224, 0.5); }
    .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator::before{
      background:rgba(255, 255, 255, 0.8); }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator{
    background:#137cbd; }
  .bp3-control.bp3-switch:hover input:checked ~ .bp3-control-indicator{
    background:#106ba3; }
  .bp3-control.bp3-switch input:checked:not(:disabled):active ~ .bp3-control-indicator{
    background:#0e5a8a; }
  .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5); }
    .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator::before{
      background:rgba(255, 255, 255, 0.8); }
  .bp3-control.bp3-switch:not(.bp3-align-right){
    padding-left:38px; }
    .bp3-control.bp3-switch:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-38px; }
  .bp3-control.bp3-switch.bp3-align-right{
    padding-right:38px; }
    .bp3-control.bp3-switch.bp3-align-right .bp3-control-indicator{
      margin-right:-38px; }
  .bp3-control.bp3-switch .bp3-control-indicator{
    border:none;
    border-radius:1.75em;
    -webkit-box-shadow:none !important;
            box-shadow:none !important;
    min-width:1.75em;
    -webkit-transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    width:auto; }
    .bp3-control.bp3-switch .bp3-control-indicator::before{
      background:#ffffff;
      border-radius:50%;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
      height:calc(1em - 4px);
      left:0;
      margin:2px;
      position:absolute;
      -webkit-transition:left 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
      transition:left 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
      width:calc(1em - 4px); }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator::before{
    left:calc(100% - 1em); }
  .bp3-control.bp3-switch.bp3-large:not(.bp3-align-right){
    padding-left:45px; }
    .bp3-control.bp3-switch.bp3-large:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-45px; }
  .bp3-control.bp3-switch.bp3-large.bp3-align-right{
    padding-right:45px; }
    .bp3-control.bp3-switch.bp3-large.bp3-align-right .bp3-control-indicator{
      margin-right:-45px; }
  .bp3-dark .bp3-control.bp3-switch input ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.5); }
  .bp3-dark .bp3-control.bp3-switch:hover input ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.7); }
  .bp3-dark .bp3-control.bp3-switch input:not(:disabled):active ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.9); }
  .bp3-dark .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator{
    background:rgba(57, 75, 89, 0.5); }
    .bp3-dark .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator::before{
      background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator{
    background:#137cbd; }
  .bp3-dark .bp3-control.bp3-switch:hover input:checked ~ .bp3-control-indicator{
    background:#106ba3; }
  .bp3-dark .bp3-control.bp3-switch input:checked:not(:disabled):active ~ .bp3-control-indicator{
    background:#0e5a8a; }
  .bp3-dark .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5); }
    .bp3-dark .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator::before{
      background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch .bp3-control-indicator::before{
    background:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator::before{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-control.bp3-switch .bp3-switch-inner-text{
    font-size:0.7em;
    text-align:center; }
  .bp3-control.bp3-switch .bp3-control-indicator-child:first-child{
    line-height:0;
    margin-left:0.5em;
    margin-right:1.2em;
    visibility:hidden; }
  .bp3-control.bp3-switch .bp3-control-indicator-child:last-child{
    line-height:1em;
    margin-left:1.2em;
    margin-right:0.5em;
    visibility:visible; }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator .bp3-control-indicator-child:first-child{
    line-height:1em;
    visibility:visible; }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator .bp3-control-indicator-child:last-child{
    line-height:0;
    visibility:hidden; }
  .bp3-dark .bp3-control{
    color:#f5f8fa; }
    .bp3-dark .bp3-control.bp3-disabled{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-control .bp3-control-indicator{
      background-color:#394b59;
      background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
      background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-control:hover .bp3-control-indicator{
      background-color:#30404d; }
    .bp3-dark .bp3-control input:not(:disabled):active ~ .bp3-control-indicator{
      background:#202b33;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-control input:disabled ~ .bp3-control-indicator{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      cursor:not-allowed; }
    .bp3-dark .bp3-control.bp3-checkbox input:disabled:checked ~ .bp3-control-indicator, .bp3-dark .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
      color:rgba(167, 182, 194, 0.6); }
.bp3-file-input{
  cursor:pointer;
  display:inline-block;
  height:30px;
  position:relative; }
  .bp3-file-input input{
    margin:0;
    min-width:200px;
    opacity:0; }
    .bp3-file-input input:disabled + .bp3-file-upload-input,
    .bp3-file-input input.bp3-disabled + .bp3-file-upload-input{
      background:rgba(206, 217, 224, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      resize:none; }
      .bp3-file-input input:disabled + .bp3-file-upload-input::after,
      .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after{
        background-color:rgba(206, 217, 224, 0.5);
        background-image:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(92, 112, 128, 0.6);
        cursor:not-allowed;
        outline:none; }
        .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active, .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active:hover,
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active,
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active:hover{
          background:rgba(206, 217, 224, 0.7); }
      .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input, .bp3-dark
      .bp3-file-input input.bp3-disabled + .bp3-file-upload-input{
        background:rgba(57, 75, 89, 0.5);
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(167, 182, 194, 0.6); }
        .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input::after, .bp3-dark
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after{
          background-color:rgba(57, 75, 89, 0.5);
          background-image:none;
          -webkit-box-shadow:none;
                  box-shadow:none;
          color:rgba(167, 182, 194, 0.6); }
          .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active, .bp3-dark
          .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active{
            background:rgba(57, 75, 89, 0.7); }
  .bp3-file-input.bp3-file-input-has-selection .bp3-file-upload-input{
    color:#182026; }
  .bp3-dark .bp3-file-input.bp3-file-input-has-selection .bp3-file-upload-input{
    color:#f5f8fa; }
  .bp3-file-input.bp3-fill{
    width:100%; }
  .bp3-file-input.bp3-large,
  .bp3-large .bp3-file-input{
    height:40px; }
  .bp3-file-input .bp3-file-upload-input-custom-text::after{
    content:attr(bp3-button-text); }

.bp3-file-upload-input{
  -webkit-appearance:none;
     -moz-appearance:none;
          appearance:none;
  background:#ffffff;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#182026;
  font-size:14px;
  font-weight:400;
  height:30px;
  line-height:30px;
  outline:none;
  padding:0 10px;
  -webkit-transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  vertical-align:middle;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  color:rgba(92, 112, 128, 0.6);
  left:0;
  padding-right:80px;
  position:absolute;
  right:0;
  top:0;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-file-upload-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input:focus, .bp3-file-upload-input.bp3-active{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-file-upload-input[type="search"], .bp3-file-upload-input.bp3-round{
    border-radius:30px;
    -webkit-box-sizing:border-box;
            box-sizing:border-box;
    padding-left:10px; }
  .bp3-file-upload-input[readonly]{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-file-upload-input:disabled, .bp3-file-upload-input.bp3-disabled{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    resize:none; }
  .bp3-file-upload-input::after{
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    color:#182026;
    min-height:24px;
    min-width:24px;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    border-radius:3px;
    content:"Browse";
    line-height:24px;
    margin:3px;
    position:absolute;
    right:0;
    text-align:center;
    top:0;
    width:70px; }
    .bp3-file-upload-input::after:hover{
      background-clip:padding-box;
      background-color:#ebf1f5;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
    .bp3-file-upload-input::after:active, .bp3-file-upload-input::after.bp3-active{
      background-color:#d8e1e8;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-file-upload-input::after:disabled, .bp3-file-upload-input::after.bp3-disabled{
      background-color:rgba(206, 217, 224, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      outline:none; }
      .bp3-file-upload-input::after:disabled.bp3-active, .bp3-file-upload-input::after:disabled.bp3-active:hover, .bp3-file-upload-input::after.bp3-disabled.bp3-active, .bp3-file-upload-input::after.bp3-disabled.bp3-active:hover{
        background:rgba(206, 217, 224, 0.7); }
  .bp3-file-upload-input:hover::after{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-file-upload-input:active::after{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-large .bp3-file-upload-input{
    font-size:16px;
    height:40px;
    line-height:40px;
    padding-right:95px; }
    .bp3-large .bp3-file-upload-input[type="search"], .bp3-large .bp3-file-upload-input.bp3-round{
      padding:0 15px; }
    .bp3-large .bp3-file-upload-input::after{
      min-height:30px;
      min-width:30px;
      line-height:30px;
      margin:5px;
      width:85px; }
  .bp3-dark .bp3-file-upload-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa;
    color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input:disabled, .bp3-dark .bp3-file-upload-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::after{
      background-color:#394b59;
      background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
      background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
      color:#f5f8fa; }
      .bp3-dark .bp3-file-upload-input::after:hover, .bp3-dark .bp3-file-upload-input::after:active, .bp3-dark .bp3-file-upload-input::after.bp3-active{
        color:#f5f8fa; }
      .bp3-dark .bp3-file-upload-input::after:hover{
        background-color:#30404d;
        -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-file-upload-input::after:active, .bp3-dark .bp3-file-upload-input::after.bp3-active{
        background-color:#202b33;
        background-image:none;
        -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
                box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
      .bp3-dark .bp3-file-upload-input::after:disabled, .bp3-dark .bp3-file-upload-input::after.bp3-disabled{
        background-color:rgba(57, 75, 89, 0.5);
        background-image:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(167, 182, 194, 0.6); }
        .bp3-dark .bp3-file-upload-input::after:disabled.bp3-active, .bp3-dark .bp3-file-upload-input::after.bp3-disabled.bp3-active{
          background:rgba(57, 75, 89, 0.7); }
      .bp3-dark .bp3-file-upload-input::after .bp3-button-spinner .bp3-spinner-head{
        background:rgba(16, 22, 26, 0.5);
        stroke:#8a9ba8; }
    .bp3-dark .bp3-file-upload-input:hover::after{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input:active::after{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
.bp3-file-upload-input::after{
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
.bp3-form-group{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:0 0 15px; }
  .bp3-form-group label.bp3-label{
    margin-bottom:5px; }
  .bp3-form-group .bp3-control{
    margin-top:7px; }
  .bp3-form-group .bp3-form-helper-text{
    color:#5c7080;
    font-size:12px;
    margin-top:5px; }
  .bp3-form-group.bp3-intent-primary .bp3-form-helper-text{
    color:#106ba3; }
  .bp3-form-group.bp3-intent-success .bp3-form-helper-text{
    color:#0d8050; }
  .bp3-form-group.bp3-intent-warning .bp3-form-helper-text{
    color:#bf7326; }
  .bp3-form-group.bp3-intent-danger .bp3-form-helper-text{
    color:#c23030; }
  .bp3-form-group.bp3-inline{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start;
    -webkit-box-orient:horizontal;
    -webkit-box-direction:normal;
        -ms-flex-direction:row;
            flex-direction:row; }
    .bp3-form-group.bp3-inline.bp3-large label.bp3-label{
      line-height:40px;
      margin:0 10px 0 0; }
    .bp3-form-group.bp3-inline label.bp3-label{
      line-height:30px;
      margin:0 10px 0 0; }
  .bp3-form-group.bp3-disabled .bp3-label,
  .bp3-form-group.bp3-disabled .bp3-text-muted,
  .bp3-form-group.bp3-disabled .bp3-form-helper-text{
    color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-dark .bp3-form-group.bp3-intent-primary .bp3-form-helper-text{
    color:#48aff0; }
  .bp3-dark .bp3-form-group.bp3-intent-success .bp3-form-helper-text{
    color:#3dcc91; }
  .bp3-dark .bp3-form-group.bp3-intent-warning .bp3-form-helper-text{
    color:#ffb366; }
  .bp3-dark .bp3-form-group.bp3-intent-danger .bp3-form-helper-text{
    color:#ff7373; }
  .bp3-dark .bp3-form-group .bp3-form-helper-text{
    color:#a7b6c2; }
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-label,
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-text-muted,
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-form-helper-text{
    color:rgba(167, 182, 194, 0.6) !important; }
.bp3-input-group{
  display:block;
  position:relative; }
  .bp3-input-group .bp3-input{
    position:relative;
    width:100%; }
    .bp3-input-group .bp3-input:not(:first-child){
      padding-left:30px; }
    .bp3-input-group .bp3-input:not(:last-child){
      padding-right:30px; }
  .bp3-input-group .bp3-input-action,
  .bp3-input-group > .bp3-input-left-container,
  .bp3-input-group > .bp3-button,
  .bp3-input-group > .bp3-icon{
    position:absolute;
    top:0; }
    .bp3-input-group .bp3-input-action:first-child,
    .bp3-input-group > .bp3-input-left-container:first-child,
    .bp3-input-group > .bp3-button:first-child,
    .bp3-input-group > .bp3-icon:first-child{
      left:0; }
    .bp3-input-group .bp3-input-action:last-child,
    .bp3-input-group > .bp3-input-left-container:last-child,
    .bp3-input-group > .bp3-button:last-child,
    .bp3-input-group > .bp3-icon:last-child{
      right:0; }
  .bp3-input-group .bp3-button{
    min-height:24px;
    min-width:24px;
    margin:3px;
    padding:0 7px; }
    .bp3-input-group .bp3-button:empty{
      padding:0; }
  .bp3-input-group > .bp3-input-left-container,
  .bp3-input-group > .bp3-icon{
    z-index:1; }
  .bp3-input-group > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group > .bp3-icon{
    color:#5c7080; }
    .bp3-input-group > .bp3-input-left-container > .bp3-icon:empty,
    .bp3-input-group > .bp3-icon:empty{
      font-family:"Icons16", sans-serif;
      font-size:16px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased; }
  .bp3-input-group > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group > .bp3-icon,
  .bp3-input-group .bp3-input-action > .bp3-spinner{
    margin:7px; }
  .bp3-input-group .bp3-tag{
    margin:5px; }
  .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus),
  .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus){
    color:#5c7080; }
    .bp3-dark .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus), .bp3-dark
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus){
      color:#a7b6c2; }
    .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-standard, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-large,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-standard,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-large{
      color:#5c7080; }
  .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled,
  .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled{
    color:rgba(92, 112, 128, 0.6) !important; }
    .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon-standard, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon-large,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon-standard,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon-large{
      color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-input-group.bp3-disabled{
    cursor:not-allowed; }
    .bp3-input-group.bp3-disabled .bp3-icon{
      color:rgba(92, 112, 128, 0.6); }
  .bp3-input-group.bp3-large .bp3-button{
    min-height:30px;
    min-width:30px;
    margin:5px; }
  .bp3-input-group.bp3-large > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group.bp3-large > .bp3-icon,
  .bp3-input-group.bp3-large .bp3-input-action > .bp3-spinner{
    margin:12px; }
  .bp3-input-group.bp3-large .bp3-input{
    font-size:16px;
    height:40px;
    line-height:40px; }
    .bp3-input-group.bp3-large .bp3-input[type="search"], .bp3-input-group.bp3-large .bp3-input.bp3-round{
      padding:0 15px; }
    .bp3-input-group.bp3-large .bp3-input:not(:first-child){
      padding-left:40px; }
    .bp3-input-group.bp3-large .bp3-input:not(:last-child){
      padding-right:40px; }
  .bp3-input-group.bp3-small .bp3-button{
    min-height:20px;
    min-width:20px;
    margin:2px; }
  .bp3-input-group.bp3-small .bp3-tag{
    min-height:20px;
    min-width:20px;
    margin:2px; }
  .bp3-input-group.bp3-small > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group.bp3-small > .bp3-icon,
  .bp3-input-group.bp3-small .bp3-input-action > .bp3-spinner{
    margin:4px; }
  .bp3-input-group.bp3-small .bp3-input{
    font-size:12px;
    height:24px;
    line-height:24px;
    padding-left:8px;
    padding-right:8px; }
    .bp3-input-group.bp3-small .bp3-input[type="search"], .bp3-input-group.bp3-small .bp3-input.bp3-round{
      padding:0 12px; }
    .bp3-input-group.bp3-small .bp3-input:not(:first-child){
      padding-left:24px; }
    .bp3-input-group.bp3-small .bp3-input:not(:last-child){
      padding-right:24px; }
  .bp3-input-group.bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    width:100%; }
  .bp3-input-group.bp3-round .bp3-button,
  .bp3-input-group.bp3-round .bp3-input,
  .bp3-input-group.bp3-round .bp3-tag{
    border-radius:30px; }
  .bp3-dark .bp3-input-group .bp3-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-input-group.bp3-disabled .bp3-icon{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-input-group.bp3-intent-primary .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-primary .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-primary .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #137cbd;
              box-shadow:inset 0 0 0 1px #137cbd; }
    .bp3-input-group.bp3-intent-primary .bp3-input:disabled, .bp3-input-group.bp3-intent-primary .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-primary > .bp3-icon{
    color:#106ba3; }
    .bp3-dark .bp3-input-group.bp3-intent-primary > .bp3-icon{
      color:#48aff0; }
  .bp3-input-group.bp3-intent-success .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-success .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-success .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #0f9960;
              box-shadow:inset 0 0 0 1px #0f9960; }
    .bp3-input-group.bp3-intent-success .bp3-input:disabled, .bp3-input-group.bp3-intent-success .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-success > .bp3-icon{
    color:#0d8050; }
    .bp3-dark .bp3-input-group.bp3-intent-success > .bp3-icon{
      color:#3dcc91; }
  .bp3-input-group.bp3-intent-warning .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-warning .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-warning .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #d9822b;
              box-shadow:inset 0 0 0 1px #d9822b; }
    .bp3-input-group.bp3-intent-warning .bp3-input:disabled, .bp3-input-group.bp3-intent-warning .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-warning > .bp3-icon{
    color:#bf7326; }
    .bp3-dark .bp3-input-group.bp3-intent-warning > .bp3-icon{
      color:#ffb366; }
  .bp3-input-group.bp3-intent-danger .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-danger .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-danger .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #db3737;
              box-shadow:inset 0 0 0 1px #db3737; }
    .bp3-input-group.bp3-intent-danger .bp3-input:disabled, .bp3-input-group.bp3-intent-danger .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-danger > .bp3-icon{
    color:#c23030; }
    .bp3-dark .bp3-input-group.bp3-intent-danger > .bp3-icon{
      color:#ff7373; }
.bp3-input{
  -webkit-appearance:none;
     -moz-appearance:none;
          appearance:none;
  background:#ffffff;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#182026;
  font-size:14px;
  font-weight:400;
  height:30px;
  line-height:30px;
  outline:none;
  padding:0 10px;
  -webkit-transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  vertical-align:middle; }
  .bp3-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input:focus, .bp3-input.bp3-active{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-input[type="search"], .bp3-input.bp3-round{
    border-radius:30px;
    -webkit-box-sizing:border-box;
            box-sizing:border-box;
    padding-left:10px; }
  .bp3-input[readonly]{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-input:disabled, .bp3-input.bp3-disabled{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    resize:none; }
  .bp3-input.bp3-large{
    font-size:16px;
    height:40px;
    line-height:40px; }
    .bp3-input.bp3-large[type="search"], .bp3-input.bp3-large.bp3-round{
      padding:0 15px; }
  .bp3-input.bp3-small{
    font-size:12px;
    height:24px;
    line-height:24px;
    padding-left:8px;
    padding-right:8px; }
    .bp3-input.bp3-small[type="search"], .bp3-input.bp3-small.bp3-round{
      padding:0 12px; }
  .bp3-input.bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    width:100%; }
  .bp3-dark .bp3-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-input:disabled, .bp3-dark .bp3-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
  .bp3-input.bp3-intent-primary{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-primary:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-primary[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #137cbd;
              box-shadow:inset 0 0 0 1px #137cbd; }
    .bp3-input.bp3-intent-primary:disabled, .bp3-input.bp3-intent-primary.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-primary:focus{
        -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-primary[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #137cbd;
                box-shadow:inset 0 0 0 1px #137cbd; }
      .bp3-dark .bp3-input.bp3-intent-primary:disabled, .bp3-dark .bp3-input.bp3-intent-primary.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-success{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-success:focus{
      -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-success[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #0f9960;
              box-shadow:inset 0 0 0 1px #0f9960; }
    .bp3-input.bp3-intent-success:disabled, .bp3-input.bp3-intent-success.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-success{
      -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-success:focus{
        -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #0f9960, 0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-success[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #0f9960;
                box-shadow:inset 0 0 0 1px #0f9960; }
      .bp3-dark .bp3-input.bp3-intent-success:disabled, .bp3-dark .bp3-input.bp3-intent-success.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-warning{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-warning:focus{
      -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-warning[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #d9822b;
              box-shadow:inset 0 0 0 1px #d9822b; }
    .bp3-input.bp3-intent-warning:disabled, .bp3-input.bp3-intent-warning.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-warning:focus{
        -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #d9822b, 0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-warning[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #d9822b;
                box-shadow:inset 0 0 0 1px #d9822b; }
      .bp3-dark .bp3-input.bp3-intent-warning:disabled, .bp3-dark .bp3-input.bp3-intent-warning.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-danger{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-danger:focus{
      -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-danger[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #db3737;
              box-shadow:inset 0 0 0 1px #db3737; }
    .bp3-input.bp3-intent-danger:disabled, .bp3-input.bp3-intent-danger.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-danger:focus{
        -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #db3737, 0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-danger[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #db3737;
                box-shadow:inset 0 0 0 1px #db3737; }
      .bp3-dark .bp3-input.bp3-intent-danger:disabled, .bp3-dark .bp3-input.bp3-intent-danger.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input::-ms-clear{
    display:none; }
textarea.bp3-input{
  max-width:100%;
  padding:10px; }
  textarea.bp3-input, textarea.bp3-input.bp3-large, textarea.bp3-input.bp3-small{
    height:auto;
    line-height:inherit; }
  textarea.bp3-input.bp3-small{
    padding:8px; }
  .bp3-dark textarea.bp3-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark textarea.bp3-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark textarea.bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark textarea.bp3-input:disabled, .bp3-dark textarea.bp3-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
label.bp3-label{
  display:block;
  margin-bottom:15px;
  margin-top:0; }
  label.bp3-label .bp3-html-select,
  label.bp3-label .bp3-input,
  label.bp3-label .bp3-select,
  label.bp3-label .bp3-slider,
  label.bp3-label .bp3-popover-wrapper{
    display:block;
    margin-top:5px;
    text-transform:none; }
  label.bp3-label .bp3-button-group{
    margin-top:5px; }
  label.bp3-label .bp3-select select,
  label.bp3-label .bp3-html-select select{
    font-weight:400;
    vertical-align:top;
    width:100%; }
  label.bp3-label.bp3-disabled,
  label.bp3-label.bp3-disabled .bp3-text-muted{
    color:rgba(92, 112, 128, 0.6); }
  label.bp3-label.bp3-inline{
    line-height:30px; }
    label.bp3-label.bp3-inline .bp3-html-select,
    label.bp3-label.bp3-inline .bp3-input,
    label.bp3-label.bp3-inline .bp3-input-group,
    label.bp3-label.bp3-inline .bp3-select,
    label.bp3-label.bp3-inline .bp3-popover-wrapper{
      display:inline-block;
      margin:0 0 0 5px;
      vertical-align:top; }
    label.bp3-label.bp3-inline .bp3-button-group{
      margin:0 0 0 5px; }
    label.bp3-label.bp3-inline .bp3-input-group .bp3-input{
      margin-left:0; }
    label.bp3-label.bp3-inline.bp3-large{
      line-height:40px; }
  label.bp3-label:not(.bp3-inline) .bp3-popover-target{
    display:block; }
  .bp3-dark label.bp3-label{
    color:#f5f8fa; }
    .bp3-dark label.bp3-label.bp3-disabled,
    .bp3-dark label.bp3-label.bp3-disabled .bp3-text-muted{
      color:rgba(167, 182, 194, 0.6); }
.bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button{
  -webkit-box-flex:1;
      -ms-flex:1 1 14px;
          flex:1 1 14px;
  min-height:0;
  padding:0;
  width:30px; }
  .bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button:first-child{
    border-radius:0 3px 0 0; }
  .bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button:last-child{
    border-radius:0 0 3px 0; }

.bp3-numeric-input .bp3-button-group.bp3-vertical:first-child > .bp3-button:first-child{
  border-radius:3px 0 0 0; }

.bp3-numeric-input .bp3-button-group.bp3-vertical:first-child > .bp3-button:last-child{
  border-radius:0 0 0 3px; }

.bp3-numeric-input.bp3-large .bp3-button-group.bp3-vertical > .bp3-button{
  width:40px; }

form{
  display:block; }
.bp3-html-select select,
.bp3-select select{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border:none;
  border-radius:3px;
  cursor:pointer;
  font-size:14px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  padding:5px 10px;
  text-align:left;
  vertical-align:middle;
  background-color:#f5f8fa;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
  color:#182026;
  -moz-appearance:none;
  -webkit-appearance:none;
  border-radius:3px;
  height:30px;
  padding:0 25px 0 10px;
  width:100%; }
  .bp3-html-select select > *, .bp3-select select > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-html-select select > .bp3-fill, .bp3-select select > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-html-select select::before,
  .bp3-select select::before, .bp3-html-select select > *, .bp3-select select > *{
    margin-right:7px; }
  .bp3-html-select select:empty::before,
  .bp3-select select:empty::before,
  .bp3-html-select select > :last-child,
  .bp3-select select > :last-child{
    margin-right:0; }
  .bp3-html-select select:hover,
  .bp3-select select:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-html-select select:active,
  .bp3-select select:active, .bp3-html-select select.bp3-active,
  .bp3-select select.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-html-select select:disabled,
  .bp3-select select:disabled, .bp3-html-select select.bp3-disabled,
  .bp3-select select.bp3-disabled{
    background-color:rgba(206, 217, 224, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    outline:none; }
    .bp3-html-select select:disabled.bp3-active,
    .bp3-select select:disabled.bp3-active, .bp3-html-select select:disabled.bp3-active:hover,
    .bp3-select select:disabled.bp3-active:hover, .bp3-html-select select.bp3-disabled.bp3-active,
    .bp3-select select.bp3-disabled.bp3-active, .bp3-html-select select.bp3-disabled.bp3-active:hover,
    .bp3-select select.bp3-disabled.bp3-active:hover{
      background:rgba(206, 217, 224, 0.7); }

.bp3-html-select.bp3-minimal select,
.bp3-select.bp3-minimal select{
  background:none;
  -webkit-box-shadow:none;
          box-shadow:none; }
  .bp3-html-select.bp3-minimal select:hover,
  .bp3-select.bp3-minimal select:hover{
    background:rgba(167, 182, 194, 0.3);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:#182026;
    text-decoration:none; }
  .bp3-html-select.bp3-minimal select:active,
  .bp3-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal select.bp3-active,
  .bp3-select.bp3-minimal select.bp3-active{
    background:rgba(115, 134, 148, 0.3);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:#182026; }
  .bp3-html-select.bp3-minimal select:disabled,
  .bp3-select.bp3-minimal select:disabled, .bp3-html-select.bp3-minimal select:disabled:hover,
  .bp3-select.bp3-minimal select:disabled:hover, .bp3-html-select.bp3-minimal select.bp3-disabled,
  .bp3-select.bp3-minimal select.bp3-disabled, .bp3-html-select.bp3-minimal select.bp3-disabled:hover,
  .bp3-select.bp3-minimal select.bp3-disabled:hover{
    background:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
    .bp3-html-select.bp3-minimal select:disabled.bp3-active,
    .bp3-select.bp3-minimal select:disabled.bp3-active, .bp3-html-select.bp3-minimal select:disabled:hover.bp3-active,
    .bp3-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-html-select.bp3-minimal select.bp3-disabled.bp3-active,
    .bp3-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-disabled:hover.bp3-active,
    .bp3-select.bp3-minimal select.bp3-disabled:hover.bp3-active{
      background:rgba(115, 134, 148, 0.3); }
  .bp3-dark .bp3-html-select.bp3-minimal select, .bp3-html-select.bp3-minimal .bp3-dark select,
  .bp3-dark .bp3-select.bp3-minimal select, .bp3-select.bp3-minimal .bp3-dark select{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:inherit; }
    .bp3-dark .bp3-html-select.bp3-minimal select:hover, .bp3-html-select.bp3-minimal .bp3-dark select:hover,
    .bp3-dark .bp3-select.bp3-minimal select:hover, .bp3-select.bp3-minimal .bp3-dark select:hover, .bp3-dark .bp3-html-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal .bp3-dark select:active,
    .bp3-dark .bp3-select.bp3-minimal select:active, .bp3-select.bp3-minimal .bp3-dark select:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-active,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-html-select.bp3-minimal select:hover, .bp3-html-select.bp3-minimal .bp3-dark select:hover,
    .bp3-dark .bp3-select.bp3-minimal select:hover, .bp3-select.bp3-minimal .bp3-dark select:hover{
      background:rgba(138, 155, 168, 0.15); }
    .bp3-dark .bp3-html-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal .bp3-dark select:active,
    .bp3-dark .bp3-select.bp3-minimal select:active, .bp3-select.bp3-minimal .bp3-dark select:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-active,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-active{
      background:rgba(138, 155, 168, 0.3);
      color:#f5f8fa; }
    .bp3-dark .bp3-html-select.bp3-minimal select:disabled, .bp3-html-select.bp3-minimal .bp3-dark select:disabled,
    .bp3-dark .bp3-select.bp3-minimal select:disabled, .bp3-select.bp3-minimal .bp3-dark select:disabled, .bp3-dark .bp3-html-select.bp3-minimal select:disabled:hover, .bp3-html-select.bp3-minimal .bp3-dark select:disabled:hover,
    .bp3-dark .bp3-select.bp3-minimal select:disabled:hover, .bp3-select.bp3-minimal .bp3-dark select:disabled:hover, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled:hover,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled:hover{
      background:none;
      color:rgba(167, 182, 194, 0.6);
      cursor:not-allowed; }
      .bp3-dark .bp3-html-select.bp3-minimal select:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select:disabled.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select:disabled:hover.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-select.bp3-minimal .bp3-dark select:disabled:hover.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled:hover.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled:hover.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled:hover.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled:hover.bp3-active{
        background:rgba(138, 155, 168, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-primary,
  .bp3-select.bp3-minimal select.bp3-intent-primary{
    color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover,
    .bp3-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-html-select.bp3-minimal select.bp3-intent-primary:active,
    .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover,
    .bp3-select.bp3-minimal select.bp3-intent-primary:hover{
      background:rgba(19, 124, 189, 0.15);
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:active,
    .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active{
      background:rgba(19, 124, 189, 0.3);
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled{
      background:none;
      color:rgba(16, 107, 163, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active{
        background:rgba(19, 124, 189, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
      stroke:#106ba3; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary{
      color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.2);
        color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(72, 175, 240, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-success,
  .bp3-select.bp3-minimal select.bp3-intent-success{
    color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:hover,
    .bp3-select.bp3-minimal select.bp3-intent-success:hover, .bp3-html-select.bp3-minimal select.bp3-intent-success:active,
    .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:hover,
    .bp3-select.bp3-minimal select.bp3-intent-success:hover{
      background:rgba(15, 153, 96, 0.15);
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:active,
    .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active{
      background:rgba(15, 153, 96, 0.3);
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled{
      background:none;
      color:rgba(13, 128, 80, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active{
        background:rgba(15, 153, 96, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-success .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
      stroke:#0d8050; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success{
      color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.2);
        color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(61, 204, 145, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-warning,
  .bp3-select.bp3-minimal select.bp3-intent-warning{
    color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover,
    .bp3-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-html-select.bp3-minimal select.bp3-intent-warning:active,
    .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover,
    .bp3-select.bp3-minimal select.bp3-intent-warning:hover{
      background:rgba(217, 130, 43, 0.15);
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:active,
    .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active{
      background:rgba(217, 130, 43, 0.3);
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled{
      background:none;
      color:rgba(191, 115, 38, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active{
        background:rgba(217, 130, 43, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
      stroke:#bf7326; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning{
      color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.2);
        color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(255, 179, 102, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-danger,
  .bp3-select.bp3-minimal select.bp3-intent-danger{
    color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover,
    .bp3-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-html-select.bp3-minimal select.bp3-intent-danger:active,
    .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover,
    .bp3-select.bp3-minimal select.bp3-intent-danger:hover{
      background:rgba(219, 55, 55, 0.15);
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:active,
    .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active{
      background:rgba(219, 55, 55, 0.3);
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled{
      background:none;
      color:rgba(194, 48, 48, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active{
        background:rgba(219, 55, 55, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
      stroke:#c23030; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger{
      color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.2);
        color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(255, 115, 115, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }

.bp3-html-select.bp3-large select,
.bp3-select.bp3-large select{
  font-size:16px;
  height:40px;
  padding-right:35px; }

.bp3-dark .bp3-html-select select, .bp3-dark .bp3-select select{
  background-color:#394b59;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
  color:#f5f8fa; }
  .bp3-dark .bp3-html-select select:hover, .bp3-dark .bp3-select select:hover, .bp3-dark .bp3-html-select select:active, .bp3-dark .bp3-select select:active, .bp3-dark .bp3-html-select select.bp3-active, .bp3-dark .bp3-select select.bp3-active{
    color:#f5f8fa; }
  .bp3-dark .bp3-html-select select:hover, .bp3-dark .bp3-select select:hover{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-html-select select:active, .bp3-dark .bp3-select select:active, .bp3-dark .bp3-html-select select.bp3-active, .bp3-dark .bp3-select select.bp3-active{
    background-color:#202b33;
    background-image:none;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-html-select select:disabled, .bp3-dark .bp3-select select:disabled, .bp3-dark .bp3-html-select select.bp3-disabled, .bp3-dark .bp3-select select.bp3-disabled{
    background-color:rgba(57, 75, 89, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-html-select select:disabled.bp3-active, .bp3-dark .bp3-select select:disabled.bp3-active, .bp3-dark .bp3-html-select select.bp3-disabled.bp3-active, .bp3-dark .bp3-select select.bp3-disabled.bp3-active{
      background:rgba(57, 75, 89, 0.7); }
  .bp3-dark .bp3-html-select select .bp3-button-spinner .bp3-spinner-head, .bp3-dark .bp3-select select .bp3-button-spinner .bp3-spinner-head{
    background:rgba(16, 22, 26, 0.5);
    stroke:#8a9ba8; }

.bp3-html-select select:disabled,
.bp3-select select:disabled{
  background-color:rgba(206, 217, 224, 0.5);
  -webkit-box-shadow:none;
          box-shadow:none;
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-html-select .bp3-icon,
.bp3-select .bp3-icon, .bp3-select::after{
  color:#5c7080;
  pointer-events:none;
  position:absolute;
  right:7px;
  top:7px; }
  .bp3-html-select .bp3-disabled.bp3-icon,
  .bp3-select .bp3-disabled.bp3-icon, .bp3-disabled.bp3-select::after{
    color:rgba(92, 112, 128, 0.6); }
.bp3-html-select,
.bp3-select{
  display:inline-block;
  letter-spacing:normal;
  position:relative;
  vertical-align:middle; }
  .bp3-html-select select::-ms-expand,
  .bp3-select select::-ms-expand{
    display:none; }
  .bp3-html-select .bp3-icon,
  .bp3-select .bp3-icon{
    color:#5c7080; }
    .bp3-html-select .bp3-icon:hover,
    .bp3-select .bp3-icon:hover{
      color:#182026; }
    .bp3-dark .bp3-html-select .bp3-icon, .bp3-dark
    .bp3-select .bp3-icon{
      color:#a7b6c2; }
      .bp3-dark .bp3-html-select .bp3-icon:hover, .bp3-dark
      .bp3-select .bp3-icon:hover{
        color:#f5f8fa; }
  .bp3-html-select.bp3-large::after,
  .bp3-html-select.bp3-large .bp3-icon,
  .bp3-select.bp3-large::after,
  .bp3-select.bp3-large .bp3-icon{
    right:12px;
    top:12px; }
  .bp3-html-select.bp3-fill,
  .bp3-html-select.bp3-fill select,
  .bp3-select.bp3-fill,
  .bp3-select.bp3-fill select{
    width:100%; }
  .bp3-dark .bp3-html-select option, .bp3-dark
  .bp3-select option{
    background-color:#30404d;
    color:#f5f8fa; }
  .bp3-dark .bp3-html-select option:disabled, .bp3-dark
  .bp3-select option:disabled{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-html-select::after, .bp3-dark
  .bp3-select::after{
    color:#a7b6c2; }

.bp3-select::after{
  font-family:"Icons16", sans-serif;
  font-size:16px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  content:""; }
.bp3-running-text table, table.bp3-html-table{
  border-spacing:0;
  font-size:14px; }
  .bp3-running-text table th, table.bp3-html-table th,
  .bp3-running-text table td,
  table.bp3-html-table td{
    padding:11px;
    text-align:left;
    vertical-align:top; }
  .bp3-running-text table th, table.bp3-html-table th{
    color:#182026;
    font-weight:600; }
  
  .bp3-running-text table td,
  table.bp3-html-table td{
    color:#182026; }
  .bp3-running-text table tbody tr:first-child th, table.bp3-html-table tbody tr:first-child th,
  .bp3-running-text table tbody tr:first-child td,
  table.bp3-html-table tbody tr:first-child td,
  .bp3-running-text table tfoot tr:first-child th,
  table.bp3-html-table tfoot tr:first-child th,
  .bp3-running-text table tfoot tr:first-child td,
  table.bp3-html-table tfoot tr:first-child td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15); }
  .bp3-dark .bp3-running-text table th, .bp3-running-text .bp3-dark table th, .bp3-dark table.bp3-html-table th{
    color:#f5f8fa; }
  .bp3-dark .bp3-running-text table td, .bp3-running-text .bp3-dark table td, .bp3-dark table.bp3-html-table td{
    color:#f5f8fa; }
  .bp3-dark .bp3-running-text table tbody tr:first-child th, .bp3-running-text .bp3-dark table tbody tr:first-child th, .bp3-dark table.bp3-html-table tbody tr:first-child th,
  .bp3-dark .bp3-running-text table tbody tr:first-child td,
  .bp3-running-text .bp3-dark table tbody tr:first-child td,
  .bp3-dark table.bp3-html-table tbody tr:first-child td,
  .bp3-dark .bp3-running-text table tfoot tr:first-child th,
  .bp3-running-text .bp3-dark table tfoot tr:first-child th,
  .bp3-dark table.bp3-html-table tfoot tr:first-child th,
  .bp3-dark .bp3-running-text table tfoot tr:first-child td,
  .bp3-running-text .bp3-dark table tfoot tr:first-child td,
  .bp3-dark table.bp3-html-table tfoot tr:first-child td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15); }

table.bp3-html-table.bp3-html-table-condensed th,
table.bp3-html-table.bp3-html-table-condensed td, table.bp3-html-table.bp3-small th,
table.bp3-html-table.bp3-small td{
  padding-bottom:6px;
  padding-top:6px; }

table.bp3-html-table.bp3-html-table-striped tbody tr:nth-child(odd) td{
  background:rgba(191, 204, 214, 0.15); }

table.bp3-html-table.bp3-html-table-bordered th:not(:first-child){
  -webkit-box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-html-table-bordered tbody tr td,
table.bp3-html-table.bp3-html-table-bordered tfoot tr td{
  -webkit-box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15); }
  table.bp3-html-table.bp3-html-table-bordered tbody tr td:not(:first-child),
  table.bp3-html-table.bp3-html-table-bordered tfoot tr td:not(:first-child){
    -webkit-box-shadow:inset 1px 1px 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 1px 1px 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td{
  -webkit-box-shadow:none;
          box-shadow:none; }
  table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td:not(:first-child){
    -webkit-box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-interactive tbody tr:hover td{
  background-color:rgba(191, 204, 214, 0.3);
  cursor:pointer; }

table.bp3-html-table.bp3-interactive tbody tr:active td{
  background-color:rgba(191, 204, 214, 0.4); }

.bp3-dark table.bp3-html-table{ }
  .bp3-dark table.bp3-html-table.bp3-html-table-striped tbody tr:nth-child(odd) td{
    background:rgba(92, 112, 128, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered th:not(:first-child){
    -webkit-box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered tbody tr td,
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered tfoot tr td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15); }
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered tbody tr td:not(:first-child),
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered tfoot tr td:not(:first-child){
      -webkit-box-shadow:inset 1px 1px 0 0 rgba(255, 255, 255, 0.15);
              box-shadow:inset 1px 1px 0 0 rgba(255, 255, 255, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td{
    -webkit-box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15); }
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td:first-child{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-dark table.bp3-html-table.bp3-interactive tbody tr:hover td{
    background-color:rgba(92, 112, 128, 0.3);
    cursor:pointer; }
  .bp3-dark table.bp3-html-table.bp3-interactive tbody tr:active td{
    background-color:rgba(92, 112, 128, 0.4); }

.bp3-key-combo{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center; }
  .bp3-key-combo > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-key-combo > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-key-combo::before,
  .bp3-key-combo > *{
    margin-right:5px; }
  .bp3-key-combo:empty::before,
  .bp3-key-combo > :last-child{
    margin-right:0; }

.bp3-hotkey-dialog{
  padding-bottom:0;
  top:40px; }
  .bp3-hotkey-dialog .bp3-dialog-body{
    margin:0;
    padding:0; }
  .bp3-hotkey-dialog .bp3-hotkey-label{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1; }

.bp3-hotkey-column{
  margin:auto;
  max-height:80vh;
  overflow-y:auto;
  padding:30px; }
  .bp3-hotkey-column .bp3-heading{
    margin-bottom:20px; }
    .bp3-hotkey-column .bp3-heading:not(:first-child){
      margin-top:40px; }

.bp3-hotkey{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:justify;
      -ms-flex-pack:justify;
          justify-content:space-between;
  margin-left:0;
  margin-right:0; }
  .bp3-hotkey:not(:last-child){
    margin-bottom:10px; }
.bp3-icon{
  display:inline-block;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  vertical-align:text-bottom; }
  .bp3-icon:not(:empty)::before{
    content:"" !important;
    content:unset !important; }
  .bp3-icon > svg{
    display:block; }
    .bp3-icon > svg:not([fill]){
      fill:currentColor; }

.bp3-icon.bp3-intent-primary, .bp3-icon-standard.bp3-intent-primary, .bp3-icon-large.bp3-intent-primary{
  color:#106ba3; }
  .bp3-dark .bp3-icon.bp3-intent-primary, .bp3-dark .bp3-icon-standard.bp3-intent-primary, .bp3-dark .bp3-icon-large.bp3-intent-primary{
    color:#48aff0; }

.bp3-icon.bp3-intent-success, .bp3-icon-standard.bp3-intent-success, .bp3-icon-large.bp3-intent-success{
  color:#0d8050; }
  .bp3-dark .bp3-icon.bp3-intent-success, .bp3-dark .bp3-icon-standard.bp3-intent-success, .bp3-dark .bp3-icon-large.bp3-intent-success{
    color:#3dcc91; }

.bp3-icon.bp3-intent-warning, .bp3-icon-standard.bp3-intent-warning, .bp3-icon-large.bp3-intent-warning{
  color:#bf7326; }
  .bp3-dark .bp3-icon.bp3-intent-warning, .bp3-dark .bp3-icon-standard.bp3-intent-warning, .bp3-dark .bp3-icon-large.bp3-intent-warning{
    color:#ffb366; }

.bp3-icon.bp3-intent-danger, .bp3-icon-standard.bp3-intent-danger, .bp3-icon-large.bp3-intent-danger{
  color:#c23030; }
  .bp3-dark .bp3-icon.bp3-intent-danger, .bp3-dark .bp3-icon-standard.bp3-intent-danger, .bp3-dark .bp3-icon-large.bp3-intent-danger{
    color:#ff7373; }

span.bp3-icon-standard{
  font-family:"Icons16", sans-serif;
  font-size:16px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  display:inline-block; }

span.bp3-icon-large{
  font-family:"Icons20", sans-serif;
  font-size:20px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  display:inline-block; }

span.bp3-icon:empty{
  font-family:"Icons20";
  font-size:inherit;
  font-style:normal;
  font-weight:400;
  line-height:1; }
  span.bp3-icon:empty::before{
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased; }

.bp3-icon-add::before{
  content:""; }

.bp3-icon-add-column-left::before{
  content:""; }

.bp3-icon-add-column-right::before{
  content:""; }

.bp3-icon-add-row-bottom::before{
  content:""; }

.bp3-icon-add-row-top::before{
  content:""; }

.bp3-icon-add-to-artifact::before{
  content:""; }

.bp3-icon-add-to-folder::before{
  content:""; }

.bp3-icon-airplane::before{
  content:""; }

.bp3-icon-align-center::before{
  content:""; }

.bp3-icon-align-justify::before{
  content:""; }

.bp3-icon-align-left::before{
  content:""; }

.bp3-icon-align-right::before{
  content:""; }

.bp3-icon-alignment-bottom::before{
  content:""; }

.bp3-icon-alignment-horizontal-center::before{
  content:""; }

.bp3-icon-alignment-left::before{
  content:""; }

.bp3-icon-alignment-right::before{
  content:""; }

.bp3-icon-alignment-top::before{
  content:""; }

.bp3-icon-alignment-vertical-center::before{
  content:""; }

.bp3-icon-annotation::before{
  content:""; }

.bp3-icon-application::before{
  content:""; }

.bp3-icon-applications::before{
  content:""; }

.bp3-icon-archive::before{
  content:""; }

.bp3-icon-arrow-bottom-left::before{
  content:"↙"; }

.bp3-icon-arrow-bottom-right::before{
  content:"↘"; }

.bp3-icon-arrow-down::before{
  content:"↓"; }

.bp3-icon-arrow-left::before{
  content:"←"; }

.bp3-icon-arrow-right::before{
  content:"→"; }

.bp3-icon-arrow-top-left::before{
  content:"↖"; }

.bp3-icon-arrow-top-right::before{
  content:"↗"; }

.bp3-icon-arrow-up::before{
  content:"↑"; }

.bp3-icon-arrows-horizontal::before{
  content:"↔"; }

.bp3-icon-arrows-vertical::before{
  content:"↕"; }

.bp3-icon-asterisk::before{
  content:"*"; }

.bp3-icon-automatic-updates::before{
  content:""; }

.bp3-icon-badge::before{
  content:""; }

.bp3-icon-ban-circle::before{
  content:""; }

.bp3-icon-bank-account::before{
  content:""; }

.bp3-icon-barcode::before{
  content:""; }

.bp3-icon-blank::before{
  content:""; }

.bp3-icon-blocked-person::before{
  content:""; }

.bp3-icon-bold::before{
  content:""; }

.bp3-icon-book::before{
  content:""; }

.bp3-icon-bookmark::before{
  content:""; }

.bp3-icon-box::before{
  content:""; }

.bp3-icon-briefcase::before{
  content:""; }

.bp3-icon-bring-data::before{
  content:""; }

.bp3-icon-build::before{
  content:""; }

.bp3-icon-calculator::before{
  content:""; }

.bp3-icon-calendar::before{
  content:""; }

.bp3-icon-camera::before{
  content:""; }

.bp3-icon-caret-down::before{
  content:"⌄"; }

.bp3-icon-caret-left::before{
  content:"〈"; }

.bp3-icon-caret-right::before{
  content:"〉"; }

.bp3-icon-caret-up::before{
  content:"⌃"; }

.bp3-icon-cell-tower::before{
  content:""; }

.bp3-icon-changes::before{
  content:""; }

.bp3-icon-chart::before{
  content:""; }

.bp3-icon-chat::before{
  content:""; }

.bp3-icon-chevron-backward::before{
  content:""; }

.bp3-icon-chevron-down::before{
  content:""; }

.bp3-icon-chevron-forward::before{
  content:""; }

.bp3-icon-chevron-left::before{
  content:""; }

.bp3-icon-chevron-right::before{
  content:""; }

.bp3-icon-chevron-up::before{
  content:""; }

.bp3-icon-circle::before{
  content:""; }

.bp3-icon-circle-arrow-down::before{
  content:""; }

.bp3-icon-circle-arrow-left::before{
  content:""; }

.bp3-icon-circle-arrow-right::before{
  content:""; }

.bp3-icon-circle-arrow-up::before{
  content:""; }

.bp3-icon-citation::before{
  content:""; }

.bp3-icon-clean::before{
  content:""; }

.bp3-icon-clipboard::before{
  content:""; }

.bp3-icon-cloud::before{
  content:"☁"; }

.bp3-icon-cloud-download::before{
  content:""; }

.bp3-icon-cloud-upload::before{
  content:""; }

.bp3-icon-code::before{
  content:""; }

.bp3-icon-code-block::before{
  content:""; }

.bp3-icon-cog::before{
  content:""; }

.bp3-icon-collapse-all::before{
  content:""; }

.bp3-icon-column-layout::before{
  content:""; }

.bp3-icon-comment::before{
  content:""; }

.bp3-icon-comparison::before{
  content:""; }

.bp3-icon-compass::before{
  content:""; }

.bp3-icon-compressed::before{
  content:""; }

.bp3-icon-confirm::before{
  content:""; }

.bp3-icon-console::before{
  content:""; }

.bp3-icon-contrast::before{
  content:""; }

.bp3-icon-control::before{
  content:""; }

.bp3-icon-credit-card::before{
  content:""; }

.bp3-icon-cross::before{
  content:"✗"; }

.bp3-icon-crown::before{
  content:""; }

.bp3-icon-cube::before{
  content:""; }

.bp3-icon-cube-add::before{
  content:""; }

.bp3-icon-cube-remove::before{
  content:""; }

.bp3-icon-curved-range-chart::before{
  content:""; }

.bp3-icon-cut::before{
  content:""; }

.bp3-icon-dashboard::before{
  content:""; }

.bp3-icon-data-lineage::before{
  content:""; }

.bp3-icon-database::before{
  content:""; }

.bp3-icon-delete::before{
  content:""; }

.bp3-icon-delta::before{
  content:"Δ"; }

.bp3-icon-derive-column::before{
  content:""; }

.bp3-icon-desktop::before{
  content:""; }

.bp3-icon-diagnosis::before{
  content:""; }

.bp3-icon-diagram-tree::before{
  content:""; }

.bp3-icon-direction-left::before{
  content:""; }

.bp3-icon-direction-right::before{
  content:""; }

.bp3-icon-disable::before{
  content:""; }

.bp3-icon-document::before{
  content:""; }

.bp3-icon-document-open::before{
  content:""; }

.bp3-icon-document-share::before{
  content:""; }

.bp3-icon-dollar::before{
  content:"$"; }

.bp3-icon-dot::before{
  content:"•"; }

.bp3-icon-double-caret-horizontal::before{
  content:""; }

.bp3-icon-double-caret-vertical::before{
  content:""; }

.bp3-icon-double-chevron-down::before{
  content:""; }

.bp3-icon-double-chevron-left::before{
  content:""; }

.bp3-icon-double-chevron-right::before{
  content:""; }

.bp3-icon-double-chevron-up::before{
  content:""; }

.bp3-icon-doughnut-chart::before{
  content:""; }

.bp3-icon-download::before{
  content:""; }

.bp3-icon-drag-handle-horizontal::before{
  content:""; }

.bp3-icon-drag-handle-vertical::before{
  content:""; }

.bp3-icon-draw::before{
  content:""; }

.bp3-icon-drive-time::before{
  content:""; }

.bp3-icon-duplicate::before{
  content:""; }

.bp3-icon-edit::before{
  content:"✎"; }

.bp3-icon-eject::before{
  content:"⏏"; }

.bp3-icon-endorsed::before{
  content:""; }

.bp3-icon-envelope::before{
  content:"✉"; }

.bp3-icon-equals::before{
  content:""; }

.bp3-icon-eraser::before{
  content:""; }

.bp3-icon-error::before{
  content:""; }

.bp3-icon-euro::before{
  content:"€"; }

.bp3-icon-exchange::before{
  content:""; }

.bp3-icon-exclude-row::before{
  content:""; }

.bp3-icon-expand-all::before{
  content:""; }

.bp3-icon-export::before{
  content:""; }

.bp3-icon-eye-off::before{
  content:""; }

.bp3-icon-eye-on::before{
  content:""; }

.bp3-icon-eye-open::before{
  content:""; }

.bp3-icon-fast-backward::before{
  content:""; }

.bp3-icon-fast-forward::before{
  content:""; }

.bp3-icon-feed::before{
  content:""; }

.bp3-icon-feed-subscribed::before{
  content:""; }

.bp3-icon-film::before{
  content:""; }

.bp3-icon-filter::before{
  content:""; }

.bp3-icon-filter-keep::before{
  content:""; }

.bp3-icon-filter-list::before{
  content:""; }

.bp3-icon-filter-open::before{
  content:""; }

.bp3-icon-filter-remove::before{
  content:""; }

.bp3-icon-flag::before{
  content:"⚑"; }

.bp3-icon-flame::before{
  content:""; }

.bp3-icon-flash::before{
  content:""; }

.bp3-icon-floppy-disk::before{
  content:""; }

.bp3-icon-flow-branch::before{
  content:""; }

.bp3-icon-flow-end::before{
  content:""; }

.bp3-icon-flow-linear::before{
  content:""; }

.bp3-icon-flow-review::before{
  content:""; }

.bp3-icon-flow-review-branch::before{
  content:""; }

.bp3-icon-flows::before{
  content:""; }

.bp3-icon-folder-close::before{
  content:""; }

.bp3-icon-folder-new::before{
  content:""; }

.bp3-icon-folder-open::before{
  content:""; }

.bp3-icon-folder-shared::before{
  content:""; }

.bp3-icon-folder-shared-open::before{
  content:""; }

.bp3-icon-follower::before{
  content:""; }

.bp3-icon-following::before{
  content:""; }

.bp3-icon-font::before{
  content:""; }

.bp3-icon-fork::before{
  content:""; }

.bp3-icon-form::before{
  content:""; }

.bp3-icon-full-circle::before{
  content:""; }

.bp3-icon-full-stacked-chart::before{
  content:""; }

.bp3-icon-fullscreen::before{
  content:""; }

.bp3-icon-function::before{
  content:""; }

.bp3-icon-gantt-chart::before{
  content:""; }

.bp3-icon-geolocation::before{
  content:""; }

.bp3-icon-geosearch::before{
  content:""; }

.bp3-icon-git-branch::before{
  content:""; }

.bp3-icon-git-commit::before{
  content:""; }

.bp3-icon-git-merge::before{
  content:""; }

.bp3-icon-git-new-branch::before{
  content:""; }

.bp3-icon-git-pull::before{
  content:""; }

.bp3-icon-git-push::before{
  content:""; }

.bp3-icon-git-repo::before{
  content:""; }

.bp3-icon-glass::before{
  content:""; }

.bp3-icon-globe::before{
  content:""; }

.bp3-icon-globe-network::before{
  content:""; }

.bp3-icon-graph::before{
  content:""; }

.bp3-icon-graph-remove::before{
  content:""; }

.bp3-icon-greater-than::before{
  content:""; }

.bp3-icon-greater-than-or-equal-to::before{
  content:""; }

.bp3-icon-grid::before{
  content:""; }

.bp3-icon-grid-view::before{
  content:""; }

.bp3-icon-group-objects::before{
  content:""; }

.bp3-icon-grouped-bar-chart::before{
  content:""; }

.bp3-icon-hand::before{
  content:""; }

.bp3-icon-hand-down::before{
  content:""; }

.bp3-icon-hand-left::before{
  content:""; }

.bp3-icon-hand-right::before{
  content:""; }

.bp3-icon-hand-up::before{
  content:""; }

.bp3-icon-header::before{
  content:""; }

.bp3-icon-header-one::before{
  content:""; }

.bp3-icon-header-two::before{
  content:""; }

.bp3-icon-headset::before{
  content:""; }

.bp3-icon-heart::before{
  content:"♥"; }

.bp3-icon-heart-broken::before{
  content:""; }

.bp3-icon-heat-grid::before{
  content:""; }

.bp3-icon-heatmap::before{
  content:""; }

.bp3-icon-help::before{
  content:"?"; }

.bp3-icon-helper-management::before{
  content:""; }

.bp3-icon-highlight::before{
  content:""; }

.bp3-icon-history::before{
  content:""; }

.bp3-icon-home::before{
  content:"⌂"; }

.bp3-icon-horizontal-bar-chart::before{
  content:""; }

.bp3-icon-horizontal-bar-chart-asc::before{
  content:""; }

.bp3-icon-horizontal-bar-chart-desc::before{
  content:""; }

.bp3-icon-horizontal-distribution::before{
  content:""; }

.bp3-icon-id-number::before{
  content:""; }

.bp3-icon-image-rotate-left::before{
  content:""; }

.bp3-icon-image-rotate-right::before{
  content:""; }

.bp3-icon-import::before{
  content:""; }

.bp3-icon-inbox::before{
  content:""; }

.bp3-icon-inbox-filtered::before{
  content:""; }

.bp3-icon-inbox-geo::before{
  content:""; }

.bp3-icon-inbox-search::before{
  content:""; }

.bp3-icon-inbox-update::before{
  content:""; }

.bp3-icon-info-sign::before{
  content:"ℹ"; }

.bp3-icon-inheritance::before{
  content:""; }

.bp3-icon-inner-join::before{
  content:""; }

.bp3-icon-insert::before{
  content:""; }

.bp3-icon-intersection::before{
  content:""; }

.bp3-icon-ip-address::before{
  content:""; }

.bp3-icon-issue::before{
  content:""; }

.bp3-icon-issue-closed::before{
  content:""; }

.bp3-icon-issue-new::before{
  content:""; }

.bp3-icon-italic::before{
  content:""; }

.bp3-icon-join-table::before{
  content:""; }

.bp3-icon-key::before{
  content:""; }

.bp3-icon-key-backspace::before{
  content:""; }

.bp3-icon-key-command::before{
  content:""; }

.bp3-icon-key-control::before{
  content:""; }

.bp3-icon-key-delete::before{
  content:""; }

.bp3-icon-key-enter::before{
  content:""; }

.bp3-icon-key-escape::before{
  content:""; }

.bp3-icon-key-option::before{
  content:""; }

.bp3-icon-key-shift::before{
  content:""; }

.bp3-icon-key-tab::before{
  content:""; }

.bp3-icon-known-vehicle::before{
  content:""; }

.bp3-icon-lab-test::before{
  content:""; }

.bp3-icon-label::before{
  content:""; }

.bp3-icon-layer::before{
  content:""; }

.bp3-icon-layers::before{
  content:""; }

.bp3-icon-layout::before{
  content:""; }

.bp3-icon-layout-auto::before{
  content:""; }

.bp3-icon-layout-balloon::before{
  content:""; }

.bp3-icon-layout-circle::before{
  content:""; }

.bp3-icon-layout-grid::before{
  content:""; }

.bp3-icon-layout-group-by::before{
  content:""; }

.bp3-icon-layout-hierarchy::before{
  content:""; }

.bp3-icon-layout-linear::before{
  content:""; }

.bp3-icon-layout-skew-grid::before{
  content:""; }

.bp3-icon-layout-sorted-clusters::before{
  content:""; }

.bp3-icon-learning::before{
  content:""; }

.bp3-icon-left-join::before{
  content:""; }

.bp3-icon-less-than::before{
  content:""; }

.bp3-icon-less-than-or-equal-to::before{
  content:""; }

.bp3-icon-lifesaver::before{
  content:""; }

.bp3-icon-lightbulb::before{
  content:""; }

.bp3-icon-link::before{
  content:""; }

.bp3-icon-list::before{
  content:"☰"; }

.bp3-icon-list-columns::before{
  content:""; }

.bp3-icon-list-detail-view::before{
  content:""; }

.bp3-icon-locate::before{
  content:""; }

.bp3-icon-lock::before{
  content:""; }

.bp3-icon-log-in::before{
  content:""; }

.bp3-icon-log-out::before{
  content:""; }

.bp3-icon-manual::before{
  content:""; }

.bp3-icon-manually-entered-data::before{
  content:""; }

.bp3-icon-map::before{
  content:""; }

.bp3-icon-map-create::before{
  content:""; }

.bp3-icon-map-marker::before{
  content:""; }

.bp3-icon-maximize::before{
  content:""; }

.bp3-icon-media::before{
  content:""; }

.bp3-icon-menu::before{
  content:""; }

.bp3-icon-menu-closed::before{
  content:""; }

.bp3-icon-menu-open::before{
  content:""; }

.bp3-icon-merge-columns::before{
  content:""; }

.bp3-icon-merge-links::before{
  content:""; }

.bp3-icon-minimize::before{
  content:""; }

.bp3-icon-minus::before{
  content:"−"; }

.bp3-icon-mobile-phone::before{
  content:""; }

.bp3-icon-mobile-video::before{
  content:""; }

.bp3-icon-moon::before{
  content:""; }

.bp3-icon-more::before{
  content:""; }

.bp3-icon-mountain::before{
  content:""; }

.bp3-icon-move::before{
  content:""; }

.bp3-icon-mugshot::before{
  content:""; }

.bp3-icon-multi-select::before{
  content:""; }

.bp3-icon-music::before{
  content:""; }

.bp3-icon-new-drawing::before{
  content:""; }

.bp3-icon-new-grid-item::before{
  content:""; }

.bp3-icon-new-layer::before{
  content:""; }

.bp3-icon-new-layers::before{
  content:""; }

.bp3-icon-new-link::before{
  content:""; }

.bp3-icon-new-object::before{
  content:""; }

.bp3-icon-new-person::before{
  content:""; }

.bp3-icon-new-prescription::before{
  content:""; }

.bp3-icon-new-text-box::before{
  content:""; }

.bp3-icon-ninja::before{
  content:""; }

.bp3-icon-not-equal-to::before{
  content:""; }

.bp3-icon-notifications::before{
  content:""; }

.bp3-icon-notifications-updated::before{
  content:""; }

.bp3-icon-numbered-list::before{
  content:""; }

.bp3-icon-numerical::before{
  content:""; }

.bp3-icon-office::before{
  content:""; }

.bp3-icon-offline::before{
  content:""; }

.bp3-icon-oil-field::before{
  content:""; }

.bp3-icon-one-column::before{
  content:""; }

.bp3-icon-outdated::before{
  content:""; }

.bp3-icon-page-layout::before{
  content:""; }

.bp3-icon-panel-stats::before{
  content:""; }

.bp3-icon-panel-table::before{
  content:""; }

.bp3-icon-paperclip::before{
  content:""; }

.bp3-icon-paragraph::before{
  content:""; }

.bp3-icon-path::before{
  content:""; }

.bp3-icon-path-search::before{
  content:""; }

.bp3-icon-pause::before{
  content:""; }

.bp3-icon-people::before{
  content:""; }

.bp3-icon-percentage::before{
  content:""; }

.bp3-icon-person::before{
  content:""; }

.bp3-icon-phone::before{
  content:"☎"; }

.bp3-icon-pie-chart::before{
  content:""; }

.bp3-icon-pin::before{
  content:""; }

.bp3-icon-pivot::before{
  content:""; }

.bp3-icon-pivot-table::before{
  content:""; }

.bp3-icon-play::before{
  content:""; }

.bp3-icon-plus::before{
  content:"+"; }

.bp3-icon-polygon-filter::before{
  content:""; }

.bp3-icon-power::before{
  content:""; }

.bp3-icon-predictive-analysis::before{
  content:""; }

.bp3-icon-prescription::before{
  content:""; }

.bp3-icon-presentation::before{
  content:""; }

.bp3-icon-print::before{
  content:"⎙"; }

.bp3-icon-projects::before{
  content:""; }

.bp3-icon-properties::before{
  content:""; }

.bp3-icon-property::before{
  content:""; }

.bp3-icon-publish-function::before{
  content:""; }

.bp3-icon-pulse::before{
  content:""; }

.bp3-icon-random::before{
  content:""; }

.bp3-icon-record::before{
  content:""; }

.bp3-icon-redo::before{
  content:""; }

.bp3-icon-refresh::before{
  content:""; }

.bp3-icon-regression-chart::before{
  content:""; }

.bp3-icon-remove::before{
  content:""; }

.bp3-icon-remove-column::before{
  content:""; }

.bp3-icon-remove-column-left::before{
  content:""; }

.bp3-icon-remove-column-right::before{
  content:""; }

.bp3-icon-remove-row-bottom::before{
  content:""; }

.bp3-icon-remove-row-top::before{
  content:""; }

.bp3-icon-repeat::before{
  content:""; }

.bp3-icon-reset::before{
  content:""; }

.bp3-icon-resolve::before{
  content:""; }

.bp3-icon-rig::before{
  content:""; }

.bp3-icon-right-join::before{
  content:""; }

.bp3-icon-ring::before{
  content:""; }

.bp3-icon-rotate-document::before{
  content:""; }

.bp3-icon-rotate-page::before{
  content:""; }

.bp3-icon-satellite::before{
  content:""; }

.bp3-icon-saved::before{
  content:""; }

.bp3-icon-scatter-plot::before{
  content:""; }

.bp3-icon-search::before{
  content:""; }

.bp3-icon-search-around::before{
  content:""; }

.bp3-icon-search-template::before{
  content:""; }

.bp3-icon-search-text::before{
  content:""; }

.bp3-icon-segmented-control::before{
  content:""; }

.bp3-icon-select::before{
  content:""; }

.bp3-icon-selection::before{
  content:"⦿"; }

.bp3-icon-send-to::before{
  content:""; }

.bp3-icon-send-to-graph::before{
  content:""; }

.bp3-icon-send-to-map::before{
  content:""; }

.bp3-icon-series-add::before{
  content:""; }

.bp3-icon-series-configuration::before{
  content:""; }

.bp3-icon-series-derived::before{
  content:""; }

.bp3-icon-series-filtered::before{
  content:""; }

.bp3-icon-series-search::before{
  content:""; }

.bp3-icon-settings::before{
  content:""; }

.bp3-icon-share::before{
  content:""; }

.bp3-icon-shield::before{
  content:""; }

.bp3-icon-shop::before{
  content:""; }

.bp3-icon-shopping-cart::before{
  content:""; }

.bp3-icon-signal-search::before{
  content:""; }

.bp3-icon-sim-card::before{
  content:""; }

.bp3-icon-slash::before{
  content:""; }

.bp3-icon-small-cross::before{
  content:""; }

.bp3-icon-small-minus::before{
  content:""; }

.bp3-icon-small-plus::before{
  content:""; }

.bp3-icon-small-tick::before{
  content:""; }

.bp3-icon-snowflake::before{
  content:""; }

.bp3-icon-social-media::before{
  content:""; }

.bp3-icon-sort::before{
  content:""; }

.bp3-icon-sort-alphabetical::before{
  content:""; }

.bp3-icon-sort-alphabetical-desc::before{
  content:""; }

.bp3-icon-sort-asc::before{
  content:""; }

.bp3-icon-sort-desc::before{
  content:""; }

.bp3-icon-sort-numerical::before{
  content:""; }

.bp3-icon-sort-numerical-desc::before{
  content:""; }

.bp3-icon-split-columns::before{
  content:""; }

.bp3-icon-square::before{
  content:""; }

.bp3-icon-stacked-chart::before{
  content:""; }

.bp3-icon-star::before{
  content:"★"; }

.bp3-icon-star-empty::before{
  content:"☆"; }

.bp3-icon-step-backward::before{
  content:""; }

.bp3-icon-step-chart::before{
  content:""; }

.bp3-icon-step-forward::before{
  content:""; }

.bp3-icon-stop::before{
  content:""; }

.bp3-icon-stopwatch::before{
  content:""; }

.bp3-icon-strikethrough::before{
  content:""; }

.bp3-icon-style::before{
  content:""; }

.bp3-icon-swap-horizontal::before{
  content:""; }

.bp3-icon-swap-vertical::before{
  content:""; }

.bp3-icon-symbol-circle::before{
  content:""; }

.bp3-icon-symbol-cross::before{
  content:""; }

.bp3-icon-symbol-diamond::before{
  content:""; }

.bp3-icon-symbol-square::before{
  content:""; }

.bp3-icon-symbol-triangle-down::before{
  content:""; }

.bp3-icon-symbol-triangle-up::before{
  content:""; }

.bp3-icon-tag::before{
  content:""; }

.bp3-icon-take-action::before{
  content:""; }

.bp3-icon-taxi::before{
  content:""; }

.bp3-icon-text-highlight::before{
  content:""; }

.bp3-icon-th::before{
  content:""; }

.bp3-icon-th-derived::before{
  content:""; }

.bp3-icon-th-disconnect::before{
  content:""; }

.bp3-icon-th-filtered::before{
  content:""; }

.bp3-icon-th-list::before{
  content:""; }

.bp3-icon-thumbs-down::before{
  content:""; }

.bp3-icon-thumbs-up::before{
  content:""; }

.bp3-icon-tick::before{
  content:"✓"; }

.bp3-icon-tick-circle::before{
  content:""; }

.bp3-icon-time::before{
  content:"⏲"; }

.bp3-icon-timeline-area-chart::before{
  content:""; }

.bp3-icon-timeline-bar-chart::before{
  content:""; }

.bp3-icon-timeline-events::before{
  content:""; }

.bp3-icon-timeline-line-chart::before{
  content:""; }

.bp3-icon-tint::before{
  content:""; }

.bp3-icon-torch::before{
  content:""; }

.bp3-icon-tractor::before{
  content:""; }

.bp3-icon-train::before{
  content:""; }

.bp3-icon-translate::before{
  content:""; }

.bp3-icon-trash::before{
  content:""; }

.bp3-icon-tree::before{
  content:""; }

.bp3-icon-trending-down::before{
  content:""; }

.bp3-icon-trending-up::before{
  content:""; }

.bp3-icon-truck::before{
  content:""; }

.bp3-icon-two-columns::before{
  content:""; }

.bp3-icon-unarchive::before{
  content:""; }

.bp3-icon-underline::before{
  content:"⎁"; }

.bp3-icon-undo::before{
  content:"⎌"; }

.bp3-icon-ungroup-objects::before{
  content:""; }

.bp3-icon-unknown-vehicle::before{
  content:""; }

.bp3-icon-unlock::before{
  content:""; }

.bp3-icon-unpin::before{
  content:""; }

.bp3-icon-unresolve::before{
  content:""; }

.bp3-icon-updated::before{
  content:""; }

.bp3-icon-upload::before{
  content:""; }

.bp3-icon-user::before{
  content:""; }

.bp3-icon-variable::before{
  content:""; }

.bp3-icon-vertical-bar-chart-asc::before{
  content:""; }

.bp3-icon-vertical-bar-chart-desc::before{
  content:""; }

.bp3-icon-vertical-distribution::before{
  content:""; }

.bp3-icon-video::before{
  content:""; }

.bp3-icon-volume-down::before{
  content:""; }

.bp3-icon-volume-off::before{
  content:""; }

.bp3-icon-volume-up::before{
  content:""; }

.bp3-icon-walk::before{
  content:""; }

.bp3-icon-warning-sign::before{
  content:""; }

.bp3-icon-waterfall-chart::before{
  content:""; }

.bp3-icon-widget::before{
  content:""; }

.bp3-icon-widget-button::before{
  content:""; }

.bp3-icon-widget-footer::before{
  content:""; }

.bp3-icon-widget-header::before{
  content:""; }

.bp3-icon-wrench::before{
  content:""; }

.bp3-icon-zoom-in::before{
  content:""; }

.bp3-icon-zoom-out::before{
  content:""; }

.bp3-icon-zoom-to-fit::before{
  content:""; }
.bp3-submenu > .bp3-popover-wrapper{
  display:block; }

.bp3-submenu .bp3-popover-target{
  display:block; }
  .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{ }

.bp3-submenu.bp3-popover{
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0 5px; }
  .bp3-submenu.bp3-popover > .bp3-popover-content{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-submenu.bp3-popover, .bp3-submenu.bp3-popover.bp3-dark{
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-dark .bp3-submenu.bp3-popover > .bp3-popover-content, .bp3-submenu.bp3-popover.bp3-dark > .bp3-popover-content{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
.bp3-menu{
  background:#ffffff;
  border-radius:3px;
  color:#182026;
  list-style:none;
  margin:0;
  min-width:180px;
  padding:5px;
  text-align:left; }

.bp3-menu-divider{
  border-top:1px solid rgba(16, 22, 26, 0.15);
  display:block;
  margin:5px; }
  .bp3-dark .bp3-menu-divider{
    border-top-color:rgba(255, 255, 255, 0.15); }

.bp3-menu-item{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  border-radius:2px;
  color:inherit;
  line-height:20px;
  padding:5px 7px;
  text-decoration:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-menu-item > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-menu-item > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-menu-item::before,
  .bp3-menu-item > *{
    margin-right:7px; }
  .bp3-menu-item:empty::before,
  .bp3-menu-item > :last-child{
    margin-right:0; }
  .bp3-menu-item > .bp3-fill{
    word-break:break-word; }
  .bp3-menu-item:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
    background-color:rgba(167, 182, 194, 0.3);
    cursor:pointer;
    text-decoration:none; }
  .bp3-menu-item.bp3-disabled{
    background-color:inherit;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-dark .bp3-menu-item{
    color:inherit; }
    .bp3-dark .bp3-menu-item:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
      background-color:rgba(138, 155, 168, 0.15);
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-disabled{
      background-color:inherit;
      color:rgba(167, 182, 194, 0.6); }
  .bp3-menu-item.bp3-intent-primary{
    color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-primary::before, .bp3-menu-item.bp3-intent-primary::after,
    .bp3-menu-item.bp3-intent-primary .bp3-menu-item-label{
      color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-menu-item.bp3-intent-primary.bp3-active{
      background-color:#137cbd; }
    .bp3-menu-item.bp3-intent-primary:active{
      background-color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-menu-item.bp3-intent-primary:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-menu-item.bp3-intent-primary:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-primary:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-primary:active, .bp3-menu-item.bp3-intent-primary:active::before, .bp3-menu-item.bp3-intent-primary:active::after,
    .bp3-menu-item.bp3-intent-primary:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-primary.bp3-active, .bp3-menu-item.bp3-intent-primary.bp3-active::before, .bp3-menu-item.bp3-intent-primary.bp3-active::after,
    .bp3-menu-item.bp3-intent-primary.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-success{
    color:#0d8050; }
    .bp3-menu-item.bp3-intent-success .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-success::before, .bp3-menu-item.bp3-intent-success::after,
    .bp3-menu-item.bp3-intent-success .bp3-menu-item-label{
      color:#0d8050; }
    .bp3-menu-item.bp3-intent-success:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-menu-item.bp3-intent-success.bp3-active{
      background-color:#0f9960; }
    .bp3-menu-item.bp3-intent-success:active{
      background-color:#0d8050; }
    .bp3-menu-item.bp3-intent-success:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-menu-item.bp3-intent-success:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-menu-item.bp3-intent-success:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-success:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-success:active, .bp3-menu-item.bp3-intent-success:active::before, .bp3-menu-item.bp3-intent-success:active::after,
    .bp3-menu-item.bp3-intent-success:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-success.bp3-active, .bp3-menu-item.bp3-intent-success.bp3-active::before, .bp3-menu-item.bp3-intent-success.bp3-active::after,
    .bp3-menu-item.bp3-intent-success.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-warning{
    color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-warning::before, .bp3-menu-item.bp3-intent-warning::after,
    .bp3-menu-item.bp3-intent-warning .bp3-menu-item-label{
      color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-menu-item.bp3-intent-warning.bp3-active{
      background-color:#d9822b; }
    .bp3-menu-item.bp3-intent-warning:active{
      background-color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-menu-item.bp3-intent-warning:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-menu-item.bp3-intent-warning:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-warning:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-warning:active, .bp3-menu-item.bp3-intent-warning:active::before, .bp3-menu-item.bp3-intent-warning:active::after,
    .bp3-menu-item.bp3-intent-warning:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-warning.bp3-active, .bp3-menu-item.bp3-intent-warning.bp3-active::before, .bp3-menu-item.bp3-intent-warning.bp3-active::after,
    .bp3-menu-item.bp3-intent-warning.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-danger{
    color:#c23030; }
    .bp3-menu-item.bp3-intent-danger .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-danger::before, .bp3-menu-item.bp3-intent-danger::after,
    .bp3-menu-item.bp3-intent-danger .bp3-menu-item-label{
      color:#c23030; }
    .bp3-menu-item.bp3-intent-danger:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-menu-item.bp3-intent-danger.bp3-active{
      background-color:#db3737; }
    .bp3-menu-item.bp3-intent-danger:active{
      background-color:#c23030; }
    .bp3-menu-item.bp3-intent-danger:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-menu-item.bp3-intent-danger:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-menu-item.bp3-intent-danger:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-danger:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-danger:active, .bp3-menu-item.bp3-intent-danger:active::before, .bp3-menu-item.bp3-intent-danger:active::after,
    .bp3-menu-item.bp3-intent-danger:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-danger.bp3-active, .bp3-menu-item.bp3-intent-danger.bp3-active::before, .bp3-menu-item.bp3-intent-danger.bp3-active::after,
    .bp3-menu-item.bp3-intent-danger.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    margin-right:7px; }
  .bp3-menu-item::before,
  .bp3-menu-item > .bp3-icon{
    color:#5c7080;
    margin-top:2px; }
  .bp3-menu-item .bp3-menu-item-label{
    color:#5c7080; }
  .bp3-menu-item:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
    color:inherit; }
  .bp3-menu-item.bp3-active, .bp3-menu-item:active{
    background-color:rgba(115, 134, 148, 0.3); }
  .bp3-menu-item.bp3-disabled{
    background-color:inherit !important;
    color:rgba(92, 112, 128, 0.6) !important;
    cursor:not-allowed !important;
    outline:none !important; }
    .bp3-menu-item.bp3-disabled::before,
    .bp3-menu-item.bp3-disabled > .bp3-icon,
    .bp3-menu-item.bp3-disabled .bp3-menu-item-label{
      color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-large .bp3-menu-item{
    font-size:16px;
    line-height:22px;
    padding:9px 7px; }
    .bp3-large .bp3-menu-item .bp3-icon{
      margin-top:3px; }
    .bp3-large .bp3-menu-item::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased;
      margin-right:10px;
      margin-top:1px; }

button.bp3-menu-item{
  background:none;
  border:none;
  text-align:left;
  width:100%; }
.bp3-menu-header{
  border-top:1px solid rgba(16, 22, 26, 0.15);
  display:block;
  margin:5px;
  cursor:default;
  padding-left:2px; }
  .bp3-dark .bp3-menu-header{
    border-top-color:rgba(255, 255, 255, 0.15); }
  .bp3-menu-header:first-of-type{
    border-top:none; }
  .bp3-menu-header > h6{
    color:#182026;
    font-weight:600;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    line-height:17px;
    margin:0;
    padding:10px 7px 0 1px; }
    .bp3-dark .bp3-menu-header > h6{
      color:#f5f8fa; }
  .bp3-menu-header:first-of-type > h6{
    padding-top:0; }
  .bp3-large .bp3-menu-header > h6{
    font-size:18px;
    padding-bottom:5px;
    padding-top:15px; }
  .bp3-large .bp3-menu-header:first-of-type > h6{
    padding-top:0; }

.bp3-dark .bp3-menu{
  background:#30404d;
  color:#f5f8fa; }

.bp3-dark .bp3-menu-item{ }
  .bp3-dark .bp3-menu-item.bp3-intent-primary{
    color:#48aff0; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary::before, .bp3-dark .bp3-menu-item.bp3-intent-primary::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary .bp3-menu-item-label{
      color:#48aff0; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active{
      background-color:#137cbd; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:active{
      background-color:#106ba3; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-primary:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-primary:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-primary:active, .bp3-dark .bp3-menu-item.bp3-intent-primary:active::before, .bp3-dark .bp3-menu-item.bp3-intent-primary:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-success{
    color:#3dcc91; }
    .bp3-dark .bp3-menu-item.bp3-intent-success .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-success::before, .bp3-dark .bp3-menu-item.bp3-intent-success::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success .bp3-menu-item-label{
      color:#3dcc91; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active{
      background-color:#0f9960; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:active{
      background-color:#0d8050; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-success:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-success:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-success:active, .bp3-dark .bp3-menu-item.bp3-intent-success:active::before, .bp3-dark .bp3-menu-item.bp3-intent-success:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-warning{
    color:#ffb366; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning::before, .bp3-dark .bp3-menu-item.bp3-intent-warning::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning .bp3-menu-item-label{
      color:#ffb366; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active{
      background-color:#d9822b; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:active{
      background-color:#bf7326; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-warning:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-warning:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-warning:active, .bp3-dark .bp3-menu-item.bp3-intent-warning:active::before, .bp3-dark .bp3-menu-item.bp3-intent-warning:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-danger{
    color:#ff7373; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger::before, .bp3-dark .bp3-menu-item.bp3-intent-danger::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger .bp3-menu-item-label{
      color:#ff7373; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active{
      background-color:#db3737; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:active{
      background-color:#c23030; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-danger:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-danger:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-danger:active, .bp3-dark .bp3-menu-item.bp3-intent-danger:active::before, .bp3-dark .bp3-menu-item.bp3-intent-danger:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item::before,
  .bp3-dark .bp3-menu-item > .bp3-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-menu-item .bp3-menu-item-label{
    color:#a7b6c2; }
  .bp3-dark .bp3-menu-item.bp3-active, .bp3-dark .bp3-menu-item:active{
    background-color:rgba(138, 155, 168, 0.3); }
  .bp3-dark .bp3-menu-item.bp3-disabled{
    color:rgba(167, 182, 194, 0.6) !important; }
    .bp3-dark .bp3-menu-item.bp3-disabled::before,
    .bp3-dark .bp3-menu-item.bp3-disabled > .bp3-icon,
    .bp3-dark .bp3-menu-item.bp3-disabled .bp3-menu-item-label{
      color:rgba(167, 182, 194, 0.6) !important; }

.bp3-dark .bp3-menu-divider,
.bp3-dark .bp3-menu-header{
  border-color:rgba(255, 255, 255, 0.15); }

.bp3-dark .bp3-menu-header > h6{
  color:#f5f8fa; }

.bp3-label .bp3-menu{
  margin-top:5px; }
.bp3-navbar{
  background-color:#ffffff;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  height:50px;
  padding:0 15px;
  position:relative;
  width:100%;
  z-index:10; }
  .bp3-navbar.bp3-dark,
  .bp3-dark .bp3-navbar{
    background-color:#394b59; }
  .bp3-navbar.bp3-dark{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-navbar{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-navbar.bp3-fixed-top{
    left:0;
    position:fixed;
    right:0;
    top:0; }

.bp3-navbar-heading{
  font-size:16px;
  margin-right:15px; }

.bp3-navbar-group{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:50px; }
  .bp3-navbar-group.bp3-align-left{
    float:left; }
  .bp3-navbar-group.bp3-align-right{
    float:right; }

.bp3-navbar-divider{
  border-left:1px solid rgba(16, 22, 26, 0.15);
  height:20px;
  margin:0 10px; }
  .bp3-dark .bp3-navbar-divider{
    border-left-color:rgba(255, 255, 255, 0.15); }
.bp3-non-ideal-state{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  height:100%;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  text-align:center;
  width:100%; }
  .bp3-non-ideal-state > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-non-ideal-state > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-non-ideal-state::before,
  .bp3-non-ideal-state > *{
    margin-bottom:20px; }
  .bp3-non-ideal-state:empty::before,
  .bp3-non-ideal-state > :last-child{
    margin-bottom:0; }
  .bp3-non-ideal-state > *{
    max-width:400px; }

.bp3-non-ideal-state-visual{
  color:rgba(92, 112, 128, 0.6);
  font-size:60px; }
  .bp3-dark .bp3-non-ideal-state-visual{
    color:rgba(167, 182, 194, 0.6); }

.bp3-overflow-list{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-wrap:nowrap;
      flex-wrap:nowrap;
  min-width:0; }

.bp3-overflow-list-spacer{
  -ms-flex-negative:1;
      flex-shrink:1;
  width:1px; }

body.bp3-overlay-open{
  overflow:hidden; }

.bp3-overlay{
  bottom:0;
  left:0;
  position:static;
  right:0;
  top:0;
  z-index:20; }
  .bp3-overlay:not(.bp3-overlay-open){
    pointer-events:none; }
  .bp3-overlay.bp3-overlay-container{
    overflow:hidden;
    position:fixed; }
    .bp3-overlay.bp3-overlay-container.bp3-overlay-inline{
      position:absolute; }
  .bp3-overlay.bp3-overlay-scroll-container{
    overflow:auto;
    position:fixed; }
    .bp3-overlay.bp3-overlay-scroll-container.bp3-overlay-inline{
      position:absolute; }
  .bp3-overlay.bp3-overlay-inline{
    display:inline;
    overflow:visible; }

.bp3-overlay-content{
  position:fixed;
  z-index:20; }
  .bp3-overlay-inline .bp3-overlay-content,
  .bp3-overlay-scroll-container .bp3-overlay-content{
    position:absolute; }

.bp3-overlay-backdrop{
  bottom:0;
  left:0;
  position:fixed;
  right:0;
  top:0;
  opacity:1;
  background-color:rgba(16, 22, 26, 0.7);
  overflow:auto;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none;
  z-index:20; }
  .bp3-overlay-backdrop.bp3-overlay-enter, .bp3-overlay-backdrop.bp3-overlay-appear{
    opacity:0; }
  .bp3-overlay-backdrop.bp3-overlay-enter-active, .bp3-overlay-backdrop.bp3-overlay-appear-active{
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-overlay-backdrop.bp3-overlay-exit{
    opacity:1; }
  .bp3-overlay-backdrop.bp3-overlay-exit-active{
    opacity:0;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-overlay-backdrop:focus{
    outline:none; }
  .bp3-overlay-inline .bp3-overlay-backdrop{
    position:absolute; }
.bp3-panel-stack{
  overflow:hidden;
  position:relative; }

.bp3-panel-stack-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  -webkit-box-shadow:0 1px rgba(16, 22, 26, 0.15);
          box-shadow:0 1px rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-negative:0;
      flex-shrink:0;
  height:30px;
  z-index:1; }
  .bp3-dark .bp3-panel-stack-header{
    -webkit-box-shadow:0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 1px rgba(255, 255, 255, 0.15); }
  .bp3-panel-stack-header > span{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1;
            flex:1; }
  .bp3-panel-stack-header .bp3-heading{
    margin:0 5px; }

.bp3-button.bp3-panel-stack-header-back{
  margin-left:5px;
  padding-left:0;
  white-space:nowrap; }
  .bp3-button.bp3-panel-stack-header-back .bp3-icon{
    margin:0 2px; }

.bp3-panel-stack-view{
  bottom:0;
  left:0;
  position:absolute;
  right:0;
  top:0;
  background-color:#ffffff;
  border-right:1px solid rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin-right:-1px;
  overflow-y:auto;
  z-index:1; }
  .bp3-dark .bp3-panel-stack-view{
    background-color:#30404d; }
  .bp3-panel-stack-view:nth-last-child(n + 4){
    display:none; }

.bp3-panel-stack-push .bp3-panel-stack-enter, .bp3-panel-stack-push .bp3-panel-stack-appear{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0; }

.bp3-panel-stack-push .bp3-panel-stack-enter-active, .bp3-panel-stack-push .bp3-panel-stack-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-push .bp3-panel-stack-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack-push .bp3-panel-stack-exit-active{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-pop .bp3-panel-stack-enter, .bp3-panel-stack-pop .bp3-panel-stack-appear{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0; }

.bp3-panel-stack-pop .bp3-panel-stack-enter-active, .bp3-panel-stack-pop .bp3-panel-stack-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-pop .bp3-panel-stack-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack-pop .bp3-panel-stack-exit-active{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }
.bp3-panel-stack2{
  overflow:hidden;
  position:relative; }

.bp3-panel-stack2-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  -webkit-box-shadow:0 1px rgba(16, 22, 26, 0.15);
          box-shadow:0 1px rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-negative:0;
      flex-shrink:0;
  height:30px;
  z-index:1; }
  .bp3-dark .bp3-panel-stack2-header{
    -webkit-box-shadow:0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 1px rgba(255, 255, 255, 0.15); }
  .bp3-panel-stack2-header > span{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1;
            flex:1; }
  .bp3-panel-stack2-header .bp3-heading{
    margin:0 5px; }

.bp3-button.bp3-panel-stack2-header-back{
  margin-left:5px;
  padding-left:0;
  white-space:nowrap; }
  .bp3-button.bp3-panel-stack2-header-back .bp3-icon{
    margin:0 2px; }

.bp3-panel-stack2-view{
  bottom:0;
  left:0;
  position:absolute;
  right:0;
  top:0;
  background-color:#ffffff;
  border-right:1px solid rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin-right:-1px;
  overflow-y:auto;
  z-index:1; }
  .bp3-dark .bp3-panel-stack2-view{
    background-color:#30404d; }
  .bp3-panel-stack2-view:nth-last-child(n + 4){
    display:none; }

.bp3-panel-stack2-push .bp3-panel-stack2-enter, .bp3-panel-stack2-push .bp3-panel-stack2-appear{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0; }

.bp3-panel-stack2-push .bp3-panel-stack2-enter-active, .bp3-panel-stack2-push .bp3-panel-stack2-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-push .bp3-panel-stack2-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack2-push .bp3-panel-stack2-exit-active{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-pop .bp3-panel-stack2-enter, .bp3-panel-stack2-pop .bp3-panel-stack2-appear{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0; }

.bp3-panel-stack2-pop .bp3-panel-stack2-enter-active, .bp3-panel-stack2-pop .bp3-panel-stack2-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-pop .bp3-panel-stack2-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack2-pop .bp3-panel-stack2-exit-active{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }
.bp3-popover{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  -webkit-transform:scale(1);
          transform:scale(1);
  border-radius:3px;
  display:inline-block;
  z-index:20; }
  .bp3-popover .bp3-popover-arrow{
    height:30px;
    position:absolute;
    width:30px; }
    .bp3-popover .bp3-popover-arrow::before{
      height:20px;
      margin:5px;
      width:20px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover{
    margin-bottom:17px;
    margin-top:-17px; }
    .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow{
      bottom:-11px; }
      .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(-90deg);
                transform:rotate(-90deg); }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover{
    margin-left:17px; }
    .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow{
      left:-11px; }
      .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(0);
                transform:rotate(0); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover{
    margin-top:17px; }
    .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow{
      top:-11px; }
      .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(90deg);
                transform:rotate(90deg); }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover{
    margin-left:-17px;
    margin-right:17px; }
    .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow{
      right:-11px; }
      .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(180deg);
                transform:rotate(180deg); }
  .bp3-tether-element-attached-middle > .bp3-popover > .bp3-popover-arrow{
    top:50%;
    -webkit-transform:translateY(-50%);
            transform:translateY(-50%); }
  .bp3-tether-element-attached-center > .bp3-popover > .bp3-popover-arrow{
    right:50%;
    -webkit-transform:translateX(50%);
            transform:translateX(50%); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow{
    top:-0.3934px; }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow{
    right:-0.3934px; }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow{
    left:-0.3934px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow{
    bottom:-0.3934px; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:top left;
            transform-origin:top left; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:top center;
            transform-origin:top center; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:top right;
            transform-origin:top right; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:center left;
            transform-origin:center left; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:center center;
            transform-origin:center center; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:center right;
            transform-origin:center right; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:bottom left;
            transform-origin:bottom left; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:bottom center;
            transform-origin:bottom center; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:bottom right;
            transform-origin:bottom right; }
  .bp3-popover .bp3-popover-content{
    background:#ffffff;
    color:inherit; }
  .bp3-popover .bp3-popover-arrow::before{
    -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2);
            box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2); }
  .bp3-popover .bp3-popover-arrow-border{
    fill:#10161a;
    fill-opacity:0.1; }
  .bp3-popover .bp3-popover-arrow-fill{
    fill:#ffffff; }
  .bp3-popover-enter > .bp3-popover, .bp3-popover-appear > .bp3-popover{
    -webkit-transform:scale(0.3);
            transform:scale(0.3); }
  .bp3-popover-enter-active > .bp3-popover, .bp3-popover-appear-active > .bp3-popover{
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-popover-exit > .bp3-popover{
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-popover-exit-active > .bp3-popover{
    -webkit-transform:scale(0.3);
            transform:scale(0.3);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-popover .bp3-popover-content{
    border-radius:3px;
    position:relative; }
  .bp3-popover.bp3-popover-content-sizing .bp3-popover-content{
    max-width:350px;
    padding:20px; }
  .bp3-popover-target + .bp3-overlay .bp3-popover.bp3-popover-content-sizing{
    width:350px; }
  .bp3-popover.bp3-minimal{
    margin:0 !important; }
    .bp3-popover.bp3-minimal .bp3-popover-arrow{
      display:none; }
    .bp3-popover.bp3-minimal.bp3-popover{
      -webkit-transform:scale(1);
              transform:scale(1); }
      .bp3-popover-enter > .bp3-popover.bp3-minimal.bp3-popover, .bp3-popover-appear > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1); }
      .bp3-popover-enter-active > .bp3-popover.bp3-minimal.bp3-popover, .bp3-popover-appear-active > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1);
        -webkit-transition-delay:0;
                transition-delay:0;
        -webkit-transition-duration:100ms;
                transition-duration:100ms;
        -webkit-transition-property:-webkit-transform;
        transition-property:-webkit-transform;
        transition-property:transform;
        transition-property:transform, -webkit-transform;
        -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
                transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
      .bp3-popover-exit > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1); }
      .bp3-popover-exit-active > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1);
        -webkit-transition-delay:0;
                transition-delay:0;
        -webkit-transition-duration:100ms;
                transition-duration:100ms;
        -webkit-transition-property:-webkit-transform;
        transition-property:-webkit-transform;
        transition-property:transform;
        transition-property:transform, -webkit-transform;
        -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
                transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-popover.bp3-dark,
  .bp3-dark .bp3-popover{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-popover.bp3-dark .bp3-popover-content,
    .bp3-dark .bp3-popover .bp3-popover-content{
      background:#30404d;
      color:inherit; }
    .bp3-popover.bp3-dark .bp3-popover-arrow::before,
    .bp3-dark .bp3-popover .bp3-popover-arrow::before{
      -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4);
              box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4); }
    .bp3-popover.bp3-dark .bp3-popover-arrow-border,
    .bp3-dark .bp3-popover .bp3-popover-arrow-border{
      fill:#10161a;
      fill-opacity:0.2; }
    .bp3-popover.bp3-dark .bp3-popover-arrow-fill,
    .bp3-dark .bp3-popover .bp3-popover-arrow-fill{
      fill:#30404d; }

.bp3-popover-arrow::before{
  border-radius:2px;
  content:"";
  display:block;
  position:absolute;
  -webkit-transform:rotate(45deg);
          transform:rotate(45deg); }

.bp3-tether-pinned .bp3-popover-arrow{
  display:none; }

.bp3-popover-backdrop{
  background:rgba(255, 255, 255, 0); }

.bp3-transition-container{
  opacity:1;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  z-index:20; }
  .bp3-transition-container.bp3-popover-enter, .bp3-transition-container.bp3-popover-appear{
    opacity:0; }
  .bp3-transition-container.bp3-popover-enter-active, .bp3-transition-container.bp3-popover-appear-active{
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-transition-container.bp3-popover-exit{
    opacity:1; }
  .bp3-transition-container.bp3-popover-exit-active{
    opacity:0;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-transition-container:focus{
    outline:none; }
  .bp3-transition-container.bp3-popover-leave .bp3-popover-content{
    pointer-events:none; }
  .bp3-transition-container[data-x-out-of-boundaries]{
    display:none; }

span.bp3-popover-target{
  display:inline-block; }

.bp3-popover-wrapper.bp3-fill{
  width:100%; }

.bp3-portal{
  left:0;
  position:absolute;
  right:0;
  top:0; }
@-webkit-keyframes linear-progress-bar-stripes{
  from{
    background-position:0 0; }
  to{
    background-position:30px 0; } }
@keyframes linear-progress-bar-stripes{
  from{
    background-position:0 0; }
  to{
    background-position:30px 0; } }

.bp3-progress-bar{
  background:rgba(92, 112, 128, 0.2);
  border-radius:40px;
  display:block;
  height:8px;
  overflow:hidden;
  position:relative;
  width:100%; }
  .bp3-progress-bar .bp3-progress-meter{
    background:linear-gradient(-45deg, rgba(255, 255, 255, 0.2) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.2) 50%, rgba(255, 255, 255, 0.2) 75%, transparent 75%);
    background-color:rgba(92, 112, 128, 0.8);
    background-size:30px 30px;
    border-radius:40px;
    height:100%;
    position:absolute;
    -webkit-transition:width 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:width 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    width:100%; }
  .bp3-progress-bar:not(.bp3-no-animation):not(.bp3-no-stripes) .bp3-progress-meter{
    animation:linear-progress-bar-stripes 300ms linear infinite reverse; }
  .bp3-progress-bar.bp3-no-stripes .bp3-progress-meter{
    background-image:none; }

.bp3-dark .bp3-progress-bar{
  background:rgba(16, 22, 26, 0.5); }
  .bp3-dark .bp3-progress-bar .bp3-progress-meter{
    background-color:#8a9ba8; }

.bp3-progress-bar.bp3-intent-primary .bp3-progress-meter{
  background-color:#137cbd; }

.bp3-progress-bar.bp3-intent-success .bp3-progress-meter{
  background-color:#0f9960; }

.bp3-progress-bar.bp3-intent-warning .bp3-progress-meter{
  background-color:#d9822b; }

.bp3-progress-bar.bp3-intent-danger .bp3-progress-meter{
  background-color:#db3737; }
@-webkit-keyframes skeleton-glow{
  from{
    background:rgba(206, 217, 224, 0.2);
    border-color:rgba(206, 217, 224, 0.2); }
  to{
    background:rgba(92, 112, 128, 0.2);
    border-color:rgba(92, 112, 128, 0.2); } }
@keyframes skeleton-glow{
  from{
    background:rgba(206, 217, 224, 0.2);
    border-color:rgba(206, 217, 224, 0.2); }
  to{
    background:rgba(92, 112, 128, 0.2);
    border-color:rgba(92, 112, 128, 0.2); } }
.bp3-skeleton{
  -webkit-animation:1000ms linear infinite alternate skeleton-glow;
          animation:1000ms linear infinite alternate skeleton-glow;
  background:rgba(206, 217, 224, 0.2);
  background-clip:padding-box !important;
  border-color:rgba(206, 217, 224, 0.2) !important;
  border-radius:2px;
  -webkit-box-shadow:none !important;
          box-shadow:none !important;
  color:transparent !important;
  cursor:default;
  pointer-events:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-skeleton::before, .bp3-skeleton::after,
  .bp3-skeleton *{
    visibility:hidden !important; }
.bp3-slider{
  height:40px;
  min-width:150px;
  width:100%;
  cursor:default;
  outline:none;
  position:relative;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-slider:hover{
    cursor:pointer; }
  .bp3-slider:active{
    cursor:-webkit-grabbing;
    cursor:grabbing; }
  .bp3-slider.bp3-disabled{
    cursor:not-allowed;
    opacity:0.5; }
  .bp3-slider.bp3-slider-unlabeled{
    height:16px; }

.bp3-slider-track,
.bp3-slider-progress{
  height:6px;
  left:0;
  right:0;
  top:5px;
  position:absolute; }

.bp3-slider-track{
  border-radius:3px;
  overflow:hidden; }

.bp3-slider-progress{
  background:rgba(92, 112, 128, 0.2); }
  .bp3-dark .bp3-slider-progress{
    background:rgba(16, 22, 26, 0.5); }
  .bp3-slider-progress.bp3-intent-primary{
    background-color:#137cbd; }
  .bp3-slider-progress.bp3-intent-success{
    background-color:#0f9960; }
  .bp3-slider-progress.bp3-intent-warning{
    background-color:#d9822b; }
  .bp3-slider-progress.bp3-intent-danger{
    background-color:#db3737; }

.bp3-slider-handle{
  background-color:#f5f8fa;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
  color:#182026;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
  cursor:pointer;
  height:16px;
  left:0;
  position:absolute;
  top:0;
  width:16px; }
  .bp3-slider-handle:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-slider-handle:active, .bp3-slider-handle.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-slider-handle:disabled, .bp3-slider-handle.bp3-disabled{
    background-color:rgba(206, 217, 224, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    outline:none; }
    .bp3-slider-handle:disabled.bp3-active, .bp3-slider-handle:disabled.bp3-active:hover, .bp3-slider-handle.bp3-disabled.bp3-active, .bp3-slider-handle.bp3-disabled.bp3-active:hover{
      background:rgba(206, 217, 224, 0.7); }
  .bp3-slider-handle:focus{
    z-index:1; }
  .bp3-slider-handle:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
    cursor:-webkit-grab;
    cursor:grab;
    z-index:2; }
  .bp3-slider-handle.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 1px rgba(16, 22, 26, 0.1);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 1px rgba(16, 22, 26, 0.1);
    cursor:-webkit-grabbing;
    cursor:grabbing; }
  .bp3-disabled .bp3-slider-handle{
    background:#bfccd6;
    -webkit-box-shadow:none;
            box-shadow:none;
    pointer-events:none; }
  .bp3-dark .bp3-slider-handle{
    background-color:#394b59;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-slider-handle:hover, .bp3-dark .bp3-slider-handle:active, .bp3-dark .bp3-slider-handle.bp3-active{
      color:#f5f8fa; }
    .bp3-dark .bp3-slider-handle:hover{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-slider-handle:active, .bp3-dark .bp3-slider-handle.bp3-active{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-slider-handle:disabled, .bp3-dark .bp3-slider-handle.bp3-disabled{
      background-color:rgba(57, 75, 89, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
      .bp3-dark .bp3-slider-handle:disabled.bp3-active, .bp3-dark .bp3-slider-handle.bp3-disabled.bp3-active{
        background:rgba(57, 75, 89, 0.7); }
    .bp3-dark .bp3-slider-handle .bp3-button-spinner .bp3-spinner-head{
      background:rgba(16, 22, 26, 0.5);
      stroke:#8a9ba8; }
    .bp3-dark .bp3-slider-handle, .bp3-dark .bp3-slider-handle:hover{
      background-color:#394b59; }
    .bp3-dark .bp3-slider-handle.bp3-active{
      background-color:#293742; }
  .bp3-dark .bp3-disabled .bp3-slider-handle{
    background:#5c7080;
    border-color:#5c7080;
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-slider-handle .bp3-slider-label{
    background:#394b59;
    border-radius:3px;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
    color:#f5f8fa;
    margin-left:8px; }
    .bp3-dark .bp3-slider-handle .bp3-slider-label{
      background:#e1e8ed;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
      color:#394b59; }
    .bp3-disabled .bp3-slider-handle .bp3-slider-label{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-slider-handle.bp3-start, .bp3-slider-handle.bp3-end{
    width:8px; }
  .bp3-slider-handle.bp3-start{
    border-bottom-right-radius:0;
    border-top-right-radius:0; }
  .bp3-slider-handle.bp3-end{
    border-bottom-left-radius:0;
    border-top-left-radius:0;
    margin-left:8px; }
    .bp3-slider-handle.bp3-end .bp3-slider-label{
      margin-left:0; }

.bp3-slider-label{
  -webkit-transform:translate(-50%, 20px);
          transform:translate(-50%, 20px);
  display:inline-block;
  font-size:12px;
  line-height:1;
  padding:2px 5px;
  position:absolute;
  vertical-align:top; }

.bp3-slider.bp3-vertical{
  height:150px;
  min-width:40px;
  width:40px; }
  .bp3-slider.bp3-vertical .bp3-slider-track,
  .bp3-slider.bp3-vertical .bp3-slider-progress{
    bottom:0;
    height:auto;
    left:5px;
    top:0;
    width:6px; }
  .bp3-slider.bp3-vertical .bp3-slider-progress{
    top:auto; }
  .bp3-slider.bp3-vertical .bp3-slider-label{
    -webkit-transform:translate(20px, 50%);
            transform:translate(20px, 50%); }
  .bp3-slider.bp3-vertical .bp3-slider-handle{
    top:auto; }
    .bp3-slider.bp3-vertical .bp3-slider-handle .bp3-slider-label{
      margin-left:0;
      margin-top:-8px; }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-end, .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start{
      height:8px;
      margin-left:0;
      width:16px; }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start{
      border-bottom-right-radius:3px;
      border-top-left-radius:0; }
      .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start .bp3-slider-label{
        -webkit-transform:translate(20px);
                transform:translate(20px); }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-end{
      border-bottom-left-radius:0;
      border-bottom-right-radius:0;
      border-top-left-radius:3px;
      margin-bottom:8px; }

@-webkit-keyframes pt-spinner-animation{
  from{
    -webkit-transform:rotate(0deg);
            transform:rotate(0deg); }
  to{
    -webkit-transform:rotate(360deg);
            transform:rotate(360deg); } }

@keyframes pt-spinner-animation{
  from{
    -webkit-transform:rotate(0deg);
            transform:rotate(0deg); }
  to{
    -webkit-transform:rotate(360deg);
            transform:rotate(360deg); } }

.bp3-spinner{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  overflow:visible;
  vertical-align:middle; }
  .bp3-spinner svg{
    display:block; }
  .bp3-spinner path{
    fill-opacity:0; }
  .bp3-spinner .bp3-spinner-head{
    stroke:rgba(92, 112, 128, 0.8);
    stroke-linecap:round;
    -webkit-transform-origin:center;
            transform-origin:center;
    -webkit-transition:stroke-dashoffset 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:stroke-dashoffset 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-spinner .bp3-spinner-track{
    stroke:rgba(92, 112, 128, 0.2); }

.bp3-spinner-animation{
  -webkit-animation:pt-spinner-animation 500ms linear infinite;
          animation:pt-spinner-animation 500ms linear infinite; }
  .bp3-no-spin > .bp3-spinner-animation{
    -webkit-animation:none;
            animation:none; }

.bp3-dark .bp3-spinner .bp3-spinner-head{
  stroke:#8a9ba8; }

.bp3-dark .bp3-spinner .bp3-spinner-track{
  stroke:rgba(16, 22, 26, 0.5); }

.bp3-spinner.bp3-intent-primary .bp3-spinner-head{
  stroke:#137cbd; }

.bp3-spinner.bp3-intent-success .bp3-spinner-head{
  stroke:#0f9960; }

.bp3-spinner.bp3-intent-warning .bp3-spinner-head{
  stroke:#d9822b; }

.bp3-spinner.bp3-intent-danger .bp3-spinner-head{
  stroke:#db3737; }
.bp3-tabs.bp3-vertical{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }
  .bp3-tabs.bp3-vertical > .bp3-tab-list{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start;
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column; }
    .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab{
      border-radius:3px;
      padding:0 10px;
      width:100%; }
      .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab[aria-selected="true"]{
        background-color:rgba(19, 124, 189, 0.2);
        -webkit-box-shadow:none;
                box-shadow:none; }
    .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab-indicator-wrapper .bp3-tab-indicator{
      background-color:rgba(19, 124, 189, 0.2);
      border-radius:3px;
      bottom:0;
      height:auto;
      left:0;
      right:0;
      top:0; }
  .bp3-tabs.bp3-vertical > .bp3-tab-panel{
    margin-top:0;
    padding-left:20px; }

.bp3-tab-list{
  -webkit-box-align:end;
      -ms-flex-align:end;
          align-items:flex-end;
  border:none;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  list-style:none;
  margin:0;
  padding:0;
  position:relative; }
  .bp3-tab-list > *:not(:last-child){
    margin-right:20px; }

.bp3-tab{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  color:#182026;
  cursor:pointer;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  font-size:14px;
  line-height:30px;
  max-width:100%;
  position:relative;
  vertical-align:top; }
  .bp3-tab a{
    color:inherit;
    display:block;
    text-decoration:none; }
  .bp3-tab-indicator-wrapper ~ .bp3-tab{
    background-color:transparent !important;
    -webkit-box-shadow:none !important;
            box-shadow:none !important; }
  .bp3-tab[aria-disabled="true"]{
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-tab[aria-selected="true"]{
    border-radius:0;
    -webkit-box-shadow:inset 0 -3px 0 #106ba3;
            box-shadow:inset 0 -3px 0 #106ba3; }
  .bp3-tab[aria-selected="true"], .bp3-tab:not([aria-disabled="true"]):hover{
    color:#106ba3; }
  .bp3-tab:focus{
    -moz-outline-radius:0; }
  .bp3-large > .bp3-tab{
    font-size:16px;
    line-height:40px; }

.bp3-tab-panel{
  margin-top:20px; }
  .bp3-tab-panel[aria-hidden="true"]{
    display:none; }

.bp3-tab-indicator-wrapper{
  left:0;
  pointer-events:none;
  position:absolute;
  top:0;
  -webkit-transform:translateX(0), translateY(0);
          transform:translateX(0), translateY(0);
  -webkit-transition:height, width, -webkit-transform;
  transition:height, width, -webkit-transform;
  transition:height, transform, width;
  transition:height, transform, width, -webkit-transform;
  -webkit-transition-duration:200ms;
          transition-duration:200ms;
  -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
          transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tab-indicator-wrapper .bp3-tab-indicator{
    background-color:#106ba3;
    bottom:0;
    height:3px;
    left:0;
    position:absolute;
    right:0; }
  .bp3-tab-indicator-wrapper.bp3-no-animation{
    -webkit-transition:none;
    transition:none; }

.bp3-dark .bp3-tab{
  color:#f5f8fa; }
  .bp3-dark .bp3-tab[aria-disabled="true"]{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-tab[aria-selected="true"]{
    -webkit-box-shadow:inset 0 -3px 0 #48aff0;
            box-shadow:inset 0 -3px 0 #48aff0; }
  .bp3-dark .bp3-tab[aria-selected="true"], .bp3-dark .bp3-tab:not([aria-disabled="true"]):hover{
    color:#48aff0; }

.bp3-dark .bp3-tab-indicator{
  background-color:#48aff0; }

.bp3-flex-expander{
  -webkit-box-flex:1;
      -ms-flex:1 1;
          flex:1 1; }
.bp3-tag{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:#5c7080;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:none;
          box-shadow:none;
  color:#f5f8fa;
  font-size:12px;
  line-height:16px;
  max-width:100%;
  min-height:20px;
  min-width:20px;
  padding:2px 6px;
  position:relative; }
  .bp3-tag.bp3-interactive{
    cursor:pointer; }
    .bp3-tag.bp3-interactive:hover{
      background-color:rgba(92, 112, 128, 0.85); }
    .bp3-tag.bp3-interactive.bp3-active, .bp3-tag.bp3-interactive:active{
      background-color:rgba(92, 112, 128, 0.7); }
  .bp3-tag > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-tag > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-tag::before,
  .bp3-tag > *{
    margin-right:4px; }
  .bp3-tag:empty::before,
  .bp3-tag > :last-child{
    margin-right:0; }
  .bp3-tag:focus{
    outline:rgba(19, 124, 189, 0.6) auto 2px;
    outline-offset:0;
    -moz-outline-radius:6px; }
  .bp3-tag.bp3-round{
    border-radius:30px;
    padding-left:8px;
    padding-right:8px; }
  .bp3-dark .bp3-tag{
    background-color:#bfccd6;
    color:#182026; }
    .bp3-dark .bp3-tag.bp3-interactive{
      cursor:pointer; }
      .bp3-dark .bp3-tag.bp3-interactive:hover{
        background-color:rgba(191, 204, 214, 0.85); }
      .bp3-dark .bp3-tag.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-interactive:active{
        background-color:rgba(191, 204, 214, 0.7); }
    .bp3-dark .bp3-tag > .bp3-icon, .bp3-dark .bp3-tag .bp3-icon-standard, .bp3-dark .bp3-tag .bp3-icon-large{
      fill:currentColor; }
  .bp3-tag > .bp3-icon, .bp3-tag .bp3-icon-standard, .bp3-tag .bp3-icon-large{
    fill:#ffffff; }
  .bp3-tag.bp3-large,
  .bp3-large .bp3-tag{
    font-size:14px;
    line-height:20px;
    min-height:30px;
    min-width:30px;
    padding:5px 10px; }
    .bp3-tag.bp3-large::before,
    .bp3-tag.bp3-large > *,
    .bp3-large .bp3-tag::before,
    .bp3-large .bp3-tag > *{
      margin-right:7px; }
    .bp3-tag.bp3-large:empty::before,
    .bp3-tag.bp3-large > :last-child,
    .bp3-large .bp3-tag:empty::before,
    .bp3-large .bp3-tag > :last-child{
      margin-right:0; }
    .bp3-tag.bp3-large.bp3-round,
    .bp3-large .bp3-tag.bp3-round{
      padding-left:12px;
      padding-right:12px; }
  .bp3-tag.bp3-intent-primary{
    background:#137cbd;
    color:#ffffff; }
    .bp3-tag.bp3-intent-primary.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-primary.bp3-interactive:hover{
        background-color:rgba(19, 124, 189, 0.85); }
      .bp3-tag.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-primary.bp3-interactive:active{
        background-color:rgba(19, 124, 189, 0.7); }
  .bp3-tag.bp3-intent-success{
    background:#0f9960;
    color:#ffffff; }
    .bp3-tag.bp3-intent-success.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-success.bp3-interactive:hover{
        background-color:rgba(15, 153, 96, 0.85); }
      .bp3-tag.bp3-intent-success.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-success.bp3-interactive:active{
        background-color:rgba(15, 153, 96, 0.7); }
  .bp3-tag.bp3-intent-warning{
    background:#d9822b;
    color:#ffffff; }
    .bp3-tag.bp3-intent-warning.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-warning.bp3-interactive:hover{
        background-color:rgba(217, 130, 43, 0.85); }
      .bp3-tag.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-warning.bp3-interactive:active{
        background-color:rgba(217, 130, 43, 0.7); }
  .bp3-tag.bp3-intent-danger{
    background:#db3737;
    color:#ffffff; }
    .bp3-tag.bp3-intent-danger.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-danger.bp3-interactive:hover{
        background-color:rgba(219, 55, 55, 0.85); }
      .bp3-tag.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-danger.bp3-interactive:active{
        background-color:rgba(219, 55, 55, 0.7); }
  .bp3-tag.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-tag.bp3-minimal > .bp3-icon, .bp3-tag.bp3-minimal .bp3-icon-standard, .bp3-tag.bp3-minimal .bp3-icon-large{
    fill:#5c7080; }
  .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]){
    background-color:rgba(138, 155, 168, 0.2);
    color:#182026; }
    .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:hover{
        background-color:rgba(92, 112, 128, 0.3); }
      .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive.bp3-active, .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:active{
        background-color:rgba(92, 112, 128, 0.4); }
    .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]){
      color:#f5f8fa; }
      .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:hover{
          background-color:rgba(191, 204, 214, 0.3); }
        .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:active{
          background-color:rgba(191, 204, 214, 0.4); }
      .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) > .bp3-icon, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) .bp3-icon-standard, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) .bp3-icon-large{
        fill:#a7b6c2; }
  .bp3-tag.bp3-minimal.bp3-intent-primary{
    background-color:rgba(19, 124, 189, 0.15);
    color:#106ba3; }
    .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:hover{
        background-color:rgba(19, 124, 189, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:active{
        background-color:rgba(19, 124, 189, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-primary > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-primary .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-primary .bp3-icon-large{
      fill:#137cbd; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary{
      background-color:rgba(19, 124, 189, 0.25);
      color:#48aff0; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:hover{
          background-color:rgba(19, 124, 189, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:active{
          background-color:rgba(19, 124, 189, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-success{
    background-color:rgba(15, 153, 96, 0.15);
    color:#0d8050; }
    .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:hover{
        background-color:rgba(15, 153, 96, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:active{
        background-color:rgba(15, 153, 96, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-success > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-success .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-success .bp3-icon-large{
      fill:#0f9960; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success{
      background-color:rgba(15, 153, 96, 0.25);
      color:#3dcc91; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:hover{
          background-color:rgba(15, 153, 96, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:active{
          background-color:rgba(15, 153, 96, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-warning{
    background-color:rgba(217, 130, 43, 0.15);
    color:#bf7326; }
    .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:hover{
        background-color:rgba(217, 130, 43, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:active{
        background-color:rgba(217, 130, 43, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-warning > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-warning .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-warning .bp3-icon-large{
      fill:#d9822b; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning{
      background-color:rgba(217, 130, 43, 0.25);
      color:#ffb366; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:hover{
          background-color:rgba(217, 130, 43, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:active{
          background-color:rgba(217, 130, 43, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-danger{
    background-color:rgba(219, 55, 55, 0.15);
    color:#c23030; }
    .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:hover{
        background-color:rgba(219, 55, 55, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:active{
        background-color:rgba(219, 55, 55, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-danger > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-danger .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-danger .bp3-icon-large{
      fill:#db3737; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger{
      background-color:rgba(219, 55, 55, 0.25);
      color:#ff7373; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:hover{
          background-color:rgba(219, 55, 55, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:active{
          background-color:rgba(219, 55, 55, 0.45); }

.bp3-tag-remove{
  background:none;
  border:none;
  color:inherit;
  cursor:pointer;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin-bottom:-2px;
  margin-right:-6px !important;
  margin-top:-2px;
  opacity:0.5;
  padding:2px;
  padding-left:0; }
  .bp3-tag-remove:hover{
    background:none;
    opacity:0.8;
    text-decoration:none; }
  .bp3-tag-remove:active{
    opacity:1; }
  .bp3-tag-remove:empty::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    content:""; }
  .bp3-large .bp3-tag-remove{
    margin-right:-10px !important;
    padding:0 5px 0 0; }
    .bp3-large .bp3-tag-remove:empty::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1; }
.bp3-tag-input{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  cursor:text;
  height:auto;
  line-height:inherit;
  min-height:30px;
  padding-left:5px;
  padding-right:0; }
  .bp3-tag-input > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-tag-input > .bp3-tag-input-values{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-tag-input .bp3-tag-input-icon{
    color:#5c7080;
    margin-left:2px;
    margin-right:7px;
    margin-top:7px; }
  .bp3-tag-input .bp3-tag-input-values{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-orient:horizontal;
    -webkit-box-direction:normal;
        -ms-flex-direction:row;
            flex-direction:row;
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    -ms-flex-item-align:stretch;
        align-self:stretch;
    -ms-flex-wrap:wrap;
        flex-wrap:wrap;
    margin-right:7px;
    margin-top:5px;
    min-width:0; }
    .bp3-tag-input .bp3-tag-input-values > *{
      -webkit-box-flex:0;
          -ms-flex-positive:0;
              flex-grow:0;
      -ms-flex-negative:0;
          flex-shrink:0; }
    .bp3-tag-input .bp3-tag-input-values > .bp3-fill{
      -webkit-box-flex:1;
          -ms-flex-positive:1;
              flex-grow:1;
      -ms-flex-negative:1;
          flex-shrink:1; }
    .bp3-tag-input .bp3-tag-input-values::before,
    .bp3-tag-input .bp3-tag-input-values > *{
      margin-right:5px; }
    .bp3-tag-input .bp3-tag-input-values:empty::before,
    .bp3-tag-input .bp3-tag-input-values > :last-child{
      margin-right:0; }
    .bp3-tag-input .bp3-tag-input-values:first-child .bp3-input-ghost:first-child{
      padding-left:5px; }
    .bp3-tag-input .bp3-tag-input-values > *{
      margin-bottom:5px; }
  .bp3-tag-input .bp3-tag{
    overflow-wrap:break-word; }
    .bp3-tag-input .bp3-tag.bp3-active{
      outline:rgba(19, 124, 189, 0.6) auto 2px;
      outline-offset:0;
      -moz-outline-radius:6px; }
  .bp3-tag-input .bp3-input-ghost{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:20px;
    width:80px; }
    .bp3-tag-input .bp3-input-ghost:disabled, .bp3-tag-input .bp3-input-ghost.bp3-disabled{
      cursor:not-allowed; }
  .bp3-tag-input .bp3-button,
  .bp3-tag-input .bp3-spinner{
    margin:3px;
    margin-left:0; }
  .bp3-tag-input .bp3-button{
    min-height:24px;
    min-width:24px;
    padding:0 7px; }
  .bp3-tag-input.bp3-large{
    height:auto;
    min-height:40px; }
    .bp3-tag-input.bp3-large::before,
    .bp3-tag-input.bp3-large > *{
      margin-right:10px; }
    .bp3-tag-input.bp3-large:empty::before,
    .bp3-tag-input.bp3-large > :last-child{
      margin-right:0; }
    .bp3-tag-input.bp3-large .bp3-tag-input-icon{
      margin-left:5px;
      margin-top:10px; }
    .bp3-tag-input.bp3-large .bp3-input-ghost{
      line-height:30px; }
    .bp3-tag-input.bp3-large .bp3-button{
      min-height:30px;
      min-width:30px;
      padding:5px 10px;
      margin:5px;
      margin-left:0; }
    .bp3-tag-input.bp3-large .bp3-spinner{
      margin:8px;
      margin-left:0; }
  .bp3-tag-input.bp3-active{
    background-color:#ffffff;
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-success{
      -webkit-box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-tag-input .bp3-tag-input-icon, .bp3-tag-input.bp3-dark .bp3-tag-input-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-tag-input .bp3-input-ghost, .bp3-tag-input.bp3-dark .bp3-input-ghost{
    color:#f5f8fa; }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-webkit-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-moz-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost:-ms-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-ms-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::placeholder{
      color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-tag-input.bp3-active, .bp3-tag-input.bp3-dark.bp3-active{
    background-color:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-primary, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-success, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-success{
      -webkit-box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-warning, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-danger, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-input-ghost{
  background:none;
  border:none;
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0; }
  .bp3-input-ghost::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost:focus{
    outline:none !important; }
.bp3-toast{
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin:20px 0 0;
  max-width:500px;
  min-width:300px;
  pointer-events:all;
  position:relative !important; }
  .bp3-toast.bp3-toast-enter, .bp3-toast.bp3-toast-appear{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px); }
  .bp3-toast.bp3-toast-enter-active, .bp3-toast.bp3-toast-appear-active{
    -webkit-transform:translateY(0);
            transform:translateY(0);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-toast.bp3-toast-enter ~ .bp3-toast, .bp3-toast.bp3-toast-appear ~ .bp3-toast{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px); }
  .bp3-toast.bp3-toast-enter-active ~ .bp3-toast, .bp3-toast.bp3-toast-appear-active ~ .bp3-toast{
    -webkit-transform:translateY(0);
            transform:translateY(0);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-toast.bp3-toast-exit{
    opacity:1;
    -webkit-filter:blur(0);
            filter:blur(0); }
  .bp3-toast.bp3-toast-exit-active{
    opacity:0;
    -webkit-filter:blur(10px);
            filter:blur(10px);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:opacity, filter;
    transition-property:opacity, filter, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-toast.bp3-toast-exit ~ .bp3-toast{
    -webkit-transform:translateY(0);
            transform:translateY(0); }
  .bp3-toast.bp3-toast-exit-active ~ .bp3-toast{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px);
    -webkit-transition-delay:50ms;
            transition-delay:50ms;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-toast .bp3-button-group{
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    padding:5px;
    padding-left:0; }
  .bp3-toast > .bp3-icon{
    color:#5c7080;
    margin:12px;
    margin-right:0; }
  .bp3-toast.bp3-dark,
  .bp3-dark .bp3-toast{
    background-color:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-toast.bp3-dark > .bp3-icon,
    .bp3-dark .bp3-toast > .bp3-icon{
      color:#a7b6c2; }
  .bp3-toast[class*="bp3-intent-"] a{
    color:rgba(255, 255, 255, 0.7); }
    .bp3-toast[class*="bp3-intent-"] a:hover{
      color:#ffffff; }
  .bp3-toast[class*="bp3-intent-"] > .bp3-icon{
    color:#ffffff; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button, .bp3-toast[class*="bp3-intent-"] .bp3-button::before,
  .bp3-toast[class*="bp3-intent-"] .bp3-button .bp3-icon, .bp3-toast[class*="bp3-intent-"] .bp3-button:active{
    color:rgba(255, 255, 255, 0.7) !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:focus{
    outline-color:rgba(255, 255, 255, 0.5); }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:hover{
    background-color:rgba(255, 255, 255, 0.15) !important;
    color:#ffffff !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:active{
    background-color:rgba(255, 255, 255, 0.3) !important;
    color:#ffffff !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button::after{
    background:rgba(255, 255, 255, 0.3) !important; }
  .bp3-toast.bp3-intent-primary{
    background-color:#137cbd;
    color:#ffffff; }
  .bp3-toast.bp3-intent-success{
    background-color:#0f9960;
    color:#ffffff; }
  .bp3-toast.bp3-intent-warning{
    background-color:#d9822b;
    color:#ffffff; }
  .bp3-toast.bp3-intent-danger{
    background-color:#db3737;
    color:#ffffff; }

.bp3-toast-message{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  padding:11px;
  word-break:break-word; }

.bp3-toast-container{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box !important;
  display:-ms-flexbox !important;
  display:flex !important;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  left:0;
  overflow:hidden;
  padding:0 20px 20px;
  pointer-events:none;
  right:0;
  z-index:40; }
  .bp3-toast-container.bp3-toast-container-in-portal{
    position:fixed; }
  .bp3-toast-container.bp3-toast-container-inline{
    position:absolute; }
  .bp3-toast-container.bp3-toast-container-top{
    top:0; }
  .bp3-toast-container.bp3-toast-container-bottom{
    bottom:0;
    -webkit-box-orient:vertical;
    -webkit-box-direction:reverse;
        -ms-flex-direction:column-reverse;
            flex-direction:column-reverse;
    top:auto; }
  .bp3-toast-container.bp3-toast-container-left{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start; }
  .bp3-toast-container.bp3-toast-container-right{
    -webkit-box-align:end;
        -ms-flex-align:end;
            align-items:flex-end; }

.bp3-toast-container-bottom .bp3-toast.bp3-toast-enter:not(.bp3-toast-enter-active),
.bp3-toast-container-bottom .bp3-toast.bp3-toast-enter:not(.bp3-toast-enter-active) ~ .bp3-toast, .bp3-toast-container-bottom .bp3-toast.bp3-toast-appear:not(.bp3-toast-appear-active),
.bp3-toast-container-bottom .bp3-toast.bp3-toast-appear:not(.bp3-toast-appear-active) ~ .bp3-toast,
.bp3-toast-container-bottom .bp3-toast.bp3-toast-exit-active ~ .bp3-toast,
.bp3-toast-container-bottom .bp3-toast.bp3-toast-leave-active ~ .bp3-toast{
  -webkit-transform:translateY(60px);
          transform:translateY(60px); }
.bp3-tooltip{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  -webkit-transform:scale(1);
          transform:scale(1); }
  .bp3-tooltip .bp3-popover-arrow{
    height:22px;
    position:absolute;
    width:22px; }
    .bp3-tooltip .bp3-popover-arrow::before{
      height:14px;
      margin:4px;
      width:14px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip{
    margin-bottom:11px;
    margin-top:-11px; }
    .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow{
      bottom:-8px; }
      .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(-90deg);
                transform:rotate(-90deg); }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip{
    margin-left:11px; }
    .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow{
      left:-8px; }
      .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(0);
                transform:rotate(0); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip{
    margin-top:11px; }
    .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow{
      top:-8px; }
      .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(90deg);
                transform:rotate(90deg); }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip{
    margin-left:-11px;
    margin-right:11px; }
    .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow{
      right:-8px; }
      .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(180deg);
                transform:rotate(180deg); }
  .bp3-tether-element-attached-middle > .bp3-tooltip > .bp3-popover-arrow{
    top:50%;
    -webkit-transform:translateY(-50%);
            transform:translateY(-50%); }
  .bp3-tether-element-attached-center > .bp3-tooltip > .bp3-popover-arrow{
    right:50%;
    -webkit-transform:translateX(50%);
            transform:translateX(50%); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow{
    top:-0.22183px; }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow{
    right:-0.22183px; }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow{
    left:-0.22183px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow{
    bottom:-0.22183px; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:top left;
            transform-origin:top left; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:top center;
            transform-origin:top center; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:top right;
            transform-origin:top right; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:center left;
            transform-origin:center left; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:center center;
            transform-origin:center center; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:center right;
            transform-origin:center right; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:bottom left;
            transform-origin:bottom left; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:bottom center;
            transform-origin:bottom center; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:bottom right;
            transform-origin:bottom right; }
  .bp3-tooltip .bp3-popover-content{
    background:#394b59;
    color:#f5f8fa; }
  .bp3-tooltip .bp3-popover-arrow::before{
    -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2);
            box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2); }
  .bp3-tooltip .bp3-popover-arrow-border{
    fill:#10161a;
    fill-opacity:0.1; }
  .bp3-tooltip .bp3-popover-arrow-fill{
    fill:#394b59; }
  .bp3-popover-enter > .bp3-tooltip, .bp3-popover-appear > .bp3-tooltip{
    -webkit-transform:scale(0.8);
            transform:scale(0.8); }
  .bp3-popover-enter-active > .bp3-tooltip, .bp3-popover-appear-active > .bp3-tooltip{
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-popover-exit > .bp3-tooltip{
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-popover-exit-active > .bp3-tooltip{
    -webkit-transform:scale(0.8);
            transform:scale(0.8);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tooltip .bp3-popover-content{
    padding:10px 12px; }
  .bp3-tooltip.bp3-dark,
  .bp3-dark .bp3-tooltip{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-tooltip.bp3-dark .bp3-popover-content,
    .bp3-dark .bp3-tooltip .bp3-popover-content{
      background:#e1e8ed;
      color:#394b59; }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow::before,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow::before{
      -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4);
              box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4); }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow-border,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow-border{
      fill:#10161a;
      fill-opacity:0.2; }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow-fill,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow-fill{
      fill:#e1e8ed; }
  .bp3-tooltip.bp3-intent-primary .bp3-popover-content{
    background:#137cbd;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-primary .bp3-popover-arrow-fill{
    fill:#137cbd; }
  .bp3-tooltip.bp3-intent-success .bp3-popover-content{
    background:#0f9960;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-success .bp3-popover-arrow-fill{
    fill:#0f9960; }
  .bp3-tooltip.bp3-intent-warning .bp3-popover-content{
    background:#d9822b;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-warning .bp3-popover-arrow-fill{
    fill:#d9822b; }
  .bp3-tooltip.bp3-intent-danger .bp3-popover-content{
    background:#db3737;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-danger .bp3-popover-arrow-fill{
    fill:#db3737; }

.bp3-tooltip-indicator{
  border-bottom:dotted 1px;
  cursor:help; }
.bp3-tree .bp3-icon, .bp3-tree .bp3-icon-standard, .bp3-tree .bp3-icon-large{
  color:#5c7080; }
  .bp3-tree .bp3-icon.bp3-intent-primary, .bp3-tree .bp3-icon-standard.bp3-intent-primary, .bp3-tree .bp3-icon-large.bp3-intent-primary{
    color:#137cbd; }
  .bp3-tree .bp3-icon.bp3-intent-success, .bp3-tree .bp3-icon-standard.bp3-intent-success, .bp3-tree .bp3-icon-large.bp3-intent-success{
    color:#0f9960; }
  .bp3-tree .bp3-icon.bp3-intent-warning, .bp3-tree .bp3-icon-standard.bp3-intent-warning, .bp3-tree .bp3-icon-large.bp3-intent-warning{
    color:#d9822b; }
  .bp3-tree .bp3-icon.bp3-intent-danger, .bp3-tree .bp3-icon-standard.bp3-intent-danger, .bp3-tree .bp3-icon-large.bp3-intent-danger{
    color:#db3737; }

.bp3-tree-node-list{
  list-style:none;
  margin:0;
  padding-left:0; }

.bp3-tree-root{
  background-color:transparent;
  cursor:default;
  padding-left:0;
  position:relative; }

.bp3-tree-node-content-0{
  padding-left:0px; }

.bp3-tree-node-content-1{
  padding-left:23px; }

.bp3-tree-node-content-2{
  padding-left:46px; }

.bp3-tree-node-content-3{
  padding-left:69px; }

.bp3-tree-node-content-4{
  padding-left:92px; }

.bp3-tree-node-content-5{
  padding-left:115px; }

.bp3-tree-node-content-6{
  padding-left:138px; }

.bp3-tree-node-content-7{
  padding-left:161px; }

.bp3-tree-node-content-8{
  padding-left:184px; }

.bp3-tree-node-content-9{
  padding-left:207px; }

.bp3-tree-node-content-10{
  padding-left:230px; }

.bp3-tree-node-content-11{
  padding-left:253px; }

.bp3-tree-node-content-12{
  padding-left:276px; }

.bp3-tree-node-content-13{
  padding-left:299px; }

.bp3-tree-node-content-14{
  padding-left:322px; }

.bp3-tree-node-content-15{
  padding-left:345px; }

.bp3-tree-node-content-16{
  padding-left:368px; }

.bp3-tree-node-content-17{
  padding-left:391px; }

.bp3-tree-node-content-18{
  padding-left:414px; }

.bp3-tree-node-content-19{
  padding-left:437px; }

.bp3-tree-node-content-20{
  padding-left:460px; }

.bp3-tree-node-content{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:30px;
  padding-right:5px;
  width:100%; }
  .bp3-tree-node-content:hover{
    background-color:rgba(191, 204, 214, 0.4); }

.bp3-tree-node-caret,
.bp3-tree-node-caret-none{
  min-width:30px; }

.bp3-tree-node-caret{
  color:#5c7080;
  cursor:pointer;
  padding:7px;
  -webkit-transform:rotate(0deg);
          transform:rotate(0deg);
  -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tree-node-caret:hover{
    color:#182026; }
  .bp3-dark .bp3-tree-node-caret{
    color:#a7b6c2; }
    .bp3-dark .bp3-tree-node-caret:hover{
      color:#f5f8fa; }
  .bp3-tree-node-caret.bp3-tree-node-caret-open{
    -webkit-transform:rotate(90deg);
            transform:rotate(90deg); }
  .bp3-tree-node-caret.bp3-icon-standard::before{
    content:""; }

.bp3-tree-node-icon{
  margin-right:7px;
  position:relative; }

.bp3-tree-node-label{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  position:relative;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-tree-node-label span{
    display:inline; }

.bp3-tree-node-secondary-label{
  padding:0 5px;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-tree-node-secondary-label .bp3-popover-wrapper,
  .bp3-tree-node-secondary-label .bp3-popover-target{
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex; }

.bp3-tree-node.bp3-disabled .bp3-tree-node-content{
  background-color:inherit;
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-tree-node.bp3-disabled .bp3-tree-node-caret,
.bp3-tree-node.bp3-disabled .bp3-tree-node-icon{
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content{
  background-color:#137cbd; }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content,
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon, .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon-standard, .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon-large{
    color:#ffffff; }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-tree-node-caret::before{
    color:rgba(255, 255, 255, 0.7); }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-tree-node-caret:hover::before{
    color:#ffffff; }

.bp3-dark .bp3-tree-node-content:hover{
  background-color:rgba(92, 112, 128, 0.3); }

.bp3-dark .bp3-tree .bp3-icon, .bp3-dark .bp3-tree .bp3-icon-standard, .bp3-dark .bp3-tree .bp3-icon-large{
  color:#a7b6c2; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-primary, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-primary, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-primary{
    color:#137cbd; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-success, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-success, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-success{
    color:#0f9960; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-warning, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-warning, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-warning{
    color:#d9822b; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-danger, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-danger, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-danger{
    color:#db3737; }

.bp3-dark .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content{
  background-color:#137cbd; }
.bp3-omnibar{
  -webkit-filter:blur(0);
          filter:blur(0);
  opacity:1;
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  left:calc(50% - 250px);
  top:20vh;
  width:500px;
  z-index:21; }
  .bp3-omnibar.bp3-overlay-enter, .bp3-omnibar.bp3-overlay-appear{
    -webkit-filter:blur(20px);
            filter:blur(20px);
    opacity:0.2; }
  .bp3-omnibar.bp3-overlay-enter-active, .bp3-omnibar.bp3-overlay-appear-active{
    -webkit-filter:blur(0);
            filter:blur(0);
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:filter, opacity;
    transition-property:filter, opacity, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-omnibar.bp3-overlay-exit{
    -webkit-filter:blur(0);
            filter:blur(0);
    opacity:1; }
  .bp3-omnibar.bp3-overlay-exit-active{
    -webkit-filter:blur(20px);
            filter:blur(20px);
    opacity:0.2;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:filter, opacity;
    transition-property:filter, opacity, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-omnibar .bp3-input{
    background-color:transparent;
    border-radius:0; }
    .bp3-omnibar .bp3-input, .bp3-omnibar .bp3-input:focus{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-omnibar .bp3-menu{
    background-color:transparent;
    border-radius:0;
    -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
    max-height:calc(60vh - 40px);
    overflow:auto; }
    .bp3-omnibar .bp3-menu:empty{
      display:none; }
  .bp3-dark .bp3-omnibar, .bp3-omnibar.bp3-dark{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4); }

.bp3-omnibar-overlay .bp3-overlay-backdrop{
  background-color:rgba(16, 22, 26, 0.2); }

.bp3-select-popover .bp3-popover-content{
  padding:5px; }

.bp3-select-popover .bp3-input-group{
  margin-bottom:0; }

.bp3-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto;
  padding:0; }
  .bp3-select-popover .bp3-menu:not(:first-child){
    padding-top:5px; }

.bp3-multi-select{
  min-width:150px; }

.bp3-multi-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto; }

.bp3-select-popover .bp3-popover-content{
  padding:5px; }

.bp3-select-popover .bp3-input-group{
  margin-bottom:0; }

.bp3-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto;
  padding:0; }
  .bp3-select-popover .bp3-menu:not(:first-child){
    padding-top:5px; }
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/* This file was auto-generated by ensureUiComponents() in @jupyterlab/buildutils */

/**
 * (DEPRECATED) Support for consuming icons as CSS background images
 */

/* Icons urls */

:root {
  --jp-icon-add: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE5IDEzaC02djZoLTJ2LTZINXYtMmg2VjVoMnY2aDZ2MnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-bug: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik0yMCA4aC0yLjgxYy0uNDUtLjc4LTEuMDctMS40NS0xLjgyLTEuOTZMMTcgNC40MSAxNS41OSAzbC0yLjE3IDIuMTdDMTIuOTYgNS4wNiAxMi40OSA1IDEyIDVjLS40OSAwLS45Ni4wNi0xLjQxLjE3TDguNDEgMyA3IDQuNDFsMS42MiAxLjYzQzcuODggNi41NSA3LjI2IDcuMjIgNi44MSA4SDR2MmgyLjA5Yy0uMDUuMzMtLjA5LjY2LS4wOSAxdjFINHYyaDJ2MWMwIC4zNC4wNC42Ny4wOSAxSDR2MmgyLjgxYzEuMDQgMS43OSAyLjk3IDMgNS4xOSAzczQuMTUtMS4yMSA1LjE5LTNIMjB2LTJoLTIuMDljLjA1LS4zMy4wOS0uNjYuMDktMXYtMWgydi0yaC0ydi0xYzAtLjM0LS4wNC0uNjctLjA5LTFIMjBWOHptLTYgOGgtNHYtMmg0djJ6bTAtNGgtNHYtMmg0djJ6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-build: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTYiIHZpZXdCb3g9IjAgMCAyNCAyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE0LjkgMTcuNDVDMTYuMjUgMTcuNDUgMTcuMzUgMTYuMzUgMTcuMzUgMTVDMTcuMzUgMTMuNjUgMTYuMjUgMTIuNTUgMTQuOSAxMi41NUMxMy41NCAxMi41NSAxMi40NSAxMy42NSAxMi40NSAxNUMxMi40NSAxNi4zNSAxMy41NCAxNy40NSAxNC45IDE3LjQ1Wk0yMC4xIDE1LjY4TDIxLjU4IDE2Ljg0QzIxLjcxIDE2Ljk1IDIxLjc1IDE3LjEzIDIxLjY2IDE3LjI5TDIwLjI2IDE5LjcxQzIwLjE3IDE5Ljg2IDIwIDE5LjkyIDE5LjgzIDE5Ljg2TDE4LjA5IDE5LjE2QzE3LjczIDE5LjQ0IDE3LjMzIDE5LjY3IDE2LjkxIDE5Ljg1TDE2LjY0IDIxLjdDMTYuNjIgMjEuODcgMTYuNDcgMjIgMTYuMyAyMkgxMy41QzEzLjMyIDIyIDEzLjE4IDIxLjg3IDEzLjE1IDIxLjdMMTIuODkgMTkuODVDMTIuNDYgMTkuNjcgMTIuMDcgMTkuNDQgMTEuNzEgMTkuMTZMOS45NjAwMiAxOS44NkM5LjgxMDAyIDE5LjkyIDkuNjIwMDIgMTkuODYgOS41NDAwMiAxOS43MUw4LjE0MDAyIDE3LjI5QzguMDUwMDIgMTcuMTMgOC4wOTAwMiAxNi45NSA4LjIyMDAyIDE2Ljg0TDkuNzAwMDIgMTUuNjhMOS42NTAwMSAxNUw5LjcwMDAyIDE0LjMxTDguMjIwMDIgMTMuMTZDOC4wOTAwMiAxMy4wNSA4LjA1MDAyIDEyLjg2IDguMTQwMDIgMTIuNzFMOS41NDAwMiAxMC4yOUM5LjYyMDAyIDEwLjEzIDkuODEwMDIgMTAuMDcgOS45NjAwMiAxMC4xM0wxMS43MSAxMC44NEMxMi4wNyAxMC41NiAxMi40NiAxMC4zMiAxMi44OSAxMC4xNUwxMy4xNSA4LjI4OTk4QzEzLjE4IDguMTI5OTggMTMuMzIgNy45OTk5OCAxMy41IDcuOTk5OThIMTYuM0MxNi40NyA3Ljk5OTk4IDE2LjYyIDguMTI5OTggMTYuNjQgOC4yODk5OEwxNi45MSAxMC4xNUMxNy4zMyAxMC4zMiAxNy43MyAxMC41NiAxOC4wOSAxMC44NEwxOS44MyAxMC4xM0MyMCAxMC4wNyAyMC4xNyAxMC4xMyAyMC4yNiAxMC4yOUwyMS42NiAxMi43MUMyMS43NSAxMi44NiAyMS43MSAxMy4wNSAyMS41OCAxMy4xNkwyMC4xIDE0LjMxTDIwLjE1IDE1TDIwLjEgMTUuNjhaIi8+CiAgICA8cGF0aCBkPSJNNy4zMjk2NiA3LjQ0NDU0QzguMDgzMSA3LjAwOTU0IDguMzM5MzIgNi4wNTMzMiA3LjkwNDMyIDUuMjk5ODhDNy40NjkzMiA0LjU0NjQzIDYuNTA4MSA0LjI4MTU2IDUuNzU0NjYgNC43MTY1NkM1LjM5MTc2IDQuOTI2MDggNS4xMjY5NSA1LjI3MTE4IDUuMDE4NDkgNS42NzU5NEM0LjkxMDA0IDYuMDgwNzEgNC45NjY4MiA2LjUxMTk4IDUuMTc2MzQgNi44NzQ4OEM1LjYxMTM0IDcuNjI4MzIgNi41NzYyMiA3Ljg3OTU0IDcuMzI5NjYgNy40NDQ1NFpNOS42NTcxOCA0Ljc5NTkzTDEwLjg2NzIgNC45NTE3OUMxMC45NjI4IDQuOTc3NDEgMTEuMDQwMiA1LjA3MTMzIDExLjAzODIgNS4xODc5M0wxMS4wMzg4IDYuOTg4OTNDMTEuMDQ1NSA3LjEwMDU0IDEwLjk2MTYgNy4xOTUxOCAxMC44NTUgNy4yMTA1NEw5LjY2MDAxIDcuMzgwODNMOS4yMzkxNSA4LjEzMTg4TDkuNjY5NjEgOS4yNTc0NUM5LjcwNzI5IDkuMzYyNzEgOS42NjkzNCA5LjQ3Njk5IDkuNTc0MDggOS41MzE5OUw4LjAxNTIzIDEwLjQzMkM3LjkxMTMxIDEwLjQ5MiA3Ljc5MzM3IDEwLjQ2NzcgNy43MjEwNSAxMC4zODI0TDYuOTg3NDggOS40MzE4OEw2LjEwOTMxIDkuNDMwODNMNS4zNDcwNCAxMC4zOTA1QzUuMjg5MDkgMTAuNDcwMiA1LjE3MzgzIDEwLjQ5MDUgNS4wNzE4NyAxMC40MzM5TDMuNTEyNDUgOS41MzI5M0MzLjQxMDQ5IDkuNDc2MzMgMy4zNzY0NyA5LjM1NzQxIDMuNDEwNzUgOS4yNTY3OUwzLjg2MzQ3IDguMTQwOTNMMy42MTc0OSA3Ljc3NDg4TDMuNDIzNDcgNy4zNzg4M0wyLjIzMDc1IDcuMjEyOTdDMi4xMjY0NyA3LjE5MjM1IDIuMDQwNDkgNy4xMDM0MiAyLjA0MjQ1IDYuOTg2ODJMMi4wNDE4NyA1LjE4NTgyQzIuMDQzODMgNS4wNjkyMiAyLjExOTA5IDQuOTc5NTggMi4yMTcwNCA0Ljk2OTIyTDMuNDIwNjUgNC43OTM5M0wzLjg2NzQ5IDQuMDI3ODhMMy40MTEwNSAyLjkxNzMxQzMuMzczMzcgMi44MTIwNCAzLjQxMTMxIDIuNjk3NzYgMy41MTUyMyAyLjYzNzc2TDUuMDc0MDggMS43Mzc3NkM1LjE2OTM0IDEuNjgyNzYgNS4yODcyOSAxLjcwNzA0IDUuMzU5NjEgMS43OTIzMUw2LjExOTE1IDIuNzI3ODhMNi45ODAwMSAyLjczODkzTDcuNzI0OTYgMS43ODkyMkM3Ljc5MTU2IDEuNzA0NTggNy45MTU0OCAxLjY3OTIyIDguMDA4NzkgMS43NDA4Mkw5LjU2ODIxIDIuNjQxODJDOS42NzAxNyAyLjY5ODQyIDkuNzEyODUgMi44MTIzNCA5LjY4NzIzIDIuOTA3OTdMOS4yMTcxOCA0LjAzMzgzTDkuNDYzMTYgNC4zOTk4OEw5LjY1NzE4IDQuNzk1OTNaIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down-empty-thin: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwb2x5Z29uIGNsYXNzPSJzdDEiIHBvaW50cz0iOS45LDEzLjYgMy42LDcuNCA0LjQsNi42IDkuOSwxMi4yIDE1LjQsNi43IDE2LjEsNy40ICIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down-empty: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik01LjIsNS45TDksOS43bDMuOC0zLjhsMS4yLDEuMmwtNC45LDVsLTQuOS01TDUuMiw1Ljl6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik01LjIsNy41TDksMTEuMmwzLjgtMy44SDUuMnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-caret-left: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwYXRoIGQ9Ik0xMC44LDEyLjhMNy4xLDlsMy44LTMuOGwwLDcuNkgxMC44eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-caret-right: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik03LjIsNS4yTDEwLjksOWwtMy44LDMuOFY1LjJINy4yeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-caret-up-empty-thin: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwb2x5Z29uIGNsYXNzPSJzdDEiIHBvaW50cz0iMTUuNCwxMy4zIDkuOSw3LjcgNC40LDEzLjIgMy42LDEyLjUgOS45LDYuMyAxNi4xLDEyLjYgIi8+Cgk8L2c+Cjwvc3ZnPgo=);
  --jp-icon-caret-up: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwYXRoIGQ9Ik01LjIsMTAuNUw5LDYuOGwzLjgsMy44SDUuMnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-case-sensitive: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0MTQxNDEiPgogICAgPHJlY3QgeD0iMiIgeT0iMiIgd2lkdGg9IjE2IiBoZWlnaHQ9IjE2Ii8+CiAgPC9nPgogIDxnIGNsYXNzPSJqcC1pY29uLWFjY2VudDIiIGZpbGw9IiNGRkYiPgogICAgPHBhdGggZD0iTTcuNiw4aDAuOWwzLjUsOGgtMS4xTDEwLDE0SDZsLTAuOSwySDRMNy42LDh6IE04LDkuMUw2LjQsMTNoMy4yTDgsOS4xeiIvPgogICAgPHBhdGggZD0iTTE2LjYsOS44Yy0wLjIsMC4xLTAuNCwwLjEtMC43LDAuMWMtMC4yLDAtMC40LTAuMS0wLjYtMC4yYy0wLjEtMC4xLTAuMi0wLjQtMC4yLTAuNyBjLTAuMywwLjMtMC42LDAuNS0wLjksMC43Yy0wLjMsMC4xLTAuNywwLjItMS4xLDAuMmMtMC4zLDAtMC41LDAtMC43LTAuMWMtMC4yLTAuMS0wLjQtMC4yLTAuNi0wLjNjLTAuMi0wLjEtMC4zLTAuMy0wLjQtMC41IGMtMC4xLTAuMi0wLjEtMC40LTAuMS0wLjdjMC0wLjMsMC4xLTAuNiwwLjItMC44YzAuMS0wLjIsMC4zLTAuNCwwLjQtMC41QzEyLDcsMTIuMiw2LjksMTIuNSw2LjhjMC4yLTAuMSwwLjUtMC4xLDAuNy0wLjIgYzAuMy0wLjEsMC41LTAuMSwwLjctMC4xYzAuMiwwLDAuNC0wLjEsMC42LTAuMWMwLjIsMCwwLjMtMC4xLDAuNC0wLjJjMC4xLTAuMSwwLjItMC4yLDAuMi0wLjRjMC0xLTEuMS0xLTEuMy0xIGMtMC40LDAtMS40LDAtMS40LDEuMmgtMC45YzAtMC40LDAuMS0wLjcsMC4yLTFjMC4xLTAuMiwwLjMtMC40LDAuNS0wLjZjMC4yLTAuMiwwLjUtMC4zLDAuOC0wLjNDMTMuMyw0LDEzLjYsNCwxMy45LDQgYzAuMywwLDAuNSwwLDAuOCwwLjFjMC4zLDAsMC41LDAuMSwwLjcsMC4yYzAuMiwwLjEsMC40LDAuMywwLjUsMC41QzE2LDUsMTYsNS4yLDE2LDUuNnYyLjljMCwwLjIsMCwwLjQsMCwwLjUgYzAsMC4xLDAuMSwwLjIsMC4zLDAuMmMwLjEsMCwwLjIsMCwwLjMsMFY5Ljh6IE0xNS4yLDYuOWMtMS4yLDAuNi0zLjEsMC4yLTMuMSwxLjRjMCwxLjQsMy4xLDEsMy4xLTAuNVY2Ljl6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-check: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik05IDE2LjE3TDQuODMgMTJsLTEuNDIgMS40MUw5IDE5IDIxIDdsLTEuNDEtMS40MXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-circle-empty: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyIDJDNi40NyAyIDIgNi40NyAyIDEyczQuNDcgMTAgMTAgMTAgMTAtNC40NyAxMC0xMFMxNy41MyAyIDEyIDJ6bTAgMThjLTQuNDEgMC04LTMuNTktOC04czMuNTktOCA4LTggOCAzLjU5IDggOC0zLjU5IDgtOCA4eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-circle: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPGNpcmNsZSBjeD0iOSIgY3k9IjkiIHI9IjgiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-clear: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8bWFzayBpZD0iZG9udXRIb2xlIj4KICAgIDxyZWN0IHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgZmlsbD0id2hpdGUiIC8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSI4IiBmaWxsPSJibGFjayIvPgogIDwvbWFzaz4KCiAgPGcgY2xhc3M9ImpwLWljb24zIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxyZWN0IGhlaWdodD0iMTgiIHdpZHRoPSIyIiB4PSIxMSIgeT0iMyIgdHJhbnNmb3JtPSJyb3RhdGUoMzE1LCAxMiwgMTIpIi8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIxMCIgbWFzaz0idXJsKCNkb251dEhvbGUpIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-close: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1ub25lIGpwLWljb24tc2VsZWN0YWJsZS1pbnZlcnNlIGpwLWljb24zLWhvdmVyIiBmaWxsPSJub25lIj4KICAgIDxjaXJjbGUgY3g9IjEyIiBjeT0iMTIiIHI9IjExIi8+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIGpwLWljb24tYWNjZW50Mi1ob3ZlciIgZmlsbD0iIzYxNjE2MSI+CiAgICA8cGF0aCBkPSJNMTkgNi40MUwxNy41OSA1IDEyIDEwLjU5IDYuNDEgNSA1IDYuNDEgMTAuNTkgMTIgNSAxNy41OSA2LjQxIDE5IDEyIDEzLjQxIDE3LjU5IDE5IDE5IDE3LjU5IDEzLjQxIDEyeiIvPgogIDwvZz4KCiAgPGcgY2xhc3M9ImpwLWljb24tbm9uZSBqcC1pY29uLWJ1c3kiIGZpbGw9Im5vbmUiPgogICAgPGNpcmNsZSBjeD0iMTIiIGN5PSIxMiIgcj0iNyIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-code: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyOCAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTExLjQgMTguNkw2LjggMTRMMTEuNCA5LjRMMTAgOEw0IDE0TDEwIDIwTDExLjQgMTguNlpNMTYuNiAxOC42TDIxLjIgMTRMMTYuNiA5LjRMMTggOEwyNCAxNEwxOCAyMEwxNi42IDE4LjZWMTguNloiLz4KCTwvZz4KPC9zdmc+Cg==);
  --jp-icon-console: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwMCAyMDAiPgogIDxnIGNsYXNzPSJqcC1pY29uLWJyYW5kMSBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiMwMjg4RDEiPgogICAgPHBhdGggZD0iTTIwIDE5LjhoMTYwdjE1OS45SDIweiIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNmZmYiPgogICAgPHBhdGggZD0iTTEwNSAxMjcuM2g0MHYxMi44aC00MHpNNTEuMSA3N0w3NCA5OS45bC0yMy4zIDIzLjMgMTAuNSAxMC41IDIzLjMtMjMuM0w5NSA5OS45IDg0LjUgODkuNCA2MS42IDY2LjV6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-copy: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTExLjksMUgzLjJDMi40LDEsMS43LDEuNywxLjcsMi41djEwLjJoMS41VjIuNWg4LjdWMXogTTE0LjEsMy45aC04Yy0wLjgsMC0xLjUsMC43LTEuNSwxLjV2MTAuMmMwLDAuOCwwLjcsMS41LDEuNSwxLjVoOCBjMC44LDAsMS41LTAuNywxLjUtMS41VjUuNEMxNS41LDQuNiwxNC45LDMuOSwxNC4xLDMuOXogTTE0LjEsMTUuNWgtOFY1LjRoOFYxNS41eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-copyright: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcgMCAwIDI0IDI0IiBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCI+CiAgPGcgY2xhc3M9ImpwLWljb24zIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik0xMS44OCw5LjE0YzEuMjgsMC4wNiwxLjYxLDEuMTUsMS42MywxLjY2aDEuNzljLTAuMDgtMS45OC0xLjQ5LTMuMTktMy40NS0zLjE5QzkuNjQsNy42MSw4LDksOCwxMi4xNCBjMCwxLjk0LDAuOTMsNC4yNCwzLjg0LDQuMjRjMi4yMiwwLDMuNDEtMS42NSwzLjQ0LTIuOTVoLTEuNzljLTAuMDMsMC41OS0wLjQ1LDEuMzgtMS42MywxLjQ0QzEwLjU1LDE0LjgzLDEwLDEzLjgxLDEwLDEyLjE0IEMxMCw5LjI1LDExLjI4LDkuMTYsMTEuODgsOS4xNHogTTEyLDJDNi40OCwyLDIsNi40OCwyLDEyczQuNDgsMTAsMTAsMTBzMTAtNC40OCwxMC0xMFMxNy41MiwyLDEyLDJ6IE0xMiwyMGMtNC40MSwwLTgtMy41OS04LTggczMuNTktOCw4LThzOCwzLjU5LDgsOFMxNi40MSwyMCwxMiwyMHoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-cut: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTkuNjQgNy42NGMuMjMtLjUuMzYtMS4wNS4zNi0xLjY0IDAtMi4yMS0xLjc5LTQtNC00UzIgMy43OSAyIDZzMS43OSA0IDQgNGMuNTkgMCAxLjE0LS4xMyAxLjY0LS4zNkwxMCAxMmwtMi4zNiAyLjM2QzcuMTQgMTQuMTMgNi41OSAxNCA2IDE0Yy0yLjIxIDAtNCAxLjc5LTQgNHMxLjc5IDQgNCA0IDQtMS43OSA0LTRjMC0uNTktLjEzLTEuMTQtLjM2LTEuNjRMMTIgMTRsNyA3aDN2LTFMOS42NCA3LjY0ek02IDhjLTEuMSAwLTItLjg5LTItMnMuOS0yIDItMiAyIC44OSAyIDItLjkgMi0yIDJ6bTAgMTJjLTEuMSAwLTItLjg5LTItMnMuOS0yIDItMiAyIC44OSAyIDItLjkgMi0yIDJ6bTYtNy41Yy0uMjggMC0uNS0uMjItLjUtLjVzLjIyLS41LjUtLjUuNS4yMi41LjUtLjIyLjUtLjUuNXpNMTkgM2wtNiA2IDIgMiA3LTdWM3oiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-download: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE5IDloLTRWM0g5djZINWw3IDcgNy03ek01IDE4djJoMTR2LTJINXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-edit: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTMgMTcuMjVWMjFoMy43NUwxNy44MSA5Ljk0bC0zLjc1LTMuNzVMMyAxNy4yNXpNMjAuNzEgNy4wNGMuMzktLjM5LjM5LTEuMDIgMC0xLjQxbC0yLjM0LTIuMzRjLS4zOS0uMzktMS4wMi0uMzktMS40MSAwbC0xLjgzIDEuODMgMy43NSAzLjc1IDEuODMtMS44M3oiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-ellipses: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPGNpcmNsZSBjeD0iNSIgY3k9IjEyIiByPSIyIi8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIyIi8+CiAgICA8Y2lyY2xlIGN4PSIxOSIgY3k9IjEyIiByPSIyIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-extension: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIwLjUgMTFIMTlWN2MwLTEuMS0uOS0yLTItMmgtNFYzLjVDMTMgMi4xMiAxMS44OCAxIDEwLjUgMVM4IDIuMTIgOCAzLjVWNUg0Yy0xLjEgMC0xLjk5LjktMS45OSAydjMuOEgzLjVjMS40OSAwIDIuNyAxLjIxIDIuNyAyLjdzLTEuMjEgMi43LTIuNyAyLjdIMlYyMGMwIDEuMS45IDIgMiAyaDMuOHYtMS41YzAtMS40OSAxLjIxLTIuNyAyLjctMi43IDEuNDkgMCAyLjcgMS4yMSAyLjcgMi43VjIySDE3YzEuMSAwIDItLjkgMi0ydi00aDEuNWMxLjM4IDAgMi41LTEuMTIgMi41LTIuNVMyMS44OCAxMSAyMC41IDExeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-fast-forward: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTQgMThsOC41LTZMNCA2djEyem05LTEydjEybDguNS02TDEzIDZ6Ii8+CiAgICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-file-upload: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTkgMTZoNnYtNmg0bC03LTctNyA3aDR6bS00IDJoMTR2Mkg1eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-file: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkuMyA4LjJsLTUuNS01LjVjLS4zLS4zLS43LS41LTEuMi0uNUgzLjljLS44LjEtMS42LjktMS42IDEuOHYxNC4xYzAgLjkuNyAxLjYgMS42IDEuNmgxNC4yYy45IDAgMS42LS43IDEuNi0xLjZWOS40Yy4xLS41LS4xLS45LS40LTEuMnptLTUuOC0zLjNsMy40IDMuNmgtMy40VjQuOXptMy45IDEyLjdINC43Yy0uMSAwLS4yIDAtLjItLjJWNC43YzAtLjIuMS0uMy4yLS4zaDcuMnY0LjRzMCAuOC4zIDEuMWMuMy4zIDEuMS4zIDEuMS4zaDQuM3Y3LjJzLS4xLjItLjIuMnoiLz4KPC9zdmc+Cg==);
  --jp-icon-filter-list: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEwIDE4aDR2LTJoLTR2MnpNMyA2djJoMThWNkgzem0zIDdoMTJ2LTJINnYyeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-folder: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTAgNEg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMThjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY4YzAtMS4xLS45LTItMi0yaC04bC0yLTJ6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-html5: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDUxMiA1MTIiPgogIDxwYXRoIGNsYXNzPSJqcC1pY29uMCBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiMwMDAiIGQ9Ik0xMDguNCAwaDIzdjIyLjhoMjEuMlYwaDIzdjY5aC0yM1Y0NmgtMjF2MjNoLTIzLjJNMjA2IDIzaC0yMC4zVjBoNjMuN3YyM0gyMjl2NDZoLTIzbTUzLjUtNjloMjQuMWwxNC44IDI0LjNMMzEzLjIgMGgyNC4xdjY5aC0yM1YzNC44bC0xNi4xIDI0LjgtMTYuMS0yNC44VjY5aC0yMi42bTg5LjItNjloMjN2NDYuMmgzMi42VjY5aC01NS42Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iI2U0NGQyNiIgZD0iTTEwNy42IDQ3MWwtMzMtMzcwLjRoMzYyLjhsLTMzIDM3MC4yTDI1NS43IDUxMiIvPgogIDxwYXRoIGNsYXNzPSJqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNmMTY1MjkiIGQ9Ik0yNTYgNDgwLjVWMTMxaDE0OC4zTDM3NiA0NDciLz4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNlYmViZWIiIGQ9Ik0xNDIgMTc2LjNoMTE0djQ1LjRoLTY0LjJsNC4yIDQ2LjVoNjB2NDUuM0gxNTQuNG0yIDIyLjhIMjAybDMuMiAzNi4zIDUwLjggMTMuNnY0Ny40bC05My4yLTI2Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZS1pbnZlcnNlIiBmaWxsPSIjZmZmIiBkPSJNMzY5LjYgMTc2LjNIMjU1Ljh2NDUuNGgxMDkuNm0tNC4xIDQ2LjVIMjU1Ljh2NDUuNGg1NmwtNS4zIDU5LTUwLjcgMTMuNnY0Ny4ybDkzLTI1LjgiLz4KPC9zdmc+Cg==);
  --jp-icon-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1icmFuZDQganAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNGRkYiIGQ9Ik0yLjIgMi4yaDE3LjV2MTcuNUgyLjJ6Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzNGNTFCNSIgZD0iTTIuMiAyLjJ2MTcuNWgxNy41bC4xLTE3LjVIMi4yem0xMi4xIDIuMmMxLjIgMCAyLjIgMSAyLjIgMi4ycy0xIDIuMi0yLjIgMi4yLTIuMi0xLTIuMi0yLjIgMS0yLjIgMi4yLTIuMnpNNC40IDE3LjZsMy4zLTguOCAzLjMgNi42IDIuMi0zLjIgNC40IDUuNEg0LjR6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-inspector: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMjAgNEg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMThjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY2YzAtMS4xLS45LTItMi0yem0tNSAxNEg0di00aDExdjR6bTAtNUg0VjloMTF2NHptNSA1aC00VjloNHY5eiIvPgo8L3N2Zz4K);
  --jp-icon-json: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMSBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNGOUE4MjUiPgogICAgPHBhdGggZD0iTTIwLjIgMTEuOGMtMS42IDAtMS43LjUtMS43IDEgMCAuNC4xLjkuMSAxLjMuMS41LjEuOS4xIDEuMyAwIDEuNy0xLjQgMi4zLTMuNSAyLjNoLS45di0xLjloLjVjMS4xIDAgMS40IDAgMS40LS44IDAtLjMgMC0uNi0uMS0xIDAtLjQtLjEtLjgtLjEtMS4yIDAtMS4zIDAtMS44IDEuMy0yLTEuMy0uMi0xLjMtLjctMS4zLTIgMC0uNC4xLS44LjEtMS4yLjEtLjQuMS0uNy4xLTEgMC0uOC0uNC0uNy0xLjQtLjhoLS41VjQuMWguOWMyLjIgMCAzLjUuNyAzLjUgMi4zIDAgLjQtLjEuOS0uMSAxLjMtLjEuNS0uMS45LS4xIDEuMyAwIC41LjIgMSAxLjcgMXYxLjh6TTEuOCAxMC4xYzEuNiAwIDEuNy0uNSAxLjctMSAwLS40LS4xLS45LS4xLTEuMy0uMS0uNS0uMS0uOS0uMS0xLjMgMC0xLjYgMS40LTIuMyAzLjUtMi4zaC45djEuOWgtLjVjLTEgMC0xLjQgMC0xLjQuOCAwIC4zIDAgLjYuMSAxIDAgLjIuMS42LjEgMSAwIDEuMyAwIDEuOC0xLjMgMkM2IDExLjIgNiAxMS43IDYgMTNjMCAuNC0uMS44LS4xIDEuMi0uMS4zLS4xLjctLjEgMSAwIC44LjMuOCAxLjQuOGguNXYxLjloLS45Yy0yLjEgMC0zLjUtLjYtMy41LTIuMyAwLS40LjEtLjkuMS0xLjMuMS0uNS4xLS45LjEtMS4zIDAtLjUtLjItMS0xLjctMXYtMS45eiIvPgogICAgPGNpcmNsZSBjeD0iMTEiIGN5PSIxMy44IiByPSIyLjEiLz4KICAgIDxjaXJjbGUgY3g9IjExIiBjeT0iOC4yIiByPSIyLjEiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-julia: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDMyNSAzMDAiPgogIDxnIGNsYXNzPSJqcC1icmFuZDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjY2IzYzMzIj4KICAgIDxwYXRoIGQ9Ik0gMTUwLjg5ODQzOCAyMjUgQyAxNTAuODk4NDM4IDI2Ni40MjE4NzUgMTE3LjMyMDMxMiAzMDAgNzUuODk4NDM4IDMwMCBDIDM0LjQ3NjU2MiAzMDAgMC44OTg0MzggMjY2LjQyMTg3NSAwLjg5ODQzOCAyMjUgQyAwLjg5ODQzOCAxODMuNTc4MTI1IDM0LjQ3NjU2MiAxNTAgNzUuODk4NDM4IDE1MCBDIDExNy4zMjAzMTIgMTUwIDE1MC44OTg0MzggMTgzLjU3ODEyNSAxNTAuODk4NDM4IDIyNSIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzM4OTgyNiI+CiAgICA8cGF0aCBkPSJNIDIzNy41IDc1IEMgMjM3LjUgMTE2LjQyMTg3NSAyMDMuOTIxODc1IDE1MCAxNjIuNSAxNTAgQyAxMjEuMDc4MTI1IDE1MCA4Ny41IDExNi40MjE4NzUgODcuNSA3NSBDIDg3LjUgMzMuNTc4MTI1IDEyMS4wNzgxMjUgMCAxNjIuNSAwIEMgMjAzLjkyMTg3NSAwIDIzNy41IDMzLjU3ODEyNSAyMzcuNSA3NSIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzk1NThiMiI+CiAgICA8cGF0aCBkPSJNIDMyNC4xMDE1NjIgMjI1IEMgMzI0LjEwMTU2MiAyNjYuNDIxODc1IDI5MC41MjM0MzggMzAwIDI0OS4xMDE1NjIgMzAwIEMgMjA3LjY3OTY4OCAzMDAgMTc0LjEwMTU2MiAyNjYuNDIxODc1IDE3NC4xMDE1NjIgMjI1IEMgMTc0LjEwMTU2MiAxODMuNTc4MTI1IDIwNy42Nzk2ODggMTUwIDI0OS4xMDE1NjIgMTUwIEMgMjkwLjUyMzQzOCAxNTAgMzI0LjEwMTU2MiAxODMuNTc4MTI1IDMyNC4xMDE1NjIgMjI1Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-jupyter-favicon: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTUyIiBoZWlnaHQ9IjE2NSIgdmlld0JveD0iMCAwIDE1MiAxNjUiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCIgZmlsbD0iI0YzNzcyNiI+CiAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLjA3ODk0NywgMTEwLjU4MjkyNykiIGQ9Ik03NS45NDIyODQyLDI5LjU4MDQ1NjEgQzQzLjMwMjM5NDcsMjkuNTgwNDU2MSAxNC43OTY3ODMyLDE3LjY1MzQ2MzQgMCwwIEM1LjUxMDgzMjExLDE1Ljg0MDY4MjkgMTUuNzgxNTM4OSwyOS41NjY3NzMyIDI5LjM5MDQ5NDcsMzkuMjc4NDE3MSBDNDIuOTk5Nyw0OC45ODk4NTM3IDU5LjI3MzcsNTQuMjA2NzgwNSA3NS45NjA1Nzg5LDU0LjIwNjc4MDUgQzkyLjY0NzQ1NzksNTQuMjA2NzgwNSAxMDguOTIxNDU4LDQ4Ljk4OTg1MzcgMTIyLjUzMDY2MywzOS4yNzg0MTcxIEMxMzYuMTM5NDUzLDI5LjU2Njc3MzIgMTQ2LjQxMDI4NCwxNS44NDA2ODI5IDE1MS45MjExNTgsMCBDMTM3LjA4Nzg2OCwxNy42NTM0NjM0IDEwOC41ODI1ODksMjkuNTgwNDU2MSA3NS45NDIyODQyLDI5LjU4MDQ1NjEgTDc1Ljk0MjI4NDIsMjkuNTgwNDU2MSBaIiAvPgogICAgPHBhdGggdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC4wMzczNjgsIDAuNzA0ODc4KSIgZD0iTTc1Ljk3ODQ1NzksMjQuNjI2NDA3MyBDMTA4LjYxODc2MywyNC42MjY0MDczIDEzNy4xMjQ0NTgsMzYuNTUzNDQxNSAxNTEuOTIxMTU4LDU0LjIwNjc4MDUgQzE0Ni40MTAyODQsMzguMzY2MjIyIDEzNi4xMzk0NTMsMjQuNjQwMTMxNyAxMjIuNTMwNjYzLDE0LjkyODQ4NzggQzEwOC45MjE0NTgsNS4yMTY4NDM5IDkyLjY0NzQ1NzksMCA3NS45NjA1Nzg5LDAgQzU5LjI3MzcsMCA0Mi45OTk3LDUuMjE2ODQzOSAyOS4zOTA0OTQ3LDE0LjkyODQ4NzggQzE1Ljc4MTUzODksMjQuNjQwMTMxNyA1LjUxMDgzMjExLDM4LjM2NjIyMiAwLDU0LjIwNjc4MDUgQzE0LjgzMzA4MTYsMzYuNTg5OTI5MyA0My4zMzg1Njg0LDI0LjYyNjQwNzMgNzUuOTc4NDU3OSwyNC42MjY0MDczIEw3NS45Nzg0NTc5LDI0LjYyNjQwNzMgWiIgLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-jupyter: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzkiIGhlaWdodD0iNTEiIHZpZXdCb3g9IjAgMCAzOSA1MSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMTYzOCAtMjI4MSkiPgogICAgPGcgY2xhc3M9ImpwLWljb24td2FybjAiIGZpbGw9IiNGMzc3MjYiPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM5Ljc0IDIzMTEuOTgpIiBkPSJNIDE4LjI2NDYgNy4xMzQxMUMgMTAuNDE0NSA3LjEzNDExIDMuNTU4NzIgNC4yNTc2IDAgMEMgMS4zMjUzOSAzLjgyMDQgMy43OTU1NiA3LjEzMDgxIDcuMDY4NiA5LjQ3MzAzQyAxMC4zNDE3IDExLjgxNTIgMTQuMjU1NyAxMy4wNzM0IDE4LjI2OSAxMy4wNzM0QyAyMi4yODIzIDEzLjA3MzQgMjYuMTk2MyAxMS44MTUyIDI5LjQ2OTQgOS40NzMwM0MgMzIuNzQyNCA3LjEzMDgxIDM1LjIxMjYgMy44MjA0IDM2LjUzOCAwQyAzMi45NzA1IDQuMjU3NiAyNi4xMTQ4IDcuMTM0MTEgMTguMjY0NiA3LjEzNDExWiIvPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM5LjczIDIyODUuNDgpIiBkPSJNIDE4LjI3MzMgNS45MzkzMUMgMjYuMTIzNSA1LjkzOTMxIDMyLjk3OTMgOC44MTU4MyAzNi41MzggMTMuMDczNEMgMzUuMjEyNiA5LjI1MzAzIDMyLjc0MjQgNS45NDI2MiAyOS40Njk0IDMuNjAwNEMgMjYuMTk2MyAxLjI1ODE4IDIyLjI4MjMgMCAxOC4yNjkgMEMgMTQuMjU1NyAwIDEwLjM0MTcgMS4yNTgxOCA3LjA2ODYgMy42MDA0QyAzLjc5NTU2IDUuOTQyNjIgMS4zMjUzOSA5LjI1MzAzIDAgMTMuMDczNEMgMy41Njc0NSA4LjgyNDYzIDEwLjQyMzIgNS45MzkzMSAxOC4yNzMzIDUuOTM5MzFaIi8+CiAgICA8L2c+CiAgICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjY5LjMgMjI4MS4zMSkiIGQ9Ik0gNS44OTM1MyAyLjg0NEMgNS45MTg4OSAzLjQzMTY1IDUuNzcwODUgNC4wMTM2NyA1LjQ2ODE1IDQuNTE2NDVDIDUuMTY1NDUgNS4wMTkyMiA0LjcyMTY4IDUuNDIwMTUgNC4xOTI5OSA1LjY2ODUxQyAzLjY2NDMgNS45MTY4OCAzLjA3NDQ0IDYuMDAxNTEgMi40OTgwNSA1LjkxMTcxQyAxLjkyMTY2IDUuODIxOSAxLjM4NDYzIDUuNTYxNyAwLjk1NDg5OCA1LjE2NDAxQyAwLjUyNTE3IDQuNzY2MzMgMC4yMjIwNTYgNC4yNDkwMyAwLjA4MzkwMzcgMy42Nzc1N0MgLTAuMDU0MjQ4MyAzLjEwNjExIC0wLjAyMTIzIDIuNTA2MTcgMC4xNzg3ODEgMS45NTM2NEMgMC4zNzg3OTMgMS40MDExIDAuNzM2ODA5IDAuOTIwODE3IDEuMjA3NTQgMC41NzM1MzhDIDEuNjc4MjYgMC4yMjYyNTkgMi4yNDA1NSAwLjAyNzU5MTkgMi44MjMyNiAwLjAwMjY3MjI5QyAzLjYwMzg5IC0wLjAzMDcxMTUgNC4zNjU3MyAwLjI0OTc4OSA0Ljk0MTQyIDAuNzgyNTUxQyA1LjUxNzExIDEuMzE1MzEgNS44NTk1NiAyLjA1Njc2IDUuODkzNTMgMi44NDRaIi8+CiAgICAgIDxwYXRoIHRyYW5zZm9ybT0idHJhbnNsYXRlKDE2MzkuOCAyMzIzLjgxKSIgZD0iTSA3LjQyNzg5IDMuNTgzMzhDIDcuNDYwMDggNC4zMjQzIDcuMjczNTUgNS4wNTgxOSA2Ljg5MTkzIDUuNjkyMTNDIDYuNTEwMzEgNi4zMjYwNyA1Ljk1MDc1IDYuODMxNTYgNS4yODQxMSA3LjE0NDZDIDQuNjE3NDcgNy40NTc2MyAzLjg3MzcxIDcuNTY0MTQgMy4xNDcwMiA3LjQ1MDYzQyAyLjQyMDMyIDcuMzM3MTIgMS43NDMzNiA3LjAwODcgMS4yMDE4NCA2LjUwNjk1QyAwLjY2MDMyOCA2LjAwNTIgMC4yNzg2MSA1LjM1MjY4IDAuMTA1MDE3IDQuNjMyMDJDIC0wLjA2ODU3NTcgMy45MTEzNSAtMC4wMjYyMzYxIDMuMTU0OTQgMC4yMjY2NzUgMi40NTg1NkMgMC40Nzk1ODcgMS43NjIxNyAwLjkzMTY5NyAxLjE1NzEzIDEuNTI1NzYgMC43MjAwMzNDIDIuMTE5ODMgMC4yODI5MzUgMi44MjkxNCAwLjAzMzQzOTUgMy41NjM4OSAwLjAwMzEzMzQ0QyA0LjU0NjY3IC0wLjAzNzQwMzMgNS41MDUyOSAwLjMxNjcwNiA2LjIyOTYxIDAuOTg3ODM1QyA2Ljk1MzkzIDEuNjU4OTYgNy4zODQ4NCAyLjU5MjM1IDcuNDI3ODkgMy41ODMzOEwgNy40Mjc4OSAzLjU4MzM4WiIvPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM4LjM2IDIyODYuMDYpIiBkPSJNIDIuMjc0NzEgNC4zOTYyOUMgMS44NDM2MyA0LjQxNTA4IDEuNDE2NzEgNC4zMDQ0NSAxLjA0Nzk5IDQuMDc4NDNDIDAuNjc5MjY4IDMuODUyNCAwLjM4NTMyOCAzLjUyMTE0IDAuMjAzMzcxIDMuMTI2NTZDIDAuMDIxNDEzNiAyLjczMTk4IC0wLjA0MDM3OTggMi4yOTE4MyAwLjAyNTgxMTYgMS44NjE4MUMgMC4wOTIwMDMxIDEuNDMxOCAwLjI4MzIwNCAxLjAzMTI2IDAuNTc1MjEzIDAuNzEwODgzQyAwLjg2NzIyMiAwLjM5MDUxIDEuMjQ2OTEgMC4xNjQ3MDggMS42NjYyMiAwLjA2MjA1OTJDIDIuMDg1NTMgLTAuMDQwNTg5NyAyLjUyNTYxIC0wLjAxNTQ3MTQgMi45MzA3NiAwLjEzNDIzNUMgMy4zMzU5MSAwLjI4Mzk0MSAzLjY4NzkyIDAuNTUxNTA1IDMuOTQyMjIgMC45MDMwNkMgNC4xOTY1MiAxLjI1NDYyIDQuMzQxNjkgMS42NzQzNiA0LjM1OTM1IDIuMTA5MTZDIDQuMzgyOTkgMi42OTEwNyA0LjE3Njc4IDMuMjU4NjkgMy43ODU5NyAzLjY4NzQ2QyAzLjM5NTE2IDQuMTE2MjQgMi44NTE2NiA0LjM3MTE2IDIuMjc0NzEgNC4zOTYyOUwgMi4yNzQ3MSA0LjM5NjI5WiIvPgogICAgPC9nPgogIDwvZz4+Cjwvc3ZnPgo=);
  --jp-icon-jupyterlab-wordmark: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIHZpZXdCb3g9IjAgMCAxODYwLjggNDc1Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0RTRFNEUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDQ4MC4xMzY0MDEsIDY0LjI3MTQ5MykiPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC4wMDAwMDAsIDU4Ljg3NTU2NikiPgogICAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLjA4NzYwMywgMC4xNDAyOTQpIj4KICAgICAgICA8cGF0aCBkPSJNLTQyNi45LDE2OS44YzAsNDguNy0zLjcsNjQuNy0xMy42LDc2LjRjLTEwLjgsMTAtMjUsMTUuNS0zOS43LDE1LjVsMy43LDI5IGMyMi44LDAuMyw0NC44LTcuOSw2MS45LTIzLjFjMTcuOC0xOC41LDI0LTQ0LjEsMjQtODMuM1YwSC00Mjd2MTcwLjFMLTQyNi45LDE2OS44TC00MjYuOSwxNjkuOHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTU1LjA0NTI5NiwgNTYuODM3MTA0KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDEuNTYyNDUzLCAxLjc5OTg0MikiPgogICAgICAgIDxwYXRoIGQ9Ik0tMzEyLDE0OGMwLDIxLDAsMzkuNSwxLjcsNTUuNGgtMzEuOGwtMi4xLTMzLjNoLTAuOGMtNi43LDExLjYtMTYuNCwyMS4zLTI4LDI3LjkgYy0xMS42LDYuNi0yNC44LDEwLTM4LjIsOS44Yy0zMS40LDAtNjktMTcuNy02OS04OVYwaDM2LjR2MTEyLjdjMCwzOC43LDExLjYsNjQuNyw0NC42LDY0LjdjMTAuMy0wLjIsMjAuNC0zLjUsMjguOS05LjQgYzguNS01LjksMTUuMS0xNC4zLDE4LjktMjMuOWMyLjItNi4xLDMuMy0xMi41LDMuMy0xOC45VjAuMmgzNi40VjE0OEgtMzEyTC0zMTIsMTQ4eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzOTAuMDEzMzIyLCA1My40Nzk2MzgpIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMS43MDY0NTgsIDAuMjMxNDI1KSI+CiAgICAgICAgPHBhdGggZD0iTS00NzguNiw3MS40YzAtMjYtMC44LTQ3LTEuNy02Ni43aDMyLjdsMS43LDM0LjhoMC44YzcuMS0xMi41LDE3LjUtMjIuOCwzMC4xLTI5LjcgYzEyLjUtNywyNi43LTEwLjMsNDEtOS44YzQ4LjMsMCw4NC43LDQxLjcsODQuNywxMDMuM2MwLDczLjEtNDMuNywxMDkuMi05MSwxMDkuMmMtMTIuMSwwLjUtMjQuMi0yLjItMzUtNy44IGMtMTAuOC01LjYtMTkuOS0xMy45LTI2LjYtMjQuMmgtMC44VjI5MWgtMzZ2LTIyMEwtNDc4LjYsNzEuNEwtNDc4LjYsNzEuNHogTS00NDIuNiwxMjUuNmMwLjEsNS4xLDAuNiwxMC4xLDEuNywxNS4xIGMzLDEyLjMsOS45LDIzLjMsMTkuOCwzMS4xYzkuOSw3LjgsMjIuMSwxMi4xLDM0LjcsMTIuMWMzOC41LDAsNjAuNy0zMS45LDYwLjctNzguNWMwLTQwLjctMjEuMS03NS42LTU5LjUtNzUuNiBjLTEyLjksMC40LTI1LjMsNS4xLTM1LjMsMTMuNGMtOS45LDguMy0xNi45LDE5LjctMTkuNiwzMi40Yy0xLjUsNC45LTIuMywxMC0yLjUsMTUuMVYxMjUuNkwtNDQyLjYsMTI1LjZMLTQ0Mi42LDEyNS42eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSg2MDYuNzQwNzI2LCA1Ni44MzcxMDQpIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC43NTEyMjYsIDEuOTg5Mjk5KSI+CiAgICAgICAgPHBhdGggZD0iTS00NDAuOCwwbDQzLjcsMTIwLjFjNC41LDEzLjQsOS41LDI5LjQsMTIuOCw0MS43aDAuOGMzLjctMTIuMiw3LjktMjcuNywxMi44LTQyLjQgbDM5LjctMTE5LjJoMzguNUwtMzQ2LjksMTQ1Yy0yNiw2OS43LTQzLjcsMTA1LjQtNjguNiwxMjcuMmMtMTIuNSwxMS43LTI3LjksMjAtNDQuNiwyMy45bC05LjEtMzEuMSBjMTEuNy0zLjksMjIuNS0xMC4xLDMxLjgtMTguMWMxMy4yLTExLjEsMjMuNy0yNS4yLDMwLjYtNDEuMmMxLjUtMi44LDIuNS01LjcsMi45LTguOGMtMC4zLTMuMy0xLjItNi42LTIuNS05LjdMLTQ4MC4yLDAuMSBoMzkuN0wtNDQwLjgsMEwtNDQwLjgsMHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoODIyLjc0ODEwNCwgMC4wMDAwMDApIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMS40NjQwNTAsIDAuMzc4OTE0KSI+CiAgICAgICAgPHBhdGggZD0iTS00MTMuNywwdjU4LjNoNTJ2MjguMmgtNTJWMTk2YzAsMjUsNywzOS41LDI3LjMsMzkuNWM3LjEsMC4xLDE0LjItMC43LDIxLjEtMi41IGwxLjcsMjcuN2MtMTAuMywzLjctMjEuMyw1LjQtMzIuMiw1Yy03LjMsMC40LTE0LjYtMC43LTIxLjMtMy40Yy02LjgtMi43LTEyLjktNi44LTE3LjktMTIuMWMtMTAuMy0xMC45LTE0LjEtMjktMTQuMS01Mi45IFY4Ni41aC0zMVY1OC4zaDMxVjkuNkwtNDEzLjcsMEwtNDEzLjcsMHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOTc0LjQzMzI4NiwgNTMuNDc5NjM4KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAuOTkwMDM0LCAwLjYxMDMzOSkiPgogICAgICAgIDxwYXRoIGQ9Ik0tNDQ1LjgsMTEzYzAuOCw1MCwzMi4yLDcwLjYsNjguNiw3MC42YzE5LDAuNiwzNy45LTMsNTUuMy0xMC41bDYuMiwyNi40IGMtMjAuOSw4LjktNDMuNSwxMy4xLTY2LjIsMTIuNmMtNjEuNSwwLTk4LjMtNDEuMi05OC4zLTEwMi41Qy00ODAuMiw0OC4yLTQ0NC43LDAtMzg2LjUsMGM2NS4yLDAsODIuNyw1OC4zLDgyLjcsOTUuNyBjLTAuMSw1LjgtMC41LDExLjUtMS4yLDE3LjJoLTE0MC42SC00NDUuOEwtNDQ1LjgsMTEzeiBNLTMzOS4yLDg2LjZjMC40LTIzLjUtOS41LTYwLjEtNTAuNC02MC4xIGMtMzYuOCwwLTUyLjgsMzQuNC01NS43LDYwLjFILTMzOS4yTC0zMzkuMiw4Ni42TC0zMzkuMiw4Ni42eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxMjAxLjk2MTA1OCwgNTMuNDc5NjM4KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDEuMTc5NjQwLCAwLjcwNTA2OCkiPgogICAgICAgIDxwYXRoIGQ9Ik0tNDc4LjYsNjhjMC0yMy45LTAuNC00NC41LTEuNy02My40aDMxLjhsMS4yLDM5LjloMS43YzkuMS0yNy4zLDMxLTQ0LjUsNTUuMy00NC41IGMzLjUtMC4xLDcsMC40LDEwLjMsMS4ydjM0LjhjLTQuMS0wLjktOC4yLTEuMy0xMi40LTEuMmMtMjUuNiwwLTQzLjcsMTkuNy00OC43LDQ3LjRjLTEsNS43LTEuNiwxMS41LTEuNywxNy4ydjEwOC4zaC0zNlY2OCBMLTQ3OC42LDY4eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCIgZmlsbD0iI0YzNzcyNiI+CiAgICA8cGF0aCBkPSJNMTM1Mi4zLDMyNi4yaDM3VjI4aC0zN1YzMjYuMnogTTE2MDQuOCwzMjYuMmMtMi41LTEzLjktMy40LTMxLjEtMy40LTQ4Ljd2LTc2IGMwLTQwLjctMTUuMS04My4xLTc3LjMtODMuMWMtMjUuNiwwLTUwLDcuMS02Ni44LDE4LjFsOC40LDI0LjRjMTQuMy05LjIsMzQtMTUuMSw1My0xNS4xYzQxLjYsMCw0Ni4yLDMwLjIsNDYuMiw0N3Y0LjIgYy03OC42LTAuNC0xMjIuMywyNi41LTEyMi4zLDc1LjZjMCwyOS40LDIxLDU4LjQsNjIuMiw1OC40YzI5LDAsNTAuOS0xNC4zLDYyLjItMzAuMmgxLjNsMi45LDI1LjZIMTYwNC44eiBNMTU2NS43LDI1Ny43IGMwLDMuOC0wLjgsOC0yLjEsMTEuOGMtNS45LDE3LjItMjIuNywzNC00OS4yLDM0Yy0xOC45LDAtMzQuOS0xMS4zLTM0LjktMzUuM2MwLTM5LjUsNDUuOC00Ni42LDg2LjItNDUuOFYyNTcuN3ogTTE2OTguNSwzMjYuMiBsMS43LTMzLjZoMS4zYzE1LjEsMjYuOSwzOC43LDM4LjIsNjguMSwzOC4yYzQ1LjQsMCw5MS4yLTM2LjEsOTEuMi0xMDguOGMwLjQtNjEuNy0zNS4zLTEwMy43LTg1LjctMTAzLjcgYy0zMi44LDAtNTYuMywxNC43LTY5LjMsMzcuNGgtMC44VjI4aC0zNi42djI0NS43YzAsMTguMS0wLjgsMzguNi0xLjcsNTIuNUgxNjk4LjV6IE0xNzA0LjgsMjA4LjJjMC01LjksMS4zLTEwLjksMi4xLTE1LjEgYzcuNi0yOC4xLDMxLjEtNDUuNCw1Ni4zLTQ1LjRjMzkuNSwwLDYwLjUsMzQuOSw2MC41LDc1LjZjMCw0Ni42LTIzLjEsNzguMS02MS44LDc4LjFjLTI2LjksMC00OC4zLTE3LjYtNTUuNS00My4zIGMtMC44LTQuMi0xLjctOC44LTEuNy0xMy40VjIwOC4yeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-kernel: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgZmlsbD0iIzYxNjE2MSIgZD0iTTE1IDlIOXY2aDZWOXptLTIgNGgtMnYtMmgydjJ6bTgtMlY5aC0yVjdjMC0xLjEtLjktMi0yLTJoLTJWM2gtMnYyaC0yVjNIOXYySDdjLTEuMSAwLTIgLjktMiAydjJIM3YyaDJ2MkgzdjJoMnYyYzAgMS4xLjkgMiAyIDJoMnYyaDJ2LTJoMnYyaDJ2LTJoMmMxLjEgMCAyLS45IDItMnYtMmgydi0yaC0ydi0yaDJ6bS00IDZIN1Y3aDEwdjEweiIvPgo8L3N2Zz4K);
  --jp-icon-keyboard: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMjAgNUg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMTdjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY3YzAtMS4xLS45LTItMi0yem0tOSAzaDJ2MmgtMlY4em0wIDNoMnYyaC0ydi0yek04IDhoMnYySDhWOHptMCAzaDJ2Mkg4di0yem0tMSAySDV2LTJoMnYyem0wLTNINVY4aDJ2MnptOSA3SDh2LTJoOHYyem0wLTRoLTJ2LTJoMnYyem0wLTNoLTJWOGgydjJ6bTMgM2gtMnYtMmgydjJ6bTAtM2gtMlY4aDJ2MnoiLz4KPC9zdmc+Cg==);
  --jp-icon-launcher: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkgMTlINVY1aDdWM0g1YTIgMiAwIDAwLTIgMnYxNGEyIDIgMCAwMDIgMmgxNGMxLjEgMCAyLS45IDItMnYtN2gtMnY3ek0xNCAzdjJoMy41OWwtOS44MyA5LjgzIDEuNDEgMS40MUwxOSA2LjQxVjEwaDJWM2gtN3oiLz4KPC9zdmc+Cg==);
  --jp-icon-line-form: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGZpbGw9IndoaXRlIiBkPSJNNS44OCA0LjEyTDEzLjc2IDEybC03Ljg4IDcuODhMOCAyMmwxMC0xMEw4IDJ6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-link: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTMuOSAxMmMwLTEuNzEgMS4zOS0zLjEgMy4xLTMuMWg0VjdIN2MtMi43NiAwLTUgMi4yNC01IDVzMi4yNCA1IDUgNWg0di0xLjlIN2MtMS43MSAwLTMuMS0xLjM5LTMuMS0zLjF6TTggMTNoOHYtMkg4djJ6bTktNmgtNHYxLjloNGMxLjcxIDAgMy4xIDEuMzkgMy4xIDMuMXMtMS4zOSAzLjEtMy4xIDMuMWgtNFYxN2g0YzIuNzYgMCA1LTIuMjQgNS01cy0yLjI0LTUtNS01eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-list: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiM2MTYxNjEiIGQ9Ik0xOSA1djE0SDVWNWgxNG0xLjEtMkgzLjljLS41IDAtLjkuNC0uOS45djE2LjJjMCAuNC40LjkuOS45aDE2LjJjLjQgMCAuOS0uNS45LS45VjMuOWMwLS41LS41LS45LS45LS45ek0xMSA3aDZ2MmgtNlY3em0wIDRoNnYyaC02di0yem0wIDRoNnYyaC02ek03IDdoMnYySDd6bTAgNGgydjJIN3ptMCA0aDJ2Mkg3eiIvPgo8L3N2Zz4=);
  --jp-icon-listings-info: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MC45NzggNTAuOTc4IiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCA1MC45NzggNTAuOTc4OyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+Cgk8Zz4KCQk8cGF0aCBzdHlsZT0iZmlsbDojMDEwMDAyOyIgZD0iTTQzLjUyLDcuNDU4QzM4LjcxMSwyLjY0OCwzMi4zMDcsMCwyNS40ODksMEMxOC42NywwLDEyLjI2NiwyLjY0OCw3LjQ1OCw3LjQ1OAoJCQljLTkuOTQzLDkuOTQxLTkuOTQzLDI2LjExOSwwLDM2LjA2MmM0LjgwOSw0LjgwOSwxMS4yMTIsNy40NTYsMTguMDMxLDcuNDU4YzAsMCwwLjAwMSwwLDAuMDAyLDAKCQkJYzYuODE2LDAsMTMuMjIxLTIuNjQ4LDE4LjAyOS03LjQ1OGM0LjgwOS00LjgwOSw3LjQ1Ny0xMS4yMTIsNy40NTctMTguMDNDNTAuOTc3LDE4LjY3LDQ4LjMyOCwxMi4yNjYsNDMuNTIsNy40NTh6CgkJCSBNNDIuMTA2LDQyLjEwNWMtNC40MzIsNC40MzEtMTAuMzMyLDYuODcyLTE2LjYxNSw2Ljg3MmgtMC4wMDJjLTYuMjg1LTAuMDAxLTEyLjE4Ny0yLjQ0MS0xNi42MTctNi44NzIKCQkJYy05LjE2Mi05LjE2My05LjE2Mi0yNC4wNzEsMC0zMy4yMzNDMTMuMzAzLDQuNDQsMTkuMjA0LDIsMjUuNDg5LDJjNi4yODQsMCwxMi4xODYsMi40NCwxNi42MTcsNi44NzIKCQkJYzQuNDMxLDQuNDMxLDYuODcxLDEwLjMzMiw2Ljg3MSwxNi42MTdDNDguOTc3LDMxLjc3Miw0Ni41MzYsMzcuNjc1LDQyLjEwNiw0Mi4xMDV6Ii8+CgkJPHBhdGggc3R5bGU9ImZpbGw6IzAxMDAwMjsiIGQ9Ik0yMy41NzgsMzIuMjE4Yy0wLjAyMy0xLjczNCwwLjE0My0zLjA1OSwwLjQ5Ni0zLjk3MmMwLjM1My0wLjkxMywxLjExLTEuOTk3LDIuMjcyLTMuMjUzCgkJCWMwLjQ2OC0wLjUzNiwwLjkyMy0xLjA2MiwxLjM2Ny0xLjU3NWMwLjYyNi0wLjc1MywxLjEwNC0xLjQ3OCwxLjQzNi0yLjE3NWMwLjMzMS0wLjcwNywwLjQ5NS0xLjU0MSwwLjQ5NS0yLjUKCQkJYzAtMS4wOTYtMC4yNi0yLjA4OC0wLjc3OS0yLjk3OWMtMC41NjUtMC44NzktMS41MDEtMS4zMzYtMi44MDYtMS4zNjljLTEuODAyLDAuMDU3LTIuOTg1LDAuNjY3LTMuNTUsMS44MzIKCQkJYy0wLjMwMSwwLjUzNS0wLjUwMywxLjE0MS0wLjYwNywxLjgxNGMtMC4xMzksMC43MDctMC4yMDcsMS40MzItMC4yMDcsMi4xNzRoLTIuOTM3Yy0wLjA5MS0yLjIwOCwwLjQwNy00LjExNCwxLjQ5My01LjcxOQoJCQljMS4wNjItMS42NCwyLjg1NS0yLjQ4MSw1LjM3OC0yLjUyN2MyLjE2LDAuMDIzLDMuODc0LDAuNjA4LDUuMTQxLDEuNzU4YzEuMjc4LDEuMTYsMS45MjksMi43NjQsMS45NSw0LjgxMQoJCQljMCwxLjE0Mi0wLjEzNywyLjExMS0wLjQxLDIuOTExYy0wLjMwOSwwLjg0NS0wLjczMSwxLjU5My0xLjI2OCwyLjI0M2MtMC40OTIsMC42NS0xLjA2OCwxLjMxOC0xLjczLDIuMDAyCgkJCWMtMC42NSwwLjY5Ny0xLjMxMywxLjQ3OS0xLjk4NywyLjM0NmMtMC4yMzksMC4zNzctMC40MjksMC43NzctMC41NjUsMS4xOTljLTAuMTYsMC45NTktMC4yMTcsMS45NTEtMC4xNzEsMi45NzkKCQkJQzI2LjU4OSwzMi4yMTgsMjMuNTc4LDMyLjIxOCwyMy41NzgsMzIuMjE4eiBNMjMuNTc4LDM4LjIydi0zLjQ4NGgzLjA3NnYzLjQ4NEgyMy41Nzh6Ii8+Cgk8L2c+Cjwvc3ZnPgo=);
  --jp-icon-markdown: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjN0IxRkEyIiBkPSJNNSAxNC45aDEybC02LjEgNnptOS40LTYuOGMwLTEuMy0uMS0yLjktLjEtNC41LS40IDEuNC0uOSAyLjktMS4zIDQuM2wtMS4zIDQuM2gtMkw4LjUgNy45Yy0uNC0xLjMtLjctMi45LTEtNC4zLS4xIDEuNi0uMSAzLjItLjIgNC42TDcgMTIuNEg0LjhsLjctMTFoMy4zTDEwIDVjLjQgMS4yLjcgMi43IDEgMy45LjMtMS4yLjctMi42IDEtMy45bDEuMi0zLjdoMy4zbC42IDExaC0yLjRsLS4zLTQuMnoiLz4KPC9zdmc+Cg==);
  --jp-icon-new-folder: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIwIDZoLThsLTItMkg0Yy0xLjExIDAtMS45OS44OS0xLjk5IDJMMiAxOGMwIDEuMTEuODkgMiAyIDJoMTZjMS4xMSAwIDItLjg5IDItMlY4YzAtMS4xMS0uODktMi0yLTJ6bS0xIDhoLTN2M2gtMnYtM2gtM3YtMmgzVjloMnYzaDN2MnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-not-trusted: url(data:image/svg+xml;base64,PHN2ZyBmaWxsPSJub25lIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI1IDI1Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDMgMykiIGQ9Ik0xLjg2MDk0IDExLjQ0MDlDMC44MjY0NDggOC43NzAyNyAwLjg2Mzc3OSA2LjA1NzY0IDEuMjQ5MDcgNC4xOTkzMkMyLjQ4MjA2IDMuOTMzNDcgNC4wODA2OCAzLjQwMzQ3IDUuNjAxMDIgMi44NDQ5QzcuMjM1NDkgMi4yNDQ0IDguODU2NjYgMS41ODE1IDkuOTg3NiAxLjA5NTM5QzExLjA1OTcgMS41ODM0MSAxMi42MDk0IDIuMjQ0NCAxNC4yMTggMi44NDMzOUMxNS43NTAzIDMuNDEzOTQgMTcuMzk5NSAzLjk1MjU4IDE4Ljc1MzkgNC4yMTM4NUMxOS4xMzY0IDYuMDcxNzcgMTkuMTcwOSA4Ljc3NzIyIDE4LjEzOSAxMS40NDA5QzE3LjAzMDMgMTQuMzAzMiAxNC42NjY4IDE3LjE4NDQgOS45OTk5OSAxOC45MzU0QzUuMzMzMTkgMTcuMTg0NCAyLjk2OTY4IDE0LjMwMzIgMS44NjA5NCAxMS40NDA5WiIvPgogICAgPHBhdGggY2xhc3M9ImpwLWljb24yIiBzdHJva2U9IiMzMzMzMzMiIHN0cm9rZS13aWR0aD0iMiIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOS4zMTU5MiA5LjMyMDMxKSIgZD0iTTcuMzY4NDIgMEwwIDcuMzY0NzkiLz4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDkuMzE1OTIgMTYuNjgzNikgc2NhbGUoMSAtMSkiIGQ9Ik03LjM2ODQyIDBMMCA3LjM2NDc5Ii8+Cjwvc3ZnPgo=);
  --jp-icon-notebook: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNFRjZDMDAiPgogICAgPHBhdGggZD0iTTE4LjcgMy4zdjE1LjRIMy4zVjMuM2gxNS40bTEuNS0xLjVIMS44djE4LjNoMTguM2wuMS0xOC4zeiIvPgogICAgPHBhdGggZD0iTTE2LjUgMTYuNWwtNS40LTQuMy01LjYgNC4zdi0xMWgxMXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-numbering: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyOCAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTQgMTlINlYxOS41SDVWMjAuNUg2VjIxSDRWMjJIN1YxOEg0VjE5Wk01IDEwSDZWNkg0VjdINVYxMFpNNCAxM0g1LjhMNCAxNS4xVjE2SDdWMTVINS4yTDcgMTIuOVYxMkg0VjEzWk05IDdWOUgyM1Y3SDlaTTkgMjFIMjNWMTlIOVYyMVpNOSAxNUgyM1YxM0g5VjE1WiIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-offline-bolt: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgd2lkdGg9IjE2Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyIDIuMDJjLTUuNTEgMC05Ljk4IDQuNDctOS45OCA5Ljk4czQuNDcgOS45OCA5Ljk4IDkuOTggOS45OC00LjQ3IDkuOTgtOS45OFMxNy41MSAyLjAyIDEyIDIuMDJ6TTExLjQ4IDIwdi02LjI2SDhMMTMgNHY2LjI2aDMuMzVMMTEuNDggMjB6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-palette: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE4IDEzVjIwSDRWNkg5LjAyQzkuMDcgNS4yOSA5LjI0IDQuNjIgOS41IDRINEMyLjkgNCAyIDQuOSAyIDZWMjBDMiAyMS4xIDIuOSAyMiA0IDIySDE4QzE5LjEgMjIgMjAgMjEuMSAyMCAyMFYxNUwxOCAxM1pNMTkuMyA4Ljg5QzE5Ljc0IDguMTkgMjAgNy4zOCAyMCA2LjVDMjAgNC4wMSAxNy45OSAyIDE1LjUgMkMxMy4wMSAyIDExIDQuMDEgMTEgNi41QzExIDguOTkgMTMuMDEgMTEgMTUuNDkgMTFDMTYuMzcgMTEgMTcuMTkgMTAuNzQgMTcuODggMTAuM0wyMSAxMy40MkwyMi40MiAxMkwxOS4zIDguODlaTTE1LjUgOUMxNC4xMiA5IDEzIDcuODggMTMgNi41QzEzIDUuMTIgMTQuMTIgNCAxNS41IDRDMTYuODggNCAxOCA1LjEyIDE4IDYuNUMxOCA3Ljg4IDE2Ljg4IDkgMTUuNSA5WiIvPgogICAgPHBhdGggZmlsbC1ydWxlPSJldmVub2RkIiBjbGlwLXJ1bGU9ImV2ZW5vZGQiIGQ9Ik00IDZIOS4wMTg5NEM5LjAwNjM5IDYuMTY1MDIgOSA2LjMzMTc2IDkgNi41QzkgOC44MTU3NyAxMC4yMTEgMTAuODQ4NyAxMi4wMzQzIDEySDlWMTRIMTZWMTIuOTgxMUMxNi41NzAzIDEyLjkzNzcgMTcuMTIgMTIuODIwNyAxNy42Mzk2IDEyLjYzOTZMMTggMTNWMjBINFY2Wk04IDhINlYxMEg4VjhaTTYgMTJIOFYxNEg2VjEyWk04IDE2SDZWMThIOFYxNlpNOSAxNkgxNlYxOEg5VjE2WiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-paste: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTE5IDJoLTQuMThDMTQuNC44NCAxMy4zIDAgMTIgMGMtMS4zIDAtMi40Ljg0LTIuODIgMkg1Yy0xLjEgMC0yIC45LTIgMnYxNmMwIDEuMS45IDIgMiAyaDE0YzEuMSAwIDItLjkgMi0yVjRjMC0xLjEtLjktMi0yLTJ6bS03IDBjLjU1IDAgMSAuNDUgMSAxcy0uNDUgMS0xIDEtMS0uNDUtMS0xIC40NS0xIDEtMXptNyAxOEg1VjRoMnYzaDEwVjRoMnYxNnoiLz4KICAgIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-pdf: url(data:image/svg+xml;base64,PHN2ZwogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyMiAyMiIgd2lkdGg9IjE2Ij4KICAgIDxwYXRoIHRyYW5zZm9ybT0icm90YXRlKDQ1KSIgY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iI0ZGMkEyQSIKICAgICAgIGQ9Im0gMjIuMzQ0MzY5LC0zLjAxNjM2NDIgaCA1LjYzODYwNCB2IDEuNTc5MjQzMyBoIC0zLjU0OTIyNyB2IDEuNTA4NjkyOTkgaCAzLjMzNzU3NiBWIDEuNjUwODE1NCBoIC0zLjMzNzU3NiB2IDMuNDM1MjYxMyBoIC0yLjA4OTM3NyB6IG0gLTcuMTM2NDQ0LDEuNTc5MjQzMyB2IDQuOTQzOTU0MyBoIDAuNzQ4OTIgcSAxLjI4MDc2MSwwIDEuOTUzNzAzLC0wLjYzNDk1MzUgMC42NzgzNjksLTAuNjM0OTUzNSAwLjY3ODM2OSwtMS44NDUxNjQxIDAsLTEuMjA0NzgzNTUgLTAuNjcyOTQyLC0xLjgzNDMxMDExIC0wLjY3Mjk0MiwtMC42Mjk1MjY1OSAtMS45NTkxMywtMC42Mjk1MjY1OSB6IG0gLTIuMDg5Mzc3LC0xLjU3OTI0MzMgaCAyLjIwMzM0MyBxIDEuODQ1MTY0LDAgMi43NDYwMzksMC4yNjU5MjA3IDAuOTA2MzAxLDAuMjYwNDkzNyAxLjU1MjEwOCwwLjg5MDAyMDMgMC41Njk4MywwLjU0ODEyMjMgMC44NDY2MDUsMS4yNjQ0ODAwNiAwLjI3Njc3NCwwLjcxNjM1NzgxIDAuMjc2Nzc0LDEuNjIyNjU4OTQgMCwwLjkxNzE1NTEgLTAuMjc2Nzc0LDEuNjM4OTM5OSAtMC4yNzY3NzUsMC43MTYzNTc4IC0wLjg0NjYwNSwxLjI2NDQ4IC0wLjY1MTIzNCwwLjYyOTUyNjYgLTEuNTYyOTYyLDAuODk1NDQ3MyAtMC45MTE3MjgsMC4yNjA0OTM3IC0yLjczNTE4NSwwLjI2MDQ5MzcgaCAtMi4yMDMzNDMgeiBtIC04LjE0NTg1NjUsMCBoIDMuNDY3ODIzIHEgMS41NDY2ODE2LDAgMi4zNzE1Nzg1LDAuNjg5MjIzIDAuODMwMzI0LDAuNjgzNzk2MSAwLjgzMDMyNCwxLjk1MzcwMzE0IDAsMS4yNzUzMzM5NyAtMC44MzAzMjQsMS45NjQ1NTcwNiBRIDkuOTg3MTk2MSwyLjI3NDkxNSA4LjQ0MDUxNDUsMi4yNzQ5MTUgSCA3LjA2MjA2ODQgViA1LjA4NjA3NjcgSCA0Ljk3MjY5MTUgWiBtIDIuMDg5Mzc2OSwxLjUxNDExOTkgdiAyLjI2MzAzOTQzIGggMS4xNTU5NDEgcSAwLjYwNzgxODgsMCAwLjkzODg2MjksLTAuMjkzMDU1NDcgMC4zMzEwNDQxLC0wLjI5ODQ4MjQxIDAuMzMxMDQ0MSwtMC44NDExNzc3MiAwLC0wLjU0MjY5NTMxIC0wLjMzMTA0NDEsLTAuODM1NzUwNzQgLTAuMzMxMDQ0MSwtMC4yOTMwNTU1IC0wLjkzODg2MjksLTAuMjkzMDU1NSB6IgovPgo8L3N2Zz4K);
  --jp-icon-python: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1icmFuZDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMEQ0N0ExIj4KICAgIDxwYXRoIGQ9Ik0xMS4xIDYuOVY1LjhINi45YzAtLjUgMC0xLjMuMi0xLjYuNC0uNy44LTEuMSAxLjctMS40IDEuNy0uMyAyLjUtLjMgMy45LS4xIDEgLjEgMS45LjkgMS45IDEuOXY0LjJjMCAuNS0uOSAxLjYtMiAxLjZIOC44Yy0xLjUgMC0yLjQgMS40LTIuNCAyLjh2Mi4ySDQuN0MzLjUgMTUuMSAzIDE0IDMgMTMuMVY5Yy0uMS0xIC42LTIgMS44LTIgMS41LS4xIDYuMy0uMSA2LjMtLjF6Ii8+CiAgICA8cGF0aCBkPSJNMTAuOSAxNS4xdjEuMWg0LjJjMCAuNSAwIDEuMy0uMiAxLjYtLjQuNy0uOCAxLjEtMS43IDEuNC0xLjcuMy0yLjUuMy0zLjkuMS0xLS4xLTEuOS0uOS0xLjktMS45di00LjJjMC0uNS45LTEuNiAyLTEuNmgzLjhjMS41IDAgMi40LTEuNCAyLjQtMi44VjYuNmgxLjdDMTguNSA2LjkgMTkgOCAxOSA4LjlWMTNjMCAxLS43IDIuMS0xLjkgMi4xaC02LjJ6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-r-kernel: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMjE5NkYzIiBkPSJNNC40IDIuNWMxLjItLjEgMi45LS4zIDQuOS0uMyAyLjUgMCA0LjEuNCA1LjIgMS4zIDEgLjcgMS41IDEuOSAxLjUgMy41IDAgMi0xLjQgMy41LTIuOSA0LjEgMS4yLjQgMS43IDEuNiAyLjIgMyAuNiAxLjkgMSAzLjkgMS4zIDQuNmgtMy44Yy0uMy0uNC0uOC0xLjctMS4yLTMuN3MtMS4yLTIuNi0yLjYtMi42aC0uOXY2LjRINC40VjIuNXptMy43IDYuOWgxLjRjMS45IDAgMi45LS45IDIuOS0yLjNzLTEtMi4zLTIuOC0yLjNjLS43IDAtMS4zIDAtMS42LjJ2NC41aC4xdi0uMXoiLz4KPC9zdmc+Cg==);
  --jp-icon-react: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMTUwIDE1MCA1NDEuOSAyOTUuMyI+CiAgPGcgY2xhc3M9ImpwLWljb24tYnJhbmQyIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzYxREFGQiI+CiAgICA8cGF0aCBkPSJNNjY2LjMgMjk2LjVjMC0zMi41LTQwLjctNjMuMy0xMDMuMS04Mi40IDE0LjQtNjMuNiA4LTExNC4yLTIwLjItMTMwLjQtNi41LTMuOC0xNC4xLTUuNi0yMi40LTUuNnYyMi4zYzQuNiAwIDguMy45IDExLjQgMi42IDEzLjYgNy44IDE5LjUgMzcuNSAxNC45IDc1LjctMS4xIDkuNC0yLjkgMTkuMy01LjEgMjkuNC0xOS42LTQuOC00MS04LjUtNjMuNS0xMC45LTEzLjUtMTguNS0yNy41LTM1LjMtNDEuNi01MCAzMi42LTMwLjMgNjMuMi00Ni45IDg0LTQ2LjlWNzhjLTI3LjUgMC02My41IDE5LjYtOTkuOSA1My42LTM2LjQtMzMuOC03Mi40LTUzLjItOTkuOS01My4ydjIyLjNjMjAuNyAwIDUxLjQgMTYuNSA4NCA0Ni42LTE0IDE0LjctMjggMzEuNC00MS4zIDQ5LjktMjIuNiAyLjQtNDQgNi4xLTYzLjYgMTEtMi4zLTEwLTQtMTkuNy01LjItMjktNC43LTM4LjIgMS4xLTY3LjkgMTQuNi03NS44IDMtMS44IDYuOS0yLjYgMTEuNS0yLjZWNzguNWMtOC40IDAtMTYgMS44LTIyLjYgNS42LTI4LjEgMTYuMi0zNC40IDY2LjctMTkuOSAxMzAuMS02Mi4yIDE5LjItMTAyLjcgNDkuOS0xMDIuNyA4Mi4zIDAgMzIuNSA0MC43IDYzLjMgMTAzLjEgODIuNC0xNC40IDYzLjYtOCAxMTQuMiAyMC4yIDEzMC40IDYuNSAzLjggMTQuMSA1LjYgMjIuNSA1LjYgMjcuNSAwIDYzLjUtMTkuNiA5OS45LTUzLjYgMzYuNCAzMy44IDcyLjQgNTMuMiA5OS45IDUzLjIgOC40IDAgMTYtMS44IDIyLjYtNS42IDI4LjEtMTYuMiAzNC40LTY2LjcgMTkuOS0xMzAuMSA2Mi0xOS4xIDEwMi41LTQ5LjkgMTAyLjUtODIuM3ptLTEzMC4yLTY2LjdjLTMuNyAxMi45LTguMyAyNi4yLTEzLjUgMzkuNS00LjEtOC04LjQtMTYtMTMuMS0yNC00LjYtOC05LjUtMTUuOC0xNC40LTIzLjQgMTQuMiAyLjEgMjcuOSA0LjcgNDEgNy45em0tNDUuOCAxMDYuNWMtNy44IDEzLjUtMTUuOCAyNi4zLTI0LjEgMzguMi0xNC45IDEuMy0zMCAyLTQ1LjIgMi0xNS4xIDAtMzAuMi0uNy00NS0xLjktOC4zLTExLjktMTYuNC0yNC42LTI0LjItMzgtNy42LTEzLjEtMTQuNS0yNi40LTIwLjgtMzkuOCA2LjItMTMuNCAxMy4yLTI2LjggMjAuNy0zOS45IDcuOC0xMy41IDE1LjgtMjYuMyAyNC4xLTM4LjIgMTQuOS0xLjMgMzAtMiA0NS4yLTIgMTUuMSAwIDMwLjIuNyA0NSAxLjkgOC4zIDExLjkgMTYuNCAyNC42IDI0LjIgMzggNy42IDEzLjEgMTQuNSAyNi40IDIwLjggMzkuOC02LjMgMTMuNC0xMy4yIDI2LjgtMjAuNyAzOS45em0zMi4zLTEzYzUuNCAxMy40IDEwIDI2LjggMTMuOCAzOS44LTEzLjEgMy4yLTI2LjkgNS45LTQxLjIgOCA0LjktNy43IDkuOC0xNS42IDE0LjQtMjMuNyA0LjYtOCA4LjktMTYuMSAxMy0yNC4xek00MjEuMiA0MzBjLTkuMy05LjYtMTguNi0yMC4zLTI3LjgtMzIgOSAuNCAxOC4yLjcgMjcuNS43IDkuNCAwIDE4LjctLjIgMjcuOC0uNy05IDExLjctMTguMyAyMi40LTI3LjUgMzJ6bS03NC40LTU4LjljLTE0LjItMi4xLTI3LjktNC43LTQxLTcuOSAzLjctMTIuOSA4LjMtMjYuMiAxMy41LTM5LjUgNC4xIDggOC40IDE2IDEzLjEgMjQgNC43IDggOS41IDE1LjggMTQuNCAyMy40ek00MjAuNyAxNjNjOS4zIDkuNiAxOC42IDIwLjMgMjcuOCAzMi05LS40LTE4LjItLjctMjcuNS0uNy05LjQgMC0xOC43LjItMjcuOC43IDktMTEuNyAxOC4zLTIyLjQgMjcuNS0zMnptLTc0IDU4LjljLTQuOSA3LjctOS44IDE1LjYtMTQuNCAyMy43LTQuNiA4LTguOSAxNi0xMyAyNC01LjQtMTMuNC0xMC0yNi44LTEzLjgtMzkuOCAxMy4xLTMuMSAyNi45LTUuOCA0MS4yLTcuOXptLTkwLjUgMTI1LjJjLTM1LjQtMTUuMS01OC4zLTM0LjktNTguMy01MC42IDAtMTUuNyAyMi45LTM1LjYgNTguMy01MC42IDguNi0zLjcgMTgtNyAyNy43LTEwLjEgNS43IDE5LjYgMTMuMiA0MCAyMi41IDYwLjktOS4yIDIwLjgtMTYuNiA0MS4xLTIyLjIgNjAuNi05LjktMy4xLTE5LjMtNi41LTI4LTEwLjJ6TTMxMCA0OTBjLTEzLjYtNy44LTE5LjUtMzcuNS0xNC45LTc1LjcgMS4xLTkuNCAyLjktMTkuMyA1LjEtMjkuNCAxOS42IDQuOCA0MSA4LjUgNjMuNSAxMC45IDEzLjUgMTguNSAyNy41IDM1LjMgNDEuNiA1MC0zMi42IDMwLjMtNjMuMiA0Ni45LTg0IDQ2LjktNC41LS4xLTguMy0xLTExLjMtMi43em0yMzcuMi03Ni4yYzQuNyAzOC4yLTEuMSA2Ny45LTE0LjYgNzUuOC0zIDEuOC02LjkgMi42LTExLjUgMi42LTIwLjcgMC01MS40LTE2LjUtODQtNDYuNiAxNC0xNC43IDI4LTMxLjQgNDEuMy00OS45IDIyLjYtMi40IDQ0LTYuMSA2My42LTExIDIuMyAxMC4xIDQuMSAxOS44IDUuMiAyOS4xem0zOC41LTY2LjdjLTguNiAzLjctMTggNy0yNy43IDEwLjEtNS43LTE5LjYtMTMuMi00MC0yMi41LTYwLjkgOS4yLTIwLjggMTYuNi00MS4xIDIyLjItNjAuNiA5LjkgMy4xIDE5LjMgNi41IDI4LjEgMTAuMiAzNS40IDE1LjEgNTguMyAzNC45IDU4LjMgNTAuNi0uMSAxNS43LTIzIDM1LjYtNTguNCA1MC42ek0zMjAuOCA3OC40eiIvPgogICAgPGNpcmNsZSBjeD0iNDIwLjkiIGN5PSIyOTYuNSIgcj0iNDUuNyIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-redo: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgd2lkdGg9IjE2Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgICA8cGF0aCBkPSJNMCAwaDI0djI0SDB6IiBmaWxsPSJub25lIi8+PHBhdGggZD0iTTE4LjQgMTAuNkMxNi41NSA4Ljk5IDE0LjE1IDggMTEuNSA4Yy00LjY1IDAtOC41OCAzLjAzLTkuOTYgNy4yMkwzLjkgMTZjMS4wNS0zLjE5IDQuMDUtNS41IDcuNi01LjUgMS45NSAwIDMuNzMuNzIgNS4xMiAxLjg4TDEzIDE2aDlWN2wtMy42IDMuNnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-refresh: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTkgMTMuNWMtMi40OSAwLTQuNS0yLjAxLTQuNS00LjVTNi41MSA0LjUgOSA0LjVjMS4yNCAwIDIuMzYuNTIgMy4xNyAxLjMzTDEwIDhoNVYzbC0xLjc2IDEuNzZDMTIuMTUgMy42OCAxMC42NiAzIDkgMyA1LjY5IDMgMy4wMSA1LjY5IDMuMDEgOVM1LjY5IDE1IDkgMTVjMi45NyAwIDUuNDMtMi4xNiA1LjktNWgtMS41MmMtLjQ2IDItMi4yNCAzLjUtNC4zOCAzLjV6Ii8+CiAgICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-regex: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0MTQxNDEiPgogICAgPHJlY3QgeD0iMiIgeT0iMiIgd2lkdGg9IjE2IiBoZWlnaHQ9IjE2Ii8+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbi1hY2NlbnQyIiBmaWxsPSIjRkZGIj4KICAgIDxjaXJjbGUgY2xhc3M9InN0MiIgY3g9IjUuNSIgY3k9IjE0LjUiIHI9IjEuNSIvPgogICAgPHJlY3QgeD0iMTIiIHk9IjQiIGNsYXNzPSJzdDIiIHdpZHRoPSIxIiBoZWlnaHQ9IjgiLz4KICAgIDxyZWN0IHg9IjguNSIgeT0iNy41IiB0cmFuc2Zvcm09Im1hdHJpeCgwLjg2NiAtMC41IDAuNSAwLjg2NiAtMi4zMjU1IDcuMzIxOSkiIGNsYXNzPSJzdDIiIHdpZHRoPSI4IiBoZWlnaHQ9IjEiLz4KICAgIDxyZWN0IHg9IjEyIiB5PSI0IiB0cmFuc2Zvcm09Im1hdHJpeCgwLjUgLTAuODY2IDAuODY2IDAuNSAtMC42Nzc5IDE0LjgyNTIpIiBjbGFzcz0ic3QyIiB3aWR0aD0iMSIgaGVpZ2h0PSI4Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-run: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTggNXYxNGwxMS03eiIvPgogICAgPC9nPgo8L3N2Zz4K);
  --jp-icon-running: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDUxMiA1MTIiPgogIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICA8cGF0aCBkPSJNMjU2IDhDMTE5IDggOCAxMTkgOCAyNTZzMTExIDI0OCAyNDggMjQ4IDI0OC0xMTEgMjQ4LTI0OFMzOTMgOCAyNTYgOHptOTYgMzI4YzAgOC44LTcuMiAxNi0xNiAxNkgxNzZjLTguOCAwLTE2LTcuMi0xNi0xNlYxNzZjMC04LjggNy4yLTE2IDE2LTE2aDE2MGM4LjggMCAxNiA3LjIgMTYgMTZ2MTYweiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-save: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTE3IDNINWMtMS4xMSAwLTIgLjktMiAydjE0YzAgMS4xLjg5IDIgMiAyaDE0YzEuMSAwIDItLjkgMi0yVjdsLTQtNHptLTUgMTZjLTEuNjYgMC0zLTEuMzQtMy0zczEuMzQtMyAzLTMgMyAxLjM0IDMgMy0xLjM0IDMtMyAzem0zLTEwSDVWNWgxMHY0eiIvPgogICAgPC9nPgo8L3N2Zz4K);
  --jp-icon-search: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyLjEsMTAuOWgtMC43bC0wLjItMC4yYzAuOC0wLjksMS4zLTIuMiwxLjMtMy41YzAtMy0yLjQtNS40LTUuNC01LjRTMS44LDQuMiwxLjgsNy4xczIuNCw1LjQsNS40LDUuNCBjMS4zLDAsMi41LTAuNSwzLjUtMS4zbDAuMiwwLjJ2MC43bDQuMSw0LjFsMS4yLTEuMkwxMi4xLDEwLjl6IE03LjEsMTAuOWMtMi4xLDAtMy43LTEuNy0zLjctMy43czEuNy0zLjcsMy43LTMuN3MzLjcsMS43LDMuNywzLjcgUzkuMiwxMC45LDcuMSwxMC45eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-settings: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkuNDMgMTIuOThjLjA0LS4zMi4wNy0uNjQuMDctLjk4cy0uMDMtLjY2LS4wNy0uOThsMi4xMS0xLjY1Yy4xOS0uMTUuMjQtLjQyLjEyLS42NGwtMi0zLjQ2Yy0uMTItLjIyLS4zOS0uMy0uNjEtLjIybC0yLjQ5IDFjLS41Mi0uNC0xLjA4LS43My0xLjY5LS45OGwtLjM4LTIuNjVBLjQ4OC40ODggMCAwMDE0IDJoLTRjLS4yNSAwLS40Ni4xOC0uNDkuNDJsLS4zOCAyLjY1Yy0uNjEuMjUtMS4xNy41OS0xLjY5Ljk4bC0yLjQ5LTFjLS4yMy0uMDktLjQ5IDAtLjYxLjIybC0yIDMuNDZjLS4xMy4yMi0uMDcuNDkuMTIuNjRsMi4xMSAxLjY1Yy0uMDQuMzItLjA3LjY1LS4wNy45OHMuMDMuNjYuMDcuOThsLTIuMTEgMS42NWMtLjE5LjE1LS4yNC40Mi0uMTIuNjRsMiAzLjQ2Yy4xMi4yMi4zOS4zLjYxLjIybDIuNDktMWMuNTIuNCAxLjA4LjczIDEuNjkuOThsLjM4IDIuNjVjLjAzLjI0LjI0LjQyLjQ5LjQyaDRjLjI1IDAgLjQ2LS4xOC40OS0uNDJsLjM4LTIuNjVjLjYxLS4yNSAxLjE3LS41OSAxLjY5LS45OGwyLjQ5IDFjLjIzLjA5LjQ5IDAgLjYxLS4yMmwyLTMuNDZjLjEyLS4yMi4wNy0uNDktLjEyLS42NGwtMi4xMS0xLjY1ek0xMiAxNS41Yy0xLjkzIDAtMy41LTEuNTctMy41LTMuNXMxLjU3LTMuNSAzLjUtMy41IDMuNSAxLjU3IDMuNSAzLjUtMS41NyAzLjUtMy41IDMuNXoiLz4KPC9zdmc+Cg==);
  --jp-icon-spreadsheet: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDEganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNENBRjUwIiBkPSJNMi4yIDIuMnYxNy42aDE3LjZWMi4ySDIuMnptMTUuNCA3LjdoLTUuNVY0LjRoNS41djUuNXpNOS45IDQuNHY1LjVINC40VjQuNGg1LjV6bS01LjUgNy43aDUuNXY1LjVINC40di01LjV6bTcuNyA1LjV2LTUuNWg1LjV2NS41aC01LjV6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-stop: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik02IDZoMTJ2MTJINnoiLz4KICAgIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-tab: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIxIDNIM2MtMS4xIDAtMiAuOS0yIDJ2MTRjMCAxLjEuOSAyIDIgMmgxOGMxLjEgMCAyLS45IDItMlY1YzAtMS4xLS45LTItMi0yem0wIDE2SDNWNWgxMHY0aDh2MTB6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-table-rows: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik0yMSw4SDNWNGgxOFY4eiBNMjEsMTBIM3Y0aDE4VjEweiBNMjEsMTZIM3Y0aDE4VjE2eiIvPgogICAgPC9nPgo8L3N2Zz4=);
  --jp-icon-tag: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjgiIGhlaWdodD0iMjgiIHZpZXdCb3g9IjAgMCA0MyAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTI4LjgzMzIgMTIuMzM0TDMyLjk5OTggMTYuNTAwN0wzNy4xNjY1IDEyLjMzNEgyOC44MzMyWiIvPgoJCTxwYXRoIGQ9Ik0xNi4yMDk1IDIxLjYxMDRDMTUuNjg3MyAyMi4xMjk5IDE0Ljg0NDMgMjIuMTI5OSAxNC4zMjQ4IDIxLjYxMDRMNi45ODI5IDE0LjcyNDVDNi41NzI0IDE0LjMzOTQgNi4wODMxMyAxMy42MDk4IDYuMDQ3ODYgMTMuMDQ4MkM1Ljk1MzQ3IDExLjUyODggNi4wMjAwMiA4LjYxOTQ0IDYuMDY2MjEgNy4wNzY5NUM2LjA4MjgxIDYuNTE0NzcgNi41NTU0OCA2LjA0MzQ3IDcuMTE4MDQgNi4wMzA1NUM5LjA4ODYzIDUuOTg0NzMgMTMuMjYzOCA1LjkzNTc5IDEzLjY1MTggNi4zMjQyNUwyMS43MzY5IDEzLjYzOUMyMi4yNTYgMTQuMTU4NSAyMS43ODUxIDE1LjQ3MjQgMjEuMjYyIDE1Ljk5NDZMMTYuMjA5NSAyMS42MTA0Wk05Ljc3NTg1IDguMjY1QzkuMzM1NTEgNy44MjU2NiA4LjYyMzUxIDcuODI1NjYgOC4xODI4IDguMjY1QzcuNzQzNDYgOC43MDU3MSA3Ljc0MzQ2IDkuNDE3MzMgOC4xODI4IDkuODU2NjdDOC42MjM4MiAxMC4yOTY0IDkuMzM1ODIgMTAuMjk2NCA5Ljc3NTg1IDkuODU2NjdDMTAuMjE1NiA5LjQxNzMzIDEwLjIxNTYgOC43MDUzMyA5Ljc3NTg1IDguMjY1WiIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-terminal: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0IiA+CiAgICA8cmVjdCBjbGFzcz0ianAtaWNvbjIganAtaWNvbi1zZWxlY3RhYmxlIiB3aWR0aD0iMjAiIGhlaWdodD0iMjAiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDIgMikiIGZpbGw9IiMzMzMzMzMiLz4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uLWFjY2VudDIganAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGQ9Ik01LjA1NjY0IDguNzYxNzJDNS4wNTY2NCA4LjU5NzY2IDUuMDMxMjUgOC40NTMxMiA0Ljk4MDQ3IDguMzI4MTJDNC45MzM1OSA4LjE5OTIyIDQuODU1NDcgOC4wODIwMyA0Ljc0NjA5IDcuOTc2NTZDNC42NDA2MiA3Ljg3MTA5IDQuNSA3Ljc3NTM5IDQuMzI0MjIgNy42ODk0NUM0LjE1MjM0IDcuNTk5NjEgMy45NDMzNiA3LjUxMTcyIDMuNjk3MjcgNy40MjU3OEMzLjMwMjczIDcuMjg1MTYgMi45NDMzNiA3LjEzNjcyIDIuNjE5MTQgNi45ODA0N0MyLjI5NDkyIDYuODI0MjIgMi4wMTc1OCA2LjY0MjU4IDEuNzg3MTEgNi40MzU1NUMxLjU2MDU1IDYuMjI4NTIgMS4zODQ3NyA1Ljk4ODI4IDEuMjU5NzcgNS43MTQ4NEMxLjEzNDc3IDUuNDM3NSAxLjA3MjI3IDUuMTA5MzggMS4wNzIyNyA0LjczMDQ3QzEuMDcyMjcgNC4zOTg0NCAxLjEyODkxIDQuMDk1NyAxLjI0MjE5IDMuODIyMjdDMS4zNTU0NyAzLjU0NDkyIDEuNTE1NjIgMy4zMDQ2OSAxLjcyMjY2IDMuMTAxNTZDMS45Mjk2OSAyLjg5ODQ0IDIuMTc5NjkgMi43MzQzNyAyLjQ3MjY2IDIuNjA5MzhDMi43NjU2MiAyLjQ4NDM4IDMuMDkxOCAyLjQwNDMgMy40NTExNyAyLjM2OTE0VjEuMTA5MzhINC4zODg2N1YyLjM4MDg2QzQuNzQwMjMgMi40Mjc3MyA1LjA1NjY0IDIuNTIzNDQgNS4zMzc4OSAyLjY2Nzk3QzUuNjE5MTQgMi44MTI1IDUuODU3NDIgMy4wMDE5NSA2LjA1MjczIDMuMjM2MzNDNi4yNTE5NSAzLjQ2NjggNi40MDQzIDMuNzQwMjMgNi41MDk3NyA0LjA1NjY0QzYuNjE5MTQgNC4zNjkxNCA2LjY3MzgzIDQuNzIwNyA2LjY3MzgzIDUuMTExMzNINS4wNDQ5MkM1LjA0NDkyIDQuNjM4NjcgNC45Mzc1IDQuMjgxMjUgNC43MjI2NiA0LjAzOTA2QzQuNTA3ODEgMy43OTI5NyA0LjIxNjggMy42Njk5MiAzLjg0OTYxIDMuNjY5OTJDMy42NTAzOSAzLjY2OTkyIDMuNDc2NTYgMy42OTcyNyAzLjMyODEyIDMuNzUxOTVDMy4xODM1OSAzLjgwMjczIDMuMDY0NDUgMy44NzY5NSAyLjk3MDcgMy45NzQ2MUMyLjg3Njk1IDQuMDY4MzYgMi44MDY2NCA0LjE3OTY5IDIuNzU5NzcgNC4zMDg1OUMyLjcxNjggNC40Mzc1IDIuNjk1MzEgNC41NzgxMiAyLjY5NTMxIDQuNzMwNDdDMi42OTUzMSA0Ljg4MjgxIDIuNzE2OCA1LjAxOTUzIDIuNzU5NzcgNS4xNDA2MkMyLjgwNjY0IDUuMjU3ODEgMi44ODI4MSA1LjM2NzE5IDIuOTg4MjggNS40Njg3NUMzLjA5NzY2IDUuNTcwMzEgMy4yNDAyMyA1LjY2Nzk3IDMuNDE2MDIgNS43NjE3MkMzLjU5MTggNS44NTE1NiAzLjgxMDU1IDUuOTQzMzYgNC4wNzIyNyA2LjAzNzExQzQuNDY2OCA2LjE4NTU1IDQuODI0MjIgNi4zMzk4NCA1LjE0NDUzIDYuNUM1LjQ2NDg0IDYuNjU2MjUgNS43MzgyOCA2LjgzOTg0IDUuOTY0ODQgNy4wNTA3OEM2LjE5NTMxIDcuMjU3ODEgNi4zNzEwOSA3LjUgNi40OTIxOSA3Ljc3NzM0QzYuNjE3MTkgOC4wNTA3OCA2LjY3OTY5IDguMzc1IDYuNjc5NjkgOC43NUM2LjY3OTY5IDkuMDkzNzUgNi42MjMwNSA5LjQwNDMgNi41MDk3NyA5LjY4MTY0QzYuMzk2NDggOS45NTUwOCA2LjIzNDM4IDEwLjE5MTQgNi4wMjM0NCAxMC4zOTA2QzUuODEyNSAxMC41ODk4IDUuNTU4NTkgMTAuNzUgNS4yNjE3MiAxMC44NzExQzQuOTY0ODQgMTAuOTg4MyA0LjYzMjgxIDExLjA2NDUgNC4yNjU2MiAxMS4wOTk2VjEyLjI0OEgzLjMzMzk4VjExLjA5OTZDMy4wMDE5NSAxMS4wNjg0IDIuNjc5NjkgMTAuOTk2MSAyLjM2NzE5IDEwLjg4MjhDMi4wNTQ2OSAxMC43NjU2IDEuNzc3MzQgMTAuNTk3NyAxLjUzNTE2IDEwLjM3ODlDMS4yOTY4OCAxMC4xNjAyIDEuMTA1NDcgOS44ODQ3NyAwLjk2MDkzOCA5LjU1MjczQzAuODE2NDA2IDkuMjE2OCAwLjc0NDE0MSA4LjgxNDQ1IDAuNzQ0MTQxIDguMzQ1N0gyLjM3ODkxQzIuMzc4OTEgOC42MjY5NSAyLjQxOTkyIDguODYzMjggMi41MDE5NSA5LjA1NDY5QzIuNTgzOTggOS4yNDIxOSAyLjY4OTQ1IDkuMzkyNTggMi44MTgzNiA5LjUwNTg2QzIuOTUxMTcgOS42MTUyMyAzLjEwMTU2IDkuNjkzMzYgMy4yNjk1MyA5Ljc0MDIzQzMuNDM3NSA5Ljc4NzExIDMuNjA5MzggOS44MTA1NSAzLjc4NTE2IDkuODEwNTVDNC4yMDMxMiA5LjgxMDU1IDQuNTE5NTMgOS43MTI4OSA0LjczNDM4IDkuNTE3NThDNC45NDkyMiA5LjMyMjI3IDUuMDU2NjQgOS4wNzAzMSA1LjA1NjY0IDguNzYxNzJaTTEzLjQxOCAxMi4yNzE1SDguMDc0MjJWMTFIMTMuNDE4VjEyLjI3MTVaIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzLjk1MjY0IDYpIiBmaWxsPSJ3aGl0ZSIvPgo8L3N2Zz4K);
  --jp-icon-text-editor: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTUgMTVIM3YyaDEydi0yem0wLThIM3YyaDEyVjd6TTMgMTNoMTh2LTJIM3Yyem0wIDhoMTh2LTJIM3Yyek0zIDN2MmgxOFYzSDN6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-toc: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik03LDVIMjFWN0g3VjVNNywxM1YxMUgyMVYxM0g3TTQsNC41QTEuNSwxLjUgMCAwLDEgNS41LDZBMS41LDEuNSAwIDAsMSA0LDcuNUExLjUsMS41IDAgMCwxIDIuNSw2QTEuNSwxLjUgMCAwLDEgNCw0LjVNNCwxMC41QTEuNSwxLjUgMCAwLDEgNS41LDEyQTEuNSwxLjUgMCAwLDEgNCwxMy41QTEuNSwxLjUgMCAwLDEgMi41LDEyQTEuNSwxLjUgMCAwLDEgNCwxMC41TTcsMTlWMTdIMjFWMTlIN000LDE2LjVBMS41LDEuNSAwIDAsMSA1LjUsMThBMS41LDEuNSAwIDAsMSA0LDE5LjVBMS41LDEuNSAwIDAsMSAyLjUsMThBMS41LDEuNSAwIDAsMSA0LDE2LjVaIiAvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-tree-view: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik0yMiAxMVYzaC03djNIOVYzSDJ2OGg3VjhoMnYxMGg0djNoN3YtOGgtN3YzaC0yVjhoMnYzeiIvPgogICAgPC9nPgo8L3N2Zz4=);
  --jp-icon-trusted: url(data:image/svg+xml;base64,PHN2ZyBmaWxsPSJub25lIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI1Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDIgMykiIGQ9Ik0xLjg2MDk0IDExLjQ0MDlDMC44MjY0NDggOC43NzAyNyAwLjg2Mzc3OSA2LjA1NzY0IDEuMjQ5MDcgNC4xOTkzMkMyLjQ4MjA2IDMuOTMzNDcgNC4wODA2OCAzLjQwMzQ3IDUuNjAxMDIgMi44NDQ5QzcuMjM1NDkgMi4yNDQ0IDguODU2NjYgMS41ODE1IDkuOTg3NiAxLjA5NTM5QzExLjA1OTcgMS41ODM0MSAxMi42MDk0IDIuMjQ0NCAxNC4yMTggMi44NDMzOUMxNS43NTAzIDMuNDEzOTQgMTcuMzk5NSAzLjk1MjU4IDE4Ljc1MzkgNC4yMTM4NUMxOS4xMzY0IDYuMDcxNzcgMTkuMTcwOSA4Ljc3NzIyIDE4LjEzOSAxMS40NDA5QzE3LjAzMDMgMTQuMzAzMiAxNC42NjY4IDE3LjE4NDQgOS45OTk5OSAxOC45MzU0QzUuMzMzMiAxNy4xODQ0IDIuOTY5NjggMTQuMzAzMiAxLjg2MDk0IDExLjQ0MDlaIi8+CiAgICA8cGF0aCBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiMzMzMzMzMiIHN0cm9rZT0iIzMzMzMzMyIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOCA5Ljg2NzE5KSIgZD0iTTIuODYwMTUgNC44NjUzNUwwLjcyNjU0OSAyLjk5OTU5TDAgMy42MzA0NUwyLjg2MDE1IDYuMTMxNTdMOCAwLjYzMDg3Mkw3LjI3ODU3IDBMMi44NjAxNSA0Ljg2NTM1WiIvPgo8L3N2Zz4K);
  --jp-icon-undo: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyLjUgOGMtMi42NSAwLTUuMDUuOTktNi45IDIuNkwyIDd2OWg5bC0zLjYyLTMuNjJjMS4zOS0xLjE2IDMuMTYtMS44OCA1LjEyLTEuODggMy41NCAwIDYuNTUgMi4zMSA3LjYgNS41bDIuMzctLjc4QzIxLjA4IDExLjAzIDE3LjE1IDggMTIuNSA4eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-vega: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbjEganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMjEyMTIxIj4KICAgIDxwYXRoIGQ9Ik0xMC42IDUuNGwyLjItMy4ySDIuMnY3LjNsNC02LjZ6Ii8+CiAgICA8cGF0aCBkPSJNMTUuOCAyLjJsLTQuNCA2LjZMNyA2LjNsLTQuOCA4djUuNWgxNy42VjIuMmgtNHptLTcgMTUuNEg1LjV2LTQuNGgzLjN2NC40em00LjQgMEg5LjhWOS44aDMuNHY3Ljh6bTQuNCAwaC0zLjRWNi41aDMuNHYxMS4xeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-yaml: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1jb250cmFzdDIganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjRDgxQjYwIj4KICAgIDxwYXRoIGQ9Ik03LjIgMTguNnYtNS40TDMgNS42aDMuM2wxLjQgMy4xYy4zLjkuNiAxLjYgMSAyLjUuMy0uOC42LTEuNiAxLTIuNWwxLjQtMy4xaDMuNGwtNC40IDcuNnY1LjVsLTIuOS0uMXoiLz4KICAgIDxjaXJjbGUgY2xhc3M9InN0MCIgY3g9IjE3LjYiIGN5PSIxNi41IiByPSIyLjEiLz4KICAgIDxjaXJjbGUgY2xhc3M9InN0MCIgY3g9IjE3LjYiIGN5PSIxMSIgcj0iMi4xIi8+CiAgPC9nPgo8L3N2Zz4K);
}

/* Icon CSS class declarations */

.jp-AddIcon {
  background-image: var(--jp-icon-add);
}
.jp-BugIcon {
  background-image: var(--jp-icon-bug);
}
.jp-BuildIcon {
  background-image: var(--jp-icon-build);
}
.jp-CaretDownEmptyIcon {
  background-image: var(--jp-icon-caret-down-empty);
}
.jp-CaretDownEmptyThinIcon {
  background-image: var(--jp-icon-caret-down-empty-thin);
}
.jp-CaretDownIcon {
  background-image: var(--jp-icon-caret-down);
}
.jp-CaretLeftIcon {
  background-image: var(--jp-icon-caret-left);
}
.jp-CaretRightIcon {
  background-image: var(--jp-icon-caret-right);
}
.jp-CaretUpEmptyThinIcon {
  background-image: var(--jp-icon-caret-up-empty-thin);
}
.jp-CaretUpIcon {
  background-image: var(--jp-icon-caret-up);
}
.jp-CaseSensitiveIcon {
  background-image: var(--jp-icon-case-sensitive);
}
.jp-CheckIcon {
  background-image: var(--jp-icon-check);
}
.jp-CircleEmptyIcon {
  background-image: var(--jp-icon-circle-empty);
}
.jp-CircleIcon {
  background-image: var(--jp-icon-circle);
}
.jp-ClearIcon {
  background-image: var(--jp-icon-clear);
}
.jp-CloseIcon {
  background-image: var(--jp-icon-close);
}
.jp-CodeIcon {
  background-image: var(--jp-icon-code);
}
.jp-ConsoleIcon {
  background-image: var(--jp-icon-console);
}
.jp-CopyIcon {
  background-image: var(--jp-icon-copy);
}
.jp-CopyrightIcon {
  background-image: var(--jp-icon-copyright);
}
.jp-CutIcon {
  background-image: var(--jp-icon-cut);
}
.jp-DownloadIcon {
  background-image: var(--jp-icon-download);
}
.jp-EditIcon {
  background-image: var(--jp-icon-edit);
}
.jp-EllipsesIcon {
  background-image: var(--jp-icon-ellipses);
}
.jp-ExtensionIcon {
  background-image: var(--jp-icon-extension);
}
.jp-FastForwardIcon {
  background-image: var(--jp-icon-fast-forward);
}
.jp-FileIcon {
  background-image: var(--jp-icon-file);
}
.jp-FileUploadIcon {
  background-image: var(--jp-icon-file-upload);
}
.jp-FilterListIcon {
  background-image: var(--jp-icon-filter-list);
}
.jp-FolderIcon {
  background-image: var(--jp-icon-folder);
}
.jp-Html5Icon {
  background-image: var(--jp-icon-html5);
}
.jp-ImageIcon {
  background-image: var(--jp-icon-image);
}
.jp-InspectorIcon {
  background-image: var(--jp-icon-inspector);
}
.jp-JsonIcon {
  background-image: var(--jp-icon-json);
}
.jp-JuliaIcon {
  background-image: var(--jp-icon-julia);
}
.jp-JupyterFaviconIcon {
  background-image: var(--jp-icon-jupyter-favicon);
}
.jp-JupyterIcon {
  background-image: var(--jp-icon-jupyter);
}
.jp-JupyterlabWordmarkIcon {
  background-image: var(--jp-icon-jupyterlab-wordmark);
}
.jp-KernelIcon {
  background-image: var(--jp-icon-kernel);
}
.jp-KeyboardIcon {
  background-image: var(--jp-icon-keyboard);
}
.jp-LauncherIcon {
  background-image: var(--jp-icon-launcher);
}
.jp-LineFormIcon {
  background-image: var(--jp-icon-line-form);
}
.jp-LinkIcon {
  background-image: var(--jp-icon-link);
}
.jp-ListIcon {
  background-image: var(--jp-icon-list);
}
.jp-ListingsInfoIcon {
  background-image: var(--jp-icon-listings-info);
}
.jp-MarkdownIcon {
  background-image: var(--jp-icon-markdown);
}
.jp-NewFolderIcon {
  background-image: var(--jp-icon-new-folder);
}
.jp-NotTrustedIcon {
  background-image: var(--jp-icon-not-trusted);
}
.jp-NotebookIcon {
  background-image: var(--jp-icon-notebook);
}
.jp-NumberingIcon {
  background-image: var(--jp-icon-numbering);
}
.jp-OfflineBoltIcon {
  background-image: var(--jp-icon-offline-bolt);
}
.jp-PaletteIcon {
  background-image: var(--jp-icon-palette);
}
.jp-PasteIcon {
  background-image: var(--jp-icon-paste);
}
.jp-PdfIcon {
  background-image: var(--jp-icon-pdf);
}
.jp-PythonIcon {
  background-image: var(--jp-icon-python);
}
.jp-RKernelIcon {
  background-image: var(--jp-icon-r-kernel);
}
.jp-ReactIcon {
  background-image: var(--jp-icon-react);
}
.jp-RedoIcon {
  background-image: var(--jp-icon-redo);
}
.jp-RefreshIcon {
  background-image: var(--jp-icon-refresh);
}
.jp-RegexIcon {
  background-image: var(--jp-icon-regex);
}
.jp-RunIcon {
  background-image: var(--jp-icon-run);
}
.jp-RunningIcon {
  background-image: var(--jp-icon-running);
}
.jp-SaveIcon {
  background-image: var(--jp-icon-save);
}
.jp-SearchIcon {
  background-image: var(--jp-icon-search);
}
.jp-SettingsIcon {
  background-image: var(--jp-icon-settings);
}
.jp-SpreadsheetIcon {
  background-image: var(--jp-icon-spreadsheet);
}
.jp-StopIcon {
  background-image: var(--jp-icon-stop);
}
.jp-TabIcon {
  background-image: var(--jp-icon-tab);
}
.jp-TableRowsIcon {
  background-image: var(--jp-icon-table-rows);
}
.jp-TagIcon {
  background-image: var(--jp-icon-tag);
}
.jp-TerminalIcon {
  background-image: var(--jp-icon-terminal);
}
.jp-TextEditorIcon {
  background-image: var(--jp-icon-text-editor);
}
.jp-TocIcon {
  background-image: var(--jp-icon-toc);
}
.jp-TreeViewIcon {
  background-image: var(--jp-icon-tree-view);
}
.jp-TrustedIcon {
  background-image: var(--jp-icon-trusted);
}
.jp-UndoIcon {
  background-image: var(--jp-icon-undo);
}
.jp-VegaIcon {
  background-image: var(--jp-icon-vega);
}
.jp-YamlIcon {
  background-image: var(--jp-icon-yaml);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * (DEPRECATED) Support for consuming icons as CSS background images
 */

.jp-Icon,
.jp-MaterialIcon {
  background-position: center;
  background-repeat: no-repeat;
  background-size: 16px;
  min-width: 16px;
  min-height: 16px;
}

.jp-Icon-cover {
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

/**
 * (DEPRECATED) Support for specific CSS icon sizes
 */

.jp-Icon-16 {
  background-size: 16px;
  min-width: 16px;
  min-height: 16px;
}

.jp-Icon-18 {
  background-size: 18px;
  min-width: 18px;
  min-height: 18px;
}

.jp-Icon-20 {
  background-size: 20px;
  min-width: 20px;
  min-height: 20px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * Support for icons as inline SVG HTMLElements
 */

/* recolor the primary elements of an icon */
.jp-icon0[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon1[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon2[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon3[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon4[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon0[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon1[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon2[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon3[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon4[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}
/* recolor the accent elements of an icon */
.jp-icon-accent0[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-accent1[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-accent2[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-accent3[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-accent4[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-accent0[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-accent1[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-accent2[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-accent3[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-accent4[stroke] {
  stroke: var(--jp-layout-color4);
}
/* set the color of an icon to transparent */
.jp-icon-none[fill] {
  fill: none;
}

.jp-icon-none[stroke] {
  stroke: none;
}
/* brand icon colors. Same for light and dark */
.jp-icon-brand0[fill] {
  fill: var(--jp-brand-color0);
}
.jp-icon-brand1[fill] {
  fill: var(--jp-brand-color1);
}
.jp-icon-brand2[fill] {
  fill: var(--jp-brand-color2);
}
.jp-icon-brand3[fill] {
  fill: var(--jp-brand-color3);
}
.jp-icon-brand4[fill] {
  fill: var(--jp-brand-color4);
}

.jp-icon-brand0[stroke] {
  stroke: var(--jp-brand-color0);
}
.jp-icon-brand1[stroke] {
  stroke: var(--jp-brand-color1);
}
.jp-icon-brand2[stroke] {
  stroke: var(--jp-brand-color2);
}
.jp-icon-brand3[stroke] {
  stroke: var(--jp-brand-color3);
}
.jp-icon-brand4[stroke] {
  stroke: var(--jp-brand-color4);
}
/* warn icon colors. Same for light and dark */
.jp-icon-warn0[fill] {
  fill: var(--jp-warn-color0);
}
.jp-icon-warn1[fill] {
  fill: var(--jp-warn-color1);
}
.jp-icon-warn2[fill] {
  fill: var(--jp-warn-color2);
}
.jp-icon-warn3[fill] {
  fill: var(--jp-warn-color3);
}

.jp-icon-warn0[stroke] {
  stroke: var(--jp-warn-color0);
}
.jp-icon-warn1[stroke] {
  stroke: var(--jp-warn-color1);
}
.jp-icon-warn2[stroke] {
  stroke: var(--jp-warn-color2);
}
.jp-icon-warn3[stroke] {
  stroke: var(--jp-warn-color3);
}
/* icon colors that contrast well with each other and most backgrounds */
.jp-icon-contrast0[fill] {
  fill: var(--jp-icon-contrast-color0);
}
.jp-icon-contrast1[fill] {
  fill: var(--jp-icon-contrast-color1);
}
.jp-icon-contrast2[fill] {
  fill: var(--jp-icon-contrast-color2);
}
.jp-icon-contrast3[fill] {
  fill: var(--jp-icon-contrast-color3);
}

.jp-icon-contrast0[stroke] {
  stroke: var(--jp-icon-contrast-color0);
}
.jp-icon-contrast1[stroke] {
  stroke: var(--jp-icon-contrast-color1);
}
.jp-icon-contrast2[stroke] {
  stroke: var(--jp-icon-contrast-color2);
}
.jp-icon-contrast3[stroke] {
  stroke: var(--jp-icon-contrast-color3);
}

/* CSS for icons in selected items in the settings editor */
#setting-editor .jp-PluginList .jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}
#setting-editor
  .jp-PluginList
  .jp-mod-selected
  .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}

/* CSS for icons in selected filebrowser listing items */
.jp-DirListing-item.jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}
.jp-DirListing-item.jp-mod-selected .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}

/* CSS for icons in selected tabs in the sidebar tab manager */
#tab-manager .lm-TabBar-tab.jp-mod-active .jp-icon-selectable[fill] {
  fill: #fff;
}

#tab-manager .lm-TabBar-tab.jp-mod-active .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}
#tab-manager
  .lm-TabBar-tab.jp-mod-active
  .jp-icon-hover
  :hover
  .jp-icon-selectable[fill] {
  fill: var(--jp-brand-color1);
}

#tab-manager
  .lm-TabBar-tab.jp-mod-active
  .jp-icon-hover
  :hover
  .jp-icon-selectable-inverse[fill] {
  fill: #fff;
}

/**
 * TODO: come up with non css-hack solution for showing the busy icon on top
 *  of the close icon
 * CSS for complex behavior of close icon of tabs in the sidebar tab manager
 */
#tab-manager
  .lm-TabBar-tab.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon3[fill] {
  fill: none;
}
#tab-manager
  .lm-TabBar-tab.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: var(--jp-inverse-layout-color3);
}

#tab-manager
  .lm-TabBar-tab.jp-mod-dirty.jp-mod-active
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: #fff;
}

/**
* TODO: come up with non css-hack solution for showing the busy icon on top
*  of the close icon
* CSS for complex behavior of close icon of tabs in the main area tabbar
*/
.lm-DockPanel-tabBar
  .lm-TabBar-tab.lm-mod-closable.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon3[fill] {
  fill: none;
}
.lm-DockPanel-tabBar
  .lm-TabBar-tab.lm-mod-closable.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: var(--jp-inverse-layout-color3);
}

/* CSS for icons in status bar */
#jp-main-statusbar .jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}

#jp-main-statusbar .jp-mod-selected .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}
/* special handling for splash icon CSS. While the theme CSS reloads during
   splash, the splash icon can loose theming. To prevent that, we set a
   default for its color variable */
:root {
  --jp-warn-color0: var(--md-orange-700);
}

/* not sure what to do with this one, used in filebrowser listing */
.jp-DragIcon {
  margin-right: 4px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * Support for alt colors for icons as inline SVG HTMLElements
 */

/* alt recolor the primary elements of an icon */
.jp-icon-alt .jp-icon0[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-alt .jp-icon1[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-alt .jp-icon2[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-alt .jp-icon3[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-alt .jp-icon4[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-alt .jp-icon0[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-alt .jp-icon1[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-alt .jp-icon2[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-alt .jp-icon3[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-alt .jp-icon4[stroke] {
  stroke: var(--jp-layout-color4);
}

/* alt recolor the accent elements of an icon */
.jp-icon-alt .jp-icon-accent0[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-alt .jp-icon-accent1[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-alt .jp-icon-accent2[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-alt .jp-icon-accent3[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-alt .jp-icon-accent4[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-alt .jp-icon-accent0[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-alt .jp-icon-accent1[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-alt .jp-icon-accent2[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-alt .jp-icon-accent3[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-alt .jp-icon-accent4[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-icon-hoverShow:not(:hover) svg {
  display: none !important;
}

/**
 * Support for hover colors for icons as inline SVG HTMLElements
 */

/**
 * regular colors
 */

/* recolor the primary elements of an icon */
.jp-icon-hover :hover .jp-icon0-hover[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-hover :hover .jp-icon1-hover[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-hover :hover .jp-icon2-hover[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-hover :hover .jp-icon3-hover[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-hover :hover .jp-icon4-hover[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-hover :hover .jp-icon0-hover[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-hover :hover .jp-icon1-hover[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-hover :hover .jp-icon2-hover[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-hover :hover .jp-icon3-hover[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-hover :hover .jp-icon4-hover[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/* recolor the accent elements of an icon */
.jp-icon-hover :hover .jp-icon-accent0-hover[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-hover :hover .jp-icon-accent1-hover[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-hover :hover .jp-icon-accent2-hover[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-hover :hover .jp-icon-accent3-hover[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-hover :hover .jp-icon-accent4-hover[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-hover :hover .jp-icon-accent0-hover[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-hover :hover .jp-icon-accent1-hover[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-hover :hover .jp-icon-accent2-hover[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-hover :hover .jp-icon-accent3-hover[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-hover :hover .jp-icon-accent4-hover[stroke] {
  stroke: var(--jp-layout-color4);
}

/* set the color of an icon to transparent */
.jp-icon-hover :hover .jp-icon-none-hover[fill] {
  fill: none;
}

.jp-icon-hover :hover .jp-icon-none-hover[stroke] {
  stroke: none;
}

/**
 * inverse colors
 */

/* inverse recolor the primary elements of an icon */
.jp-icon-hover.jp-icon-alt :hover .jp-icon0-hover[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon1-hover[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon2-hover[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon3-hover[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon4-hover[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-hover.jp-icon-alt :hover .jp-icon0-hover[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon1-hover[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon2-hover[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon3-hover[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon4-hover[stroke] {
  stroke: var(--jp-layout-color4);
}

/* inverse recolor the accent elements of an icon */
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent0-hover[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent1-hover[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent2-hover[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent3-hover[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent4-hover[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent0-hover[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent1-hover[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent2-hover[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent3-hover[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent4-hover[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-switch {
  display: flex;
  align-items: center;
  padding-left: 4px;
  padding-right: 4px;
  font-size: var(--jp-ui-font-size1);
  background-color: transparent;
  color: var(--jp-ui-font-color1);
  border: none;
  height: 20px;
}

.jp-switch:hover {
  background-color: var(--jp-layout-color2);
}

.jp-switch-label {
  margin-right: 5px;
}

.jp-switch-track {
  cursor: pointer;
  background-color: var(--jp-border-color1);
  -webkit-transition: 0.4s;
  transition: 0.4s;
  border-radius: 34px;
  height: 16px;
  width: 35px;
  position: relative;
}

.jp-switch-track::before {
  content: '';
  position: absolute;
  height: 10px;
  width: 10px;
  margin: 3px;
  left: 0px;
  background-color: var(--jp-ui-inverse-font-color1);
  -webkit-transition: 0.4s;
  transition: 0.4s;
  border-radius: 50%;
}

.jp-switch[aria-checked='true'] .jp-switch-track {
  background-color: var(--jp-warn-color0);
}

.jp-switch[aria-checked='true'] .jp-switch-track::before {
  /* track width (35) - margins (3 + 3) - thumb width (10) */
  left: 19px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/* Sibling imports */

/* Override Blueprint's _reset.scss styles */
html {
  box-sizing: unset;
}

*,
*::before,
*::after {
  box-sizing: unset;
}

body {
  color: unset;
  font-family: var(--jp-ui-font-family);
}

p {
  margin-top: unset;
  margin-bottom: unset;
}

small {
  font-size: unset;
}

strong {
  font-weight: unset;
}

/* Override Blueprint's _typography.scss styles */
a {
  text-decoration: unset;
  color: unset;
}
a:hover {
  text-decoration: unset;
  color: unset;
}

/* Override Blueprint's _accessibility.scss styles */
:focus {
  outline: unset;
  outline-offset: unset;
  -moz-outline-radius: unset;
}

/* Styles for ui-components */
.jp-Button {
  border-radius: var(--jp-border-radius);
  padding: 0px 12px;
  font-size: var(--jp-ui-font-size1);
}

/* Use our own theme for hover styles */
button.jp-Button.bp3-button.bp3-minimal:hover {
  background-color: var(--jp-layout-color2);
}
.jp-Button.minimal {
  color: unset !important;
}

.jp-Button.jp-ToolbarButtonComponent {
  text-transform: none;
}

.jp-InputGroup input {
  box-sizing: border-box;
  border-radius: 0;
  background-color: transparent;
  color: var(--jp-ui-font-color0);
  box-shadow: inset 0 0 0 var(--jp-border-width) var(--jp-input-border-color);
}

.jp-InputGroup input:focus {
  box-shadow: inset 0 0 0 var(--jp-border-width)
      var(--jp-input-active-box-shadow-color),
    inset 0 0 0 3px var(--jp-input-active-box-shadow-color);
}

.jp-InputGroup input::placeholder,
input::placeholder {
  color: var(--jp-ui-font-color3);
}

.jp-BPIcon {
  display: inline-block;
  vertical-align: middle;
  margin: auto;
}

/* Stop blueprint futzing with our icon fills */
.bp3-icon.jp-BPIcon > svg:not([fill]) {
  fill: var(--jp-inverse-layout-color3);
}

.jp-InputGroupAction {
  padding: 6px;
}

.jp-HTMLSelect.jp-DefaultStyle select {
  background-color: initial;
  border: none;
  border-radius: 0;
  box-shadow: none;
  color: var(--jp-ui-font-color0);
  display: block;
  font-size: var(--jp-ui-font-size1);
  height: 24px;
  line-height: 14px;
  padding: 0 25px 0 10px;
  text-align: left;
  -moz-appearance: none;
  -webkit-appearance: none;
}

/* Use our own theme for hover and option styles */
.jp-HTMLSelect.jp-DefaultStyle select:hover,
.jp-HTMLSelect.jp-DefaultStyle select > option {
  background-color: var(--jp-layout-color2);
  color: var(--jp-ui-font-color0);
}
select {
  box-sizing: border-box;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Collapse {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-top: 1px solid var(--jp-border-color2);
  border-bottom: 1px solid var(--jp-border-color2);
}

.jp-Collapse-header {
  padding: 1px 12px;
  color: var(--jp-ui-font-color1);
  background-color: var(--jp-layout-color1);
  font-size: var(--jp-ui-font-size2);
}

.jp-Collapse-header:hover {
  background-color: var(--jp-layout-color2);
}

.jp-Collapse-contents {
  padding: 0px 12px 0px 12px;
  background-color: var(--jp-layout-color1);
  color: var(--jp-ui-font-color1);
  overflow: auto;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-commandpalette-search-height: 28px;
}

/*-----------------------------------------------------------------------------
| Overall styles
|----------------------------------------------------------------------------*/

.lm-CommandPalette {
  padding-bottom: 0px;
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
}

/*-----------------------------------------------------------------------------
| Modal variant
|----------------------------------------------------------------------------*/

.jp-ModalCommandPalette {
  position: absolute;
  z-index: 10000;
  top: 38px;
  left: 30%;
  margin: 0;
  padding: 4px;
  width: 40%;
  box-shadow: var(--jp-elevation-z4);
  border-radius: 4px;
  background: var(--jp-layout-color0);
}

.jp-ModalCommandPalette .lm-CommandPalette {
  max-height: 40vh;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-close-icon::after {
  display: none;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-CommandPalette-header {
  display: none;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-CommandPalette-item {
  margin-left: 4px;
  margin-right: 4px;
}

.jp-ModalCommandPalette
  .lm-CommandPalette
  .lm-CommandPalette-item.lm-mod-disabled {
  display: none;
}

/*-----------------------------------------------------------------------------
| Search
|----------------------------------------------------------------------------*/

.lm-CommandPalette-search {
  padding: 4px;
  background-color: var(--jp-layout-color1);
  z-index: 2;
}

.lm-CommandPalette-wrapper {
  overflow: overlay;
  padding: 0px 9px;
  background-color: var(--jp-input-active-background);
  height: 30px;
  box-shadow: inset 0 0 0 var(--jp-border-width) var(--jp-input-border-color);
}

.lm-CommandPalette.lm-mod-focused .lm-CommandPalette-wrapper {
  box-shadow: inset 0 0 0 1px var(--jp-input-active-box-shadow-color),
    inset 0 0 0 3px var(--jp-input-active-box-shadow-color);
}

.jp-SearchIconGroup {
  color: white;
  background-color: var(--jp-brand-color1);
  position: absolute;
  top: 4px;
  right: 4px;
  padding: 5px 5px 1px 5px;
}

.jp-SearchIconGroup svg {
  height: 20px;
  width: 20px;
}

.jp-SearchIconGroup .jp-icon3[fill] {
  fill: var(--jp-layout-color0);
}

.lm-CommandPalette-input {
  background: transparent;
  width: calc(100% - 18px);
  float: left;
  border: none;
  outline: none;
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  line-height: var(--jp-private-commandpalette-search-height);
}

.lm-CommandPalette-input::-webkit-input-placeholder,
.lm-CommandPalette-input::-moz-placeholder,
.lm-CommandPalette-input:-ms-input-placeholder {
  color: var(--jp-ui-font-color2);
  font-size: var(--jp-ui-font-size1);
}

/*-----------------------------------------------------------------------------
| Results
|----------------------------------------------------------------------------*/

.lm-CommandPalette-header:first-child {
  margin-top: 0px;
}

.lm-CommandPalette-header {
  border-bottom: solid var(--jp-border-width) var(--jp-border-color2);
  color: var(--jp-ui-font-color1);
  cursor: pointer;
  display: flex;
  font-size: var(--jp-ui-font-size0);
  font-weight: 600;
  letter-spacing: 1px;
  margin-top: 8px;
  padding: 8px 0 8px 12px;
  text-transform: uppercase;
}

.lm-CommandPalette-header.lm-mod-active {
  background: var(--jp-layout-color2);
}

.lm-CommandPalette-header > mark {
  background-color: transparent;
  font-weight: bold;
  color: var(--jp-ui-font-color1);
}

.lm-CommandPalette-item {
  padding: 4px 12px 4px 4px;
  color: var(--jp-ui-font-color1);
  font-size: var(--jp-ui-font-size1);
  font-weight: 400;
  display: flex;
}

.lm-CommandPalette-item.lm-mod-disabled {
  color: var(--jp-ui-font-color2);
}

.lm-CommandPalette-item.lm-mod-active {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.lm-CommandPalette-item.lm-mod-active .lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-inverse-font-color0);
}

.lm-CommandPalette-item.lm-mod-active .jp-icon-selectable[fill] {
  fill: var(--jp-layout-color0);
}

.lm-CommandPalette-item.lm-mod-active .lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-inverse-font-color0);
}

.lm-CommandPalette-item.lm-mod-active:hover:not(.lm-mod-disabled) {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.lm-CommandPalette-item:hover:not(.lm-mod-active):not(.lm-mod-disabled) {
  background: var(--jp-layout-color2);
}

.lm-CommandPalette-itemContent {
  overflow: hidden;
}

.lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-font-color0);
  background-color: transparent;
  font-weight: bold;
}

.lm-CommandPalette-item.lm-mod-disabled mark {
  color: var(--jp-ui-font-color2);
}

.lm-CommandPalette-item .lm-CommandPalette-itemIcon {
  margin: 0 4px 0 0;
  position: relative;
  width: 16px;
  top: 2px;
  flex: 0 0 auto;
}

.lm-CommandPalette-item.lm-mod-disabled .lm-CommandPalette-itemIcon {
  opacity: 0.6;
}

.lm-CommandPalette-item .lm-CommandPalette-itemShortcut {
  flex: 0 0 auto;
}

.lm-CommandPalette-itemCaption {
  display: none;
}

.lm-CommandPalette-content {
  background-color: var(--jp-layout-color1);
}

.lm-CommandPalette-content:empty:after {
  content: 'No results';
  margin: auto;
  margin-top: 20px;
  width: 100px;
  display: block;
  font-size: var(--jp-ui-font-size2);
  font-family: var(--jp-ui-font-family);
  font-weight: lighter;
}

.lm-CommandPalette-emptyMessage {
  text-align: center;
  margin-top: 24px;
  line-height: 1.32;
  padding: 0px 8px;
  color: var(--jp-content-font-color3);
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Dialog {
  position: absolute;
  z-index: 10000;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  top: 0px;
  left: 0px;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background: var(--jp-dialog-background);
}

.jp-Dialog-content {
  display: flex;
  flex-direction: column;
  margin-left: auto;
  margin-right: auto;
  background: var(--jp-layout-color1);
  padding: 24px;
  padding-bottom: 12px;
  min-width: 300px;
  min-height: 150px;
  max-width: 1000px;
  max-height: 500px;
  box-sizing: border-box;
  box-shadow: var(--jp-elevation-z20);
  word-wrap: break-word;
  border-radius: var(--jp-border-radius);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color1);
  resize: both;
}

.jp-Dialog-button {
  overflow: visible;
}

button.jp-Dialog-button:focus {
  outline: 1px solid var(--jp-brand-color1);
  outline-offset: 4px;
  -moz-outline-radius: 0px;
}

button.jp-Dialog-button:focus::-moz-focus-inner {
  border: 0;
}

button.jp-Dialog-close-button {
  padding: 0;
  height: 100%;
  min-width: unset;
  min-height: unset;
}

.jp-Dialog-header {
  display: flex;
  justify-content: space-between;
  flex: 0 0 auto;
  padding-bottom: 12px;
  font-size: var(--jp-ui-font-size3);
  font-weight: 400;
  color: var(--jp-ui-font-color0);
}

.jp-Dialog-body {
  display: flex;
  flex-direction: column;
  flex: 1 1 auto;
  font-size: var(--jp-ui-font-size1);
  background: var(--jp-layout-color1);
  overflow: auto;
}

.jp-Dialog-footer {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  flex: 0 0 auto;
  margin-left: -12px;
  margin-right: -12px;
  padding: 12px;
}

.jp-Dialog-title {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.jp-Dialog-body > .jp-select-wrapper {
  width: 100%;
}

.jp-Dialog-body > button {
  padding: 0px 16px;
}

.jp-Dialog-body > label {
  line-height: 1.4;
  color: var(--jp-ui-font-color0);
}

.jp-Dialog-button.jp-mod-styled:not(:last-child) {
  margin-right: 12px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-HoverBox {
  position: fixed;
}

.jp-HoverBox.jp-mod-outofview {
  display: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-IFrame {
  width: 100%;
  height: 100%;
}

.jp-IFrame > iframe {
  border: none;
}

/*
When drag events occur, `p-mod-override-cursor` is added to the body.
Because iframes steal all cursor events, the following two rules are necessary
to suppress pointer events while resize drags are occurring. There may be a
better solution to this problem.
*/
body.lm-mod-override-cursor .jp-IFrame {
  position: relative;
}

body.lm-mod-override-cursor .jp-IFrame:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: transparent;
}

.jp-Input-Boolean-Dialog {
  flex-direction: row-reverse;
  align-items: end;
  width: 100%;
}

.jp-Input-Boolean-Dialog > label {
  flex: 1 1 auto;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-MainAreaWidget > :focus {
  outline: none;
}

/**
 * google-material-color v1.2.6
 * https://github.com/danlevan/google-material-color
 */
:root {
  --md-red-50: #ffebee;
  --md-red-100: #ffcdd2;
  --md-red-200: #ef9a9a;
  --md-red-300: #e57373;
  --md-red-400: #ef5350;
  --md-red-500: #f44336;
  --md-red-600: #e53935;
  --md-red-700: #d32f2f;
  --md-red-800: #c62828;
  --md-red-900: #b71c1c;
  --md-red-A100: #ff8a80;
  --md-red-A200: #ff5252;
  --md-red-A400: #ff1744;
  --md-red-A700: #d50000;

  --md-pink-50: #fce4ec;
  --md-pink-100: #f8bbd0;
  --md-pink-200: #f48fb1;
  --md-pink-300: #f06292;
  --md-pink-400: #ec407a;
  --md-pink-500: #e91e63;
  --md-pink-600: #d81b60;
  --md-pink-700: #c2185b;
  --md-pink-800: #ad1457;
  --md-pink-900: #880e4f;
  --md-pink-A100: #ff80ab;
  --md-pink-A200: #ff4081;
  --md-pink-A400: #f50057;
  --md-pink-A700: #c51162;

  --md-purple-50: #f3e5f5;
  --md-purple-100: #e1bee7;
  --md-purple-200: #ce93d8;
  --md-purple-300: #ba68c8;
  --md-purple-400: #ab47bc;
  --md-purple-500: #9c27b0;
  --md-purple-600: #8e24aa;
  --md-purple-700: #7b1fa2;
  --md-purple-800: #6a1b9a;
  --md-purple-900: #4a148c;
  --md-purple-A100: #ea80fc;
  --md-purple-A200: #e040fb;
  --md-purple-A400: #d500f9;
  --md-purple-A700: #aa00ff;

  --md-deep-purple-50: #ede7f6;
  --md-deep-purple-100: #d1c4e9;
  --md-deep-purple-200: #b39ddb;
  --md-deep-purple-300: #9575cd;
  --md-deep-purple-400: #7e57c2;
  --md-deep-purple-500: #673ab7;
  --md-deep-purple-600: #5e35b1;
  --md-deep-purple-700: #512da8;
  --md-deep-purple-800: #4527a0;
  --md-deep-purple-900: #311b92;
  --md-deep-purple-A100: #b388ff;
  --md-deep-purple-A200: #7c4dff;
  --md-deep-purple-A400: #651fff;
  --md-deep-purple-A700: #6200ea;

  --md-indigo-50: #e8eaf6;
  --md-indigo-100: #c5cae9;
  --md-indigo-200: #9fa8da;
  --md-indigo-300: #7986cb;
  --md-indigo-400: #5c6bc0;
  --md-indigo-500: #3f51b5;
  --md-indigo-600: #3949ab;
  --md-indigo-700: #303f9f;
  --md-indigo-800: #283593;
  --md-indigo-900: #1a237e;
  --md-indigo-A100: #8c9eff;
  --md-indigo-A200: #536dfe;
  --md-indigo-A400: #3d5afe;
  --md-indigo-A700: #304ffe;

  --md-blue-50: #e3f2fd;
  --md-blue-100: #bbdefb;
  --md-blue-200: #90caf9;
  --md-blue-300: #64b5f6;
  --md-blue-400: #42a5f5;
  --md-blue-500: #2196f3;
  --md-blue-600: #1e88e5;
  --md-blue-700: #1976d2;
  --md-blue-800: #1565c0;
  --md-blue-900: #0d47a1;
  --md-blue-A100: #82b1ff;
  --md-blue-A200: #448aff;
  --md-blue-A400: #2979ff;
  --md-blue-A700: #2962ff;

  --md-light-blue-50: #e1f5fe;
  --md-light-blue-100: #b3e5fc;
  --md-light-blue-200: #81d4fa;
  --md-light-blue-300: #4fc3f7;
  --md-light-blue-400: #29b6f6;
  --md-light-blue-500: #03a9f4;
  --md-light-blue-600: #039be5;
  --md-light-blue-700: #0288d1;
  --md-light-blue-800: #0277bd;
  --md-light-blue-900: #01579b;
  --md-light-blue-A100: #80d8ff;
  --md-light-blue-A200: #40c4ff;
  --md-light-blue-A400: #00b0ff;
  --md-light-blue-A700: #0091ea;

  --md-cyan-50: #e0f7fa;
  --md-cyan-100: #b2ebf2;
  --md-cyan-200: #80deea;
  --md-cyan-300: #4dd0e1;
  --md-cyan-400: #26c6da;
  --md-cyan-500: #00bcd4;
  --md-cyan-600: #00acc1;
  --md-cyan-700: #0097a7;
  --md-cyan-800: #00838f;
  --md-cyan-900: #006064;
  --md-cyan-A100: #84ffff;
  --md-cyan-A200: #18ffff;
  --md-cyan-A400: #00e5ff;
  --md-cyan-A700: #00b8d4;

  --md-teal-50: #e0f2f1;
  --md-teal-100: #b2dfdb;
  --md-teal-200: #80cbc4;
  --md-teal-300: #4db6ac;
  --md-teal-400: #26a69a;
  --md-teal-500: #009688;
  --md-teal-600: #00897b;
  --md-teal-700: #00796b;
  --md-teal-800: #00695c;
  --md-teal-900: #004d40;
  --md-teal-A100: #a7ffeb;
  --md-teal-A200: #64ffda;
  --md-teal-A400: #1de9b6;
  --md-teal-A700: #00bfa5;

  --md-green-50: #e8f5e9;
  --md-green-100: #c8e6c9;
  --md-green-200: #a5d6a7;
  --md-green-300: #81c784;
  --md-green-400: #66bb6a;
  --md-green-500: #4caf50;
  --md-green-600: #43a047;
  --md-green-700: #388e3c;
  --md-green-800: #2e7d32;
  --md-green-900: #1b5e20;
  --md-green-A100: #b9f6ca;
  --md-green-A200: #69f0ae;
  --md-green-A400: #00e676;
  --md-green-A700: #00c853;

  --md-light-green-50: #f1f8e9;
  --md-light-green-100: #dcedc8;
  --md-light-green-200: #c5e1a5;
  --md-light-green-300: #aed581;
  --md-light-green-400: #9ccc65;
  --md-light-green-500: #8bc34a;
  --md-light-green-600: #7cb342;
  --md-light-green-700: #689f38;
  --md-light-green-800: #558b2f;
  --md-light-green-900: #33691e;
  --md-light-green-A100: #ccff90;
  --md-light-green-A200: #b2ff59;
  --md-light-green-A400: #76ff03;
  --md-light-green-A700: #64dd17;

  --md-lime-50: #f9fbe7;
  --md-lime-100: #f0f4c3;
  --md-lime-200: #e6ee9c;
  --md-lime-300: #dce775;
  --md-lime-400: #d4e157;
  --md-lime-500: #cddc39;
  --md-lime-600: #c0ca33;
  --md-lime-700: #afb42b;
  --md-lime-800: #9e9d24;
  --md-lime-900: #827717;
  --md-lime-A100: #f4ff81;
  --md-lime-A200: #eeff41;
  --md-lime-A400: #c6ff00;
  --md-lime-A700: #aeea00;

  --md-yellow-50: #fffde7;
  --md-yellow-100: #fff9c4;
  --md-yellow-200: #fff59d;
  --md-yellow-300: #fff176;
  --md-yellow-400: #ffee58;
  --md-yellow-500: #ffeb3b;
  --md-yellow-600: #fdd835;
  --md-yellow-700: #fbc02d;
  --md-yellow-800: #f9a825;
  --md-yellow-900: #f57f17;
  --md-yellow-A100: #ffff8d;
  --md-yellow-A200: #ffff00;
  --md-yellow-A400: #ffea00;
  --md-yellow-A700: #ffd600;

  --md-amber-50: #fff8e1;
  --md-amber-100: #ffecb3;
  --md-amber-200: #ffe082;
  --md-amber-300: #ffd54f;
  --md-amber-400: #ffca28;
  --md-amber-500: #ffc107;
  --md-amber-600: #ffb300;
  --md-amber-700: #ffa000;
  --md-amber-800: #ff8f00;
  --md-amber-900: #ff6f00;
  --md-amber-A100: #ffe57f;
  --md-amber-A200: #ffd740;
  --md-amber-A400: #ffc400;
  --md-amber-A700: #ffab00;

  --md-orange-50: #fff3e0;
  --md-orange-100: #ffe0b2;
  --md-orange-200: #ffcc80;
  --md-orange-300: #ffb74d;
  --md-orange-400: #ffa726;
  --md-orange-500: #ff9800;
  --md-orange-600: #fb8c00;
  --md-orange-700: #f57c00;
  --md-orange-800: #ef6c00;
  --md-orange-900: #e65100;
  --md-orange-A100: #ffd180;
  --md-orange-A200: #ffab40;
  --md-orange-A400: #ff9100;
  --md-orange-A700: #ff6d00;

  --md-deep-orange-50: #fbe9e7;
  --md-deep-orange-100: #ffccbc;
  --md-deep-orange-200: #ffab91;
  --md-deep-orange-300: #ff8a65;
  --md-deep-orange-400: #ff7043;
  --md-deep-orange-500: #ff5722;
  --md-deep-orange-600: #f4511e;
  --md-deep-orange-700: #e64a19;
  --md-deep-orange-800: #d84315;
  --md-deep-orange-900: #bf360c;
  --md-deep-orange-A100: #ff9e80;
  --md-deep-orange-A200: #ff6e40;
  --md-deep-orange-A400: #ff3d00;
  --md-deep-orange-A700: #dd2c00;

  --md-brown-50: #efebe9;
  --md-brown-100: #d7ccc8;
  --md-brown-200: #bcaaa4;
  --md-brown-300: #a1887f;
  --md-brown-400: #8d6e63;
  --md-brown-500: #795548;
  --md-brown-600: #6d4c41;
  --md-brown-700: #5d4037;
  --md-brown-800: #4e342e;
  --md-brown-900: #3e2723;

  --md-grey-50: #fafafa;
  --md-grey-100: #f5f5f5;
  --md-grey-200: #eeeeee;
  --md-grey-300: #e0e0e0;
  --md-grey-400: #bdbdbd;
  --md-grey-500: #9e9e9e;
  --md-grey-600: #757575;
  --md-grey-700: #616161;
  --md-grey-800: #424242;
  --md-grey-900: #212121;

  --md-blue-grey-50: #eceff1;
  --md-blue-grey-100: #cfd8dc;
  --md-blue-grey-200: #b0bec5;
  --md-blue-grey-300: #90a4ae;
  --md-blue-grey-400: #78909c;
  --md-blue-grey-500: #607d8b;
  --md-blue-grey-600: #546e7a;
  --md-blue-grey-700: #455a64;
  --md-blue-grey-800: #37474f;
  --md-blue-grey-900: #263238;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Spinner {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: var(--jp-layout-color0);
  outline: none;
}

.jp-SpinnerContent {
  font-size: 10px;
  margin: 50px auto;
  text-indent: -9999em;
  width: 3em;
  height: 3em;
  border-radius: 50%;
  background: var(--jp-brand-color3);
  background: linear-gradient(
    to right,
    #f37626 10%,
    rgba(255, 255, 255, 0) 42%
  );
  position: relative;
  animation: load3 1s infinite linear, fadeIn 1s;
}

.jp-SpinnerContent:before {
  width: 50%;
  height: 50%;
  background: #f37626;
  border-radius: 100% 0 0 0;
  position: absolute;
  top: 0;
  left: 0;
  content: '';
}

.jp-SpinnerContent:after {
  background: var(--jp-layout-color0);
  width: 75%;
  height: 75%;
  border-radius: 50%;
  content: '';
  margin: auto;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes load3 {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

button.jp-mod-styled {
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  border: none;
  box-sizing: border-box;
  text-align: center;
  line-height: 32px;
  height: 32px;
  padding: 0px 12px;
  letter-spacing: 0.8px;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

input.jp-mod-styled {
  background: var(--jp-input-background);
  height: 28px;
  box-sizing: border-box;
  border: var(--jp-border-width) solid var(--jp-border-color1);
  padding-left: 7px;
  padding-right: 7px;
  font-size: var(--jp-ui-font-size2);
  color: var(--jp-ui-font-color0);
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

input[type='checkbox'].jp-mod-styled {
  appearance: checkbox;
  -webkit-appearance: checkbox;
  -moz-appearance: checkbox;
  height: auto;
}

input.jp-mod-styled:focus {
  border: var(--jp-border-width) solid var(--md-blue-500);
  box-shadow: inset 0 0 4px var(--md-blue-300);
}

.jp-FileDialog-Checkbox {
  margin-top: 35px;
  display: flex;
  flex-direction: row;
  align-items: end;
  width: 100%;
}

.jp-FileDialog-Checkbox > label {
  flex: 1 1 auto;
}

.jp-select-wrapper {
  display: flex;
  position: relative;
  flex-direction: column;
  padding: 1px;
  background-color: var(--jp-layout-color1);
  height: 28px;
  box-sizing: border-box;
  margin-bottom: 12px;
}

.jp-select-wrapper.jp-mod-focused select.jp-mod-styled {
  border: var(--jp-border-width) solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
  background-color: var(--jp-input-active-background);
}

select.jp-mod-styled:hover {
  background-color: var(--jp-layout-color1);
  cursor: pointer;
  color: var(--jp-ui-font-color0);
  background-color: var(--jp-input-hover-background);
  box-shadow: inset 0 0px 1px rgba(0, 0, 0, 0.5);
}

select.jp-mod-styled {
  flex: 1 1 auto;
  height: 32px;
  width: 100%;
  font-size: var(--jp-ui-font-size2);
  background: var(--jp-input-background);
  color: var(--jp-ui-font-color0);
  padding: 0 25px 0 8px;
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  border-radius: 0px;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

:root {
  --jp-private-toolbar-height: calc(
    28px + var(--jp-border-width)
  ); /* leave 28px for content */
}

.jp-Toolbar {
  color: var(--jp-ui-font-color1);
  flex: 0 0 auto;
  display: flex;
  flex-direction: row;
  border-bottom: var(--jp-border-width) solid var(--jp-toolbar-border-color);
  box-shadow: var(--jp-toolbar-box-shadow);
  background: var(--jp-toolbar-background);
  min-height: var(--jp-toolbar-micro-height);
  padding: 2px;
  z-index: 1;
  overflow-x: auto;
}

/* Toolbar items */

.jp-Toolbar > .jp-Toolbar-item.jp-Toolbar-spacer {
  flex-grow: 1;
  flex-shrink: 1;
}

.jp-Toolbar-item.jp-Toolbar-kernelStatus {
  display: inline-block;
  width: 32px;
  background-repeat: no-repeat;
  background-position: center;
  background-size: 16px;
}

.jp-Toolbar > .jp-Toolbar-item {
  flex: 0 0 auto;
  display: flex;
  padding-left: 1px;
  padding-right: 1px;
  font-size: var(--jp-ui-font-size1);
  line-height: var(--jp-private-toolbar-height);
  height: 100%;
}

/* Toolbar buttons */

/* This is the div we use to wrap the react component into a Widget */
div.jp-ToolbarButton {
  color: transparent;
  border: none;
  box-sizing: border-box;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 0px;
  margin: 0px;
}

button.jp-ToolbarButtonComponent {
  background: var(--jp-layout-color1);
  border: none;
  box-sizing: border-box;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 0px 6px;
  margin: 0px;
  height: 24px;
  border-radius: var(--jp-border-radius);
  display: flex;
  align-items: center;
  text-align: center;
  font-size: 14px;
  min-width: unset;
  min-height: unset;
}

button.jp-ToolbarButtonComponent:disabled {
  opacity: 0.4;
}

button.jp-ToolbarButtonComponent span {
  padding: 0px;
  flex: 0 0 auto;
}

button.jp-ToolbarButtonComponent .jp-ToolbarButtonComponent-label {
  font-size: var(--jp-ui-font-size1);
  line-height: 100%;
  padding-left: 2px;
  color: var(--jp-ui-font-color1);
}

#jp-main-dock-panel[data-mode='single-document']
  .jp-MainAreaWidget
  > .jp-Toolbar.jp-Toolbar-micro {
  padding: 0;
  min-height: 0;
}

#jp-main-dock-panel[data-mode='single-document']
  .jp-MainAreaWidget
  > .jp-Toolbar {
  border: none;
  box-shadow: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ body.p-mod-override-cursor *, /* </DEPRECATED> */
body.lm-mod-override-cursor * {
  cursor: inherit !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-JSONEditor {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.jp-JSONEditor-host {
  flex: 1 1 auto;
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  border-radius: 0px;
  background: var(--jp-layout-color0);
  min-height: 50px;
  padding: 1px;
}

.jp-JSONEditor.jp-mod-error .jp-JSONEditor-host {
  border-color: red;
  outline-color: red;
}

.jp-JSONEditor-header {
  display: flex;
  flex: 1 0 auto;
  padding: 0 0 0 12px;
}

.jp-JSONEditor-header label {
  flex: 0 0 auto;
}

.jp-JSONEditor-commitButton {
  height: 16px;
  width: 16px;
  background-size: 18px;
  background-repeat: no-repeat;
  background-position: center;
}

.jp-JSONEditor-host.jp-mod-focused {
  background-color: var(--jp-input-active-background);
  border: 1px solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
}

.jp-Editor.jp-mod-dropTarget {
  border: var(--jp-border-width) solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
}

/* BASICS */

.CodeMirror {
  /* Set height, width, borders, and global font properties here */
  font-family: monospace;
  height: 300px;
  color: black;
  direction: ltr;
}

/* PADDING */

.CodeMirror-lines {
  padding: 4px 0; /* Vertical padding around content */
}
.CodeMirror pre.CodeMirror-line,
.CodeMirror pre.CodeMirror-line-like {
  padding: 0 4px; /* Horizontal padding of content */
}

.CodeMirror-scrollbar-filler, .CodeMirror-gutter-filler {
  background-color: white; /* The little square between H and V scrollbars */
}

/* GUTTER */

.CodeMirror-gutters {
  border-right: 1px solid #ddd;
  background-color: #f7f7f7;
  white-space: nowrap;
}
.CodeMirror-linenumbers {}
.CodeMirror-linenumber {
  padding: 0 3px 0 5px;
  min-width: 20px;
  text-align: right;
  color: #999;
  white-space: nowrap;
}

.CodeMirror-guttermarker { color: black; }
.CodeMirror-guttermarker-subtle { color: #999; }

/* CURSOR */

.CodeMirror-cursor {
  border-left: 1px solid black;
  border-right: none;
  width: 0;
}
/* Shown when moving in bi-directional text */
.CodeMirror div.CodeMirror-secondarycursor {
  border-left: 1px solid silver;
}
.cm-fat-cursor .CodeMirror-cursor {
  width: auto;
  border: 0 !important;
  background: #7e7;
}
.cm-fat-cursor div.CodeMirror-cursors {
  z-index: 1;
}
.cm-fat-cursor-mark {
  background-color: rgba(20, 255, 20, 0.5);
  -webkit-animation: blink 1.06s steps(1) infinite;
  -moz-animation: blink 1.06s steps(1) infinite;
  animation: blink 1.06s steps(1) infinite;
}
.cm-animate-fat-cursor {
  width: auto;
  border: 0;
  -webkit-animation: blink 1.06s steps(1) infinite;
  -moz-animation: blink 1.06s steps(1) infinite;
  animation: blink 1.06s steps(1) infinite;
  background-color: #7e7;
}
@-moz-keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}
@-webkit-keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}
@keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}

/* Can style cursor different in overwrite (non-insert) mode */
.CodeMirror-overwrite .CodeMirror-cursor {}

.cm-tab { display: inline-block; text-decoration: inherit; }

.CodeMirror-rulers {
  position: absolute;
  left: 0; right: 0; top: -50px; bottom: 0;
  overflow: hidden;
}
.CodeMirror-ruler {
  border-left: 1px solid #ccc;
  top: 0; bottom: 0;
  position: absolute;
}

/* DEFAULT THEME */

.cm-s-default .cm-header {color: blue;}
.cm-s-default .cm-quote {color: #090;}
.cm-negative {color: #d44;}
.cm-positive {color: #292;}
.cm-header, .cm-strong {font-weight: bold;}
.cm-em {font-style: italic;}
.cm-link {text-decoration: underline;}
.cm-strikethrough {text-decoration: line-through;}

.cm-s-default .cm-keyword {color: #708;}
.cm-s-default .cm-atom {color: #219;}
.cm-s-default .cm-number {color: #164;}
.cm-s-default .cm-def {color: #00f;}
.cm-s-default .cm-variable,
.cm-s-default .cm-punctuation,
.cm-s-default .cm-property,
.cm-s-default .cm-operator {}
.cm-s-default .cm-variable-2 {color: #05a;}
.cm-s-default .cm-variable-3, .cm-s-default .cm-type {color: #085;}
.cm-s-default .cm-comment {color: #a50;}
.cm-s-default .cm-string {color: #a11;}
.cm-s-default .cm-string-2 {color: #f50;}
.cm-s-default .cm-meta {color: #555;}
.cm-s-default .cm-qualifier {color: #555;}
.cm-s-default .cm-builtin {color: #30a;}
.cm-s-default .cm-bracket {color: #997;}
.cm-s-default .cm-tag {color: #170;}
.cm-s-default .cm-attribute {color: #00c;}
.cm-s-default .cm-hr {color: #999;}
.cm-s-default .cm-link {color: #00c;}

.cm-s-default .cm-error {color: #f00;}
.cm-invalidchar {color: #f00;}

.CodeMirror-composing { border-bottom: 2px solid; }

/* Default styles for common addons */

div.CodeMirror span.CodeMirror-matchingbracket {color: #0b0;}
div.CodeMirror span.CodeMirror-nonmatchingbracket {color: #a22;}
.CodeMirror-matchingtag { background: rgba(255, 150, 0, .3); }
.CodeMirror-activeline-background {background: #e8f2ff;}

/* STOP */

/* The rest of this file contains styles related to the mechanics of
   the editor. You probably shouldn't touch them. */

.CodeMirror {
  position: relative;
  overflow: hidden;
  background: white;
}

.CodeMirror-scroll {
  overflow: scroll !important; /* Things will break if this is overridden */
  /* 50px is the magic margin used to hide the element's real scrollbars */
  /* See overflow: hidden in .CodeMirror */
  margin-bottom: -50px; margin-right: -50px;
  padding-bottom: 50px;
  height: 100%;
  outline: none; /* Prevent dragging from highlighting the element */
  position: relative;
}
.CodeMirror-sizer {
  position: relative;
  border-right: 50px solid transparent;
}

/* The fake, visible scrollbars. Used to force redraw during scrolling
   before actual scrolling happens, thus preventing shaking and
   flickering artifacts. */
.CodeMirror-vscrollbar, .CodeMirror-hscrollbar, .CodeMirror-scrollbar-filler, .CodeMirror-gutter-filler {
  position: absolute;
  z-index: 6;
  display: none;
  outline: none;
}
.CodeMirror-vscrollbar {
  right: 0; top: 0;
  overflow-x: hidden;
  overflow-y: scroll;
}
.CodeMirror-hscrollbar {
  bottom: 0; left: 0;
  overflow-y: hidden;
  overflow-x: scroll;
}
.CodeMirror-scrollbar-filler {
  right: 0; bottom: 0;
}
.CodeMirror-gutter-filler {
  left: 0; bottom: 0;
}

.CodeMirror-gutters {
  position: absolute; left: 0; top: 0;
  min-height: 100%;
  z-index: 3;
}
.CodeMirror-gutter {
  white-space: normal;
  height: 100%;
  display: inline-block;
  vertical-align: top;
  margin-bottom: -50px;
}
.CodeMirror-gutter-wrapper {
  position: absolute;
  z-index: 4;
  background: none !important;
  border: none !important;
}
.CodeMirror-gutter-background {
  position: absolute;
  top: 0; bottom: 0;
  z-index: 4;
}
.CodeMirror-gutter-elt {
  position: absolute;
  cursor: default;
  z-index: 4;
}
.CodeMirror-gutter-wrapper ::selection { background-color: transparent }
.CodeMirror-gutter-wrapper ::-moz-selection { background-color: transparent }

.CodeMirror-lines {
  cursor: text;
  min-height: 1px; /* prevents collapsing before first draw */
}
.CodeMirror pre.CodeMirror-line,
.CodeMirror pre.CodeMirror-line-like {
  /* Reset some styles that the rest of the page might have set */
  -moz-border-radius: 0; -webkit-border-radius: 0; border-radius: 0;
  border-width: 0;
  background: transparent;
  font-family: inherit;
  font-size: inherit;
  margin: 0;
  white-space: pre;
  word-wrap: normal;
  line-height: inherit;
  color: inherit;
  z-index: 2;
  position: relative;
  overflow: visible;
  -webkit-tap-highlight-color: transparent;
  -webkit-font-variant-ligatures: contextual;
  font-variant-ligatures: contextual;
}
.CodeMirror-wrap pre.CodeMirror-line,
.CodeMirror-wrap pre.CodeMirror-line-like {
  word-wrap: break-word;
  white-space: pre-wrap;
  word-break: normal;
}

.CodeMirror-linebackground {
  position: absolute;
  left: 0; right: 0; top: 0; bottom: 0;
  z-index: 0;
}

.CodeMirror-linewidget {
  position: relative;
  z-index: 2;
  padding: 0.1px; /* Force widget margins to stay inside of the container */
}

.CodeMirror-widget {}

.CodeMirror-rtl pre { direction: rtl; }

.CodeMirror-code {
  outline: none;
}

/* Force content-box sizing for the elements where we expect it */
.CodeMirror-scroll,
.CodeMirror-sizer,
.CodeMirror-gutter,
.CodeMirror-gutters,
.CodeMirror-linenumber {
  -moz-box-sizing: content-box;
  box-sizing: content-box;
}

.CodeMirror-measure {
  position: absolute;
  width: 100%;
  height: 0;
  overflow: hidden;
  visibility: hidden;
}

.CodeMirror-cursor {
  position: absolute;
  pointer-events: none;
}
.CodeMirror-measure pre { position: static; }

div.CodeMirror-cursors {
  visibility: hidden;
  position: relative;
  z-index: 3;
}
div.CodeMirror-dragcursors {
  visibility: visible;
}

.CodeMirror-focused div.CodeMirror-cursors {
  visibility: visible;
}

.CodeMirror-selected { background: #d9d9d9; }
.CodeMirror-focused .CodeMirror-selected { background: #d7d4f0; }
.CodeMirror-crosshair { cursor: crosshair; }
.CodeMirror-line::selection, .CodeMirror-line > span::selection, .CodeMirror-line > span > span::selection { background: #d7d4f0; }
.CodeMirror-line::-moz-selection, .CodeMirror-line > span::-moz-selection, .CodeMirror-line > span > span::-moz-selection { background: #d7d4f0; }

.cm-searching {
  background-color: #ffa;
  background-color: rgba(255, 255, 0, .4);
}

/* Used to force a border model for a node */
.cm-force-border { padding-right: .1px; }

@media print {
  /* Hide the cursor when printing */
  .CodeMirror div.CodeMirror-cursors {
    visibility: hidden;
  }
}

/* See issue #2901 */
.cm-tab-wrap-hack:after { content: ''; }

/* Help users use markselection to safely style text background */
span.CodeMirror-selectedtext { background: none; }

.CodeMirror-dialog {
  position: absolute;
  left: 0; right: 0;
  background: inherit;
  z-index: 15;
  padding: .1em .8em;
  overflow: hidden;
  color: inherit;
}

.CodeMirror-dialog-top {
  border-bottom: 1px solid #eee;
  top: 0;
}

.CodeMirror-dialog-bottom {
  border-top: 1px solid #eee;
  bottom: 0;
}

.CodeMirror-dialog input {
  border: none;
  outline: none;
  background: transparent;
  width: 20em;
  color: inherit;
  font-family: monospace;
}

.CodeMirror-dialog button {
  font-size: 70%;
}

.CodeMirror-foldmarker {
  color: blue;
  text-shadow: #b9f 1px 1px 2px, #b9f -1px -1px 2px, #b9f 1px -1px 2px, #b9f -1px 1px 2px;
  font-family: arial;
  line-height: .3;
  cursor: pointer;
}
.CodeMirror-foldgutter {
  width: .7em;
}
.CodeMirror-foldgutter-open,
.CodeMirror-foldgutter-folded {
  cursor: pointer;
}
.CodeMirror-foldgutter-open:after {
  content: "\25BE";
}
.CodeMirror-foldgutter-folded:after {
  content: "\25B8";
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.CodeMirror {
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  font-family: var(--jp-code-font-family);
  border: 0;
  border-radius: 0;
  height: auto;
  /* Changed to auto to autogrow */
}

.CodeMirror pre {
  padding: 0 var(--jp-code-padding);
}

.jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-dialog {
  background-color: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
}

/* This causes https://github.com/jupyter/jupyterlab/issues/522 */
/* May not cause it not because we changed it! */
.CodeMirror-lines {
  padding: var(--jp-code-padding) 0;
}

.CodeMirror-linenumber {
  padding: 0 8px;
}

.jp-CodeMirrorEditor {
  cursor: text;
}

.jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
  border-left: var(--jp-code-cursor-width0) solid var(--jp-editor-cursor-color);
}

/* When zoomed out 67% and 33% on a screen of 1440 width x 900 height */
@media screen and (min-width: 2138px) and (max-width: 4319px) {
  .jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
    border-left: var(--jp-code-cursor-width1) solid
      var(--jp-editor-cursor-color);
  }
}

/* When zoomed out less than 33% */
@media screen and (min-width: 4320px) {
  .jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
    border-left: var(--jp-code-cursor-width2) solid
      var(--jp-editor-cursor-color);
  }
}

.CodeMirror.jp-mod-readOnly .CodeMirror-cursor {
  display: none;
}

.CodeMirror-gutters {
  border-right: 1px solid var(--jp-border-color2);
  background-color: var(--jp-layout-color0);
}

.jp-CollaboratorCursor {
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: none;
  border-bottom: 3px solid;
  background-clip: content-box;
  margin-left: -5px;
  margin-right: -5px;
}

.CodeMirror-selectedtext.cm-searching {
  background-color: var(--jp-search-selected-match-background-color) !important;
  color: var(--jp-search-selected-match-color) !important;
}

.cm-searching {
  background-color: var(
    --jp-search-unselected-match-background-color
  ) !important;
  color: var(--jp-search-unselected-match-color) !important;
}

.CodeMirror-focused .CodeMirror-selected {
  background-color: var(--jp-editor-selected-focused-background);
}

.CodeMirror-selected {
  background-color: var(--jp-editor-selected-background);
}

.jp-CollaboratorCursor-hover {
  position: absolute;
  z-index: 1;
  transform: translateX(-50%);
  color: white;
  border-radius: 3px;
  padding-left: 4px;
  padding-right: 4px;
  padding-top: 1px;
  padding-bottom: 1px;
  text-align: center;
  font-size: var(--jp-ui-font-size1);
  white-space: nowrap;
}

.jp-CodeMirror-ruler {
  border-left: 1px dashed var(--jp-border-color2);
}

/**
 * Here is our jupyter theme for CodeMirror syntax highlighting
 * This is used in our marked.js syntax highlighting and CodeMirror itself
 * The string "jupyter" is set in ../codemirror/widget.DEFAULT_CODEMIRROR_THEME
 * This came from the classic notebook, which came form highlight.js/GitHub
 */

/**
 * CodeMirror themes are handling the background/color in this way. This works
 * fine for CodeMirror editors outside the notebook, but the notebook styles
 * these things differently.
 */
.CodeMirror.cm-s-jupyter {
  background: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
}

/* In the notebook, we want this styling to be handled by its container */
.jp-CodeConsole .CodeMirror.cm-s-jupyter,
.jp-Notebook .CodeMirror.cm-s-jupyter {
  background: transparent;
}

.cm-s-jupyter .CodeMirror-cursor {
  border-left: var(--jp-code-cursor-width0) solid var(--jp-editor-cursor-color);
}
.cm-s-jupyter span.cm-keyword {
  color: var(--jp-mirror-editor-keyword-color);
  font-weight: bold;
}
.cm-s-jupyter span.cm-atom {
  color: var(--jp-mirror-editor-atom-color);
}
.cm-s-jupyter span.cm-number {
  color: var(--jp-mirror-editor-number-color);
}
.cm-s-jupyter span.cm-def {
  color: var(--jp-mirror-editor-def-color);
}
.cm-s-jupyter span.cm-variable {
  color: var(--jp-mirror-editor-variable-color);
}
.cm-s-jupyter span.cm-variable-2 {
  color: var(--jp-mirror-editor-variable-2-color);
}
.cm-s-jupyter span.cm-variable-3 {
  color: var(--jp-mirror-editor-variable-3-color);
}
.cm-s-jupyter span.cm-punctuation {
  color: var(--jp-mirror-editor-punctuation-color);
}
.cm-s-jupyter span.cm-property {
  color: var(--jp-mirror-editor-property-color);
}
.cm-s-jupyter span.cm-operator {
  color: var(--jp-mirror-editor-operator-color);
  font-weight: bold;
}
.cm-s-jupyter span.cm-comment {
  color: var(--jp-mirror-editor-comment-color);
  font-style: italic;
}
.cm-s-jupyter span.cm-string {
  color: var(--jp-mirror-editor-string-color);
}
.cm-s-jupyter span.cm-string-2 {
  color: var(--jp-mirror-editor-string-2-color);
}
.cm-s-jupyter span.cm-meta {
  color: var(--jp-mirror-editor-meta-color);
}
.cm-s-jupyter span.cm-qualifier {
  color: var(--jp-mirror-editor-qualifier-color);
}
.cm-s-jupyter span.cm-builtin {
  color: var(--jp-mirror-editor-builtin-color);
}
.cm-s-jupyter span.cm-bracket {
  color: var(--jp-mirror-editor-bracket-color);
}
.cm-s-jupyter span.cm-tag {
  color: var(--jp-mirror-editor-tag-color);
}
.cm-s-jupyter span.cm-attribute {
  color: var(--jp-mirror-editor-attribute-color);
}
.cm-s-jupyter span.cm-header {
  color: var(--jp-mirror-editor-header-color);
}
.cm-s-jupyter span.cm-quote {
  color: var(--jp-mirror-editor-quote-color);
}
.cm-s-jupyter span.cm-link {
  color: var(--jp-mirror-editor-link-color);
}
.cm-s-jupyter span.cm-error {
  color: var(--jp-mirror-editor-error-color);
}
.cm-s-jupyter span.cm-hr {
  color: #999;
}

.cm-s-jupyter span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}

.cm-s-jupyter .CodeMirror-activeline-background,
.cm-s-jupyter .CodeMirror-gutter {
  background-color: var(--jp-layout-color2);
}

/* Styles for shared cursors (remote cursor locations and selected ranges) */
.jp-CodeMirrorEditor .remote-caret {
  position: relative;
  border-left: 2px solid black;
  margin-left: -1px;
  margin-right: -1px;
  box-sizing: border-box;
}

.jp-CodeMirrorEditor .remote-caret > div {
  white-space: nowrap;
  position: absolute;
  top: -1.15em;
  padding-bottom: 0.05em;
  left: -2px;
  font-size: 0.95em;
  background-color: rgb(250, 129, 0);
  font-family: var(--jp-ui-font-family);
  font-weight: bold;
  line-height: normal;
  user-select: none;
  color: white;
  padding-left: 2px;
  padding-right: 2px;
  z-index: 3;
  transition: opacity 0.3s ease-in-out;
}

.jp-CodeMirrorEditor .remote-caret.hide-name > div {
  transition-delay: 0.7s;
  opacity: 0;
}

.jp-CodeMirrorEditor .remote-caret:hover > div {
  opacity: 1;
  transition-delay: 0s;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| RenderedText
|----------------------------------------------------------------------------*/

:root {
  /* This is the padding value to fill the gaps between lines containing spans with background color. */
  --jp-private-code-span-padding: calc(
    (var(--jp-code-line-height) - 1) * var(--jp-code-font-size) / 2
  );
}

.jp-RenderedText {
  text-align: left;
  padding-left: var(--jp-code-padding);
  line-height: var(--jp-code-line-height);
  font-family: var(--jp-code-font-family);
}

.jp-RenderedText pre,
.jp-RenderedJavaScript pre,
.jp-RenderedHTMLCommon pre {
  color: var(--jp-content-font-color1);
  font-size: var(--jp-code-font-size);
  border: none;
  margin: 0px;
  padding: 0px;
}

.jp-RenderedText pre a:link {
  text-decoration: none;
  color: var(--jp-content-link-color);
}
.jp-RenderedText pre a:hover {
  text-decoration: underline;
  color: var(--jp-content-link-color);
}
.jp-RenderedText pre a:visited {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

/* console foregrounds and backgrounds */
.jp-RenderedText pre .ansi-black-fg {
  color: #3e424d;
}
.jp-RenderedText pre .ansi-red-fg {
  color: #e75c58;
}
.jp-RenderedText pre .ansi-green-fg {
  color: #00a250;
}
.jp-RenderedText pre .ansi-yellow-fg {
  color: #ddb62b;
}
.jp-RenderedText pre .ansi-blue-fg {
  color: #208ffb;
}
.jp-RenderedText pre .ansi-magenta-fg {
  color: #d160c4;
}
.jp-RenderedText pre .ansi-cyan-fg {
  color: #60c6c8;
}
.jp-RenderedText pre .ansi-white-fg {
  color: #c5c1b4;
}

.jp-RenderedText pre .ansi-black-bg {
  background-color: #3e424d;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-red-bg {
  background-color: #e75c58;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-green-bg {
  background-color: #00a250;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-yellow-bg {
  background-color: #ddb62b;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-blue-bg {
  background-color: #208ffb;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-magenta-bg {
  background-color: #d160c4;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-cyan-bg {
  background-color: #60c6c8;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-white-bg {
  background-color: #c5c1b4;
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-black-intense-fg {
  color: #282c36;
}
.jp-RenderedText pre .ansi-red-intense-fg {
  color: #b22b31;
}
.jp-RenderedText pre .ansi-green-intense-fg {
  color: #007427;
}
.jp-RenderedText pre .ansi-yellow-intense-fg {
  color: #b27d12;
}
.jp-RenderedText pre .ansi-blue-intense-fg {
  color: #0065ca;
}
.jp-RenderedText pre .ansi-magenta-intense-fg {
  color: #a03196;
}
.jp-RenderedText pre .ansi-cyan-intense-fg {
  color: #258f8f;
}
.jp-RenderedText pre .ansi-white-intense-fg {
  color: #a1a6b2;
}

.jp-RenderedText pre .ansi-black-intense-bg {
  background-color: #282c36;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-red-intense-bg {
  background-color: #b22b31;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-green-intense-bg {
  background-color: #007427;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-yellow-intense-bg {
  background-color: #b27d12;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-blue-intense-bg {
  background-color: #0065ca;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-magenta-intense-bg {
  background-color: #a03196;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-cyan-intense-bg {
  background-color: #258f8f;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-white-intense-bg {
  background-color: #a1a6b2;
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-default-inverse-fg {
  color: var(--jp-ui-inverse-font-color0);
}
.jp-RenderedText pre .ansi-default-inverse-bg {
  background-color: var(--jp-inverse-layout-color0);
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-bold {
  font-weight: bold;
}
.jp-RenderedText pre .ansi-underline {
  text-decoration: underline;
}

.jp-RenderedText[data-mime-type='application/vnd.jupyter.stderr'] {
  background: var(--jp-rendermime-error-background);
  padding-top: var(--jp-code-padding);
}

/*-----------------------------------------------------------------------------
| RenderedLatex
|----------------------------------------------------------------------------*/

.jp-RenderedLatex {
  color: var(--jp-content-font-color1);
  font-size: var(--jp-content-font-size1);
  line-height: var(--jp-content-line-height);
}

/* Left-justify outputs.*/
.jp-OutputArea-output.jp-RenderedLatex {
  padding: var(--jp-code-padding);
  text-align: left;
}

/*-----------------------------------------------------------------------------
| RenderedHTML
|----------------------------------------------------------------------------*/

.jp-RenderedHTMLCommon {
  color: var(--jp-content-font-color1);
  font-family: var(--jp-content-font-family);
  font-size: var(--jp-content-font-size1);
  line-height: var(--jp-content-line-height);
  /* Give a bit more R padding on Markdown text to keep line lengths reasonable */
  padding-right: 20px;
}

.jp-RenderedHTMLCommon em {
  font-style: italic;
}

.jp-RenderedHTMLCommon strong {
  font-weight: bold;
}

.jp-RenderedHTMLCommon u {
  text-decoration: underline;
}

.jp-RenderedHTMLCommon a:link {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

.jp-RenderedHTMLCommon a:hover {
  text-decoration: underline;
  color: var(--jp-content-link-color);
}

.jp-RenderedHTMLCommon a:visited {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

/* Headings */

.jp-RenderedHTMLCommon h1,
.jp-RenderedHTMLCommon h2,
.jp-RenderedHTMLCommon h3,
.jp-RenderedHTMLCommon h4,
.jp-RenderedHTMLCommon h5,
.jp-RenderedHTMLCommon h6 {
  line-height: var(--jp-content-heading-line-height);
  font-weight: var(--jp-content-heading-font-weight);
  font-style: normal;
  margin: var(--jp-content-heading-margin-top) 0
    var(--jp-content-heading-margin-bottom) 0;
}

.jp-RenderedHTMLCommon h1:first-child,
.jp-RenderedHTMLCommon h2:first-child,
.jp-RenderedHTMLCommon h3:first-child,
.jp-RenderedHTMLCommon h4:first-child,
.jp-RenderedHTMLCommon h5:first-child,
.jp-RenderedHTMLCommon h6:first-child {
  margin-top: calc(0.5 * var(--jp-content-heading-margin-top));
}

.jp-RenderedHTMLCommon h1:last-child,
.jp-RenderedHTMLCommon h2:last-child,
.jp-RenderedHTMLCommon h3:last-child,
.jp-RenderedHTMLCommon h4:last-child,
.jp-RenderedHTMLCommon h5:last-child,
.jp-RenderedHTMLCommon h6:last-child {
  margin-bottom: calc(0.5 * var(--jp-content-heading-margin-bottom));
}

.jp-RenderedHTMLCommon h1 {
  font-size: var(--jp-content-font-size5);
}

.jp-RenderedHTMLCommon h2 {
  font-size: var(--jp-content-font-size4);
}

.jp-RenderedHTMLCommon h3 {
  font-size: var(--jp-content-font-size3);
}

.jp-RenderedHTMLCommon h4 {
  font-size: var(--jp-content-font-size2);
}

.jp-RenderedHTMLCommon h5 {
  font-size: var(--jp-content-font-size1);
}

.jp-RenderedHTMLCommon h6 {
  font-size: var(--jp-content-font-size0);
}

/* Lists */

.jp-RenderedHTMLCommon ul:not(.list-inline),
.jp-RenderedHTMLCommon ol:not(.list-inline) {
  padding-left: 2em;
}

.jp-RenderedHTMLCommon ul {
  list-style: disc;
}

.jp-RenderedHTMLCommon ul ul {
  list-style: square;
}

.jp-RenderedHTMLCommon ul ul ul {
  list-style: circle;
}

.jp-RenderedHTMLCommon ol {
  list-style: decimal;
}

.jp-RenderedHTMLCommon ol ol {
  list-style: upper-alpha;
}

.jp-RenderedHTMLCommon ol ol ol {
  list-style: lower-alpha;
}

.jp-RenderedHTMLCommon ol ol ol ol {
  list-style: lower-roman;
}

.jp-RenderedHTMLCommon ol ol ol ol ol {
  list-style: decimal;
}

.jp-RenderedHTMLCommon ol,
.jp-RenderedHTMLCommon ul {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon ul ul,
.jp-RenderedHTMLCommon ul ol,
.jp-RenderedHTMLCommon ol ul,
.jp-RenderedHTMLCommon ol ol {
  margin-bottom: 0em;
}

.jp-RenderedHTMLCommon hr {
  color: var(--jp-border-color2);
  background-color: var(--jp-border-color1);
  margin-top: 1em;
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon > pre {
  margin: 1.5em 2em;
}

.jp-RenderedHTMLCommon pre,
.jp-RenderedHTMLCommon code {
  border: 0;
  background-color: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
  font-family: var(--jp-code-font-family);
  font-size: inherit;
  line-height: var(--jp-code-line-height);
  padding: 0;
  white-space: pre-wrap;
}

.jp-RenderedHTMLCommon :not(pre) > code {
  background-color: var(--jp-layout-color2);
  padding: 1px 5px;
}

/* Tables */

.jp-RenderedHTMLCommon table {
  border-collapse: collapse;
  border-spacing: 0;
  border: none;
  color: var(--jp-ui-font-color1);
  font-size: 12px;
  table-layout: fixed;
  margin-left: auto;
  margin-right: auto;
}

.jp-RenderedHTMLCommon thead {
  border-bottom: var(--jp-border-width) solid var(--jp-border-color1);
  vertical-align: bottom;
}

.jp-RenderedHTMLCommon td,
.jp-RenderedHTMLCommon th,
.jp-RenderedHTMLCommon tr {
  vertical-align: middle;
  padding: 0.5em 0.5em;
  line-height: normal;
  white-space: normal;
  max-width: none;
  border: none;
}

.jp-RenderedMarkdown.jp-RenderedHTMLCommon td,
.jp-RenderedMarkdown.jp-RenderedHTMLCommon th {
  max-width: none;
}

:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon td,
:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon th,
:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon tr {
  text-align: right;
}

.jp-RenderedHTMLCommon th {
  font-weight: bold;
}

.jp-RenderedHTMLCommon tbody tr:nth-child(odd) {
  background: var(--jp-layout-color0);
}

.jp-RenderedHTMLCommon tbody tr:nth-child(even) {
  background: var(--jp-rendermime-table-row-background);
}

.jp-RenderedHTMLCommon tbody tr:hover {
  background: var(--jp-rendermime-table-row-hover-background);
}

.jp-RenderedHTMLCommon table {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon p {
  text-align: left;
  margin: 0px;
}

.jp-RenderedHTMLCommon p {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon img {
  -moz-force-broken-image-icon: 1;
}

/* Restrict to direct children as other images could be nested in other content. */
.jp-RenderedHTMLCommon > img {
  display: block;
  margin-left: 0;
  margin-right: 0;
  margin-bottom: 1em;
}

/* Change color behind transparent images if they need it... */
[data-jp-theme-light='false'] .jp-RenderedImage img.jp-needs-light-background {
  background-color: var(--jp-inverse-layout-color1);
}
[data-jp-theme-light='true'] .jp-RenderedImage img.jp-needs-dark-background {
  background-color: var(--jp-inverse-layout-color1);
}
/* ...or leave it untouched if they don't */
[data-jp-theme-light='false'] .jp-RenderedImage img.jp-needs-dark-background {
}
[data-jp-theme-light='true'] .jp-RenderedImage img.jp-needs-light-background {
}

.jp-RenderedHTMLCommon img,
.jp-RenderedImage img,
.jp-RenderedHTMLCommon svg,
.jp-RenderedSVG svg {
  max-width: 100%;
  height: auto;
}

.jp-RenderedHTMLCommon img.jp-mod-unconfined,
.jp-RenderedImage img.jp-mod-unconfined,
.jp-RenderedHTMLCommon svg.jp-mod-unconfined,
.jp-RenderedSVG svg.jp-mod-unconfined {
  max-width: none;
}

.jp-RenderedHTMLCommon .alert {
  padding: var(--jp-notebook-padding);
  border: var(--jp-border-width) solid transparent;
  border-radius: var(--jp-border-radius);
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon .alert-info {
  color: var(--jp-info-color0);
  background-color: var(--jp-info-color3);
  border-color: var(--jp-info-color2);
}
.jp-RenderedHTMLCommon .alert-info hr {
  border-color: var(--jp-info-color3);
}
.jp-RenderedHTMLCommon .alert-info > p:last-child,
.jp-RenderedHTMLCommon .alert-info > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-warning {
  color: var(--jp-warn-color0);
  background-color: var(--jp-warn-color3);
  border-color: var(--jp-warn-color2);
}
.jp-RenderedHTMLCommon .alert-warning hr {
  border-color: var(--jp-warn-color3);
}
.jp-RenderedHTMLCommon .alert-warning > p:last-child,
.jp-RenderedHTMLCommon .alert-warning > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-success {
  color: var(--jp-success-color0);
  background-color: var(--jp-success-color3);
  border-color: var(--jp-success-color2);
}
.jp-RenderedHTMLCommon .alert-success hr {
  border-color: var(--jp-success-color3);
}
.jp-RenderedHTMLCommon .alert-success > p:last-child,
.jp-RenderedHTMLCommon .alert-success > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-danger {
  color: var(--jp-error-color0);
  background-color: var(--jp-error-color3);
  border-color: var(--jp-error-color2);
}
.jp-RenderedHTMLCommon .alert-danger hr {
  border-color: var(--jp-error-color3);
}
.jp-RenderedHTMLCommon .alert-danger > p:last-child,
.jp-RenderedHTMLCommon .alert-danger > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon blockquote {
  margin: 1em 2em;
  padding: 0 1em;
  border-left: 5px solid var(--jp-border-color2);
}

a.jp-InternalAnchorLink {
  visibility: hidden;
  margin-left: 8px;
  color: var(--md-blue-800);
}

h1:hover .jp-InternalAnchorLink,
h2:hover .jp-InternalAnchorLink,
h3:hover .jp-InternalAnchorLink,
h4:hover .jp-InternalAnchorLink,
h5:hover .jp-InternalAnchorLink,
h6:hover .jp-InternalAnchorLink {
  visibility: visible;
}

.jp-RenderedHTMLCommon kbd {
  background-color: var(--jp-rendermime-table-row-background);
  border: 1px solid var(--jp-border-color0);
  border-bottom-color: var(--jp-border-color2);
  border-radius: 3px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
  display: inline-block;
  font-size: 0.8em;
  line-height: 1em;
  padding: 0.2em 0.5em;
}

/* Most direct children of .jp-RenderedHTMLCommon have a margin-bottom of 1.0.
 * At the bottom of cells this is a bit too much as there is also spacing
 * between cells. Going all the way to 0 gets too tight between markdown and
 * code cells.
 */
.jp-RenderedHTMLCommon > *:last-child {
  margin-bottom: 0.5em;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-MimeDocument {
  outline: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-filebrowser-button-height: 28px;
  --jp-private-filebrowser-button-width: 48px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-FileBrowser {
  display: flex;
  flex-direction: column;
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
}

.jp-FileBrowser-toolbar.jp-Toolbar {
  border-bottom: none;
  height: auto;
  margin: var(--jp-toolbar-header-margin);
  box-shadow: none;
}

.jp-BreadCrumbs {
  flex: 0 0 auto;
  margin: 8px 12px 8px 12px;
}

.jp-BreadCrumbs-item {
  margin: 0px 2px;
  padding: 0px 2px;
  border-radius: var(--jp-border-radius);
  cursor: pointer;
}

.jp-BreadCrumbs-item:hover {
  background-color: var(--jp-layout-color2);
}

.jp-BreadCrumbs-item:first-child {
  margin-left: 0px;
}

.jp-BreadCrumbs-item.jp-mod-dropTarget {
  background-color: var(--jp-brand-color2);
  opacity: 0.7;
}

/*-----------------------------------------------------------------------------
| Buttons
|----------------------------------------------------------------------------*/

.jp-FileBrowser-toolbar.jp-Toolbar {
  padding: 0px;
  margin: 8px 12px 0px 12px;
}

.jp-FileBrowser-toolbar.jp-Toolbar {
  justify-content: flex-start;
}

.jp-FileBrowser-toolbar.jp-Toolbar .jp-Toolbar-item {
  flex: 0 0 auto;
  padding-left: 0px;
  padding-right: 2px;
}

.jp-FileBrowser-toolbar.jp-Toolbar .jp-ToolbarButtonComponent {
  width: 40px;
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent {
  width: 72px;
  background: var(--jp-brand-color1);
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent:focus-visible {
  background-color: var(--jp-brand-color0);
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent
  .jp-icon3 {
  fill: white;
}

/*-----------------------------------------------------------------------------
| Other styles
|----------------------------------------------------------------------------*/

.jp-FileDialog.jp-mod-conflict input {
  color: var(--jp-error-color1);
}

.jp-FileDialog .jp-new-name-title {
  margin-top: 12px;
}

.jp-LastModified-hidden {
  display: none;
}

.jp-FileBrowser-filterBox {
  padding: 0px;
  flex: 0 0 auto;
  margin: 8px 12px 0px 12px;
}

/*-----------------------------------------------------------------------------
| DirListing
|----------------------------------------------------------------------------*/

.jp-DirListing {
  flex: 1 1 auto;
  display: flex;
  flex-direction: column;
  outline: 0;
}

.jp-DirListing:focus-visible {
  border: 1px solid var(--jp-brand-color1);
}

.jp-DirListing-header {
  flex: 0 0 auto;
  display: flex;
  flex-direction: row;
  overflow: hidden;
  border-top: var(--jp-border-width) solid var(--jp-border-color2);
  border-bottom: var(--jp-border-width) solid var(--jp-border-color1);
  box-shadow: var(--jp-toolbar-box-shadow);
  z-index: 2;
}

.jp-DirListing-headerItem {
  padding: 4px 12px 2px 12px;
  font-weight: 500;
}

.jp-DirListing-headerItem:hover {
  background: var(--jp-layout-color2);
}

.jp-DirListing-headerItem.jp-id-name {
  flex: 1 0 84px;
}

.jp-DirListing-headerItem.jp-id-modified {
  flex: 0 0 112px;
  border-left: var(--jp-border-width) solid var(--jp-border-color2);
  text-align: right;
}

.jp-id-narrow {
  display: none;
  flex: 0 0 5px;
  padding: 4px 4px;
  border-left: var(--jp-border-width) solid var(--jp-border-color2);
  text-align: right;
  color: var(--jp-border-color2);
}

.jp-DirListing-narrow .jp-id-narrow {
  display: block;
}

.jp-DirListing-narrow .jp-id-modified,
.jp-DirListing-narrow .jp-DirListing-itemModified {
  display: none;
}

.jp-DirListing-headerItem.jp-mod-selected {
  font-weight: 600;
}

/* increase specificity to override bundled default */
.jp-DirListing-content {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  list-style-type: none;
  overflow: auto;
  background-color: var(--jp-layout-color1);
}

.jp-DirListing-content mark {
  color: var(--jp-ui-font-color0);
  background-color: transparent;
  font-weight: bold;
}

.jp-DirListing-content .jp-DirListing-item.jp-mod-selected mark {
  color: var(--jp-ui-inverse-font-color0);
}

/* Style the directory listing content when a user drops a file to upload */
.jp-DirListing.jp-mod-native-drop .jp-DirListing-content {
  outline: 5px dashed rgba(128, 128, 128, 0.5);
  outline-offset: -10px;
  cursor: copy;
}

.jp-DirListing-item {
  display: flex;
  flex-direction: row;
  padding: 4px 12px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.jp-DirListing-item[data-is-dot] {
  opacity: 75%;
}

.jp-DirListing-item.jp-mod-selected {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.jp-DirListing-item.jp-mod-dropTarget {
  background: var(--jp-brand-color3);
}

.jp-DirListing-item:hover:not(.jp-mod-selected) {
  background: var(--jp-layout-color2);
}

.jp-DirListing-itemIcon {
  flex: 0 0 20px;
  margin-right: 4px;
}

.jp-DirListing-itemText {
  flex: 1 0 64px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  user-select: none;
}

.jp-DirListing-itemModified {
  flex: 0 0 125px;
  text-align: right;
}

.jp-DirListing-editor {
  flex: 1 0 64px;
  outline: none;
  border: none;
}

.jp-DirListing-item.jp-mod-running .jp-DirListing-itemIcon:before {
  color: var(--jp-success-color1);
  content: '\25CF';
  font-size: 8px;
  position: absolute;
  left: -8px;
}

.jp-DirListing-item.jp-mod-running.jp-mod-selected
  .jp-DirListing-itemIcon:before {
  color: var(--jp-ui-inverse-font-color1);
}

.jp-DirListing-item.lm-mod-drag-image,
.jp-DirListing-item.jp-mod-selected.lm-mod-drag-image {
  font-size: var(--jp-ui-font-size1);
  padding-left: 4px;
  margin-left: 4px;
  width: 160px;
  background-color: var(--jp-ui-inverse-font-color2);
  box-shadow: var(--jp-elevation-z2);
  border-radius: 0px;
  color: var(--jp-ui-font-color1);
  transform: translateX(-40%) translateY(-58%);
}

.jp-DirListing-deadSpace {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  list-style-type: none;
  overflow: auto;
  background-color: var(--jp-layout-color1);
}

.jp-Document {
  min-width: 120px;
  min-height: 120px;
  outline: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
}

/*-----------------------------------------------------------------------------
| Main OutputArea
| OutputArea has a list of Outputs
|----------------------------------------------------------------------------*/

.jp-OutputArea {
  overflow-y: auto;
}

.jp-OutputArea-child {
  display: flex;
  flex-direction: row;
}

body[data-format='mobile'] .jp-OutputArea-child {
  flex-direction: column;
}

.jp-OutputPrompt {
  flex: 0 0 var(--jp-cell-prompt-width);
  color: var(--jp-cell-outprompt-font-color);
  font-family: var(--jp-cell-prompt-font-family);
  padding: var(--jp-code-padding);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
  opacity: var(--jp-cell-prompt-opacity);
  /* Right align prompt text, don't wrap to handle large prompt numbers */
  text-align: right;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  /* Disable text selection */
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

body[data-format='mobile'] .jp-OutputPrompt {
  flex: 0 0 auto;
  text-align: left;
}

.jp-OutputArea-output {
  height: auto;
  overflow: auto;
  user-select: text;
  -moz-user-select: text;
  -webkit-user-select: text;
  -ms-user-select: text;
}

.jp-OutputArea-child .jp-OutputArea-output {
  flex-grow: 1;
  flex-shrink: 1;
}

body[data-format='mobile'] .jp-OutputArea-child .jp-OutputArea-output {
  margin-left: var(--jp-notebook-padding);
}

/**
 * Isolated output.
 */
.jp-OutputArea-output.jp-mod-isolated {
  width: 100%;
  display: block;
}

/*
When drag events occur, `p-mod-override-cursor` is added to the body.
Because iframes steal all cursor events, the following two rules are necessary
to suppress pointer events while resize drags are occurring. There may be a
better solution to this problem.
*/
body.lm-mod-override-cursor .jp-OutputArea-output.jp-mod-isolated {
  position: relative;
}

body.lm-mod-override-cursor .jp-OutputArea-output.jp-mod-isolated:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: transparent;
}

/* pre */

.jp-OutputArea-output pre {
  border: none;
  margin: 0px;
  padding: 0px;
  overflow-x: auto;
  overflow-y: auto;
  word-break: break-all;
  word-wrap: break-word;
  white-space: pre-wrap;
}

/* tables */

.jp-OutputArea-output.jp-RenderedHTMLCommon table {
  margin-left: 0;
  margin-right: 0;
}

/* description lists */

.jp-OutputArea-output dl,
.jp-OutputArea-output dt,
.jp-OutputArea-output dd {
  display: block;
}

.jp-OutputArea-output dl {
  width: 100%;
  overflow: hidden;
  padding: 0;
  margin: 0;
}

.jp-OutputArea-output dt {
  font-weight: bold;
  float: left;
  width: 20%;
  padding: 0;
  margin: 0;
}

.jp-OutputArea-output dd {
  float: left;
  width: 80%;
  padding: 0;
  margin: 0;
}

/* Hide the gutter in case of
 *  - nested output areas (e.g. in the case of output widgets)
 *  - mirrored output areas
 */
.jp-OutputArea .jp-OutputArea .jp-OutputArea-prompt {
  display: none;
}

/*-----------------------------------------------------------------------------
| executeResult is added to any Output-result for the display of the object
| returned by a cell
|----------------------------------------------------------------------------*/

.jp-OutputArea-output.jp-OutputArea-executeResult {
  margin-left: 0px;
  flex: 1 1 auto;
}

/* Text output with the Out[] prompt needs a top padding to match the
 * alignment of the Out[] prompt itself.
 */
.jp-OutputArea-executeResult .jp-RenderedText.jp-OutputArea-output {
  padding-top: var(--jp-code-padding);
  border-top: var(--jp-border-width) solid transparent;
}

/*-----------------------------------------------------------------------------
| The Stdin output
|----------------------------------------------------------------------------*/

.jp-OutputArea-stdin {
  line-height: var(--jp-code-line-height);
  padding-top: var(--jp-code-padding);
  display: flex;
}

.jp-Stdin-prompt {
  color: var(--jp-content-font-color0);
  padding-right: var(--jp-code-padding);
  vertical-align: baseline;
  flex: 0 0 auto;
}

.jp-Stdin-input {
  font-family: var(--jp-code-font-family);
  font-size: inherit;
  color: inherit;
  background-color: inherit;
  width: 42%;
  min-width: 200px;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
  flex: 0 0 70%;
}

.jp-Stdin-input:focus {
  box-shadow: none;
}

/*-----------------------------------------------------------------------------
| Output Area View
|----------------------------------------------------------------------------*/

.jp-LinkedOutputView .jp-OutputArea {
  height: 100%;
  display: block;
}

.jp-LinkedOutputView .jp-OutputArea-output:only-child {
  height: 100%;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Collapser {
  flex: 0 0 var(--jp-cell-collapser-width);
  padding: 0px;
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
  border-radius: var(--jp-border-radius);
  opacity: 1;
}

.jp-Collapser-child {
  display: block;
  width: 100%;
  box-sizing: border-box;
  /* height: 100% doesn't work because the height of its parent is computed from content */
  position: absolute;
  top: 0px;
  bottom: 0px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Header/Footer
|----------------------------------------------------------------------------*/

/* Hidden by zero height by default */
.jp-CellHeader,
.jp-CellFooter {
  height: 0px;
  width: 100%;
  padding: 0px;
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Input
|----------------------------------------------------------------------------*/

/* All input areas */
.jp-InputArea {
  display: flex;
  flex-direction: row;
  overflow: hidden;
}

body[data-format='mobile'] .jp-InputArea {
  flex-direction: column;
}

.jp-InputArea-editor {
  flex: 1 1 auto;
  overflow: hidden;
}

.jp-InputArea-editor {
  /* This is the non-active, default styling */
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  border-radius: 0px;
  background: var(--jp-cell-editor-background);
}

body[data-format='mobile'] .jp-InputArea-editor {
  margin-left: var(--jp-notebook-padding);
}

.jp-InputPrompt {
  flex: 0 0 var(--jp-cell-prompt-width);
  color: var(--jp-cell-inprompt-font-color);
  font-family: var(--jp-cell-prompt-font-family);
  padding: var(--jp-code-padding);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  opacity: var(--jp-cell-prompt-opacity);
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
  opacity: var(--jp-cell-prompt-opacity);
  /* Right align prompt text, don't wrap to handle large prompt numbers */
  text-align: right;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  /* Disable text selection */
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

body[data-format='mobile'] .jp-InputPrompt {
  flex: 0 0 auto;
  text-align: left;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Placeholder
|----------------------------------------------------------------------------*/

.jp-Placeholder {
  display: flex;
  flex-direction: row;
  flex: 1 1 auto;
}

.jp-Placeholder-prompt {
  box-sizing: border-box;
}

.jp-Placeholder-content {
  flex: 1 1 auto;
  border: none;
  background: transparent;
  height: 20px;
  box-sizing: border-box;
}

.jp-Placeholder-content .jp-MoreHorizIcon {
  width: 32px;
  height: 16px;
  border: 1px solid transparent;
  border-radius: var(--jp-border-radius);
}

.jp-Placeholder-content .jp-MoreHorizIcon:hover {
  border: 1px solid var(--jp-border-color1);
  box-shadow: 0px 0px 2px 0px rgba(0, 0, 0, 0.25);
  background-color: var(--jp-layout-color0);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-cell-scrolling-output-offset: 5px;
}

/*-----------------------------------------------------------------------------
| Cell
|----------------------------------------------------------------------------*/

.jp-Cell {
  padding: var(--jp-cell-padding);
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
}

/*-----------------------------------------------------------------------------
| Common input/output
|----------------------------------------------------------------------------*/

.jp-Cell-inputWrapper,
.jp-Cell-outputWrapper {
  display: flex;
  flex-direction: row;
  padding: 0px;
  margin: 0px;
  /* Added to reveal the box-shadow on the input and output collapsers. */
  overflow: visible;
}

/* Only input/output areas inside cells */
.jp-Cell-inputArea,
.jp-Cell-outputArea {
  flex: 1 1 auto;
}

/*-----------------------------------------------------------------------------
| Collapser
|----------------------------------------------------------------------------*/

/* Make the output collapser disappear when there is not output, but do so
 * in a manner that leaves it in the layout and preserves its width.
 */
.jp-Cell.jp-mod-noOutputs .jp-Cell-outputCollapser {
  border: none !important;
  background: transparent !important;
}

.jp-Cell:not(.jp-mod-noOutputs) .jp-Cell-outputCollapser {
  min-height: var(--jp-cell-collapser-min-height);
}

/*-----------------------------------------------------------------------------
| Output
|----------------------------------------------------------------------------*/

/* Put a space between input and output when there IS output */
.jp-Cell:not(.jp-mod-noOutputs) .jp-Cell-outputWrapper {
  margin-top: 5px;
}

.jp-CodeCell.jp-mod-outputsScrolled .jp-Cell-outputArea {
  overflow-y: auto;
  max-height: 200px;
  box-shadow: inset 0 0 6px 2px rgba(0, 0, 0, 0.3);
  margin-left: var(--jp-private-cell-scrolling-output-offset);
}

.jp-CodeCell.jp-mod-outputsScrolled .jp-OutputArea-prompt {
  flex: 0 0
    calc(
      var(--jp-cell-prompt-width) -
        var(--jp-private-cell-scrolling-output-offset)
    );
}

/*-----------------------------------------------------------------------------
| CodeCell
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| MarkdownCell
|----------------------------------------------------------------------------*/

.jp-MarkdownOutput {
  flex: 1 1 auto;
  margin-top: 0;
  margin-bottom: 0;
  padding-left: var(--jp-code-padding);
}

.jp-MarkdownOutput.jp-RenderedHTMLCommon {
  overflow: auto;
}

.jp-showHiddenCellsButton {
  margin-left: calc(var(--jp-cell-prompt-width) + 2 * var(--jp-code-padding));
  margin-top: var(--jp-code-padding);
  border: 1px solid var(--jp-border-color2);
  background-color: var(--jp-border-color3) !important;
  color: var(--jp-content-font-color0) !important;
}

.jp-showHiddenCellsButton:hover {
  background-color: var(--jp-border-color2) !important;
}

.jp-collapseHeadingButton {
  display: none;
}

.jp-MarkdownCell:hover .jp-collapseHeadingButton {
  display: flex;
  min-height: var(--jp-cell-collapser-min-height);
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------

/*-----------------------------------------------------------------------------
| Styles
|----------------------------------------------------------------------------*/

.jp-NotebookPanel-toolbar {
  padding: 2px;
}

.jp-Toolbar-item.jp-Notebook-toolbarCellType .jp-select-wrapper.jp-mod-focused {
  border: none;
  box-shadow: none;
}

.jp-Notebook-toolbarCellTypeDropdown select {
  height: 24px;
  font-size: var(--jp-ui-font-size1);
  line-height: 14px;
  border-radius: 0;
  display: block;
}

.jp-Notebook-toolbarCellTypeDropdown span {
  top: 5px !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-notebook-dragImage-width: 304px;
  --jp-private-notebook-dragImage-height: 36px;
  --jp-private-notebook-selected-color: var(--md-blue-400);
  --jp-private-notebook-active-color: var(--md-green-400);
}

/*-----------------------------------------------------------------------------
| Imports
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Notebook
|----------------------------------------------------------------------------*/

.jp-NotebookPanel {
  display: block;
  height: 100%;
}

.jp-NotebookPanel.jp-Document {
  min-width: 240px;
  min-height: 120px;
}

.jp-Notebook {
  padding: var(--jp-notebook-padding);
  outline: none;
  overflow: auto;
  background: var(--jp-layout-color0);
}

.jp-Notebook.jp-mod-scrollPastEnd::after {
  display: block;
  content: '';
  min-height: var(--jp-notebook-scroll-padding);
}

.jp-MainAreaWidget-ContainStrict .jp-Notebook * {
  contain: strict;
}

.jp-Notebook-render * {
  contain: none !important;
}

.jp-Notebook .jp-Cell {
  overflow: visible;
}

.jp-Notebook .jp-Cell .jp-InputPrompt {
  cursor: move;
  float: left;
}

/*-----------------------------------------------------------------------------
| Notebook state related styling
|
| The notebook and cells each have states, here are the possibilities:
|
| - Notebook
|   - Command
|   - Edit
| - Cell
|   - None
|   - Active (only one can be active)
|   - Selected (the cells actions are applied to)
|   - Multiselected (when multiple selected, the cursor)
|   - No outputs
|----------------------------------------------------------------------------*/

/* Command or edit modes */

.jp-Notebook .jp-Cell:not(.jp-mod-active) .jp-InputPrompt {
  opacity: var(--jp-cell-prompt-not-active-opacity);
  color: var(--jp-cell-prompt-not-active-font-color);
}

.jp-Notebook .jp-Cell:not(.jp-mod-active) .jp-OutputPrompt {
  opacity: var(--jp-cell-prompt-not-active-opacity);
  color: var(--jp-cell-prompt-not-active-font-color);
}

/* cell is active */
.jp-Notebook .jp-Cell.jp-mod-active .jp-Collapser {
  background: var(--jp-brand-color1);
}

/* cell is dirty */
.jp-Notebook .jp-Cell.jp-mod-dirty .jp-InputPrompt {
  color: var(--jp-warn-color1);
}
.jp-Notebook .jp-Cell.jp-mod-dirty .jp-InputPrompt:before {
  color: var(--jp-warn-color1);
  content: '•';
}

.jp-Notebook .jp-Cell.jp-mod-active.jp-mod-dirty .jp-Collapser {
  background: var(--jp-warn-color1);
}

/* collapser is hovered */
.jp-Notebook .jp-Cell .jp-Collapser:hover {
  box-shadow: var(--jp-elevation-z2);
  background: var(--jp-brand-color1);
  opacity: var(--jp-cell-collapser-not-active-hover-opacity);
}

/* cell is active and collapser is hovered */
.jp-Notebook .jp-Cell.jp-mod-active .jp-Collapser:hover {
  background: var(--jp-brand-color0);
  opacity: 1;
}

/* Command mode */

.jp-Notebook.jp-mod-commandMode .jp-Cell.jp-mod-selected {
  background: var(--jp-notebook-multiselected-color);
}

.jp-Notebook.jp-mod-commandMode
  .jp-Cell.jp-mod-active.jp-mod-selected:not(.jp-mod-multiSelected) {
  background: transparent;
}

/* Edit mode */

.jp-Notebook.jp-mod-editMode .jp-Cell.jp-mod-active .jp-InputArea-editor {
  border: var(--jp-border-width) solid var(--jp-cell-editor-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
  background-color: var(--jp-cell-editor-active-background);
}

/*-----------------------------------------------------------------------------
| Notebook drag and drop
|----------------------------------------------------------------------------*/

.jp-Notebook-cell.jp-mod-dropSource {
  opacity: 0.5;
}

.jp-Notebook-cell.jp-mod-dropTarget,
.jp-Notebook.jp-mod-commandMode
  .jp-Notebook-cell.jp-mod-active.jp-mod-selected.jp-mod-dropTarget {
  border-top-color: var(--jp-private-notebook-selected-color);
  border-top-style: solid;
  border-top-width: 2px;
}

.jp-dragImage {
  display: block;
  flex-direction: row;
  width: var(--jp-private-notebook-dragImage-width);
  height: var(--jp-private-notebook-dragImage-height);
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  background: var(--jp-cell-editor-background);
  overflow: visible;
}

.jp-dragImage-singlePrompt {
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, 0.12);
}

.jp-dragImage .jp-dragImage-content {
  flex: 1 1 auto;
  z-index: 2;
  font-size: var(--jp-code-font-size);
  font-family: var(--jp-code-font-family);
  line-height: var(--jp-code-line-height);
  padding: var(--jp-code-padding);
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  background: var(--jp-cell-editor-background-color);
  color: var(--jp-content-font-color3);
  text-align: left;
  margin: 4px 4px 4px 0px;
}

.jp-dragImage .jp-dragImage-prompt {
  flex: 0 0 auto;
  min-width: 36px;
  color: var(--jp-cell-inprompt-font-color);
  padding: var(--jp-code-padding);
  padding-left: 12px;
  font-family: var(--jp-cell-prompt-font-family);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  line-height: 1.9;
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
}

.jp-dragImage-multipleBack {
  z-index: -1;
  position: absolute;
  height: 32px;
  width: 300px;
  top: 8px;
  left: 8px;
  background: var(--jp-layout-color2);
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, 0.12);
}

/*-----------------------------------------------------------------------------
| Cell toolbar
|----------------------------------------------------------------------------*/

.jp-NotebookTools {
  display: block;
  min-width: var(--jp-sidebar-min-width);
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
    * relative to this base size */
  font-size: var(--jp-ui-font-size1);
  overflow: auto;
}

.jp-NotebookTools-tool {
  padding: 0px 12px 0 12px;
}

.jp-ActiveCellTool {
  padding: 12px;
  background-color: var(--jp-layout-color1);
  border-top: none !important;
}

.jp-ActiveCellTool .jp-InputArea-prompt {
  flex: 0 0 auto;
  padding-left: 0px;
}

.jp-ActiveCellTool .jp-InputArea-editor {
  flex: 1 1 auto;
  background: var(--jp-cell-editor-background);
  border-color: var(--jp-cell-editor-border-color);
}

.jp-ActiveCellTool .jp-InputArea-editor .CodeMirror {
  background: transparent;
}

.jp-MetadataEditorTool {
  flex-direction: column;
  padding: 12px 0px 12px 0px;
}

.jp-RankedPanel > :not(:first-child) {
  margin-top: 12px;
}

.jp-KeySelector select.jp-mod-styled {
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  border: var(--jp-border-width) solid var(--jp-border-color1);
}

.jp-KeySelector label,
.jp-MetadataEditorTool label {
  line-height: 1.4;
}

.jp-NotebookTools .jp-select-wrapper {
  margin-top: 4px;
  margin-bottom: 0px;
}

.jp-NotebookTools .jp-Collapse {
  margin-top: 16px;
}

/*-----------------------------------------------------------------------------
| Presentation Mode (.jp-mod-presentationMode)
|----------------------------------------------------------------------------*/

.jp-mod-presentationMode .jp-Notebook {
  --jp-content-font-size1: var(--jp-content-presentation-font-size1);
  --jp-code-font-size: var(--jp-code-presentation-font-size);
}

.jp-mod-presentationMode .jp-Notebook .jp-Cell .jp-InputPrompt,
.jp-mod-presentationMode .jp-Notebook .jp-Cell .jp-OutputPrompt {
  flex: 0 0 110px;
}

/*-----------------------------------------------------------------------------
| Placeholder
|----------------------------------------------------------------------------*/

.jp-Cell-Placeholder {
  padding-left: 55px;
}

.jp-Cell-Placeholder-wrapper {
  background: #fff;
  border: 1px solid;
  border-color: #e5e6e9 #dfe0e4 #d0d1d5;
  border-radius: 4px;
  -webkit-border-radius: 4px;
  margin: 10px 15px;
}

.jp-Cell-Placeholder-wrapper-inner {
  padding: 15px;
  position: relative;
}

.jp-Cell-Placeholder-wrapper-body {
  background-repeat: repeat;
  background-size: 50% auto;
}

.jp-Cell-Placeholder-wrapper-body div {
  background: #f6f7f8;
  background-image: -webkit-linear-gradient(
    left,
    #f6f7f8 0%,
    #edeef1 20%,
    #f6f7f8 40%,
    #f6f7f8 100%
  );
  background-repeat: no-repeat;
  background-size: 800px 104px;
  height: 104px;
  position: relative;
}

.jp-Cell-Placeholder-wrapper-body div {
  position: absolute;
  right: 15px;
  left: 15px;
  top: 15px;
}

div.jp-Cell-Placeholder-h1 {
  top: 20px;
  height: 20px;
  left: 15px;
  width: 150px;
}

div.jp-Cell-Placeholder-h2 {
  left: 15px;
  top: 50px;
  height: 10px;
  width: 100px;
}

div.jp-Cell-Placeholder-content-1,
div.jp-Cell-Placeholder-content-2,
div.jp-Cell-Placeholder-content-3 {
  left: 15px;
  right: 15px;
  height: 10px;
}

div.jp-Cell-Placeholder-content-1 {
  top: 100px;
}

div.jp-Cell-Placeholder-content-2 {
  top: 120px;
}

div.jp-Cell-Placeholder-content-3 {
  top: 140px;
}

</style>

    <style type="text/css">
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*
The following CSS variables define the main, public API for styling JupyterLab.
These variables should be used by all plugins wherever possible. In other
words, plugins should not define custom colors, sizes, etc unless absolutely
necessary. This enables users to change the visual theme of JupyterLab
by changing these variables.

Many variables appear in an ordered sequence (0,1,2,3). These sequences
are designed to work well together, so for example, `--jp-border-color1` should
be used with `--jp-layout-color1`. The numbers have the following meanings:

* 0: super-primary, reserved for special emphasis
* 1: primary, most important under normal situations
* 2: secondary, next most important under normal situations
* 3: tertiary, next most important under normal situations

Throughout JupyterLab, we are mostly following principles from Google's
Material Design when selecting colors. We are not, however, following
all of MD as it is not optimized for dense, information rich UIs.
*/

:root {
  /* Elevation
   *
   * We style box-shadows using Material Design's idea of elevation. These particular numbers are taken from here:
   *
   * https://github.com/material-components/material-components-web
   * https://material-components-web.appspot.com/elevation.html
   */

  --jp-shadow-base-lightness: 0;
  --jp-shadow-umbra-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.2
  );
  --jp-shadow-penumbra-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.14
  );
  --jp-shadow-ambient-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.12
  );
  --jp-elevation-z0: none;
  --jp-elevation-z1: 0px 2px 1px -1px var(--jp-shadow-umbra-color),
    0px 1px 1px 0px var(--jp-shadow-penumbra-color),
    0px 1px 3px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z2: 0px 3px 1px -2px var(--jp-shadow-umbra-color),
    0px 2px 2px 0px var(--jp-shadow-penumbra-color),
    0px 1px 5px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z4: 0px 2px 4px -1px var(--jp-shadow-umbra-color),
    0px 4px 5px 0px var(--jp-shadow-penumbra-color),
    0px 1px 10px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z6: 0px 3px 5px -1px var(--jp-shadow-umbra-color),
    0px 6px 10px 0px var(--jp-shadow-penumbra-color),
    0px 1px 18px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z8: 0px 5px 5px -3px var(--jp-shadow-umbra-color),
    0px 8px 10px 1px var(--jp-shadow-penumbra-color),
    0px 3px 14px 2px var(--jp-shadow-ambient-color);
  --jp-elevation-z12: 0px 7px 8px -4px var(--jp-shadow-umbra-color),
    0px 12px 17px 2px var(--jp-shadow-penumbra-color),
    0px 5px 22px 4px var(--jp-shadow-ambient-color);
  --jp-elevation-z16: 0px 8px 10px -5px var(--jp-shadow-umbra-color),
    0px 16px 24px 2px var(--jp-shadow-penumbra-color),
    0px 6px 30px 5px var(--jp-shadow-ambient-color);
  --jp-elevation-z20: 0px 10px 13px -6px var(--jp-shadow-umbra-color),
    0px 20px 31px 3px var(--jp-shadow-penumbra-color),
    0px 8px 38px 7px var(--jp-shadow-ambient-color);
  --jp-elevation-z24: 0px 11px 15px -7px var(--jp-shadow-umbra-color),
    0px 24px 38px 3px var(--jp-shadow-penumbra-color),
    0px 9px 46px 8px var(--jp-shadow-ambient-color);

  /* Borders
   *
   * The following variables, specify the visual styling of borders in JupyterLab.
   */

  --jp-border-width: 1px;
  --jp-border-color0: var(--md-grey-400);
  --jp-border-color1: var(--md-grey-400);
  --jp-border-color2: var(--md-grey-300);
  --jp-border-color3: var(--md-grey-200);
  --jp-border-radius: 2px;

  /* UI Fonts
   *
   * The UI font CSS variables are used for the typography all of the JupyterLab
   * user interface elements that are not directly user generated content.
   *
   * The font sizing here is done assuming that the body font size of --jp-ui-font-size1
   * is applied to a parent element. When children elements, such as headings, are sized
   * in em all things will be computed relative to that body size.
   */

  --jp-ui-font-scale-factor: 1.2;
  --jp-ui-font-size0: 0.83333em;
  --jp-ui-font-size1: 13px; /* Base font size */
  --jp-ui-font-size2: 1.2em;
  --jp-ui-font-size3: 1.44em;

  --jp-ui-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica,
    Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';

  /*
   * Use these font colors against the corresponding main layout colors.
   * In a light theme, these go from dark to light.
   */

  /* Defaults use Material Design specification */
  --jp-ui-font-color0: rgba(0, 0, 0, 1);
  --jp-ui-font-color1: rgba(0, 0, 0, 0.87);
  --jp-ui-font-color2: rgba(0, 0, 0, 0.54);
  --jp-ui-font-color3: rgba(0, 0, 0, 0.38);

  /*
   * Use these against the brand/accent/warn/error colors.
   * These will typically go from light to darker, in both a dark and light theme.
   */

  --jp-ui-inverse-font-color0: rgba(255, 255, 255, 1);
  --jp-ui-inverse-font-color1: rgba(255, 255, 255, 1);
  --jp-ui-inverse-font-color2: rgba(255, 255, 255, 0.7);
  --jp-ui-inverse-font-color3: rgba(255, 255, 255, 0.5);

  /* Content Fonts
   *
   * Content font variables are used for typography of user generated content.
   *
   * The font sizing here is done assuming that the body font size of --jp-content-font-size1
   * is applied to a parent element. When children elements, such as headings, are sized
   * in em all things will be computed relative to that body size.
   */

  --jp-content-line-height: 1.6;
  --jp-content-font-scale-factor: 1.2;
  --jp-content-font-size0: 0.83333em;
  --jp-content-font-size1: 14px; /* Base font size */
  --jp-content-font-size2: 1.2em;
  --jp-content-font-size3: 1.44em;
  --jp-content-font-size4: 1.728em;
  --jp-content-font-size5: 2.0736em;

  /* This gives a magnification of about 125% in presentation mode over normal. */
  --jp-content-presentation-font-size1: 17px;

  --jp-content-heading-line-height: 1;
  --jp-content-heading-margin-top: 1.2em;
  --jp-content-heading-margin-bottom: 0.8em;
  --jp-content-heading-font-weight: 500;

  /* Defaults use Material Design specification */
  --jp-content-font-color0: rgba(0, 0, 0, 1);
  --jp-content-font-color1: rgba(0, 0, 0, 0.87);
  --jp-content-font-color2: rgba(0, 0, 0, 0.54);
  --jp-content-font-color3: rgba(0, 0, 0, 0.38);

  --jp-content-link-color: var(--md-blue-700);

  --jp-content-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol';

  /*
   * Code Fonts
   *
   * Code font variables are used for typography of code and other monospaces content.
   */

  --jp-code-font-size: 13px;
  --jp-code-line-height: 1.3077; /* 17px for 13px base */
  --jp-code-padding: 5px; /* 5px for 13px base, codemirror highlighting needs integer px value */
  --jp-code-font-family-default: Menlo, Consolas, 'DejaVu Sans Mono', monospace;
  --jp-code-font-family: var(--jp-code-font-family-default);

  /* This gives a magnification of about 125% in presentation mode over normal. */
  --jp-code-presentation-font-size: 16px;

  /* may need to tweak cursor width if you change font size */
  --jp-code-cursor-width0: 1.4px;
  --jp-code-cursor-width1: 2px;
  --jp-code-cursor-width2: 4px;

  /* Layout
   *
   * The following are the main layout colors use in JupyterLab. In a light
   * theme these would go from light to dark.
   */

  --jp-layout-color0: white;
  --jp-layout-color1: white;
  --jp-layout-color2: var(--md-grey-200);
  --jp-layout-color3: var(--md-grey-400);
  --jp-layout-color4: var(--md-grey-600);

  /* Inverse Layout
   *
   * The following are the inverse layout colors use in JupyterLab. In a light
   * theme these would go from dark to light.
   */

  --jp-inverse-layout-color0: #111111;
  --jp-inverse-layout-color1: var(--md-grey-900);
  --jp-inverse-layout-color2: var(--md-grey-800);
  --jp-inverse-layout-color3: var(--md-grey-700);
  --jp-inverse-layout-color4: var(--md-grey-600);

  /* Brand/accent */

  --jp-brand-color0: var(--md-blue-900);
  --jp-brand-color1: var(--md-blue-700);
  --jp-brand-color2: var(--md-blue-300);
  --jp-brand-color3: var(--md-blue-100);
  --jp-brand-color4: var(--md-blue-50);

  --jp-accent-color0: var(--md-green-900);
  --jp-accent-color1: var(--md-green-700);
  --jp-accent-color2: var(--md-green-300);
  --jp-accent-color3: var(--md-green-100);

  /* State colors (warn, error, success, info) */

  --jp-warn-color0: var(--md-orange-900);
  --jp-warn-color1: var(--md-orange-700);
  --jp-warn-color2: var(--md-orange-300);
  --jp-warn-color3: var(--md-orange-100);

  --jp-error-color0: var(--md-red-900);
  --jp-error-color1: var(--md-red-700);
  --jp-error-color2: var(--md-red-300);
  --jp-error-color3: var(--md-red-100);

  --jp-success-color0: var(--md-green-900);
  --jp-success-color1: var(--md-green-700);
  --jp-success-color2: var(--md-green-300);
  --jp-success-color3: var(--md-green-100);

  --jp-info-color0: var(--md-cyan-900);
  --jp-info-color1: var(--md-cyan-700);
  --jp-info-color2: var(--md-cyan-300);
  --jp-info-color3: var(--md-cyan-100);

  /* Cell specific styles */

  --jp-cell-padding: 5px;

  --jp-cell-collapser-width: 8px;
  --jp-cell-collapser-min-height: 20px;
  --jp-cell-collapser-not-active-hover-opacity: 0.6;

  --jp-cell-editor-background: var(--md-grey-100);
  --jp-cell-editor-border-color: var(--md-grey-300);
  --jp-cell-editor-box-shadow: inset 0 0 2px var(--md-blue-300);
  --jp-cell-editor-active-background: var(--jp-layout-color0);
  --jp-cell-editor-active-border-color: var(--jp-brand-color1);

  --jp-cell-prompt-width: 64px;
  --jp-cell-prompt-font-family: var(--jp-code-font-family-default);
  --jp-cell-prompt-letter-spacing: 0px;
  --jp-cell-prompt-opacity: 1;
  --jp-cell-prompt-not-active-opacity: 0.5;
  --jp-cell-prompt-not-active-font-color: var(--md-grey-700);
  /* A custom blend of MD grey and blue 600
   * See https://meyerweb.com/eric/tools/color-blend/#546E7A:1E88E5:5:hex */
  --jp-cell-inprompt-font-color: #307fc1;
  /* A custom blend of MD grey and orange 600
   * https://meyerweb.com/eric/tools/color-blend/#546E7A:F4511E:5:hex */
  --jp-cell-outprompt-font-color: #bf5b3d;

  /* Notebook specific styles */

  --jp-notebook-padding: 10px;
  --jp-notebook-select-background: var(--jp-layout-color1);
  --jp-notebook-multiselected-color: var(--md-blue-50);

  /* The scroll padding is calculated to fill enough space at the bottom of the
  notebook to show one single-line cell (with appropriate padding) at the top
  when the notebook is scrolled all the way to the bottom. We also subtract one
  pixel so that no scrollbar appears if we have just one single-line cell in the
  notebook. This padding is to enable a 'scroll past end' feature in a notebook.
  */
  --jp-notebook-scroll-padding: calc(
    100% - var(--jp-code-font-size) * var(--jp-code-line-height) -
      var(--jp-code-padding) - var(--jp-cell-padding) - 1px
  );

  /* Rendermime styles */

  --jp-rendermime-error-background: #fdd;
  --jp-rendermime-table-row-background: var(--md-grey-100);
  --jp-rendermime-table-row-hover-background: var(--md-light-blue-50);

  /* Dialog specific styles */

  --jp-dialog-background: rgba(0, 0, 0, 0.25);

  /* Console specific styles */

  --jp-console-padding: 10px;

  /* Toolbar specific styles */

  --jp-toolbar-border-color: var(--jp-border-color1);
  --jp-toolbar-micro-height: 8px;
  --jp-toolbar-background: var(--jp-layout-color1);
  --jp-toolbar-box-shadow: 0px 0px 2px 0px rgba(0, 0, 0, 0.24);
  --jp-toolbar-header-margin: 4px 4px 0px 4px;
  --jp-toolbar-active-background: var(--md-grey-300);

  /* Statusbar specific styles */

  --jp-statusbar-height: 24px;

  /* Input field styles */

  --jp-input-box-shadow: inset 0 0 2px var(--md-blue-300);
  --jp-input-active-background: var(--jp-layout-color1);
  --jp-input-hover-background: var(--jp-layout-color1);
  --jp-input-background: var(--md-grey-100);
  --jp-input-border-color: var(--jp-border-color1);
  --jp-input-active-border-color: var(--jp-brand-color1);
  --jp-input-active-box-shadow-color: rgba(19, 124, 189, 0.3);

  /* General editor styles */

  --jp-editor-selected-background: #d9d9d9;
  --jp-editor-selected-focused-background: #d7d4f0;
  --jp-editor-cursor-color: var(--jp-ui-font-color0);

  /* Code mirror specific styles */

  --jp-mirror-editor-keyword-color: #008000;
  --jp-mirror-editor-atom-color: #88f;
  --jp-mirror-editor-number-color: #080;
  --jp-mirror-editor-def-color: #00f;
  --jp-mirror-editor-variable-color: var(--md-grey-900);
  --jp-mirror-editor-variable-2-color: #05a;
  --jp-mirror-editor-variable-3-color: #085;
  --jp-mirror-editor-punctuation-color: #05a;
  --jp-mirror-editor-property-color: #05a;
  --jp-mirror-editor-operator-color: #aa22ff;
  --jp-mirror-editor-comment-color: #408080;
  --jp-mirror-editor-string-color: #ba2121;
  --jp-mirror-editor-string-2-color: #708;
  --jp-mirror-editor-meta-color: #aa22ff;
  --jp-mirror-editor-qualifier-color: #555;
  --jp-mirror-editor-builtin-color: #008000;
  --jp-mirror-editor-bracket-color: #997;
  --jp-mirror-editor-tag-color: #170;
  --jp-mirror-editor-attribute-color: #00c;
  --jp-mirror-editor-header-color: blue;
  --jp-mirror-editor-quote-color: #090;
  --jp-mirror-editor-link-color: #00c;
  --jp-mirror-editor-error-color: #f00;
  --jp-mirror-editor-hr-color: #999;

  /* Vega extension styles */

  --jp-vega-background: white;

  /* Sidebar-related styles */

  --jp-sidebar-min-width: 250px;

  /* Search-related styles */

  --jp-search-toggle-off-opacity: 0.5;
  --jp-search-toggle-hover-opacity: 0.8;
  --jp-search-toggle-on-opacity: 1;
  --jp-search-selected-match-background-color: rgb(245, 200, 0);
  --jp-search-selected-match-color: black;
  --jp-search-unselected-match-background-color: var(
    --jp-inverse-layout-color0
  );
  --jp-search-unselected-match-color: var(--jp-ui-inverse-font-color0);

  /* Icon colors that work well with light or dark backgrounds */
  --jp-icon-contrast-color0: var(--md-purple-600);
  --jp-icon-contrast-color1: var(--md-green-600);
  --jp-icon-contrast-color2: var(--md-pink-600);
  --jp-icon-contrast-color3: var(--md-blue-600);
}
</style>

<style type="text/css">
/* Force rendering true colors when outputing to pdf */
* {
  -webkit-print-color-adjust: exact;
}

/* Misc */
a.anchor-link {
  display: none;
}

.highlight  {
  margin: 0.4em;
}

/* Input area styling */
.jp-InputArea {
  overflow: hidden;
}

.jp-InputArea-editor {
  overflow: hidden;
}

.CodeMirror pre {
  margin: 0;
  padding: 0;
}

/* Using table instead of flexbox so that we can use break-inside property */
/* CSS rules under this comment should not be required anymore after we move to the JupyterLab 4.0 CSS */


.jp-CodeCell.jp-mod-outputsScrolled .jp-OutputArea-prompt {
  min-width: calc(
    var(--jp-cell-prompt-width) - var(--jp-private-cell-scrolling-output-offset)
  );
}

.jp-OutputArea-child {
  display: table;
  width: 100%;
}

.jp-OutputPrompt {
  display: table-cell;
  vertical-align: top;
  min-width: var(--jp-cell-prompt-width);
}

body[data-format='mobile'] .jp-OutputPrompt {
  display: table-row;
}

.jp-OutputArea-output {
  display: table-cell;
  width: 100%;
}

body[data-format='mobile'] .jp-OutputArea-child .jp-OutputArea-output {
  display: table-row;
}

.jp-OutputArea-output.jp-OutputArea-executeResult {
  width: 100%;
}

/* Hiding the collapser by default */
.jp-Collapser {
  display: none;
}

@media print {
  .jp-Cell-inputWrapper,
  .jp-Cell-outputWrapper {
    display: block;
  }

  .jp-OutputArea-child {
    break-inside: avoid-page;
  }
}
</style>

<!-- Load mathjax -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/latest.js?config=TeX-AMS_CHTML-full,Safe"> </script>
    <!-- MathJax configuration -->
    <script type="text/x-mathjax-config">
    init_mathjax = function() {
        if (window.MathJax) {
        // MathJax loaded
            MathJax.Hub.Config({
                TeX: {
                    equationNumbers: {
                    autoNumber: "AMS",
                    useLabelIds: true
                    }
                },
                tex2jax: {
                    inlineMath: [ ['$','$'], ["\\(","\\)"] ],
                    displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
                    processEscapes: true,
                    processEnvironments: true
                },
                displayAlign: 'center',
                CommonHTML: {
                    linebreaks: {
                    automatic: true
                    }
                }
            });

            MathJax.Hub.Queue(["Typeset", MathJax.Hub]);
        }
    }
    init_mathjax();
    </script>
    <!-- End of mathjax configuration --></head>
<body class="jp-Notebook" data-jp-theme-light="true" data-jp-theme-name="JupyterLab Light">

<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h1 id="Data-Mining-on-Titanic-dataset">Data Mining on Titanic dataset<a class="anchor-link" href="#Data-Mining-on-Titanic-dataset">&#182;</a></h1><h2 id="in-this-project-we-will-be-applying-assosiation-rules-and-do-multiple-data-vizualisation-using--seaborn-and-matplotlib">in this project we will be applying assosiation rules and do multiple data vizualisation using  seaborn and matplotlib<a class="anchor-link" href="#in-this-project-we-will-be-applying-assosiation-rules-and-do-multiple-data-vizualisation-using--seaborn-and-matplotlib">&#182;</a></h2><div class="alert alert-block alert-success">
<b>what we will see along this project:</b> 
    <ul>
        <li><b>how to import a rdata file</b></li>
        <li><b>reading a radom sampling from the dataset</b></li>
        <li><b>viewing frequency in dataset item</b></li>
        <li><b>aggregating data and count plot</b></li>
        <li><b>Casting dataframe to list and transaction enconding</b></li>
    </ul>
</div>
</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[13]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">pyreadr</span>

<span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">import</span> <span class="nn">seaborn</span> <span class="k">as</span> <span class="nn">sns</span>

<span class="kn">from</span> <span class="nn">mlxtend.preprocessing</span> <span class="kn">import</span> <span class="n">TransactionEncoder</span>
<span class="kn">from</span> <span class="nn">mlxtend.frequent_patterns</span> <span class="kn">import</span> <span class="n">apriori</span> <span class="p">,</span><span class="n">association_rules</span>
<span class="kn">from</span> <span class="nn">apyori</span> <span class="kn">import</span> <span class="n">apriori</span> <span class="k">as</span> <span class="n">ap</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h2 id="importing-dataset"><li>importing dataset</li><a class="anchor-link" href="#importing-dataset">&#182;</a></h2>
</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[14]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">rdata</span><span class="o">=</span> <span class="n">pyreadr</span><span class="o">.</span><span class="n">read_r</span><span class="p">(</span><span class="s1">'C:/Datasets/R_datasets/titanic.raw.rdata'</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h2 id="Dataset-exploring-and-sampling"><li>Dataset exploring and sampling</li><a class="anchor-link" href="#Dataset-exploring-and-sampling">&#182;</a></h2>
</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[15]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">td</span><span class="o">=</span><span class="n">rdata</span><span class="p">[</span><span class="s1">'titanic.raw'</span><span class="p">]</span>
<span class="n">td</span><span class="o">.</span><span class="n">info</span><span class="p">()</span>
<span class="n">h</span><span class="o">=</span><span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">td</span><span class="p">)</span>
<span class="n">td</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">100</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="text/plain">
<pre>&lt;class &#39;pandas.core.frame.DataFrame&#39;&gt;
RangeIndex: 2201 entries, 0 to 2200
Data columns (total 4 columns):
 #   Column    Non-Null Count  Dtype   
---  ------    --------------  -----   
 0   Class     2201 non-null   category
 1   Sex       2201 non-null   category
 2   Age       2201 non-null   category
 3   Survived  2201 non-null   category
dtypes: category(4)
memory usage: 9.3 KB
</pre>
</div>
</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[15]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Class</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3rd</td>
      <td>Male</td>
      <td>Child</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3rd</td>
      <td>Male</td>
      <td>Child</td>
      <td>No</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3rd</td>
      <td>Male</td>
      <td>Child</td>
      <td>No</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3rd</td>
      <td>Male</td>
      <td>Child</td>
      <td>No</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3rd</td>
      <td>Male</td>
      <td>Child</td>
      <td>No</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>1st</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>96</th>
      <td>1st</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>97</th>
      <td>1st</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>98</th>
      <td>1st</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>99</th>
      <td>1st</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 4 columns</p>
</div>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[16]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">td</span><span class="o">.</span><span class="n">sample</span><span class="p">(</span><span class="mi">10</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[16]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Class</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>387</th>
      <td>3rd</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1609</th>
      <td>2nd</td>
      <td>Male</td>
      <td>Adult</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1144</th>
      <td>Crew</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>601</th>
      <td>3rd</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1412</th>
      <td>3rd</td>
      <td>Female</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1462</th>
      <td>3rd</td>
      <td>Female</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>321</th>
      <td>2nd</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>2091</th>
      <td>2nd</td>
      <td>Female</td>
      <td>Adult</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>2061</th>
      <td>2nd</td>
      <td>Female</td>
      <td>Adult</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1723</th>
      <td>Crew</td>
      <td>Male</td>
      <td>Adult</td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[17]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">td</span><span class="o">.</span><span class="n">describe</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[17]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Class</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2201</td>
      <td>2201</td>
      <td>2201</td>
      <td>2201</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Crew</td>
      <td>Male</td>
      <td>Adult</td>
      <td>No</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>885</td>
      <td>1731</td>
      <td>2092</td>
      <td>1490</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>

</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h2 id="Dataset-transaction-frequency-visualization"><li>Dataset transaction frequency visualization</li><a class="anchor-link" href="#Dataset-transaction-frequency-visualization">&#182;</a></h2>
</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[18]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">colors</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">cm</span><span class="o">.</span><span class="n">rainbow</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">40</span><span class="p">))</span>

<span class="n">td</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()</span><span class="o">.</span><span class="n">plot</span><span class="o">.</span><span class="n">bar</span><span class="p">(</span><span class="n">color</span> <span class="o">=</span> <span class="n">colors</span><span class="p">,</span> <span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">13</span><span class="p">,</span><span class="mi">5</span><span class="p">))</span>

<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">'frequency of transactions'</span><span class="p">,</span> <span class="n">fontsize</span> <span class="o">=</span> <span class="mi">20</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xticks</span><span class="p">(</span><span class="n">rotation</span> <span class="o">=</span> <span class="mi">85</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">grid</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABCcAAAKGCAYAAAB5pfHVAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjUuMywgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/NK7nSAAAACXBIWXMAAA9hAAAPYQGoP6dpAAEAAElEQVR4nOzdd3wUdf7H8femVwIJkBCINEHRgCIIAiooJPyQYj1UlENE5ZRDERBF9AwWVO5EFDwVpSmHWPGsQPAQRAQhgDTpNZAQSEIKaZvk+/uD27mEdFyYoK/n47EPyO53Z97Tdmc+O/MdhzHGCAAAAAAAwCYedgcAAAAAAAB/bBQnAAAAAACArShOAAAAAAAAW1GcAAAAAAAAtqI4AQAAAAAAbEVxAgAAAAAA2IriBAAAAAAAsBXFCQAAAAAAYCuKEwAAAAAAwFYUJwDgd+rgwYMaPny4WrZsKT8/PzkcDjkcDn3++ed2R8MfVG5uriZOnKjLLrtMgYGB1jo5atQou6PBBs2aNZPD4dA999xjdxQAQC3gZXcAAID7HTx4UB06dNDx48ftjgJIkpxOp3r16qVVq1bZHQUAANRCnDkBAL9Dzz//vI4fPy4vLy+9/PLL+umnn7R582Zt3rxZPXv2tDse/oA+/vhjqzBxzz33aNmyZdY6+cQTT1R7OHFxcdYZF6h9evToIYfDoR49etgdBQBwnuHMCQD4HVq6dKkk6aabbtK4ceNsTgP8b52MiIjQu+++K09PT5sTwW779++3OwIAoBbhzAkA+B06fPiwJKl169Y2JwFOca2TLVq0oDABAADKoDgBAL9DBQUFkiRvb2+bkwCn5OfnS2KdBAAA5aM4AQC/E3PmzClzLf7EiROt507vFf/0a8N37dqlv/71r2rVqpUCAgLkcDjKnHZdWFiomTNn6oYbblBkZKR8fX1Vv359XXvttZo6dary8vKqzLlt2zYNGTJEUVFR8vPzU1RUlAYNGqS1a9dKOtUfgcPhULNmzcq89/vvv7em5fvvv690PK52cXFxlbb7+eefdf/996t169YKCgpSYGCgLr74Yo0YMUK7du2q8H0l5/f+/ftVXFysGTNmqGvXrqpXr54CAwPVrl07vfDCC8rJyalqtqi4uFgffPCBbr31Vl1wwQXy9/dXWFiYLrvsMt17771atGiRCgsLJZ3qXDIiIkIOh0N9+vSpcthbtmyxsk6aNKnK9hVZuXKlBg8erGbNmsnPz09169ZV+/bt9dRTT+nYsWNl2u/fv98a7/LlyyVJy5cvL7VOlrecy+Oa3xMnTrSeKzmcksvCpabreFJSkv75z3/qtttuU6tWrRQYGChfX181btxYN954oz788EMVFxdXmLG89fOjjz5Sz5491aBBA/n7++uiiy7SuHHjlJaWVun07ty5UyNHjlR0dLSCgoLk4+OjyMhIXX755br33nv14YcfWgWfktLT0zV79mzdfffduuSSS6z3RkREqHfv3poxY4ZVvKzKsWPH9Oyzz6pbt25q2LChfH19FRUVpW7duunZZ5/Vjh07rLau7bai5Vzesq7u3Tq+/PJL3XbbbWrSpIl8fX0VFhamLl266KWXXlJ2dnaF73PnNpqQkKBhw4apdevWCgwMtD67OnTooBEjRuiLL76QMabyGQoAqJwBAPwuzJ4920iq9DFkyBCrfffu3Y0k0717d/P555+bwMDAMu337dtntd+9e7e55JJLKh1+q1atzM6dOyvM+MEHHxgfH59y3+vl5WVmzpxphgwZYiSZpk2blnn/smXLrPbLli2rdH642j3zzDPlvu50Os2DDz5Y6fR4e3ubGTNmVDm/t2zZYq6//voKh9OpUyeTnZ1dYdZ9+/aZyy+/vMrlV3KaH3vsMSPJeHh4mMTExErnxaOPPmokGU9PzyrblqeoqMiMGDGi0mwhISFmyZIlZaarqmkqbzmXpzrr9+nrbE3W8cLCQuPh4VHl8GNiYkxWVla5GUuun0uXLjWDBg2qcDgXXnihSUpKKnc4H330UYXbScnH5s2by7y3adOmVb6vffv2FY7bZd68eeXOr4qWnWu7rcmydmUt+blUUm5urrn55psrHWZkZKTZsGFDue931zY6ZcqUaq0bFa0XAIDqoTgBAL8T6enpZvPmzWbz5s3WzvKDDz5oPbd58+ZSB6auA7fmzZuboKAg06BBA/PSSy+ZH3/80axevdpMmzbNHDt2zBhjzJEjR0x4eLiRZIKDg82YMWPMt99+a9avX2+WLVtmxo8fbwICAowk06JFC3PixIky+VavXm28vLyMJOPr62ueeOIJs2LFCrNmzRrz+uuvm4iICOPt7W0uu+yyCg9a3Vmc+POf/2y16dOnj5k3b575+eefzdq1a80777xjLr30Uuv1L774osz7Sx74dO3a1Xh4eJghQ4aYr7/+2iQkJJiFCxeaLl26WG2eeOKJcnMkJyebyMhIq931119v5s6da9asWWN+/vln8+GHH5rhw4eb0NDQUtO8Y8cO6z2TJk2qcD4UFBSYBg0aWNN5JlyFENf68tZbb5mff/7ZLFu2zDz66KPG29vbSDI+Pj5m48aNpcbtWvc6duxoJJmOHTuWWid37NhRrQyu9btkQankcFyPgoIC6z01WcedTqfx8PAw119/vfn73/9uFi1aZBISEsz3339vZs2aVWpZ/vnPfy43Y8n1s2vXrkaSuemmm8xnn31mEhISzDfffGP69u1rtbnjjjvKDCM5OdkqCjRs2NA8++yzZsmSJWb9+vVm1apVZt68eeaBBx4w9evXL7c40aRJE9O5c2fz3HPPma+++sqsXbvW/Pjjj2bevHnm//7v/6xxd+/evcJ5PXfuXKudn5+fGTlypPnmm2/M+vXrzYoVK8z06dNN7969TYsWLaz3JCYmVrqcy1vWVRUnBg4caOW47LLLzHvvvWfWrl1rFi9ebIYOHWocDoeRZEJDQ8sturljG/3ll1+swkTz5s3NK6+8Yr777juzYcMG88MPP5hZs2aZwYMHm6CgIIoTAPAbUZwAgN+hqg7MjfnfgZvr18cDBw5U2LZfv35GkomKijJ79uwpt8369eutg6qnnnqqzOsdOnQw0qmzEZYvX17m9cTERNOkSZMKf2U1xn3FiU8++cR6/Z133in3/bm5udYvrc2aNTNOp7PU66f/kv/++++XGUZeXp6Jjo42kkxYWFiZYRhjzE033WQN4+WXX65werKzs01aWlqp56699lojnTpjpSKfffaZNfxPPvmkwnYV2bRpk3VwFh0dbdLT08u0+fbbb602nTp1Knc4Jc9i+C2eeeYZa3qqUpN1vLi42OzatavS4f3tb38zkozD4Sj3DKGS66ck8/zzz5c7ntjYWCOdOlsoJSWl1OszZ84sVXypSG5ursnJySnzfGVnLhljzKxZs6zhL126tMzrhw8ftgqNDRs2rDTDoUOHyjxXk+VcWXHiq6++snL27NnT5Ofnl2kzY8YMq83AgQPLvO6ObfTpp582kkxgYKBJTk6ucFpOnDhhioqKqpxmAEDF6HMCAKCXXnpJF1xwQbmvbdmyRV999ZUkafr06WrRokW57dq3b68RI0ZIkmbNmlXqtZ9//lkJCQmSpOHDh+vaa68t8/7GjRvrlVdeOeNpqIkXX3xRknTzzTfrvvvuK7eNn5+fpk+fLulU3wmV9XFxyy236O677y7zvK+vr/76179KklJTU7Vt27ZSr2/fvl3//ve/JUk33nhjpbd9DQwMVL169Uo958q+a9cu/fjjj+W+b/bs2ZKk+vXrq3///hUOvyJvvvmm1c/CO++8o7p165Zp83//93+69957JZ1a1q7+Q2qTytZx6VT/FRdeeGGlw/jb3/6m+vXryxijL774otK2HTp00JNPPlnueEaPHi3pVB8uP/30U6nXk5OTJUn16tVTdHR0hcP38/OTv79/medbtWpVaa6hQ4eqffv2kqTPP/+8zOvTpk2z+l94++23K83QpEmTSsf1W7zxxhuSTnWgOnv2bPn4+JRpc//996tXr16SpM8++0xJSUkVDu9Mt1HX8mjdurXCw8MrHH5ISIg8PNitBoDfgk9RAPiD8/Hx0Z/+9KcKX3cdPAcEBKhv376VDstVdDhy5IgOHTpkPb906VLr/0OHDq3w/TfffHO5B7/udPjwYatQMnDgwErbtmnTRvXr15ekMgeRJd11110VvtahQwfr/3v37i312jfffGN1ovfoo49WHrwct912mzW/XEWIko4ePapvv/1WknT33XeXe4BXFdeyu+SSS3TVVVdV2O7+++8v857aoqp1vDzFxcU6cuSIduzYoS1btmjLli369ddfrQPyX375pdL3Dxo0qFTntCVVtk40atRI0qmOLV3b3pkyxig5OVk7d+60pmHLli2KjIyUVP40fP3115Kk5s2b68Ybb/xN4z9ThYWFVseaMTExioqKqrCta70rLCystIB4ptuoa3ls27ZNP//8c5XZAQBnjuIEAPzBtWrVSn5+fhW+vm7dOklSTk6OvLy8yr1DguvRr18/632uXxwlafPmzZJOHSS2a9euwnF5e3tbv+qeLa7pkaQ777yz0ulxOBw6fvy4pNLTc7qLL764wtdCQ0Ot/2dlZZV6bcOGDZJOTXdlB/4V8ff316BBgySduivEyZMnS73+/vvvW3f4cJ3ZUBP5+fnWHUs6d+5cadv27dtbtwndsmVLjcd1NlW1jrsYYzRv3jxdd911CgoKUuPGjXXxxRerbdu21mPjxo2SZK0XFTnTdWLAgAFWwenmm2/W9ddfr1dffVUJCQkqKiqqchqkUwWGfv36KSQkRI0aNdJFF11UahpcBYjTp8HpdFrL7pprrqmwuHK27d271zp7o6r1ruTrla13Z7o87rzzTnl7eys/P1/dunVT//799dZbb2nr1q3cnQMA3IziBAD8wZ1+qcDpUlJSzmi4JW/Nl56eLunUQYCXl1el76vs1Gl3cMf0nC4gIKDC10qe6n36waXr4DA0NFS+vr5nlMv1y3FWVpY+/fTTUq+5zqa48sor1bZt2xoP27XcpKqXi7e3t8LCwiSpyttknmtVreOSlJeXp759+2rw4MH6/vvvlZubW2n7ql4/03UiLCxMX3zxhRo3bixjjJYtW6bRo0erY8eOCg0N1a233mpdZnU6Y4zuu+8+9evXT19//XWZA+2qpiEtLc064HadMWCHkutPVetdREREue873Zkuj4svvlgffPCB6tWrp8LCQn311Vd68MEHFR0drYYNG2rw4MH64YcfKs0IAKieyvcQAQC/e56enpW+7tpZb968eZXX2ZfUvHlz6/+uA57q/BJ7tn+NLHnw8a9//avSMzlKqs4B7pn6Lb9QX3755erQoYMSEhI0e/Zs/fnPf5YkrVmzxrp+/kzOmjiTjLX1l+Sq1nFJeuGFF6xLYLp3764RI0boiiuuUEREhPz9/a0D2GuvvVY//PDDWZ3Wa665Rrt379ann36qb775RitWrFBiYqIyMzP12Wef6bPPPlPv3r312WeflTronjVrlmbOnCnp1HoxatQode7cWY0bN1ZAQIA1H/785z/r/fffr3Qa7Dpr4nS1Icett96qXr166cMPP9TixYv1ww8/6NixYzp+/LjmzZunefPmaciQIZo1axb9TgDAb0BxAgBQKdev4UePHtXFF19c5ZkP5XGdNp2amqqioqJKDxYrO7Oh5I6/q5PG8px+eUNJrumRTh34VNbh39nm6s8iNTVVBQUFZ9QnhHSqY8yEhAQtX75ce/fuVYsWLayzJvz9/XXnnXee0XBLFmQqu6xFOnXNv+uX65KnyZ8PjDF69913JUlXX321/vOf/1R4kFnybJKzyc/PT3fddZfVV8LevXv19ddfa/r06dq5c6cWL16sCRMm6NVXX7Xe884770iSWrZsqVWrVpXbYaZU8TSEhobKw8PD6m/DLiXXn6rWu5Kvn831LiQkRA888IAeeOABSaf6oPjiiy80bdo0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BQQEyNvbWw0aNFDXrl01ZswYLV++3DrF/3R33nmnNmzYoMGDBysyMlI+Pj7Wr5IrV660fgmtjIeHhz755BO98cYbuvLKKxUYGKjAwEC1a9dOL7zwgtasWVOt07s9PT318ssva9u2bRozZozat2+vevXqydPTU8HBwbr00kt11113ae7cuUpKSqqwg0F3aNGihTZu3Kg5c+aob9++atSokXx8fFS/fn1ddtlluv/++7V06VJde+21lQ5n8ODB1v9jYmIUFRXllnweHh564403tGLFCt1111264IIL5Ovrqzp16ujyyy/Xk08+qV27dlmX/5xt8+bN0+TJk9WpUyeFhIS45faNISEh+vHHH/Xcc8+pbdu28vPzU1BQkNq0aaOxY8fql19+qXL+u8PUqVP16aef6i9/+Ys6duyoxo0by8fHR/7+/mrdurXuuecerVy5UtOnTy8z3d7e3vr666/1+uuvq2PHjgoICJC/v78uvPBC/eUvf9H69eutsxGqMmTIEO3Zs0cTJkxQhw4dVLduXfn4+OiCCy7Q1VdfrRdeeEHLli0r876goCCtWrVKjzzyiNq0aVPqVqc15efnp88++0xffPGFbrnlFuszo169eurcubNefPFF7dixQ5dffvkZj6MqH330kf71r3/pnnvu0eWXX66IiAh5eXkpKChI0dHReuihh7RhwwaNHz/+rGUAgD8KhzG19KbkAIA/pHvuuUdz585V06ZNtX//frvjnFe+++47qzPKDz/80OokEwAAoLbjzAkAAH4nZs2aJUkKCwsrdZcVAACA2o7iBAAAvwP79+/Xxx9/LOlUR5i+vr42JwIAAKg+L7sDAACAM3P48GHl5ORo3759euKJJ+R0OuXn56dRo0bZHQ0AAKBGKE4AAHCeuuuuu7R8+fJSzz377LNq3LixTYkAAADODMUJAADOcwEBAWrdurVGjRqlIUOG2B0HAACgxmrU50SzZs3kcDjKPEaMGCFJMsYoLi5OkZGR8vf3V48ePbR169ZSw8jPz9fIkSNVv359BQYGasCAAUpMTHTfFAEAzmtz5syRMYY7dVTD999/L2OMTp48qQ0bNlCYAAAA560a3Ur02LFjKioqsv7esmWLYmJitGzZMvXo0UMvv/yyXnjhBc2ZM0etW7fW888/rxUrVmjHjh0KDg6WJD344IP68ssvNWfOHIWFhWnM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an377bdas2aNwsLCNGnSJLVu3fqcZiwoKNDDDz+sDh06qEuXLqXOOnE5fPiwvvzySy1atEgnTpzQq6++qvbt25/TnJL03HPPadmyZQoJCVGbNm3UoUMHhYeHy8PDQ8nJydqwYYPWrVunXbt26cYbb9Tjjz+uhg0bntOM33zzjb777jvFxMSoS5cuZQ7oi4qKtHHjRn366adatGiRYmNj9eSTT6pOnTrnNKd06iDVx8dHvXr1Uvfu3Ut9oTqdTiUnJys+Pl5Lly7VwYMH9fTTTys2Nvacb/N79uzRAw88IG9vb4WHh+uqq65Sy5YtFRgYqOzsbP36669at26dNm/erIYNG2rChAm27Gx//PHHeu+995Sdna2CggJdfPHFatKkifVlu3PnTm3atElBQUF6+OGHdd99953znZjzZXs/evSo7rjjDoWEhCgnJ0fNmjVT06ZNFRAQoPz8fO3du1dr167VgQMH1K9fPz377LNq1qzZOc0oSV9//bVee+01XXrppbrkkkvUvn17RUVFyc/PT1lZWdqyZYtWrFihJUuWqLi4WE8//bRuvvnmc55Tkj766CMtWbJEd999d5ntIz8/X+np6dq6dasWL16sn3/+WTfccIMefPDBMr9an2333XefAgIC1K1bN1177bVq1KiR9VpBQYHWrVunxYsX68cff5Sfn58mTZqkdu3andOMkvT2228rMTFRt956qy6//PJSrxUVFSkzM1NbtmzR119/rRUrVmjQoEEaNmxYmV+tz6bHH39cmzZtktPpVFBQkFq3bq369evL4XAoJSVFmzdv1vr169WkSRNb103p1L7dk08+KUny8PBQx44d1axZM2tb2rlzpxISEpSYmKiYmBg98cQTat68+TnLl5aWpr/+9a+6/PLL1alTJ3Xq1KnUL6fFxcVatWqVvvrqKy1fvlwXXHCB/vGPf9j2A9n58H303HPP6YILLtD1119f7nzKysrS+vXrtXDhQv3444964okndPPNN5/TnDt37tTs2bPVv39/XXXVVeWO++jRo4qPj9f8+fMVGBiol156ybYzpoYMGaITJ07o2LFjatiwoVq3bq26deuquLhYiYmJ2rRpk3bv3q0uXbromWee0RVXXGFLzrFjx+rw4cO65pprFBMTo1atWlmvZWdna/369Vq6dKl+/vln+fj46Mknn9RVV111znOOHj1a27ZtU2FhoS644AK1a9dODRo0kMPh0JEjR7Rp0yYlJCTIw8NDjzzyiO677z63jZvihKTk5GTt3r270up+bm6ujhw5on//+9/auXOnhg8ffs53WtPS0pSdnV3mdB9J1odGUVGRMjIytGTJEq1fv1533333Od95KS4u1oEDB0p9eZZ3GtXJkyf1yy+/6PPPP9c111yj/v37n9Ocxhjl5uZaX7LFxcWlzvAo2W7nzp1avHixLrroIvXu3fuc5pSkL774Qo0bN1aHDh3KfT0/P1/Hjh3TmjVr9Mknn6h9+/bnfOc6Ly+vVCHM6XTK09OzzBdabm6uVq5cqV27dql///627MC4fjV3KSwsLPeMqP379+vTTz9VgwYNNGjQoHN+1lRiYqLi4+Mr/JW0qKjI2tn69NNPlZqaqpEjR57zL92FCxdq+/btuueee0odUEmntp/s7GwdPnxY8fHx+vzzz3X11VfrkUceUWho6DnLePr27nrOGFNqHS0qKtLu3btt295TUlI0c+ZM3XzzzRVeZpCZmalffvlFc+bM0dGjR/W3v/1NnTp1Oqc5Dxw4oJMnT1b6K1R+fr6OHj2qjz/+WImJiXrwwQfPeRG6JnJzc/Xll19q7969GjRoUKnv2nNhz549pXbsCwsL5enpWep7My8vT1u3btXHH3+s9u3b6/bbbz+nGWsiOztb3377rVJTU3X77bdbvwyfCzNmzFC9evXUv3//cn+gycnJUWJior744gv9+9//1h133KH77ruvzKVy58KmTZu0ZMkS/eUvfyl12ZZLXl6e0tLSlJCQoAULFigoKEhjx44tdVBzNrlOkS95Ke7p+57SqXm6ceNGLVmyRNddd51tZ0eeD99H5SnvDLi0tDTFx8fLy8vrnBcnKlJezm3btmnNmjXq0KGDLQVTSXrqqafUo0cP9erVq8xrrn2lbdu26dNPP9XmzZs1ZswYW8443bFjR6kzYcq7VCs/P19bt27VvHnzdNFFF2no0KHy8fE5pznffvtttW7dWtddd125r2dlZSk5OVnffvutFi1apP79++vee+91y2coxYkSXB2QVHUAsmnTJgUHB5/TynVJrut6q+pP4siRI/L09Cz1hXIuldyxquy0Y2OMDh06dM53BF2Kiork4eFRqpMnh8NR7nqQnp5+TnewTledfkT27NmjPXv2qFu3bqWu+T5XTv+g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UZF54403TGRkpJk1a5bp0qWLef75590+ztpxXnUtYP57OtC8efP09NNPa/z48brwwgvVunVrDRo0SG+88YbVc3/J9nb56quv9NJLL6l///5Wlg4dOuitt97SihUrdPz4cVvzuRw/fty6/mz58uUyxqhfv36qW7euAgMDlZaWJsm++eka7+rVq607N7iuoZakkSNHql69ekpISJD0v+vE7HL06FF5eHiocePGysjI0DfffKNhw4apR48euueee7R48WLbcxYXF2v79u2Ki4srlaVu3bp6/vnntWjRIttPrXQt982bNyssLEwjR460Okf08vLSrbfeqj59+mjhwoWS/nfdqh1cp/8dOnTI6ufmk08+UYsWLXT33XfriSee0NGjR5WSkmJbRul/OU+cOKGOHTtKkt577z317t1bzz33nF566SV9/vnnteJ0dNetbiVp0aJF8vf3V/fu3eVwOFRcXKzCwkKbE55y4MABtWnTRpL0+eefKyAgQOPHj9f06dN18uRJ7dq1S5J9n5+uZf7JJ59oxowZuvvuu9W4cWO1aNFCgwcP1uuvv64PP/xQubm5tuQ7XUFBgQICApSammp1fnnLLbeoQYMGatu2rZKTk8vtkPBcKfl91Lp1a+sODua/l26NHTtWbdu21RdffCHJ3s8l6VSfLEFBQTp06JD279+v77//Xn/605/k4eGhqKgopaamSrLn+8g1Lw8fPixfX1+1bNlSKSkp+uqrr/TQQw/pscce0z333GN9xtv93e7alo4cOWJt8/Pnz1e7du00cuRITZkyRb/88osyMjJszZeZmWl9hn/11Vdq0qSJrrrqKoWFhSkvL09Op9OWfBV56qmnNGXKFH3//feaPHmypkyZovj4eHXt2lUvv/yy7fvyJ0+elLe3t7Kzs/Xjjz/qyJEjVj8IPj4+KigokGT/Z7yfn58OHTokSfrwww911VVXqVmzZgoLC1Nqaqotd107nWsb/vTTT7Vv3z5t3LhR7777rp555hn961//0pw5c/Trr7/qo48+KtX+XHMtyy1btsjhcOjNN9+09onDwsL00EMPqV+/fpo7d64k+z7nXTkTExPl5+en8PBwHT58WMuWLdODDz6ooUOH6vbbb9eXX37p9pwUJ/7LdV3PyZMnS21krg2ze/fuSklJsa6ftOuaJVdOT09PpaenW1lceVq1alUrPihcOTt27KjExES98cYbmjlzppo3b66OHTvKGKPjx49bfWTYxTXfgoODlZiYqPT0dHl4eJSaz3l5eda1qnYtd9eHhKsvjKVLl+qnn37Shg0bdOutt0o6te66OvOy8ws3MzNTjRo10rJlyySpzJ0QXNex1QauD9MDBw6U6S3Zw8PDKqzZOT9d869Zs2batGmTsrKy9OGHH6pz586KiopSUVGRsrOzS/VBYpeMjAyFhYVp+/btSkpK0ldffaVbbrlFPj4+CgsLU0ZGhq3XUbvmZUxMjPbv369HHnlEs2bN0pVXXqmLLrpIJ0+e1IkTJ6w7Jtj9OR8UFKSDBw8qPz9fCxYsUJs2bawDl/T0dOv6T7v7FzLGlNtJ2pVXXqnExETbv49cioqK1LdvX40bN05jxozR1q1bdeedd0o6taPo2obsPmApKChQYWGhTp48Kel/y9ff319+fn6lOu21U506ddS9e3cNHz5cjz/+uHJzc3XbbbfJGKOtW7dat7i1M2dOTo4CAwO1a9curV69Wlu3btVNN90k6dT3u+s7wO556drmIyIitGXLFuXl5emjjz5Sz549FR4eLl9fX2VmZtp2RwlXvtjYWK1YsUJTp07VvHnz1KVLFzVv3lzZ2dk6efKktcxrw3d8amqqTpw4UeqONp6engoNDdUjjzyipUuX2v4Z3759e9WvX1933HGH/v73v6tVq1a68sorlZubq/3791uf93bnHDp0qObNm6chQ4ZozZo1+tOf/qSAgABt3rxZfn5+ioyMtDWn9L9tePv27Wrbtq0uvPBCSf+bhr59+6pp06batGlTqfZ2yczMlI+Pj3Us5/qBzNvbWxEREcrLy7Oet0PJ446goCDt3LlTK1eu1O7du63P0JIdR7szJ8WJ/3JtUDfeeKNeffVV7dmzR/n5+dbr77zzjurUqVMreh+WpMGDB+v555/X6tWrlZ6ersLCQhUXF+upp55SixYtbOsR3cU1P//0pz+pU6dOGjdunEJDQ/XQQw9JOvVLW2pqqtq2bVuq/bnm+gVg8ODB+uWXXxQXF6edO3cqLy9PKSkpGjp0qHJycqzb9NmV0zXeli1b6oYbbtBtt92m+++/Xz169FDbtm2VlpamVatW1Yq7NAQEBGjw4MF68skn9dFHHykpKUk5OTn69ttv9eCDD+rGG2+UVDsO+K+55hr5+flpzJgx1q/QxcXF+vvf/66vv/5affr0kWTvF65r3CNHjtTnn3+u6OhopaSkaNCgQfL29tbSpUvl5+enJk2a2J7V29tbt912m0aOHKmhQ4eqoKBAt99+u4qKirRu3TrrtrF2/WLhmje33HKLbrzxRi1dulQDBw7UsGHDJEmzZ89Wbm6utR3ZvUN477336ocfftCAAQOsHcLg4GBt2bJFktS0aVNbc0qndlz69++vCRMmaP/+/crMzLRemzZtmlq1alVrOr/19/fX008/rfr162v//v3617/+pbp16yolJUVz5861bkFn12eT6/soJiZGhw8f1ksvvaQTJ05Y28uMGTO0Zs0aXXPNNZKq19P72dSwYUONHz9ederUUVBQkPVL36+//lrq89MOrm3i4osvVocOHXTHHXfob3/7my666CJ17txZJ0+e1LZt2yq9Fa4d/vKXv+i1115Tjx49lJKSojvvvFMeHh5auXKl6tWrZ1sR2jU/7733Xl100UV68cUXFRsbq0GDBkmS5s6dK19fX+uAsDYUJ3Jzc3XBBRdYZ8eUdOjQIatoaudZM+3atdOYMWOsW4VOnDhRkrRkyRIlJiZat2O1e34+8MADuvnmm5Wbm6sFCxYoOjpakvTuu++qc+fOtaIj4ZIFvoMHD2rbtm2S/rd8s7Ozdfz4cWtfyS6unJdddpm8vb31zDPPKCcnx/qBbPHixfrwww+tW3javR9y2WWXqV27durTp48mTJigLl26KDo6Wmlpadq6dat11yC35nT7hSLnuT179ph27dqZ5s2bmzvuuMPce++9pn379sbHx8e89957dsezJCUlmZiYGBMWFma6du1qrr76ahMaGmrCw8PNihUr7I5XpV9//dX85z//sfX6XmNOXfvluqZu5syZplmzZsbT09P4+PgYh8Nh2rRpY5YuXWp7XxMlFRQUmEWLFpmVK1eagoICY4wxW7duNU899ZT59ttvjTH29+Fx/Phxc88995jAwEBrXvr5+ZXqWNZurqyLFi0yl1xyiZXR09PTNGjQwLz22mu2X5t4uk2bNpkPP/zQHD161Hpu+vTp5oUXXqg16+jx48fN+PHjzeOPP251eJuYmGiGDh1qnnnmGWNM7bke2ZjS18impqaaXbt2WdtVbfDBBx+YsWPHmp9++smab88//7y55557rE5H7bZ+/XrTpk0b07p1a3PHHXeYO+64wzRt2tQ0aNDALF261O54VSosLDS7du0yBw4csDuKtYynTZtmwsLCTFhYmLn44otNWFiY8ff3N3//+9+tTkZrk5LbkdPpNAcPHjTHjh2zMdH//Prrr+axxx4zb775pvXZ+cMPP5jbb7/duq67Nn0mffTRR+bvf/+71U9GQUGBeeaZZ8yTTz5pc7KKbd261axevbpWdBjvkp+fbyZOnGjCw8PNSy+9ZJYvX25++ukn8+STT5rGjRtbfQvVlmWfl5dnbUcFBQUmOTm51m3rxcXFpfaLsrKyas127rJz507TqVMnc91115l58+aZVatWma+++spcddVVplOnTlYH/XbuM7nWuXnz5pn69eubunXrmujoaBMVFWV8fX3NiBEjrPlaG/btEhMTzauvvmo+//xzk5mZaYw51efMX/7yF6sPJHduR9xKtBwFBQWaNm2aNmzYoNzcXF144YW67bbbdMUVV9SK66VL+uKLL/TLL78oLy9PrVq10v/93//VmrM7XNavX6+EhAQlJyfrvvvuU6NGjZSdnW3rtb2V2bBhg1JSUtSgQQM1bdpUYWFhdkcqpbCwsNTtxlyXchQWFpa6JKU2SE9Pt37ljYqKUkRERK29j/wvv/yivXv3KigoSM2bN1fTpk0rvZ+3HbKzs5Wfny8vLy+FhIRI+t+lMrUta35+vnx8fORwOGSMUWpqqry8vM7dragqUVxcrC+++EI//vijkpOTNWnSJEVFRWnPnj1q3LhxrVtHs7OzrduzSqfmbV5enrUO1AaZmZl66623tG3bNjmdTl1++eW69dZbbb9073T79u3T4sWL9c0336hbt256/PHHlZaWprS0NOtX39oiJSVF33//vY4cOaLg4GB17txZF110Ua3a1tetW6fFixdr6dKluuuuu3Tffffp0KFD1j5JbZKRkaGgoCB5enqquLhYmZmZ8vPzq3Xbe3FxsXJyckrdAvHkyZMyxti+32SM0dKlS5WQkKCCggKNGjVKderUsW55XFsUFhbKy8tLeXl5mjx5sqZPn271gdKmTRs9+OCDGjZsWK1Y9p9++qmWLVumtWvXavLkyerevbt++eUX1a1b1zo7zm6HDh3SvHnztHLlStWpU0fvvfeeCgsLtWHDBrVr18729VKS1V+cl5eXfvnlF40fP17Lly9Xbm6u/P39dcMNN+jJJ59U+/bt7Y5aSnp6uhYvXqz9+/fLx8dHV1xxhTp06FArbhFfUk5OjgoLC0udceS6nNzt30luK3P8jrl6Kbab6xY+BQUFpqCgoNZUeyuzaNEi06RJE+vWrK6KZVxcnHnhhRdsr7L/8MMPZuPGjWbXrl3m4MGDJi0trVb9anq6nJwcM3nyZNO+fXvjcDisu50sWrTIzJo1y9a7tOzfv98sX77cbN682ezatcscPnzYZGZm2r6My7Np0yazadMms3//fnPkyBGTkZFRK3OWtHbtWtO/f3/j7e1twsPDTU5OjsnIyDCvvfaa2bhxo93xLD/++KN59NFHTatWrcy4ceOMMcbs3bvXLF++vNb8CuS6TWPPnj2Nw+GwfqF85plnzOOPP25zuv85cOCAeeyxx0y3bt3M5ZdfbtLT043T6TQLFy6sFb/yV6Y2/Nrj4sqya9cuExsbay699FLTokULc9dddxljjNmwYYO5//77rV+A7MqemJhokpOTTXp6usnIyKg1+x6nc82fn376ybRr185cf/31pn79+tZdBr777jszZMgQ624ydiouLjbvv/++ueuuu0x4eLiZO3euMeZU9lWrVtW67/uEhARz7733moYNG5quXbsaY07d6nz+/PnW7UXtUlhYaN59913TsGFD61brx44dM3l5eebhhx82U6ZMsTVfVU6ePGmOHj1qUlJSrLsj2MW1Db3//vsmPDzc3HPPPcbDw8N88803xhhjZs+ebf785z/XirMSkpKSzJ///GfTqlUr069fP3PxxRcbY06tlyNGjDD/+Mc/jDG16zO/pMzMTJOammr73RZdjh07Zo4dO2YyMzNNdnZ2rd/3zM/PN6+//rqJjY01DofDzJ492xhjzJIlS8ySJUvOyt247OuZrBYwxsjhcGjNmjV65pln1K5dOzVs2FARERFq2LChwsPDVb9+fQUHB8vb29vqDNOunN9++61GjRqldu3aKSwsTOHh4WrQoIEaNGighg0bql69evL29lajRo1qRed4eXl5Gj16tB5++GE99thjqlOnjvWr6SWXXKJJkyZZvZHbIT09XcOGDVPDhg3l7++voKAgBQUFKTg4WPXq1VNwcLCCgoIUHh6u2267zbac0qkKpYeHh6ZNm6a5c+fq+eef13PPPWdVMIODg/XBBx8oOjpaV155pbXOnMtsH3/8sd544w01a9ZMvr6+1vx0LfeQkBB5enqqd+/eVidP55prvowdO1bZ2dmqW7eutcyDg4Otv0NCQhQQEKBbb73V6hTTzry7d+/WmDFj1KxZM7322mt65ZVX5O/vr8LCQh04cEBz5szRq6++ek6Xe0mudWDVqlV6+OGHFRUVpTp16lh348nIyNCMGTOUnZ2tG264wbac0qlO0p577jm999576tu3r0JCQqxf/Nq2bauJEyfqpZdesiVbSWlpaZowYYI2btyoPn366P3335efn58KCgq0du1arVy5Uv/4xz/OeS7Xsps/f77efPNN63uzUaNG1vdmw4YNVbduXfn4+CgoKMj266WLi4vl6empN954Q35+ftqyZYseeeQR5eTkSDrV2ezBgwe1fft2xcbG2rZ+3nrrrQoKClJYWJj1CA0NVWhoqPWZFBQUpC5dutjeCaqnp6eee+45XXvttZo2bZp69+5t/dJ36aWXavPmzdYdhFyfD+eSaxnOmTNHcXFxGjRokHWWmXTqc2DWrFl6+eWXbT9jxpV1y5YtGj16tEJDQ3Xbbbdp/fr1kk6dHec6g2bMmDG2rZ/79u3Tiy++qBkzZuj6669XixYtFBgYKG9vb7Vq1UqffvqpHn30Udvyucb7008/adeuXYqKilK9evVUt25dax8kICDgnOcqj2v+/O1vf9M//vEP3X333frmm2+sDk+vuOIKxcXF2XomrGu7/e6777Rx40bt2LFD3377rZ566ilJUv369RUYGKh169ZZ7e08u/zdd99VaGio6tevby131758beCanw8++KAyMjLUuHFj6/Pd9QgODrY6Pu7YsaOtZ8m5tqfXX39db775psaMGaNNmzZZx8IFBQV6++23deGFF1p3knOXP3RxwvXhUFhYqJycHK1evVpHjx5VWlqaTp48KafTqeLiYtWtW1eenp566KGHFBcXd86/aF05IyMjdd111yk/P1979uzRypUrdeLECeXm5io7O1v+/v7Ky8vThAkTbMl5uqNHjyo5OVmPPfaY9u3bJ09PT+vUr8aNG+vw4cOSZNsXmY+Pj8aMGaPs7GwdPXpUR48eVUZGhpKSkqzLJnbu3KmWLVvqtttus3V+mv9efTV37lw9/vjjuvnmm/XUU09ZRai2bdtq7969tnTs5Jon119/vQICApSVlaXk5GQdP35cGRkZOnDggE6ePKn09HTt2bNHc+fOVZs2bWyZnyU7ak1KStLx48d17NgxHTlyRNnZ2crKypKnp6dSUlJ0/Phx3Xzzzec03+lK7mzl5ORo7ty5+uijj6ztyFVQWbNmjST7dg5cy/KNN97QFVdcoRkzZmjw4MFW8axVq1Y6fPiwjhw5Umq6ziXXOI8cOaL8/Hz17dtXGzdulK+vr7XDGhwcbN2G2a7t3ZVzw4YNWr16tbZv366NGzfq448/lq+vrxwOhxo2bFjqNrfncpm7lpuriLd7926tXLlSqampysrKUkFBgQoKChQUFKSCggK9/vrruv/++239/HRtF5s3b1aPHj0kSTt37rQ6Oq5bt64yMjJsLUQWFRWpd+/eSklJUXJysjZu3Ki0tDTrLgheXl7y8fGRr6+v9u7da1tO6X/zc8eOHbr33nslnbrjkavH/vDwcB09etQ6KLDzQPXll19WXFychg4dqo8//ti6TLNdu3basGGD7bdjlf43P7/99lt5eXnps88+0+uvv64dO3ZIOtXBn4eHh/X3uf6cd227e/bskaenp2688UZ9++23Cg4Olq+vrzw8PFSnTp1St4618yB1yZIlmjdvnurVq6fCwkI5HA55eXnJ399f/v7+ql+/vnx8fPTwww/rsssusy1ncXGxjh49qpiYGOs4xNWxZL169XT06FFbL4N0Lffdu3crKipKDodD69atszrcdzgcys3NtfZPjY29BOTn5+vNN9+Up6endStwDw8P64dlf39/q4g+adIkWzK6vv8uueQS7dmzR0eOHNEvv/yi9PR05eTkKDc3V35+fgoJCVF2dra2bdtm3SnQTtOnT9fUqVN100036YUXXih13LF+/fqz8r3+hy5OuHTr1k0rVqxQUVGRCgoK5HA4dPLkSWVkZGjz5s16+eWXtXr16lIfvHbsZF1xxRV65513yjyfn5+vZcuWafz48frll18UGBgoyb6cLq5bmqalpSknJ0cBAQFWtr1791q/Vtr1RRYYGKgHHnigzPN5eXlatGiRnnjiCeXl5Sk2NlaSvR+8rnmUlpZm7QCmpaWpUaNGVrZjx45ZX2x27AxeccUVuuKKK8o8f/DgQb322muaOnWqGjdubN2hxU733Xdfuc8nJCRo3LhxWr9+vbp3727b2VIuRUVF8vDwKHU7xv3795c6Myo1NdXKadc66iqK7dmzx7o14759+6xrOwMDA3Xs2DFbr0t1bRPZ2dkK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
"
class="
"
>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[19]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">td</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[19]:</div>




<div class="jp-RenderedText jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/plain">
<pre>Class  Sex     Age    Survived
Crew   Male    Adult  No          670
3rd    Male    Adult  No          387
Crew   Male    Adult  Yes         192
2nd    Male    Adult  No          154
1st    Female  Adult  Yes         140
       Male    Adult  No          118
3rd    Female  Adult  No           89
2nd    Female  Adult  Yes          80
3rd    Female  Adult  Yes          76
       Male    Adult  Yes          75
1st    Male    Adult  Yes          57
3rd    Male    Child  No           35
Crew   Female  Adult  Yes          20
3rd    Female  Child  No           17
                      Yes          14
2nd    Male    Adult  Yes          14
       Female  Child  Yes          13
3rd    Male    Child  Yes          13
2nd    Female  Adult  No           13
       Male    Child  Yes          11
1st    Male    Child  Yes           5
       Female  Adult  No            4
Crew   Female  Adult  No            3
1st    Female  Child  Yes           1
dtype: int64</pre>
</div>

</div>

</div>

</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h3 id="-Grouping-the-dataset"><li> Grouping the dataset</li><a class="anchor-link" href="#-Grouping-the-dataset">&#182;</a></h3>
</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[20]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">td</span><span class="p">[</span><span class="s1">'Survived'</span><span class="p">]</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()</span>  
<span class="n">td</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'Class'</span><span class="p">,</span><span class="s1">'Sex'</span><span class="p">,</span><span class="s1">'Age'</span><span class="p">,</span><span class="s1">'Survived'</span><span class="p">])</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[20]:</div>




<div class="jp-RenderedText jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/plain">
<pre>Class  Sex     Age    Survived
1st    Female  Adult  No            4
                      Yes         140
               Child  No            0
                      Yes           1
       Male    Adult  No          118
                      Yes          57
               Child  No            0
                      Yes           5
2nd    Female  Adult  No           13
                      Yes          80
               Child  No            0
                      Yes          13
       Male    Adult  No          154
                      Yes          14
               Child  No            0
                      Yes          11
3rd    Female  Adult  No           89
                      Yes          76
               Child  No           17
                      Yes          14
       Male    Adult  No          387
                      Yes          75
               Child  No           35
                      Yes          13
Crew   Female  Adult  No            3
                      Yes          20
               Child  No            0
                      Yes           0
       Male    Adult  No          670
                      Yes         192
               Child  No            0
                      Yes           0
dtype: int64</pre>
</div>

</div>

</div>

</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h2 id="Survival-rates-view"><li>Survival rates view</li><a class="anchor-link" href="#Survival-rates-view">&#182;</a></h2>
</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[80]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">for</span> <span class="n">itr</span> <span class="ow">in</span> <span class="p">[</span><span class="s2">"Class"</span><span class="p">,</span><span class="s2">"Sex"</span><span class="p">,</span><span class="s2">"Age"</span><span class="p">,</span><span class="s2">"Survived"</span><span class="p">]:</span>
    
    <span class="n">ax</span> <span class="o">=</span> <span class="n">sns</span><span class="o">.</span><span class="n">countplot</span><span class="p">(</span><span class="n">x</span><span class="o">=</span><span class="n">itr</span><span class="p">,</span> <span class="n">hue</span><span class="o">=</span><span class="s2">"Survived"</span><span class="p">,</span><span class="n">palette</span><span class="o">=</span><span class="p">[</span><span class="s2">"#DC3535"</span><span class="p">,</span><span class="s2">"#379237"</span><span class="p">],</span><span class="n">data</span><span class="o">=</span><span class="n">td</span><span class="p">)</span>
    
    
    <span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s2">"Count"</span><span class="p">)</span>
    <span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="n">itr</span><span class="p">)</span>
    <span class="n">plt</span><span class="o">.</span><span class="n">grid</span><span class="p">()</span>
    <span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="n">label</span><span class="o">=</span><span class="s2">"Survival count plot"</span><span class="p">,</span>
          <span class="n">fontsize</span><span class="o">=</span><span class="mi">20</span><span class="p">,</span>
          <span class="n">color</span><span class="o">=</span><span class="s2">"brown"</span><span class="p">)</span>
    <span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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6+o87ADFMywXt23TO8i1OvfCqkyL556DqaUlivLyoH143Ellu8haBgaizZgxuBQVhdy0NCQ8doNNhakperz9Ngqysur9S77XBx/AVKXC+a1bkZeejpNr15bZrqzrG3WbPx+FeXlIjoxEzrVrxbcueIzS1hbPrFpV5tV9AcDn9ddx68gR3D15EndOnMCBxwKza69e6LlkCXaOHl3tvlWX96RJSP72WxRkZuLk2rV6n0Vt7v7d9a23cGbTJqQePFjqZqAAYNemDfp/+mm174sFAM3790ef5ctxeOlSFObmInH9eiSuX6/XRmFqii7z5pW6HlSJ+uo3NX4MOyRrpioVxuzfjzsnTyLt999x59QpZF++jPw7d1BUUAAzS0tYurrCoX17tBg4EJ7+/jB9eOZTWZ6cOhV2TzyBc199hbuJiSjMz4elqys8nn0WHV99tVq3a6gu1169pL+kgeLbTZRcSbcuKK2t0WLgQOn0c4WJCbyGDq10uz7Ll8Otd29c+O9/kXH2LHRaLSxdXNCsZ0+0e/FFuHTpglOPfWnVBxNTU/RcsgRtxozBhf/8B7eOHEHerVsQAKxcXYtvszBoEFr6+5faVmFigl7vvQev55/Hhf/8B+kJCci/exem5uawadECHv36of3LL8OiguvMmFlaYtCXX+LcV1/hyi+/IPvqVZiYmcG2VSu0GTUKbSdOLLULrb5YubkhMDISf37xhfQ5FGk0tX5dE6USA//1L1z4z39waccOZKWkQKfVwtbTEy2HDMGTU6fW6mD5NqNHw7VXL5z76iukxscjNzUVQghYNmsGt9690X7KlAovxllf/abGTyEa8mZARETUqNw6fBh7H96EdtDGjXCrg2PEiBoaD1AmIiIiWWPYISIiIllj2CEiIiJZY9ghIiIiWWPYISIiIlnj2VhEREQkawa9zk6rVq1wpYyLr82ePRvr16+HEAJLly7F559/joyMDPTu3Rvr169Hp0euLaLRaLBw4UJs3boVeXl5GDRoED799FO0aNGiynXodDrcvHkTtra25d4DiIiIiIyLEALZ2dnw8PCAiUkFO6uEAaWnp4vU1FTpERMTIwCI/fv3CyGE+Pjjj4Wtra3Ytm2bSExMFBMnThTu7u4iKytLeo3XX39dNG/eXMTExIhjx46JgQMHiq5du4rCwsIq13Ht2jUBgA8++OCDDz74aISP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dupBx44d8eDBAzRr1gwAkJqaKq179GDlykyZMgXffvstfv/9d1y8eBGTJk2qtP2uXbuQlJSE/fv3Y8qUKdK6Hj16IDk5Ga6urmjbtq30aNOmDezt7WFvbw93d3dpJgkACgsLkZCQUOV6iYiIjBHDTi3cvXsXzz33HLZs2YJTp04hJSUF//3vf7Fy5UqMGjUKlpaW6NOnDz7++GP8+eefOHDgAN59990qv/7YsWORlZWFN954AwMHDkTz5s0rbN+/f3+4ublhypQpaNWqFfr06SOtmzJlClxcXDBq1Cj8+uuvSElJQVxcHP72t7/h+vXrAIC33noLH3/8MaKionD27FnMnj273i9aSEREVN8YdmrBxsYGvXv3xpo1a9CvXz/4+Pjgvffew8yZM7Fu3ToAxQcBa7Va9OzZE2+99RaWL19e5de3s7PDiBEjcPLkSb1ZmvIoFAq8+OKLZba3srLCgQMH0LJlS4wdOxYdOnTAa6+9hvz8fNjZ2QEAFixYgFdeeQXTpk1D3759YWtrizFjxlTjEyEiIjI+CsELqSArKwv29vbIzMyUvvhL5OfnIyUlBa1bt4aFhYWBKqwfOp0OWVlZsLOzk44vqi45fz5yo9VqsXPnTjz//PNQKpWGLofqGce76uRyNtbt6dOb3HhX9P39KM7sEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkayZGbqAxqqhLy/e5+efq73NtGnTsHnzZoSGhuJvf/ubtPz777/HmDFjUFRUVJclEhERGSXO7MichYUFVqxYgYyMDEOXQkREZBAMOzI3ePBgqNVqhIaGlttm27Zt6NSpE1QqFVq1aoVVq1Y1YIVERET1i2FH5kxNTRESEoKwsDBcv3691PoTJ05g0qRJmDRpEhITExEcHIz33nsPmzZtavhiiYiI6gHDThMwZswYdOvWDR988EGpdevXr8dzzz2H9957D+3atcO0adMwd+5c/P3vfzdApURERHWPYaeJWLFiBTZv3ow///xTb/n58+fx9NNP6y17+umnkZyczAOYiYhIFhh2moh+/fohMDAQ77zzjt5yIQQUCkWpZURERHLBU8+bkI8//hjdunVDu3btpGXt27fHwYMH9drFx8ejXbt2MDU1begSiYiI6hxn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rZX0geNdOY5341XV/ho07PTu3RtfffUV2rVrh1u3bmH58uXw8/NDUlIS0tLSAABubm5627i5ueHKlSsAgLS0NJibm8PR0bFUm5LtyxIaGoqlS5eWWh4dHQ0rK6vadqvRiYmJMXQJ1IA43lUgo7uFc7yrgOPdaOXm5lapnUHDztChQ6WfO3fujL59++KJJ57A5s2b0adPHwCAQqHQ20YIUWrZ4yprs3jxYsyfP196npWVBU9PTwQEBMDOzq4mXWmUtFotYmJi4O/vD6UM/rKhinG8q+7I+PGGLqHWdEol7r70Ese7CjjejVfJnpnKGHw31qOsra3RuXNnJCcnY/To0QCKZ2/c3d2lNunp6dJsj1qtRkFBATIyMvRmd9LT0+Hn51fu+6hUKqhUqlLLlUplk/olKdFU+91UcbwrZyKjXQEc78pxvBuvqvbV4GdjPUqj0eDMmTNwd3dH69atoVar9abkCgoKEBcXJwUZX19fKJVKvTapqak4ffp0hWGHiIiImg6DzuwsXLgQI0aMQMuWLZGeno7ly5cjKysLU6dOhUKhQFBQEEJCQuDt7Q1vb2+EhITAysoKkydPBgDY29tjxowZWLBgAZydneHk5ISFCxeic+fO0tlZRERE1LQZNOxcv34dL774Iu7cuYNmzZqhT58+OHToELy8vAAAixYtQl5eHmbPno2MjAz07t0b0dHRsLW1lV5jzZo1MDMzw4QJE5CXl4dBgwZh06ZNMDU1NVS3iIiIyIgYNOxERkZWuF6hUCA4OBjBwcHltrGwsEBYWBjCwsLquDoiIiKSA6M6ZoeIiIiorjHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawx7BAREZGsMewQERGRrDHsEBERkawZTdgJDQ2F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
"
class="
"
>
</div>

</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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okqtWLECCxcuxLRp05CRkQFvb29MnToVH3zwgdjm7bffRn5+PqZNm4bs7Gx069YNcXFxcHR0FNssW7YMVlZWGDlyJPLz8xEcHIy1a9fC0tKy1molIiKihksmCIJQ1cbbtm0DAAwdOhTr1q2DUqkU5xUXF2PXrl2Ij4/H2bNna79SE9NqtVAqldBoNBxULQF6vR4xMTHo378/xxhQrTl4/55qZFoGuRyZEybw+JaIqn5/V6uHaOjQoQBKTlU9eJUXUDJoyc/PD1988UX1qyUiIiIyoWoFogcHMyclJcHd3b1OiiIiIiKqTzUaQ5SamlrbdRARERGZTI3vQ7Rr1y7s2rULGRkZRk+fB4AffvjhsQsjIiIiqi81CkQRERH46KOP0LlzZ3h5eT3yrtBERERE5qxGgeibb77B2rVrMXbs2Nquh4iIiKje1ehO1YWFhbzLMxERET0xahSI/va3v2Hjxo21XQsRERGRSdTolFlBQQG+++477Ny5E+3atStzY6ulS5fWSnFERERE9aFGgejEiRNo3749AODUqVNG8zjAmoiIiBqaGgWi3bt313YdRERERCZTozFERERERE+SGvUQBQUFVXpq7Pfff69xQURERET1rUaBqHT8UCm9Xo+UlBScOnWqzENfiYiIiMxdjQLRsmXLyp0eHh6O3NzcxyqIiIiIqL7V6hiiMWPG8DlmRERE1ODUaiA6cOAAbGxsanOVRERERHWuRqfMhg8fbvRaEASkpaXhyJEjWLhwYa0URkRERFRfahSIlEql0WsLCwu0bNkSH330EdRqda0URkRERFRfahSI1qxZU9t1EBEREZlMjQJRqeTkZJw5cwYymQytW7dGhw4daqsuIiIionpTo0CUkZGBUaNGYc+ePXB2doYgCNBoNAgKCkJUVBQaNWpU23USERER1ZkaXWU2c+ZMaLVanD59GllZWcjOzsapU6eg1Woxa9as2q6RiIiIqE7VqIcoNjYWO3fuRKtWrcRprVu3xpdffslB1URERNTg1KiHyGAwQC6Xl5kul8thMBgeuygiIiKi+lSjQPTiiy/izTffxM2bN8VpN27cwN///ncEBwfXWnFERERE9aFGgWjlypXIycmBn58fnn76aTRv3hxNmzZFTk4OVqxYUds1EhEREdWpGo0h8vHxwdGjRxEfH4+//voLgiCgdevW6NOnT23XR0RERFTnqtVD9Pvvv6N169bQarUAgJCQEMycOROzZs1Cly5d0KZNG+zfv79OCiUiIiKqK9UKRMuXL8fkyZPh5ORUZp5SqcTUqVOxdOnSWiuOiIiIqD5UKxAdP34cffv2rXC+Wq1GcnLyYxdFREREVJ+qFYhu3bpV7uX2paysrJCZmfnYRRERERHVp2oFosaNG+PkyZMVzj9x4gS8vLweuygiIiKi+lStQNS/f3988MEHKCgoKDMvPz8fH374IQYOHFhrxRERERHVh2pddv/+++9jy5YtaNGiBWbMmIGWLVtCJpPhzJkz+PLLL1FcXIwFCxbUVa1EREREdaJagcjT0xOJiYl44403MH/+fAiCAACQyWQIDQ3FV199BU9PzzoplIiIiKiuVPvGjL6+voiJiUF2djYuXLgAQRDg7+8PFxeXuqiPiIiIqM7V6E7VAODi4oIuXbrUZi1EREREJlGjZ5kRERERPUkYiIiIiEjyGIiIiIhI8sw+EN24cQNjxoyBm5sb7Ozs0L59e6PHgwiCgPDwcHh7e8PW1ha9e/fG6dOnjdah0+kwc+ZMuLu7w97eHoMHD8b169fre1OIiIjITJl1IMrOzsZzzz0HuVyO//73v/jzzz/xxRdfwNnZWWyzZMkSLF26FCtXrkRSUhJUKhVCQkKQk5Mjtpk9ezaio6MRFRWFhIQE5ObmYuDAgSguLjbBVhEREZG5qfFVZvVh8eLF8PHxwZo1a8Rpfn5+4s+CIGD58uVYsGABhg8fDgBYt24dPD09sXHjRkydOhUajQarV6/G+vXr0adPHwDAhg0b4OPjg507dyI0NLRet4mIiIjMj1kHom3btiE0NBQjRozA3r170bhxY0ybNg2TJ08GAKSmpiI9PR1qtVpcRqFQoFevXkhMTMTUqVORnJwMvV5v1Mbb2xsBAQFITEysMBDpdDrodDrxtVarBQDo9Xro9fq62FwyI6X7mPuaapOhkodjU/0p3Q88vqWhqvvZrAPRpUuX8PXXX2POnDl47733cPjwYcyaNQsKhQKvvfYa0tPTAaDM3bE9PT1x5coVAEB6ejqsra3L3DjS09NTXL48kZGRiIiIKDM9Li4OdnZ2j7tp1EDEx8ebugR6kkyYYOoK6AE8vqUhLy+vSu3MOhAZDAZ07twZixYtAgB06NABp0+fxtdff43XXntNbCeTyYyWEwShzLSHParN/PnzMWfOHPG1VquFj48P1Go1nJycarI51IDo9XrEx8cjJCQEcv5VT7UkacQIU5dAKOkhujNmDI9viSg9w/MoZh2IvLy80Lp1a6NprVq1wubNmwEAKpUKQEkvkJeXl9gmIyND7DVSqVQoLCxEdna2US9RRkYGAgMDK3xvhUIBhUJRZrpcLucBJCHc31SbLHiKxqzw+JaGqu5js77K7LnnnsPZs2eNpp07dw6+vr4AgKZNm0KlUhl1exYWFmLv3r1i2OnUqRPkcrlRm7S0NJw6darSQERERETSYdY9RH//+98RGBiIRYsWYeTIkTh8+DC+++47fPfddwBKTpXNnj0bixYtgr+/P/z9/bFo0SLY2dkhLCwMAKBUKjFp0iTMnTsXbm5ucHV1xbx589C2bVvxqjMiIiKSNrMORF26dEF0dDTmz5+Pjz76CE2bNsXy5csxevRosc3bb7+N/Px8TJs2DdnZ2ejWrRvi4uLg6Ogotlm2bBmsrKwwcuRI5OfnIzg4GGvXroWlpaUpNouIiIjMjEwQBMHURTQEWq0WSqUSGo2Gg6olQK/XIyYmBv379+cYA6o1BwcMMHUJhJJB1ZkTJvD4loiqfn+b9RgiIiIiovrAQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLXoAJRZGQkZDIZZs+eLU4TBAHh4eHw9vaGra0tevfujdOnTxstp9PpMHPmTLi7u8Pe3h6DBw/G9evX67l6IiIiMlcNJhAlJSXhu+++Q7t27YymL1myBEuXLsXKlSuRlJQElUqFkJAQ5OTkiG1mz56N6OhoREVFISEhAbm5uRg4cCCKi4vrezOIiIjIDDWIQJSbm4vRo0dj1apVcHFxEacLgoDly5djwYIFGD58OAICArBu3Trk5eVh48aNAACNRoPVq1fjiy++QJ8+fdChQwds2LABJ0+exM6dO021SURERGRGrExdQFVMnz4dAwYMQJ8+ffDJJ5+I01NTU5Geng61Wi1OUygU6NWrFxITEzF16lQkJydDr9cbtfH29kZAQAASExMRGhpa7nvqdDrodDrxtVarBQDo9Xro9fra3kQyM6X7mPuaapNBLjd1CYT/7Qce39JQ1f1s9oEoKioKR48eRVJSUpl56enpAABPT0+j6Z6enrhy5YrYxtra2qhnqbRN6fLliYyMRERERJnpcXFxsLOzq/Z2UMMUHx9v6hLoSTJhgqkroAfw+JaGvLy8KrUz60B07do1vPnmm4iLi4ONjU2F7WQymdFrQRDKTHvYo9rMnz8fc+bMEV9rtVr4+PhArVbDycmpiltADZVer0d8fDxCQkIg51/1VEuSRowwdQmEkh6iO2PG8PiWiNIzPI9i1oEoOTkZGRkZ6NSpkzituLgY+/btw8qVK3H27FkAJb1AXl5eYpuMjAyx10ilUqGwsBDZ2dlGvUQZGRkIDAys8L0VCgUUCkWZ6XK5nAeQhHB/U22y4Ckas8LjWxqquo/NelB1cHAwTp48iZSUFPFf586dMXr0aKSkpKBZs2ZQqVRG3Z6FhYXYu3evGHY6deoEuVxu1CYtLQ2nTp2qNBARERGRdJh1D5GjoyMCAgKMptnb28PNzU2cPnv2bCxatAj+/v7w9/fHokWLYGdnh7CwMACAUqnEpEmTMHfuXLi5ucHV1RXz5s1D27Zt0adPn3rfJiIiIjI/Zh2IquLtt99Gfn4+pk2bhuzsbHTr1g1xcXFwdHQU2yxbtgxWVlYYOXIk8vPzERwcjLVr18LS0tKElRMREZG5kAmCIJi6iIZAq9VCqVRCo9FwULUE6PV6xMTEoH///hxjQLXm4IABpi6BUDKoOnPCBB7fElHV72+zHkNEREREVB8YiIiIiEjyGvwYIiIiopoYvGwwCg2Fpi5D8na9u8vUJQBgDxERERERAxERERERAxERERFJHgMRERERSR4DEREREUkerzIjqgSvQjEP5nIVChE9udhDRERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSZ9aBKDIyEl26dIGjoyM8PDwwdOhQnD171qiNIAgIDw+Ht7c3bG1t0bt3b5w+fdqojU6nw8yZM+Hu7g57e3sMHjwY169fr89NISIiIjNm1oFo7969mD59Og4ePIj4+HgUFRVBrVbj3r17YpslS5Zg6dKlWLlyJZKSkqBSqRASEoKcnByxzezZsxEdHY2oqCgkJCQgNzcXAwcORHFxsSk2i4iIiMyMlakLqExsbKzR6zVr1sDDwwPJycno2bMnBEHA8uXLsWDBAgwfPhwAsG7dOnh6emLjxo2YOnUqNBoNVq9ejfXr16NPnz4AgA0bNsDHxwc7d+5EaGhovW8XERERmRezDkQP02g0AABXV1cAQGpqKtLT06FWq8U2CoUCvXr1QmJiIqZOnYrk5GTo9XqjNt7e3ggICEBiYmKFgUin00Gn04mvtVotAECv10Ov19f6tpF5Kd3Hcgu5iSshAE/MMWeQ8/fJHJTuBx7f5qGuj++qrr/BBCJBEDBnzhw8//zzCAgIAACkp6cDADw9PY3aenp64sqVK2Iba2truLi4lGlTunx5IiMjERERUWZ6XFwc7OzsHmtbqOGY5D/J1CUQgJiYGFOXUDsmTDB1BfQAHt/moa6P77y8vCq1azCBaMaMGThx4gQSEhLKzJPJZEavBUEoM+1hj2ozf/58zJkzR3yt1Wrh4+MDtVoNJyenalZPDY1er0d8fDxWn18NveHJ6J1oyLb9fZupS6gVSSNGmLoEQkkP0Z0xY3h8m4m6Pr5Lz/A8SoMIRDNnzsS2bduwb98+PPXUU+J0lUoFoKQXyMvLS5yekZEh9hqpVCoUFhYiOzvbqJcoIyMDgYGBFb6nQqGAQqEoM10ul0PObm/J0Bv0KDQUmroMyXtSjjmLJ+TU35OCx7d5qOvju6rrN+urzARBwIwZM7Blyxb8/vvvaNq0qdH8pk2bQqVSIT4+XpxWWFiIvXv3imGnU6dOkMvlRm3S0tJw6tSpSgMRERER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7t69i4SEBCxevBhBQUHw9fVF165dMX/+fAwYMAAAoNFoMGXKFHh4eMDJyQkvvvgijh8/DgDIzMyESqXCokWLxHUeOnQI1tbWiIuLM8k2EVHdYSAioieSg4MDHBwcsHXrVuh0ujLzBUHAgAEDkJ6ejpiYGCQnJ6Njx44IDg5GVlYWGjVqhB9++AHh4eE4cuQIcnNzMWbMGEybNg1qtdoEW0REdYkPdyWiJ9bmzZsxefJk5Ofno2PHjujVqxdGjRqFdu3a4ffff8ewYcOQkZEBhUIhLtO8eXO8/fbbmDJlCgBg+vTp2LlzJ7p06YLjx48jKSkJNjY2ptokIqojDERE9EQrKCjA/v37ceDAAcTGxuLw4cP4/vvvkZmZiXfffRe2trZG7fPz8zFv3jwsXrxYfB0QEIBr167hyJEjaNeunSk2g4jqGAMREUnK3/72N8THx2PatGlYsWIF9uzZU6aNs7Mz3N3dAQCnT59G586dodfrER0djUGDBtVzxURUH3iVGRFJSuvWrbF161Z07NgR6enpsLKygp+fX7ltCwsLMXr0aLzyyit45plnMGnSJJw8eRKenp71WzQR1Tn2EBHRE+nOnTsYMWIEJk6ciHbt2sHR0RFHjhzBzJkzMWDAAHz//ffo2bMncnJysHjxYrRs2RI3b95ETEwMhg4dis6dO+Ott97Cv//9bxw/fhwODg4ICgqCo6Mjtm/fburNI6JaxkBERE8knU6H8PBwxMXF4eLFi9Dr9fDx8cGIESPw3nvvwdbWFjk5OViwYAE2b94sXmbfs2dPREZG4uLFiwgJCcHu3bvx/PPPAwCuXr2Kdu3aITIyEm+88YaJt5CIahMDEREREUke70NEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESS9/+IhoREga+QOwAAAABJRU5ErkJggg==
"
class="
"
>
</div>

</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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fn6qUqUKfzCBGwzHtzlda7gLV5kBAADTIxABAADTIxABAADTYwwRAAAVhN1uV3Z2trvLKFesVqs8PT2vezsEIgAAKoDs7GwlJibKbre7u5Ryp2rVqgoJCbmu+wQSiAAAKOccDoeSkpLk6empsLCwq95g0EwcDocyMjKUkpIiSapVq1aJt0UgAgCgnMvJyVFGRoZCQ0Pl5+fn7nLKFV9fX0lSSkqKatasWeLTZ0RMAADKudzcXEmSt7e3myspn/JCos1mK/E2CEQAAFQQPEuzYKXxcyEQAQAA0yMQAQCAElm/fr0sFovOnDnj0tcZOXKk+vbt69LXIBABAFDBpaSkaMyYMapTp458fHwUEhKiqKgobd682aWv27FjRyUlJSkwMNClr1MWuMoMAIAKbsCAAbLZbFq8eLEaNGig48ePa926dTp9+nSJtudwOJSbmysvr6vHBG9vb4WEhJToNcobeogAAKjAzpw5o02bNunVV19Vly5dVLduXd15552aMmWKevXqpUOHDslisSghIcFpHYvFovXr10v636mv1atXq02bNvLx8VFMTIwsFot++eUXp9ebPXu26tWrJ4fD4XTKLC0tTb6+vlq1apVT+08//VT+/v46d+6cJOnYsWMaNGiQqlWrpqCgID3wwAM6dOiQ0T43N1cTJ05U1apVFRQUpGeeeUYOh8MlP7vLEYgAAKjAKleurMqVK+uzzz5TVlbWdW3rmWee0YwZM7R37149+OCDat26tT744AOnNkuXLtWQIUPyXdkVGBioXr16Fdj+gQceUOXKlZWRkaEuXbqocuXK2rhxozZt2qTKlSure/fuxiNJZs2apXfffVcxMTHatGmTTp8+reXLl1/XfhUFp8zKmS29erm7BEiyW63SqFHuLgMArsnLy0uLFi3SY489pjfeeEOtWrVSp06dNHjwYLVo0aJY23rppZcUERFhTA8dOlTz58/XP/7xD0nS/v37FR8fr/fee6/A9YcOHapHHnlEGRkZ8vPzU3p6ur766it98sknkqRly5bJw8ND77zzjhGoFi5cqKpVq2r9+vWKjIzU3LlzNWXKFA0YMECS9MYbb2j16tXF/rkUFz1EAABUcAMGDNAff/yhFStWKCoqSuvXr1erVq20aNGiYm2nTZs2TtODBw/W4cOHtWXLFknSBx98oJYtW6pJkyYFrt+rVy95eXlpxYoVkqRPPvlEAQEBioyMlCTFx8fr4MGDCggIMHq2qlevrgsXLujXX39VWlqakpKS1KFDB2ObXl5e+epyBQIRAAA3gEqVKikiIkIvvPCC4uLiNHLkSL344ovGc88uH4dT2B2d/f39naZr1aqlLl26aOnSpZKkDz/8UMOGDSu0Bm9vbz344ING+6VLl2rQoEHG4Gy73a7WrVsrISHB6Wv//v0aMmRIyXe+FBCIAAC4ATVp0kTnz5/XTTfdJElKSkoyll0+wPpahg4dqo8++kibN2/Wr7/+qsGDB1+z/apVq7Rnzx59++23Gjp0qLGsVatWOnDggGrWrKlbb73V6SswMFCBgYGqVauW0SMlXXyOW3x8fJHrLSkCEQAAFdipU6d03333acmSJdq5c6cSExP1n//8RzNnztQDDzwgX19ftW/fXv/85z/1888/a+PGjfr73/9e5O33799f6enpeuKJJ9SlSxfVrl37qu07deqk4OBgDR06VPXq1VP79u2NZUOHDlWNGjX0wAMP6LvvvlNiYqI2bNigp59+WkePHpUkPf300/rnP/+p5cuX65dfftHYsWNdfuNHyc2BaOPGjbr//vsVGhoqi8Wizz77rNC2Y8aMkcVi0dy5c53mZ2Vlady4capRo4b8/f3Vp08f44eaJzU1VcOHDzfS5/Dhw8vkhwsAgKtVrlxZ7dq105w5c3TvvfeqWbNmev755/XYY49p/vz5kqR3331XNptNbdq00dNPP62XX365yNuvUqWK7r//fv30009OvT2FsVgsevjhhwts7+fnp40bN6pOnTrq37+/GjdurEcffVSZmZmqUqWKJGnSpEl65JFHNHLkSHXo0EEBAQHq169fMX4iJWNxlMXF/YX4+uuv9f3336tVq1YaMGCAli9fXuCtuT/77DNFR0frxIkT+utf/6oJEyYYy5544gl98cUXWrRokYKCgjRp0iSdPn1a8fHx8vT0lCT16NFDR48e1VtvvSVJevzxx1WvXj198cUXRa41PT1dgYGBSktLM940V+Aqs/LBbrXqxKhR6tmzp6xWq7vLAVCKbDabVq5cWaGO7wsXLigxMVH169dXpUqV3F1OuXO1n09RP7/detl9jx491KNHj6u2OXbsmJ566imtXr1ava4IC2lpaYqJidH777+vbt26SZKWLFmisLAwrV27VlFRUdq7d69WrVqlLVu2qF27dpKkt99+Wx06dNC+ffvUqFEj1+wcAACoMMr1fYjsdruGDx+uv/71r2ratGm+5fHx8bLZbMblfJIUGhqqZs2aKS4uzniOS2BgoBGGJKl9+/YKDAxUXFxcoYEoKyvL6QZX6enpki7+Z1HY6PzSYK8g/63c6PLeB1e+1wDcI++4rkjHt81mk8PhkN1ul91ud3c55Y7dbpfD4ZDNZjPODuUp6vtcrgPRq6++Ki8vL40fP77A5cnJyfL29la1atWc5gcHBys5OdloU7NmzXzr1qxZ02hTkBkzZmjatGn55q9Zs0Z+fn7F2Y3i4WaA5UpsbKy7SwDgIhXp+Pby8lJISIjOnTtn3NEZ/5Odna3MzExt3LhROTk5TssyMjKKtI1yG4ji4+P173//Wzt27Mh3e/BrcTgcTusUtP6Vba40ZcoUTZw40ZhOT09XWFiYIiMjXTqGaNtDD7ls2yg6u9WqU8OGKSIiosKMMQBQNDabTbGxsRXq+L5w4YJ+//13Va5cmTFEBbhw4YJ8fX117733FjiGqCjKbSD67rvvlJKSojp16hjzcnNzNWnSJM2dO1eHDh1SSEiIsrOzlZqa6tRLlJKSoo4dO0qSQkJCdPz48XzbP3HihIKDgwt9fR8fH/n4+OSbb7VaXXoAeVSgLlwzcPX7DcB9KtLxnZubK4vFIg8PD+NGi/gfDw8PWSyWAt/Tor7H5fanOnz4cO3cudPpTpahoaH661//ajzTpHXr1rJarU7dnklJSdq9e7cRiDp06KC0tDRt3brVaPPDDz8oLS3NaAMAAMzNrT1E586d08GDB43pxMREJSQkqHr16qpTp46CgoKc2lutVoWEhBgDoQMDAzV69GhNmjRJQUFBql69uiZPnqzmzZsbV501btxY3bt312OPPaY333xT0sXL7nv37s0VZgAAQJKbA9H27dvVpUsXYzpvzM6IESOK/EC6OXPmyMvLSwMHDlRmZqa6du2qRYsWOY0y/+CDDzR+/HjjarQ+ffoYN6sCAABwayDq3LmzinNfyEOHDuWbV6lSJc2bN0/z5s0rdL3q1atryZIlJSkRAACYQLkdQwQAAFBWyu1VZgAA4OrK+nFP7b/6qtjrjBw5UosXL9aMGTP03HPPGfM/++wz9evXr1hnilyJHiIAAOBSlSpV0quvvqrU1FR3l1IoAhEAAHCpbt26KSQkRDNmzCi0zSeffKKmTZvKx8dH9erV06xZs8qwQgIRAABwMU9PT02fPl3z5s3T0aNH8y2Pj4/XwIEDNXjwYO3atUvR0dF6/vnni3zFeWkgEAEAAJfr16+fWrZsqRdffDHfstmzZ6tr1656/vnn1bBhQ40cOVJPPfWU/vWvf5VZfQQiAABQJl599VUtXrxYP//8s9P8vXv36q677nKad9ddd+nAgQPKzc0tk9oIRAAAoEzce++9ioqK0t/+9jen+QU9cL2srz7jsnsAAFBm/vnPf6ply5Zq2LChMa9JkybatGmTU7u4uDg1bNjQ6ckTrkQgAgAAZaZ58+YaOnSo0xMmJk2apLZt2+of//iHBg0apM2bN2v+/PlasGBBmdXFKTMAAFCm/vGPfzidEmvVqpU+/vhjLVu2TM2aNdMLL7ygl156SSNHjiyzmughAgCggirJnaPLWkGXztetW1cXLlxwmjdgwAANGDCgjKrKjx4iAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgejy6AwCACqrrP7uW6eute25dkds6HA5FRETI09NTq1evdlq2YMECTZkyRbt27VKdOnVKu8wSoYcIAACUOovFooULF+qHH37Qm2++acxPTEzUs88+q3//+9/lJgxJBCIAAOAiYWFh+ve//63JkycrMTFRDodDo0ePVteuXXXnnXeqZ8+eqly5soKDgzV8+HCdPHnSWPe///2vmjdvLl9fXwUFBalbt246f/68y2olEAEAAJcZMWKEunbtqlGjRmn+/PnavXu3/v3vf6tTp05q2bKltm/frlWrVun48eMaOHCgJCkpKUkPP/ywHn30Ue3du1fr169X//795XA4XFYnY4gAAIBLvfXWW2rWrJm+++47/fe//1VMTIxatWql6dOnG23effddhYWFaf/+/Tp37pxycnLUv39/1a1bV5LUvHlzl9ZIDxEAAHCpmjVr6vHHH1fjxo3Vr18/xcfH69tvv1XlypWNr9tuu02S9Ouvv+r2229X165d1bx5cz300EN6++23lZqa6tIaCUQAAMDlvLy85OV18cSU3W7X/fffr4SEBKevAwcO6N5775Wnp6diY2P19ddfq0mTJpo3b54aNWqkxMREl9VHIAIAAGWqVatW2rNnj+rVq6dbb73V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AACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYXokCUYMGDXTq1Kl888+cOaMGDRpcd1EAAABlqUSB6NChQ8rNzc03PysrS8eOHbvuogAAAMqSV3Ear1ixwvh+9erVCgwMNKZzc3O1bt061atXr9SKAwAAKAvF6iHq27ev+vbtK4vFohEjRhjTffv21eDBgxUbG6tZs2aVWnE5OTn6+9//rvr168vX11cNGjTQSy+9JLvdbrRxOByKjo5WaGiofH191blzZ+3Zs8dpO1lZWRo3bpxq1Kghf39/9enTR0ePHi21OgEAQMVWrEBkt9tlt9tVp04dpaSkGNN2u11ZWVnat2+fevfuXWrFvfrqq3rjjTc0f/587d27VzNnztS//vUvzZs3z2gzc+ZMzZ49W/Pnz9e2bdsUEhKiiIgInT171mgzYcIELV++XMuWLdOmTZt07tw59e7du8DTfgAAwHyKdcosT2JiYmnXUaDNmzfrgQceUK9evSRJ9erV04cffqjt27dLutg7NHfuXE2dOlX9+/eXJC1evFjBwcFaunSpxowZo7S0NMXExOj9999Xt27dJElLlixRWFiY1q5dq6ioqDLZFwAAUH6VKBBJ0rp167Ru3Tqjp+hy77777nUXJkl333233njjDe3fv18NGzbUTz/9pE2bNmnu3LmSLgaz5ORkRUZGGuv4+PioU6dOiouL05gxYxQfHy+bzebUJjQ0VM2aNVNcXFyhgSgrK0tZWVnGdHp6uiTJZrPJZrOVyv4VxG61umzbKLq898GV7zUA98g7rjm+zaGo73OJAtG0adP00ksvqU2bNqpVq5YsFktJNnNNzz77rNLS0nTbbbfJ09NTubm5euWVV/Twww9LkpKTkyVJwcHBTusFBwfr8OHDRhtvb29Vq1YtX5u89QsyY8YMTZs2Ld/8NWvWyM/P77r266pGjXLdtlFssbGx7i4BgItwfJtDRkZGkdqVKBC98cYbWrRokYYPH16S1Yvso48+0pIlS7R06VI1bdpUCQkJmjBhgkJDQzVixAij3ZWBzOFw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8piD+eT9PvF7ZQ5FfZ/LdSD67bff9Prrr2vixIn629/+pq1bt2r8+PHy8fHRI488ouTkZElScHCw03rBwcE6fPiwJCk5OVne3t6qVq1avjZ56xdkxowZmjZtWr75a9askZ+f3/XuWuFGjXLdtlFso8NHu7sESFq5cqW7S8ANKDY21t0loAxkZGQUqV25DkR2u11t2rTR9OnTJUl33HGH9uzZo9dff12PPPKI0c5isTit53A48s270rXaTJkyRRMnTjSm09PTFRYWpsjISFWpUqUku1Mk2x56yGXbRtHZrVadGjZMMQdiZLPzX6S7rfjLCneXgBuIzWZTbGysIiIiZKVX/oaXd4bnWsp1IKpVq5aaNGniNK9x48b65JNPJEkhISGSLvYC1apVy2iTkpJi9BqFhIQoOztbqampTr1EKSkp6tixY6Gv7ePjIx8fn3zzrVarSw8gD7pwyxWb3aZse7a7yzA9PrTgCq7+e47yoajvcbm+yuyuu+7Svn37nObt379fdevWlSTVr19fISEhTt2e2dnZ2rBhgxF2WrduLavV6tQmKSlJu3fvvmogAgAA5lGue4j+8pe/qGPHjpo+fboGDhyorVu36q233tJbb70l6eKpsgkTJmj69OkKDw9XeHi4pk+fLj8/Pw0ZMkSSFBgYqNGjR2vSpEkKCgpS9erVNXnyZDVv3ty46gwAAJhbuQ5Ebdu21fLlyzVlyhS99NJLql+/vubOnauhQ4cabZ555hllZmZq7NixSk1NVbt27bRmzRoFBAQYbebMmSMvLy8NHDhQmZmZ6tq1qxYtWiRPT0937BYAAChnLA6Hw+HuIiqC9PR0BQYGKi0tzaWDqrf06uWybaPo7FarTowapTf2vcEYonJg3XPr3F0CbiA2m00rV65Uz549GUNkAkX9/C7XY4gAAADKAoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACYHoEIAACY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PuX19BZl7E+LFi2M7/39/RUQEKCUlJQivQ5/J8zFVM8yg3nExMQoJydHtWvXNuY5HA5ZrValpqYWaRseHh668sk2NputVOsEUHzh4eGyWCzau3ev+vbte9W2VqvVadpischut7uwOlRU9BDhhpOTk6P33ntPs2bNUkJCgvH1008/qW7duvrggw/UuHFjp4GXkvJN33TTTUpKSjKmc3NztXv37qu+ttVqVW5ubuntDIB8qlevrqioKL322ms6f/58vuVXGyRdHEX5O4EbBz1EuOF8+eWXSk1N1ejRo/PdX+TBBx9UTEyMnnvuOY0YMUJt2rTR3XffrQ8++EB79uxRgwYNjLb33XefJk6cqK+++kq33HKL5syZc80/tPXq1dO6det01113ycfHR9WqVXPFLgKmt2DBAnXs2FF33nmnXnrpJbVo0UI5OTmKjY3V66+/rr179173azz99NPX/DuBGwc9RLjhxMTEqFu3bgXebG3AgAFKSEhQeHi4XnjhBT377LNq3bq1Dh8+rCeeeMKp7aOPPqoRI0bokUceUadOnVS/fn116dLlqq89a9YsxcbGKiwsTHfccUep7heA/6lfv7527NihLl26aNKkSWrWrJkiIiK0bt06vf7666XyGoMGDbrm3wncOCyOKwdJAAAAmAw9RAAAwPQIRAAAwPQIRAAAwPQIRAAAwPQIRAAAwPQIRAAAwPQIRAAAwPQIRAAAwPQIRABuWHFxcfL09FT37t3dXQqAco47VQO4Yf3pT39S5cqV9c477+jnn39WnTp13F0SgHKKHiIAN6Tz58/r448/1hNPPKHevXtr0aJFTstXrFih8PBw+fr6qkuXLlq8eLEsFovTA3zj4uJ07733ytfXV2FhYRo/fnyBT1cHUPERiADckD766CM1atRIjRo10rBhw7Rw4ULldYgfOnRIDz74oPr27auEhASNGTNGU6dOdVp/165dioqKUv/+/bVz50599NFH2rRpk5566il37A4AF+OUGYAb0l133aWBAwfq6aefVk5OjmrVqqUPP/xQ3bp103PPPaevvvpKu3btMtr//e9/1yuvvKLU1FRVrVpVjzzyiHx9ffXmm28abTZt2qROnTrp/PnzqlSpkjt2C4CL0EME4Iazb98+bd26VYMHD5YkeXl5adCgQXr33XeN5W3btnVa584773Sajo+P16JFi1S5cmXjKyoqSna7XYmJiWWzIwDKjJe7CwCA0hYTE6OcnBzVrl3bmOdwOGS1WpWamiqHwyGLxeK0zpWd5Xa7XWPGjNH48ePzbZ/B2cCNh0AE4IaSk5Oj9957T7NmzVJkZKTTsgEDBuiDDz7QbbfdppUrVzot2759u9N0q1attGfPHt16660urxmA+zGGCMAN5bPPPtOgQYOUkpKiwMBAp2VTp07VypUr9emnn6pRo0b6y1/+otGjRyshIUGTJk3S0aNHdebMGQUGBmrnzp1q3769Ro0apccee0z+/v7au3evYmNjNW/ePDftHQBXYQwRgBtKTEyMunXrli8MSRd7iBISEpSamqr//ve/+vTTT9WiRQu9/vrrxlVmPj4+kqQWLVpow4YNOnDggO655x7dcccdev7551WrVq0y3R8AZYMeIgCQ9Morr+iNN97Q77//7u5SALgBY4gAmNKCBQvUtm1bBQUF6fvvv9e//vUv7jEEmBiBCIApHThwQC+//LJOnz6tOnXqaNKkSZoyZYq7ywLgJpwyAwAApsegagAAYHoEIgAAYHoEIgAAYHoEIgAAYHoEIgAAYHoEIgAAYHoEIgAAYHoEIgAAYHoEIgAAYHr/H5chguPJFEN1AAAAAElFTkSuQmCC
"
class="
"
>
</div>

</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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aaPzY21tDeDeWWp9+vTBkiVLcPToUZibmyMuLk5vNTEQERERUaOaPn068vLyMHbsWBw8eBAXL15EQkICJk2ahPLychw4cADh4eE4fPgwrly5gi1btiAnJwfe3t56q4mTqomIiKhRubu74++//8aCBQsQFBQElUqFli1bYuDAgTAxMYGdnR2SkpKwZs0aKJVKtGzZEitXrsSgQYP0VhMDERERURNVlytHG1pYWBjCwsKEx15eXtiyZUuVbb29vREfH99Ild3DQ2ZEREQkegxEREREJHoMRERERCR6DEREREQkegxERERETYQ+7+XVlDXE68JAREREZORMTU0BAKWlpQauxDgVFRUBAKRSab374Gn3RERERs7MzAxWVlbIycmBVCqFiQnHM4B7I0NFRUXIzs6Gvb29EBzrg4GIiIjIyEkkEri5uSEjIwOXL182dDlGx97eHgqF4rH6YCAiIiJqAszNzeHl5cXDZg+RSqWPNTJUgYGIiIioiTAxMYGFhYWhy3gi8SAkERERiR4DEREREYkeAxERERGJHgMRERERiR4DEREREYmeQQNRUlISXnrpJbi7u0MikWDr1q3COrVajQULFsDX1xfW1tZwd3fHa6+9hhs3buj0oVKpMGPGDDg7O8Pa2hrDhg3DtWvXdNrk5+dj/PjxkMvlkMvlGD9+PG7fvt0Ie0hERERNgUED0d27d/Hss88iKiqq0rqioiIcOXIE77//Po4cOYItW7bg7NmzGDZsmE672bNnIy4uDrGxsUhOTkZhYSGGDh2K8vJyoU1ISAjS0tIQHx+P+Ph4pKWlYfz48XrfPyIiImoaDHodokGDBmHQoEFVrpPL5UhMTNRZFhkZie7du+PKlSto0aIFCgoKEB0dje+++w4DBgwAAGzcuBEeHh7YuXMngoKCcOrUKcTHx2P//v3o0aMHAOCrr75Cr169cObMGbRr106/O0lERERGr0ldmLGgoAASiQT29vYAgNTUVKjVagQGBgpt3N3d4ePjg5SUFAQFBWHfvn2Qy+VCGAKAnj17Qi6XIyUlpdpApFKpoFKphMdKpRLAvUN5arVaD3t3j+YxbkxHDafifdDne01EhlHxuebnWxxq+z43mUBUUlKCd955ByEhIbCzswMAZGVlwdzcHA4ODjptXV1dkZWVJbRxcXGp1J+Li4vQpioRERFYsmRJpeUJCQmw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NRERETU59Roh0mg0VV67QSqVCrfVICIiImoq6hWIXnzxRcyaNUvnAozXr1/Hv//9b/j7+zdYcURERESNoV6BKCoqCnfu3IGnpyeefvpptGnTBq1atcKdO3cQGRnZ0DUSERER6VW95hB5eHjgyJEjSExMxOnTp6HVatG+fXveLJWIiIiapDqNEP35559o3749lEolACAgIAAzZszAzJkz0a1bN3To0AF//fWXXgolIiIi0pc6BaI1a9ZgypQpVd4cTS6XY+rUqVi1alWDFUdERETUGOoUiI4dO4aBAwdWuz4wMBCpqamPXRQRERFRY6pTILp582aVp9tXMDMzQ05OzmMXRURERNSY6jSpulmzZjhx4gTatGlT5frjx4/Dzc2tQQojIiLSp2Grh6FUU2roMkRv1zu7DF0CgDqOEA0ePBiLFy9GSUlJpXXFxcX44IMPMHTo0AYrjoiIiKgx1GmE6L333sOWLVvQtm1bvP3222jXrh0kEglOnTqFTz/9FOXl5Vi0aJG+aiUiIiLSizoFIldXV6SkpOCtt97CwoULodVqAQASiQRBQUFYu3YtXF1d9VIoERERkb7U+cKMLVu2xPbt25Gfn4/z589Dq9XCy8sLDg4O+qiPiIiISO/qdaVqAHBwcEC3bt0ashYiIiIig6jXvcyIiIiIniQMRERERCR6DEREREQkegxEREREJHoMRERERCR6DEREREQkegxEREREJHoMRERERCR6DEREREQkegxEREREJHoMRERERCR6DEREREQkegxEREREJHoMRERERCR6DEREREQkegxEREREJHpGH4iuX7+OcePGwcnJCVZWVujUqRNSU1OF9VqtFmFhYXB3d4elpSX69++PkydP6vShUqkwY8YMODs7w9raGsOGDcO1a9cae1eIiIjISBl1IMrPz0efPn0glUrxxx9/4J9//sHKlSthb28vtFmxYgVWrVqFqKgoHDp0CAqFAgEBAbhz547QZvbs2YiLi0NsbCySk5NRWFiIoUOHory83AB7RURERMbGzNAF1GT58uXw8PDAunXrhGWenp7C71qtFmvWrMGiRYswcuRIAMCGDRvg6uqKmJgYTJ06FQUFBYiOjsZ3332HAQMGAAA2btwIDw8P7Ny5E0FBQY26T0RERGR8jDoQbdu2DUFBQRg1ahT27t2LZs2aYdq0aZgyZQoAICMj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Hh4IDAyEnZ1dHauvvUOjRumtb6o9jVSK3HHjEBAQAClH7YieKGq1GomJiYg+Fw21hqPAhrbt39v02n/FEZ5HaRKBaMaMGdi2bRuSkpLQvHlzYblCoQBwbxTIzc1NWJ6dnS2MGikUCpSWliI/P19nlCg7Oxu9e/eu9jllMhlkMlml5VKpVK9fkCY8RGNU9P1+E5HhqDVqlGpKDV2G6On7b2xt+zfqs8y0Wi3efvttbNmyBX/++SdatWqls75Vq1ZQKBQ6hzVKS0uxd+9eIex06dIFUqlUp01mZibS09NrDEREREQkHkY9QjR9+nTExMTgl19+ga2trTDnRy6Xw9LSEhKJBLNnz0Z4eDi8vLzg5eWF8PBwWFlZISQkRGg7efJkzJ07F05OTnB0dMS8efPg6+srnHVGRERE4mbUgeizzz4DAPTv319n+bp16zBhwgQAwPz581FcXIxp06YhPz8fPXr0QEJCAmxtbYX2q1evhpmZGYKDg1FcXAx/f3+sX78epqamjbUrREREZMSMOhBptdpHtpFIJAgLC0NYWFi1bSwsLBAZGYnIyMgGrI6IiIieFEY9h4iIiIioMTAQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoMRARERGR6DEQERERkegxEBEREZHoiSoQrV27Fq1atYKFhQW6dOmCv/76y9AlERERkREQTSD64YcfMHv2bCxatAhHjx7FCy+8gEGDBuHKlSuGLo2IiIgMTDSBaNWqVZg8eTL+9a9/wdvbG2vWrIGHhwc+++wzQ5dGREREBiaKQFRaWorU1FQEBgbqLA8MDERKSoqBqiIiIiJjYWboAhrDrVu3UF5eDldXV53lrq6uyMrKqnIblUoFlUolPC4oKAAA5OXlQa1W663WO3rrmepCA6CoqAi5ubmQSqWGLoeIGpBarUZRURFQCpho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M6xfvx4bNmyAvb09+vTpg3fffRfHjx+vc19Lly5FQEAAnn76aTg5OSE0NBQxMTHC+rNnzyI1NRXjxo2rcvvQ0FBs3boVRUVFAAClUonff/9daB8bGwsTExN8/fXX8PX1hbe3N9atW4crV64Io1Vr1qzBwoUL8corr8Db2xuff/455ygRGSEGIiIyOq+88gpu3LiBbdu2ISgoCHv27EHnzp2xfv36OvXTtWtXncdjxozB5cuXsX//fgDApk2b0KlTJ7Rv377K7YcMGQIzMzNs27YNALB582bY2toiMDAQAJCamorz58/D1tZWGNlydHRESUkJLly4gIKCAmRmZqJXr15Cn2ZmZpXqIiLDYyAiIqNkYWGBgIAALF68GCkpKZgwYQI++OADmJjc+7P14DwctVpdZR/W1tY6j93c3ODn5yeMEn3//ffVjg4B9yZZv/rqq0L7mJgYjB49WpicrdFo0KVLF6Slpen8nD17FiEhIfXfeSJqdAxERNQktG/fHnfv3sVTTz0FAMjMzBTWPTjB+lFCQ0Pxww8/YN++fbhw4QLGjBnzyPbx8fE4efIkdu/ejdDQUGFd586dce7cObi4uKBNmzY6P3K5HHK5HG5ubsKIFACUlZUhNTW11vUSUeNgICIio5Kbm4sXX3wRGzduxPHjx5GRkYGffvoJK1aswPDhw2FpaYmePXvio48+wj///IOkpCS89957te5/5MiRUCqVeOutt+Dn54dmzZrV2L5fv35wdXVFaGgoPD090bNnT2FdaGgonJ2dMXz4cPz111/IyMjA3r17MWvWLFy7dg0AMGvWLHz00UeIi4vD6dOnMW3aNL1f+JGI6o6BiIiMio2NDXr06IHVq1ejb9++8PHxwfvvv48pU6YgKioKAPDNN99ArVaja9eumDVrFpYtW1br/u3s7PDSSy/h2LFjOqM91ZFIJBg7dmyV7a2srJCUlIQWLVpg5MiR8Pb2xqRJk1BcXAw7OzsAwNy5c/Haa69hwoQJ6NWrF2xtbTFixIg6vCJE1BgkWl4Qg4iIiESOI0REREQkegxEREREJHoMRERERCR6DEREREQkegxEREREJHoMRERERCR6DEREREQkegxEREREJHoMRERERCR6DEREREQkegxEREREJHoMRERERCR6/x94TBYHG+uR+wAAAABJRU5ErkJggg==
"
class="
"
>
</div>

</div>

</div>

</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h2 id="Pre-processing-dataset"><li>Pre-processing dataset</li><a class="anchor-link" href="#Pre-processing-dataset">&#182;</a></h2><li>we will be working with TransactionEncoder() to encode our data</li> 
<li>TransactionEncoder() works with lists not dataframe so a <b>casting from Dataframe to lists is must</b></li><h3 id="preprocessing-exemples:"><li>preprocessing exemples:</li><a class="anchor-link" href="#preprocessing-exemples:">&#182;</a></h3><li>exemple in: <a href="url">https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/apriori/</a></li>
<li>exemple in: <a href="url">https://stackabuse.com/association-rule-mining-via-apriori-algorithm-in-python/</a></li>
</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[22]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%%time</span>
<span class="c1"># %%time : for showing execution time</span>

<span class="n">dataset</span> <span class="o">=</span> <span class="n">td</span><span class="o">.</span><span class="n">values</span><span class="o">.</span><span class="n">tolist</span><span class="p">()</span>
<span class="sd">'''</span>
<span class="sd">or you can use this code to replace the above implementation:</span>

<span class="sd">dataset = []</span>
<span class="sd">for i in range(1, td.shape[0]):</span>
<span class="sd">    dataset.append([str(td.values[i,j]) for j in range(0, td.shape[1])])</span>
<span class="sd">    </span>
<span class="sd">#td.shape[0]                             -&gt; returns the number of rows</span>
<span class="sd">#td.shape[1]                             -&gt; returns the number of columns</span>
<span class="sd">#dataset.append                          -&gt;(adds transaction of type  list to the dataset list)</span>
<span class="sd">#str()                                   -&gt; casting to string</span>
<span class="sd">#td.values[i,j] for j in range(0, td.shape[1])               -&gt; extracting values from dataframe from first to last column</span>

<span class="sd">#warning execution time for thins code is not optimal</span>

<span class="sd">'''</span>

<span class="n">oht</span> <span class="o">=</span> <span class="n">TransactionEncoder</span><span class="p">()</span>
<span class="n">oht_ary</span> <span class="o">=</span> <span class="n">oht</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">dataset</span><span class="p">)</span><span class="o">.</span><span class="n">transform</span><span class="p">(</span><span class="n">dataset</span><span class="p">)</span>

<span class="n">oht_ary</span>
<span class="n">df</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">oht_ary</span><span class="p">,</span> <span class="n">columns</span><span class="o">=</span><span class="n">oht</span><span class="o">.</span><span class="n">columns_</span><span class="p">)</span>
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
 
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="text/plain">
<pre>CPU times: total: 15.6 ms
Wall time: 5 ms
</pre>
</div>
</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[22]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>Adult</th>
      <th>Child</th>
      <th>Crew</th>
      <th>Female</th>
      <th>Male</th>
      <th>No</th>
      <th>Yes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[23]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">frequent_itemsets</span> <span class="o">=</span> <span class="n">apriori</span><span class="p">(</span><span class="n">df</span><span class="p">,</span> <span class="n">min_support</span><span class="o">=</span><span class="mf">0.005</span><span class="p">,</span> <span class="n">use_colnames</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="n">frequent_itemsets</span><span class="p">[</span><span class="s1">'length'</span><span class="p">]</span> <span class="o">=</span> <span class="n">frequent_itemsets</span><span class="p">[</span><span class="s1">'itemsets'</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="nb">len</span><span class="p">(</span><span class="n">x</span><span class="p">))</span>
<span class="n">frequent_itemsets</span><span class="o">.</span><span class="n">sort_values</span><span class="p">(</span><span class="n">by</span><span class="o">=</span><span class="s1">'support'</span><span class="p">,</span><span class="n">ascending</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[23]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>support</th>
      <th>itemsets</th>
      <th>length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>0.950477</td>
      <td>(Adult)</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.786461</td>
      <td>(Male)</td>
      <td>1</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.757383</td>
      <td>(Adult, Male)</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.676965</td>
      <td>(No)</td>
      <td>1</td>
    </tr>
    <tr>
      <th>30</th>
      <td>0.653339</td>
      <td>(Adult, No)</td>
      <td>2</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>0.005906</td>
      <td>(Child, Yes, 2nd, Female)</td>
      <td>4</td>
    </tr>
    <tr>
      <th>55</th>
      <td>0.005906</td>
      <td>(Child, 2nd, Female)</td>
      <td>3</td>
    </tr>
    <tr>
      <th>57</th>
      <td>0.005906</td>
      <td>(Female, 2nd, No)</td>
      <td>3</td>
    </tr>
    <tr>
      <th>103</th>
      <td>0.005906</td>
      <td>(Male, Yes, Child, 3rd)</td>
      <td>4</td>
    </tr>
    <tr>
      <th>91</th>
      <td>0.005906</td>
      <td>(Adult, 2nd, No, Female)</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>107 rows × 3 columns</p>
</div>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[82]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1">#association_rules(frequent_itemsets,metric="confidence", min_threshold=0.7)</span>
<span class="n">a</span><span class="o">=</span><span class="n">association_rules</span><span class="p">(</span><span class="n">frequent_itemsets</span><span class="p">,</span><span class="n">metric</span><span class="o">=</span><span class="s2">"confidence"</span><span class="p">,</span> <span class="n">min_threshold</span><span class="o">=</span><span class="mf">0.7</span><span class="p">)</span>

<span class="n">a</span><span class="p">[</span><span class="s2">"antecedent_len"</span><span class="p">]</span> <span class="o">=</span> <span class="n">a</span><span class="p">[</span><span class="s2">"antecedents"</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="nb">len</span><span class="p">(</span><span class="n">x</span><span class="p">))</span>
<span class="n">a</span><span class="p">[</span><span class="s2">"survived"</span><span class="p">]</span> <span class="o">=</span> <span class="n">a</span><span class="p">[</span><span class="s2">"consequents"</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="nb">str</span><span class="p">(</span><span class="n">x</span><span class="p">)</span><span class="o">.</span><span class="n">strip</span><span class="p">(</span><span class="s2">"frozenset({</span><span class="se">\'</span><span class="s2">})"</span><span class="p">))</span>
<span class="n">a</span><span class="p">[</span><span class="s2">"anteced"</span><span class="p">]</span> <span class="o">=</span> <span class="n">a</span><span class="p">[</span><span class="s2">"antecedents"</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="nb">str</span><span class="p">(</span><span class="n">x</span><span class="p">)</span><span class="o">.</span><span class="n">strip</span><span class="p">(</span><span class="s2">"frozenset(</span><span class="si">{}</span><span class="s2">)"</span><span class="p">))</span>

<span class="n">b</span><span class="o">=</span><span class="n">a</span><span class="p">[((</span><span class="n">a</span><span class="p">[</span><span class="s2">"consequents"</span><span class="p">]</span><span class="o">==</span><span class="p">{</span><span class="s1">'No'</span><span class="p">})</span> <span class="o">|</span><span class="p">(</span><span class="n">a</span><span class="p">[</span><span class="s2">"consequents"</span><span class="p">]</span><span class="o">==</span><span class="p">{</span><span class="s1">'Yes'</span><span class="p">}))</span> <span class="o">&amp;</span><span class="p">(</span><span class="n">a</span><span class="o">.</span><span class="n">antecedent_len</span><span class="o">&gt;</span><span class="mi">2</span><span class="p">)</span> <span class="p">]</span>
<span class="n">b</span><span class="o">.</span><span class="n">sort_values</span><span class="p">(</span><span class="n">by</span><span class="o">=</span><span class="s1">'confidence'</span><span class="p">,</span><span class="n">ascending</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[82]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>antecedents</th>
      <th>consequents</th>
      <th>antecedent support</th>
      <th>consequent support</th>
      <th>support</th>
      <th>confidence</th>
      <th>lift</th>
      <th>leverage</th>
      <th>conviction</th>
      <th>antecedent_len</th>
      <th>survived</th>
      <th>anteced</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>(Female, 2nd, Child)</td>
      <td>(Yes)</td>
      <td>0.005906</td>
      <td>0.323035</td>
      <td>0.005906</td>
      <td>1.000000</td>
      <td>3.095640</td>
      <td>0.003998</td>
      <td>inf</td>
      <td>3</td>
      <td>Y</td>
      <td>'Female', '2nd', 'Child'</td>
    </tr>
    <tr>
      <th>66</th>
      <td>(Adult, 1st, Female)</td>
      <td>(Yes)</td>
      <td>0.065425</td>
      <td>0.323035</td>
      <td>0.063607</td>
      <td>0.972222</td>
      <td>3.009650</td>
      <td>0.042473</td>
      <td>24.370741</td>
      <td>3</td>
      <td>Y</td>
      <td>'Adult', '1st', 'Female'</td>
    </tr>
    <tr>
      <th>78</th>
      <td>(Adult, Male, 2nd)</td>
      <td>(No)</td>
      <td>0.076329</td>
      <td>0.676965</td>
      <td>0.069968</td>
      <td>0.916667</td>
      <td>1.354083</td>
      <td>0.018296</td>
      <td>3.876420</td>
      <td>3</td>
      <td>N</td>
      <td>'Adult', 'Male', '2nd'</td>
    </tr>
    <tr>
      <th>97</th>
      <td>(Crew, Adult, Female)</td>
      <td>(Yes)</td>
      <td>0.010450</td>
      <td>0.323035</td>
      <td>0.009087</td>
      <td>0.869565</td>
      <td>2.691861</td>
      <td>0.005711</td>
      <td>5.190065</td>
      <td>3</td>
      <td>Y</td>
      <td>'Crew', 'Adult', 'Female'</td>
    </tr>
    <tr>
      <th>75</th>
      <td>(Adult, 2nd, Female)</td>
      <td>(Yes)</td>
      <td>0.042254</td>
      <td>0.323035</td>
      <td>0.036347</td>
      <td>0.860215</td>
      <td>2.662916</td>
      <td>0.022698</td>
      <td>4.842904</td>
      <td>3</td>
      <td>Y</td>
      <td>'Adult', '2nd', 'Female'</td>
    </tr>
    <tr>
      <th>88</th>
      <td>(Adult, Male, 3rd)</td>
      <td>(No)</td>
      <td>0.209905</td>
      <td>0.676965</td>
      <td>0.175829</td>
      <td>0.837662</td>
      <td>1.237379</td>
      <td>0.033731</td>
      <td>1.989896</td>
      <td>3</td>
      <td>N</td>
      <td>'Adult', 'Male', '3rd'</td>
    </tr>
    <tr>
      <th>100</th>
      <td>(Crew, Adult, Male)</td>
      <td>(No)</td>
      <td>0.391640</td>
      <td>0.676965</td>
      <td>0.304407</td>
      <td>0.777262</td>
      <td>1.148157</td>
      <td>0.039280</td>
      <td>1.450292</td>
      <td>3</td>
      <td>N</td>
      <td>'Crew', 'Adult', 'Male'</td>
    </tr>
    <tr>
      <th>96</th>
      <td>(Male, Child, 3rd)</td>
      <td>(No)</td>
      <td>0.021808</td>
      <td>0.676965</td>
      <td>0.015902</td>
      <td>0.729167</td>
      <td>1.077111</td>
      <td>0.001138</td>
      <td>1.192745</td>
      <td>3</td>
      <td>N</td>
      <td>'Male', 'Child', '3rd'</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[84]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">ax</span> <span class="o">=</span> <span class="n">sns</span><span class="o">.</span><span class="n">barplot</span><span class="p">(</span><span class="n">x</span><span class="o">=</span><span class="n">b</span><span class="p">[</span><span class="s2">"confidence"</span><span class="p">]</span><span class="o">*</span><span class="mi">100</span><span class="p">,</span><span class="n">y</span><span class="o">=</span><span class="s2">"anteced"</span><span class="p">,</span><span class="n">hue</span><span class="o">=</span><span class="n">b</span><span class="o">.</span><span class="n">survived</span><span class="p">,</span><span class="n">data</span><span class="o">=</span><span class="n">b</span><span class="p">,</span><span class="n">palette</span><span class="o">=</span><span class="s2">"coolwarm"</span><span class="p">)</span>
<span class="n">sns</span><span class="o">.</span><span class="n">move_legend</span><span class="p">(</span><span class="n">ax</span><span class="p">,</span> <span class="s2">"upper left"</span><span class="p">,</span> <span class="n">bbox_to_anchor</span><span class="o">=</span><span class="p">(</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">))</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s2">"Rules"</span><span class="p">,</span><span class="n">color</span><span class="o">=</span><span class="s2">"green"</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s2">"confidence %"</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">grid</span><span class="p">()</span>

<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="n">label</span><span class="o">=</span><span class="s2">"Confidence of survival plot"</span><span class="p">,</span><span class="n">fontsize</span><span class="o">=</span><span class="mi">20</span><span class="p">,</span><span class="n">color</span><span class="o">=</span><span class="s2">"black"</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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YCB69eolN37/TXp6eli+fDnWrFkDBwcH9OzZU2k7qVQqd5OP0vj666/h5eWFWbNm4fnz5/Dy8oKXl5f4LbyBgQGmTp2K5s2bo3379tDV1UVMTIzG4ypOdHQ0Bg8ejEmTJqFx48b44IMP8Ndff4lnEMrC1tYWUqkUv/76Kzw8PLBgwYISh2w1b94cCQkJuH79Otq1awcvLy/MnDkT9vb2ZY6DiIiIiORJhKo6u7iKCA8PR3x8fIlXBKpolTWuyigzMxOWlpZ48OABJ1ZXoLy8PMTFxSEwMJDjWisYc68dzLv2MPfaoem8F/7/fvr0KSwsLOTWvXz5EikpKXBxcYGRkZHa911VqZKXKj2cqSrYv38/li1bpu0wFFTWuIiIiIio8mMRoWEnT57UdghKVda4iIiIiKjyq9JzIoiIiIiIqOKxiCAiIiIiIpWwiCAiIiIiIpVwTgQRERERURHizjyv0P0FvmNW6raCIKBLly7Q1dXF/v375datXLkSU6dORVJSEpycnNQdJs9EEBERERFVRRKJBNHR0fjrr7+wZs0acXlKSgqmTJmCZcuWaaSAAFhEEBERERFVWY6Ojli2bBm++OILpKSkQBAEDB06FJ06dUJoaKjG9svhTEREREREVVhISAhiY2MxZMgQ9OnTB5cuXcKlS5c0uk8WEUREREREVdzatWvRtGlTHD9+HNu2bUOtWrU0uj8OZyIiIiIiquJq1aqFTz/9FO7u7ujdu7fG98cigoiIiIjoLaCnpwc9vYoZaMQigoiIiIiIVMIigoiIiIiIVMIigoiIiIiIVMKrMxERERERFUGVO0hrW3h4OMLDwytkXzwTQUREREREKmERQUREREREKmERQUREREREKmERQUREREREKmERQUREREREKuHVmYhK6cjFLJiaG2o7jGpDkOUDAA6cy4JEh3+qKhJzrx3Mu/ZoI/dV6Yo/RMrwTAQREREREamERQQREREREamERQQREREREamERQQREREREamEM7eIiIiIiIrw8HBMhe7PptPHKrUPDQ3F+vXrMX/+fHz11Vfi8p07d6J3794QBEHdIQLgmQgiIiIioirNyMgICxcuxOPHjytsnywiiIiIiIiqsM6dO8POzg7z58+vsH2yiCAiIiIiqsJ0dXUxb948rFixAv/++2+F7JNFBBERERFRFde7d294enpi1qxZFbI/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REREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEaUUGhqKXr16qbSNRCLBzp07NRKPJoWHh8PT01PbYahNVX0diIiIiCqrt76ICA0NRXh4uMLyzZs3Q1dXFyNGjKiQOFJTUyGRSHDhwgW55eHh4QgNDVWpL2dnZ8THx4vPJRIJJBIJTp06JdcuJycHNjY2kEgkcu01QSKRIDU1tdTtpVIp/P39xefr1q1Du3btYG1tDWtra3Tu3BmnT59Wf6BQPVYiIiIikvfWFxFFiYqKwuTJkxETE4Ps7Gxth1Nujo6OiI6OllsWGxsLMzMzLUWkmvj4ePTv3x9Hjx7FyZMn4eTkhICAAPz333/aDo2IiIiI3lAti4jU1FScOHECX331Fdzc3LBt2za59QUFBZg4cSKsrKxgY2ODyZMnQxAEuTbOzs5YunSp3DJPT0+lZz0AwMXFBQDg5eUFiUQi9y28OoSEhCAmJgYvXrwQl0VFRSEkJESh7ZQpU9CoUSOYmJigfv36mDlzJvLy8ortPzo6Gu7u7jAyMoKbmxtWrlyp1vg3bdqEUaNGwdPTE25ubli3bh1kMhkOHz4stvH398fYsWMxefJk1KhRA3Z2dgr5vn79Otq3bw8jIyN4eHjg4MGDao2TiIiIiKppEREVFYUePXrA0tISgwYNQmRkpNz6iIgIREVFITIyEn/88QcePXqE2NjYcu2zcGjOoUOHkJ6ejh07dpSrvze1bNkSLi4u2L59OwDg9u3bOHbsGD755BOFtubm5pBKpbhy5QqWLVuGdevWYcmSJUX2vW7dOkyfPh1z585FcnIy5s2bh5kzZ2L9+vVqPYbXZWdnIy8vDzVq1JBbvn79epiamuKvv/7Cd999h2+++UYsFGQyGYKDg6Grq4tTp05h9erVmDJlisZiJCIiIqqu9LQdgKZJpVK55zKZDFKpFCtWrAAAfPzxx5g4cSJu3LgBV1dXAMDSpUsxdepU9OnTBwCwevVq7N+/v1xx2NraAgBsbGxgZ2cnLi/qzEVxihrPP2TIEERFRWHQoEGIjo5GYGCguN/XzZgxQ/zZ2dkZkyZNwpYtWzB58mSl/c6ZMwcREREIDg4G8OqsypUrV7BmzRrxTMebZ2pKEhoaWuxckK+++gp16tRB586d5ZY3b94cs2bNAgA0bNgQP/zwAw4fPowuXbrg0KFDSE5ORmpqKurWrQsAmDdvHrp37y7XR0mx5uTkICcnR3yemZkJAMjLyyvxjA2pT2GumfOKx9xrB/OuPcy9dmg673w9NeutLyLedODAAWRlZYkfLGvWrImAgABERUVh3rx5ePr0KdLT0+Hr6ytuo6enBx8fH5U/KFe0QYMG4auvvsKtW7cglUqxfPlype22bduGpUuX4saNG3j+/Dny8/NhYWGhtO39+/dx+/ZtDB06FMOHDxeX5+fnw9LSUiPH8d133+GXX35BfHw8jIyM5NY1b95c7rm9vT0yMjIAAMnJyXBychILCAByr2NpzZ8/H7Nnz1ZYfvToUZiYmKjcH5UPh6RpD3OvHcy79jD32qGpvL8Nc14rs2pXRERFReHRo0dyHwZlMhnOnz+POXPmlLofHR0dhaJC2xWvjY0NgoKCMHToULx8+RLdu3fHs2fP5NqcOnUKH3/8MWbPno2uXbvC0tISMTExiIiIUNqnTCYD8GpIU+vWreXW6erqqv0YFi1ahHnz5uHQoUMKBQMA6Ovryz2XSCRijMqKPIlEonIMU6dOxcSJE8XnmZmZcHR0hJfxC1ibqt4flU2+AJzNNoaPyQvoMe0VirnXDuZde9723Nfw66PtEJTKy8vDwYMH0aVLF4X/7+pQOJKANKNaFREPHz7Erl27EBMTgyZNmojLZTIZ2rVrh7179yIoKAj29vY4deoU2rdvD+DVt+6JiYnw9vYWt7G1tUV6err4PDMzEykpKUXu28DAAMCrSduaFBYWhsDAQEyZMkXph/w///wT9erVw/Tp08Vl//zzT5H91a5dG3Xq1MGtW7cwcOBAjcRc6Pvvv8e3336L/fv3w8fHR+XtPTw8kJaWhjt37sDBwQEAcPLkSZX7MTQ0hKGhocJyPQneyn8ulR3zrj3MvXYw79rztuZeEx/Q1UlfX18jMVb2467qqlURsWHDBtjY2KBv377Q0ZGfUx4UFITIyEgEBQVh3LhxWLBgARo2bAh3d3csXrwYT548kWvfsWNHSKVSvP/++7C2tsbMmTOL/Wa+Vq1aMDY2xr59+1C3bl0YGRlpZDhQt27dcP/+/SKHJ7m6uiItLQ0xMTF45513sGfPnhInjYeHh2Ps2LGwsLBA9+7dkZOTg7Nnz+Lx48dy39iXx3fffYeZM2di8+bNcHZ2xt27dwEAZmZmpb5MbefOndG4cWMMHjwYERERyMzMlCuWiIiIiEg9qtXVmaKiotC7d2+FAgIA+vTpg927d+PevXuYNGkSBg8ejNDQUPj6+sLc3By9e/eWaz916lS0b98eQUFBCAwMRK9evdCgQYMi962np4fly5djzZo1cHBwQM+ePZW2k0qlZRqCU0gikaBmzZrimY839ezZExMmTMCYMWPg6emJEydOYObMmcX2OWzYMPz000+QSqVo1qwZ/Pz8IJVKxcvWKuPs7KzSpPGVK1ciNzcXH374Iezt7cXHokWLSt2Hjo4OYmNjkZOTg1atWmHYsGGYO3duqbcnIiIiotKRCJV9tnA1Ex4ejvj4eI3fYVqTXrx4gRo1aiAuLg4dOnTQdjjllpmZCUtLS1zfGQlrM06srij5AnAqyxjvmr6dY5QrM+ZeO5h37Xnbc2/T6WNth6BUXl4e4uLiEBgYqLE5EZaWlnj69GmRIzSo7KrVcKaqYP/+/Vi2bJm2wyiXhIQEdOzY8a0oIIiIiIhIEYuISqYsE4Erm27duqFbt27aDoOIiIiINKRazYkgIiIiIqLyYxFBREREREQqYRFBREREREQqYRFBREREREQqYRFBREREREQqYRFBREREREQqYRFBREREREQqYRFBREREREQqYRFBREREREQqYRFBREREREQq0dN2AERVhXXbnrCxsdF2GNVGXl4eEBeHGn59oK+vr+1wqhXmXjuYd+1h7olUxzMRRERERESkEhYRRERERESkEhYRRERERESkEhYRRERERESkEhYRRERERESkEhYRRERERESkEhYRRERERESkEhYRRERERESkEt5sjqiUjlzMgqm5obbDqDYEWT4A4MC5LEh0+KeqIjH32sG8aw9zrx2FeaeqiWciiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwiiIiIiIhIJSwi1EQikWDnzp3aDkMtwsPD4enpWeH7lUqlsLKyKrZNaGgoevXqJT739/fH+PHji93G2dkZS5cuLXd8RERERPRKpS8iQkNDER4eLj739/eHRCJReOTn52svyDIIDw9HaGioSts4OzsjPj4eAJCamoqhQ4fCxcUFxsbGaNCgAWbNmoXc3NxKF2uho0ePIjAwEDY2NjAxMYGHhwcmTZqE//77r9T9Llu2DFKpVKVYShMbEREREZVepS8ilBk+fDjS09PlHnp6etoOq0L9/fffkMlkWLNmDS5fvowlS5Zg9erVmDZtmrZDU2rNmjXo3Lkz7OzssH37dly5cgWrV6/G06dPERERUep+LC0tSzxbQURERESaVSWLCBMTE9jZ2ck9CkVHR8Pd3R1GRkZwc3PDypUrxXWpqamQSCTYunUr2rVrB2NjY7zzzju4du0azpw5Ax8fH5iZmaFbt264f/++uN2ZM2fQpUsX1KxZE5aWlvDz88O5c+eKjfG///5Dv379YG1tDRsbG/Ts2ROpqalqy0G3bt0QHR2NgIAA1K9fHx988AG++OIL7NixQ2wTHx8PiUSCw4cPw8fHByYmJmjTpg2uXr0q19eCBQtQu3ZtmJubY+jQoXj58qXa4gSAf//9F2PHjsXYsWMRFRUFf39/ODs7o3379vjpp5/w9ddfy7Xfv38/3N3dxdciPT1dXPfmcKY3ZWRk4P3334exsTFcXFywadMmtR4LEREREQFv1df369atw6xZs/DDDz/Ay8sL58+fx/Dhw2FqaoqQkBCx3axZs7B06VI4OTkhLCwM/fv3h4WFBZYtWwYTExN89NFH+Prrr7Fq1SoAwLNnzxASEoLly5cDACIiIhAYGIjr16/D3NxcIY7s7Gx06NAB7dq1w7Fjx6Cnp4dvv/0W3bp1w//+9z8YGBho5PifPn2KGjVqKCyfPn06IiIiYGtrixEjRiAsLAx//vknAGDr1q2YNWsWfvzxR7Rr1w4bNmzA8uXLUb9+fbXF9euvvyI3NxeTJ09Wuv71MwvZ2dlYtGgRNmzYAB0dHQwaNAhffPFFqYuB0NBQ3L59G0eOHIGBgQHGjh2LjIwMleLNyclBTk6O+DwzMxMAIMgKIMiq1rC5qqww18x5xWPutYN51x7mXjsK852Xl6eR/jXVL71S6YsIZePfV65ciZ9++kl8/tlnnyEiIgJz5sxBREQEgoODAQAuLi64cuUK1qxZI1dEfPHFF+jatSsAYNy4cejfvz8OHz6M9957DwAwdOhQuf127NhRbv9r1qyBtbU1EhISEBQUpBBfTEwMdHR08NNPP0EikQB4dYbEysoK8fHxCAgIkJvnUVrFncm4efMmVqxYoXRo0Ny5c+Hn5wcA+Oqrr9CjRw+8fPkSRkZGWLp0KcLCwjBs2DAAwLfffotDhw7JnY0ob6zXr1+HhYUF7O3tS9wuLy8Pq1evRoMGDQAAY8aMwTfffFOqfV67dg179+7FqVOn0Lp1awBAZGQk3N3di4xNmfnz52P27NkKywvun0J+lkmpYiH1Kcj4U9shVFvMvXYw79rD3GvHwYMHNdJvdna2RvqlVyp9EaHMwIEDMX36dPG5lZUV7t+/j9u3b2Po0KEYPny4uC4/Px+WlpZy2zdv3lz8uXbt2gCAZs2ayS17/dvrjIwMfP311zhy5Aju3buHgoICZGdnIy0tTWl8iYmJuHHjhsJZipcvX+LmzZtlOOLi3blzB926dUPfvn3FYuB1rx9v4Qf5jIwMODk5ITk5GSNGjJBr7+vri6NHj6otPkEQxGKqJCYmJmIBURhvac8kJCcnQ09PDz4+PuIyNzc3ledQTJ06FRMnThSfZ2ZmwtHRER06dICNjY1KfVHZ5eXl4eDBg+jSpQv09fW1HU61wtxrB/OuPcy9dmg674UjCUgzqmQRYWlpCVdXV7ll9+7dA/BqSFPht9CFdHV15Z6//kYt/HD75jKZTCY+Dw0Nxf3797F06VLUq1cPhoaG8PX1LfJKSDKZDC1btlQ6BMfW1rY0h1hqd+7cQYcOHeDr64u1a9cqbaPseF8/Pk1r1KgRnj59ivT09BLPRrz5R0QikUAQhFLtp7BdaQuWohgaGsLQ0FBpbPznUvGYd+1h7rWDedce5l47NJV3vpaaVSUnVitTu3Zt1KlTB7du3YKrq6vcw8XFpVx9Hz9+HGPHjkVgYCCaNGkCQ0NDPHjwoMj23t7euH79OmrVqqUQy5tnRcrjv//+g7+/P7y9vREdHQ0dHdVfTnd3d5w6dUpu2ZvPy+vDDz+EgYEBvvvuO6Xrnzx5opb9uLu7Iz8/H2fPnhWXXb16VW39ExEREdErVfJMRFHCw8MxduxYWFhYoHv37sjJycHZs2fx+PFjueEpqnJ1dcWGDRvg4+ODzMxMfPnllzA2Ni6y/cCBA/H999+jZ8+e+Oabb1C3bl2kpaVhx44d+PLLL1G3bt0yx1Lozp078Pf3h5OTExYtWiR3NanXr1ZVknHjxiEkJAQ+Pj5o27YtNm3ahMuXL6t1YrWjoyOWLFmCMWPGIDMzE4MHD4azszP+/fdf/PzzzzAzM1PpMq9Fady4Mbp164bhw4dj7dq10NPTw/jx44t9rYiIiIhIdW/NmQgAGDZsGH766SdIpVI0a9YMfn5+kEql5T4TERUVhcePH8PLywuffPIJxo4di1q1ahXZ3sTEBMeOHYOTkxOCg4Ph7u6OsLAwvHjxAhYWFkq3kUqlKg3DOXDgAG7cuIEjR46gbt26sLe3Fx+q6NevH77++mtMmTIFLVu2xD///IORI0cWu42qsQLAqFGjcODAAfz333/o3bs33NzcMGzYMFhYWOCLL75Qqa/iREdHw9HREX5+fggODsann35a7GtFRERERKqTCKUdcE4aFR4ejvj4+CpxJ+WqFKs6ZGZmwtLSEg8ePODE6gqUl5eHuLg4BAYGclxrBWPutYN51x7mXjs0nffC/99Pnz4t8ktcKru3ajhTVbZ//34sW7ZM22GUSlWKlYiIiIjUr0xFxLn0c9DX0Uez2q8ui7rr712IvhAND1sPhPuHw0BXMzdTe5udPHlS2yGUWlWKlYiIiIjUr0xzIj7b/RmuPbwGALj1+BY+3v4xTPRN8OuVXzH5oPK7EhMRERER0duhTEXEtYfX4GnnCQD49fKvaF+vPTb32QxpTym2J29XZ3xERERERFTJlKmIEAQBMuHVzcoOpRxCoGsgAMDR0hEPsou+fwIREREREVV9ZSoifBx88O3xb7Hh4gYkpCagR6MeAICUxymobVpbrQESEREREVHlUqYiYmm3pTiXfg5j9o7B9HbT4VrDFQCw7co2tHFso9YAiYiIiIiocinT1Zma126OpJFJCsu/D/geuhLdcgdFRERERESVV5nvWP3k5RP8dO4nTD00FY9ePAIAXLl/BRlZGWoLjoiIiIiIKp8ynYn4373/odPPnWBlZIXUJ6kY3nI4ahjXQGxyLP55+g9+7v2zuuMkIiIiIqJKokxnIibun4ghnkNw/fPrMNIzEpd3b9gdx/45prbgiIiIiIio8ilTEXHmzhl81vIzheV1zOvg7vO75Q6KiIiIiIgqrzIVEUZ6RsjMyVRYfvXhVdia2pY7KCIiIiIiqrzKVET0bNwT3xz7BnkFeQAACSRIe5qGrw59hT7ufdQaIBERERERVS5lKiIWBSzC/az7qLWoFl7kvYCf1A+uy11hbmiOuR3nqjtGIiIiIiKqRMp0dSYLQwv8EfYHjqQcwbn0c5AJMnjbe6Nz/c7qjo+IiIiIiCqZMhURhTq6dERHl47qioWIiIiIiKqAUhcRy/9aXupOx7YeW6ZgiIiIiIio8it1EbHk1JJStZNAwiKCiIiIiOgtVuoiImVciibjICIiIiKiKqJMV2ciIiIiIqLqq0wTq8N2hRW7PqpnVJmCISIiIiKiyq9MRcTjl4/lnucV5OFSxiU8efmEV2siIiIiInrLlamIiO0Xq7BMJsgwas8o1LeuX+6giIiIiIio8lLbnAgdiQ4mvDuh1FdxIiIiIiKiqkmtE6tvPr6JfFm+OrskIiIiIqJKpkzDmSbunyj3XBAEpD9Px+5ruxHqGaqOuIiIiIiIqJIqUxFxLv0cJBKJ+FxHogNbE1ss7roYPRr2UFtwRERERERU+ZSpiIgPjVdYdvf5Xcw9Nhfj9o3Di+kvyhsXERERERFVUirNiXjy8gkG7hgI2+9tUWdxHSz/azlkggyzjs5Cg+UNcOq/U4j6gPeIICIiIiJ6m6l0JmLa4Wk49s8xhLQIwd4bezFh/wTsu7EPL/NfIm5AHPyc/TQVJxERERERVRIqFRF7ru9BdM9odK7fGaPeGQXX5a5oZNMIS7st1VB4RERERERU2ag0nOnOszvwsPUAANS3rg8jPSMM8x6mkcCIiIiIiKhyUqmIkAky6Ovoi891dXRhqm+q9qCIiIiIiKjyUmk4kyAICN0VCkNdQwDAy/yXGLFnhEIhsaPfDvVFWEmFhobiyZMn2LlzZ6m3kUgkiI2NRa9evTQWlyaEh4dj586duHDhgrZDKZP4+Hh06NABjx8/hpWVlbbDISIiIqryVDoTEeIZglqmtWBpZAlLI0sMaj4IDuYO4vPCR2UWGhqK8PBwheWbN2+Grq4uRowYUSFxpKamQiKRKHwwDw8PR2hoqEp9OTs7Iz4+XnwukUggkUhw6tQpuXY5OTmwsbGBRCKRa68JEokEqamppW4vlUrh7+8vPt+xYwd8fHxgZWUFU1NTeHp6YsOGDWqJLT4+Hs7Ozmrpi4iIiKg6UulMRHTPaE3FoXVRUVGYPHkyVq1ahcWLF8PExETbIZWLo6MjoqOj8e6774rLYmNjYWZmhkePHmkxstKpUaMGpk+fDjc3NxgYGGD37t0YMmQIatWqha5duyrdJjc3FwYGBhUcKREREVH1o9KZiLdVamoqTpw4ga+++gpubm7Ytm2b3PqCggJMnDgRVlZWsLGxweTJkyEIglwbZ2dnLF26VG6Zp6en0rMeAODi4gIA8PLygkQikfsWXh1CQkIQExODFy/+78Z/UVFRCAkJUWg7ZcoUNGrUCCYmJqhfvz5mzpyJvLy8YvuPjo6Gu7s7jIyM4ObmhpUrV6o1fn9/f/Tu3Rvu7u5o0KABxo0bh+bNm+OPP/6QazNmzBhMnDgRNWvWRJcuXQAAcXFxaNSoEYyNjdGhQweVzogQERERUcnKdMfqt01UVBR69OgBS0tLDBo0CJGRkRg8eLC4PiIiAlFRUYiMjISHhwciIiIQGxuLjh07lnmfp0+fRqtWrXDo0CE0adJE7d+gt2zZEi4uLti+fTsGDRqE27dv49ixY/jxxx8xZ84cubbm5uaQSqVwcHBAUlIShg8fDnNzc0yePFlp3+vWrcOsWbPwww8/wMvLC+fPn8fw4cNhamqqtEgpL0EQcOTIEVy9ehULFy6UW7d+/XqMHDkSf/75JwRBwO3btxEcHIwRI0Zg5MiROHv2LCZNmqTS/nJycpCTkyM+z8zMBADk5eWVWFyR+hTmmjmveMy9djDv2sPca4em887XU7OqXREhlUrlnstkMkilUqxYsQIA8PHHH2PixIm4ceMGXF1dAQBLly7F1KlT0adPHwDA6tWrsX///nLFYWtrCwCwsbGBnZ2duLyoMxfFKeqb9iFDhiAqKgqDBg1CdHQ0AgMDxf2+bsaMGeLPzs7OmDRpErZs2VJkETFnzhxEREQgODgYwKuzKleuXMGaNWvEIuLNMzUlCQ0NVZgL8vTpU9SpUwc5OTnQ1dXFypUrxbMNhVxdXfHdd9+Jz6dNm4b69etjyZIlkEgkaNy4MZKSkuSKD39//2LPTsyfPx+zZ89WWH706NEqP8ytKjp48KC2Q6i2mHvtYN61h7nXDk3lPTs7WyP90ivVroh404EDB5CVlYXu3bsDAGrWrImAgABERUVh3rx5ePr0KdLT0+Hr6ytuo6enBx8fH5U/KFe0QYMG4auvvsKtW7cglUqxfPlype22bduGpUuX4saNG3j+/Dny8/NhYWGhtO39+/dx+/ZtDB06FMOHDxeX5+fnw9JSvZPqzc3NceHCBTx//hyHDx/GxIkTUb9+fbmhXz4+PnLbJCcn491334VEIhGXvf7alcbUqVMxceJE8XlmZiYcHR3hZfwC1qaSYrYkdcoXgLPZxvAxeQE9pr1CMffawbxrj7pyX8Ovj/qCqgby8vJw8OBBdOnSBfr6+iVvoKLCkQSkGdW+iIiKisKjR4/kvmGWyWQ4f/68wrCf4ujo6CgUFdo+jWZjY4OgoCAMHToUL1++RPfu3fHs2TO5NqdOncLHH3+M2bNno2vXrrC0tERMTAwiIiKU9imTyQC8GtLUunVruXW6urpqjV9HR0c8G+Tp6Ynk5GTMnz9frogwNZW/vLA6CjtDQ0MYGhoqLNeTgP/YtYB51x7mXjuYd+0pb+418UG4OtDX19dI7vh6aFa1LiIePnyIXbt2ISYmBk2aNBGXy2QytGvXDnv37kVQUBDs7e1x6tQptG/fHsCrb90TExPh7e0tbmNra4v09HTxeWZmJlJSUorcd+EciIKCAnUflpywsDAEBgZiypQpSj/k//nnn6hXrx6mT58uLvvnn3+K7K927dqoU6cObt26hYEDB2ok5qIIgiA3V0EZDw8PhXt3vHmpWyIiIiIqn2pdRGzYsAE2Njbo27cvdHTkL1QVFBSEyMhIBAUFYdy4cViwYAEaNmwId3d3LF68GE+ePJFr37FjR0ilUrz//vuwtrbGzJkzi/1mvlatWjA2Nsa+fftQt25dGBkZqX04EAB069YN9+/fL3J4kqurK9LS0hATE4N33nkHe/bsQWxsbLF9hoeHY+zYsbCwsED37t2Rk5ODs2fP4vHjx3LDgMpj/vz58PHxQYMGDZCbm4u4uDj8/PPPWLVqVbHbjRgxAhEREZg4cSI+++wzJCYmKsyDISIiIqLyqdaXeI2KikLv3r0VCggA6NOnD3bv3o179+5h0qRJGDx4MEJDQ+Hr6wtzc3P07t1brv3UqVPRvn17BAUFITAwEL169UKDBg2K3Leenh6WL1+ONWvWwMHBAT179lTaTiqVyo3vV5VEIkHNmjWLvPpTz549MWHCBIwZMwaenp44ceIEZs6cWWyfw4YNw08//QSpVIpmzZrBz88PUqlUvGytMs7OzipNGs/KysKoUaPQpEkTtGnTBtu2bcPGjRsxbNiwYrdzcnLC9u3b8fvvv6NFixZYvXo15s2bV+r9EhEREVHJJEJlnx1czYWHhyM+Pl7jd5jWpBcvXqBGjRqIi4tDhw4dtB2OyjIzM2FpaYnrOyNhbcarM1WUfAE4lWWMd005ybSiMffawbxrj7pyb9PpY/UFVQ3k5eUhLi4OgYGBGptYbWlpiadPnxY5IoPKrloPZ6oK9u/fj2XLlmk7jHJJSEhAx44dq2QBQURERESKWERUcidPntR2COXWrVs3dOvWTdthEBEREZGaVOs5EUREREREpDoWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBI9bQdAVFVYt+0JGxsbbYdRbeTl5QFxcajh1wf6+vraDqdaYe61g3nXHuaeSHU8E0FERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEUFERERERCphEaEB8fHxkEgkePLkibZDURAaGopevXoV28bZ2RlLly4Vn0skEuzcubPI9qmpqZBIJLhw4YJaYlQ3qVQKKysrbYdBRERE9NaolkVEaGgowsPDxef+/v6QSCRYsGCBQtvAwEBIJBK59prg7+8PqVRa6vaFH9xfJwgC1q5di9atW8PMzAxWVlbw8fHB0qVLkZ2dXeq+z5w5g08//bTU7UsTW0nCw8MRGhoqPl+1ahWaN28OCwsLWFhYwNfXF3v37i1zTK+TSqXw9/dXS19ERERE1VG1LCKUcXR0RHR0tNyyO3fu4MiRI7C3t9dSVKr55JNPMH78ePTs2RNHjx7FhQsXMHPmTOzatQsHDhwodT+2trYwMTHRYKQlq1u3LhYsWICzZ8/i7Nmz6NixI3r27InL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QURERESKWERUQubm5rh9+7a2w1CbESNGaDsEIiIiIlIjzokgIiIiIiKVsIggIiIiIiKVsIggIiIiIiKVsIggIiIiIiKVsIggIiIiIiKVsIggIiIiIiKVsIggIiIiIiKVsIggIiIiIiKVsIggIiIiIiKVsIggIiIiIiKV6Gk7AKKqwrptT9jY2Gg7jGojLy8PiItDDb8+0NfX13Y41Qpzrx3MOxFVJTwTQUREREREKmERQUREREREKmERQUREREREKmERQUREREREKmERQUREREREKmERQUREREREKmERQUREREREKmERQUREREREKuHN5ohK6cjFLJiaG2o7jGpDkOUDAA6cy4JEh3+qKhJzrx3Mu/ZUhtwHvmOmlf0SlRXPRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRBARERERkUpYRFRCEokEO3fuLHV7qVQKKysrjcVT2YWGhqJXr17aDoOIiIio2tBqEREaGorw8HC5ZTdu3MCQIUNQt25dGBoawsXFBf3798fZs2e1HltJJBIJUlNTFZYHBARAV1cXp06dUk9wJQgPD4enp6fCcmdnZ8THx5e6n/j4eDg7O4vPpVIpJBKJwuOnn34qf9AVKDU1FRKJRNthEBEREVVZetoO4HVnz55Fp06d0LRpU6xZswZubm549uwZdu3ahUmTJiEhIUHpdnl5edDX16/gaEsnLS0NJ0+exJgxYxAZGYl3331X2yGVi4WFBa5evSq3zNLSUkvREBEREZE2VJrhTIIgIDQ0FA0bNsTx48fRo0cPNGjQAJ6enpg1axZ27doF4P++Rd66dSv8/f1hZGSEjRs3AgCio6Ph7u4OIyMjuLm5YeXKlWL/ffr0weeffy4+Hz9+PCQSCS5fvgwAyM/Ph7m5Ofbv36/W44qOjkZQUBBGjhyJLVu2ICsrS2799evX0b59exgZGcHDwwMHDx6UWx8fHw+JRIInT56Iyy5cuFDkWQ+pVIrZs2fj4sWL4pkCqVSqtuORSCSws7OTexgbGwMArly5gsDAQJiZmaF27dr45JNP8ODBA3Fbf39/fP755xg/fjysra1Ru3ZtrF27FllZWRgyZAjMzc3R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i9122LBh+OmnnyCVStGsWTP4+flBKpXCxcVFbNO0aVPUrFkTLVq0EAsGPz8/FBQUlPitdXh4uNyN14qTmJiIixcvok+fPgrrzM3NERAQgMjISOjo6CA2NhY5OTlo1aoVhg0bhrlz58q119fXxy+//IK///4bLVq0wMKFC/Htt98Wu/8+ffqgW7du6NChA2xtbfHLL78obefv74/Q0NBSHVNJHBwc8Oeff6KgoABdu3ZF06ZNMW7cOFhaWiq92lZpjRgxAsHBwejXrx9at26Nhw8fyp2VUGbOnDn4+uuvMX/+fLi7u6Nr1674/fff5d4LRERERFR2EqG4y9SQqPDDtjrvuaBtzs7OCA8PV1sh8bbKzMyEpaUlHjx4wInVFSgvLw9xcXEIDAzkuNYKxtxrB/OuPcy9dmg674X/v58+farW0Sb0SqWZE1HZJSQk4NixY9oOQ23+/vtvmJubY/DgwdoOhYiIiIiqGBYRpZSSkqLtENTKzc0NSUlJ2g6DiIiIiKqgSjUngoiIiIiIKj8WEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEUREREREpBIWEW8piUSCnTt3lrq9VCqFlZWVxuLRJGdnZyxdulTbYRARERFVG1W+iAgNDUV4eLjcshs3bmDIkCGoW7cuDA0N4eLigv79++Ps2bNaj60kEokEqampCssDAgKgq6uLU6dOqSe4EoSHh8PT01NhubOzM+Lj40vdT3x8PJydncXnUqkUEokE7u7uCm23bt0KiUQi114TpFIp/P39NboPIiIiordZlS8i3nT27Fm0bNkS165dw5o1a3DlyhXExsbCzc0NkyZNKnK7vLy8CoxSNWlpaTh58iTGjBmDyMhIbYdT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
"
class="
"
>
</div>

</div>

</div>

</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<h1 id="Summary">Summary<a class="anchor-link" href="#Summary">&#182;</a></h1>
</div>
</div>
</div>
</div>
</body>







</html>
