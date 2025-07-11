
Suggestions - Open to Vault Selector Instead of Last Workspace
https://forum.obsidian.md/t/option-open-last-used-vault-or-open-vault-selector/7093
https://forum.obsidian.md/t/how-to-change-startup-to-vault-choose-interface/58983

Vault Settings 
	https://help.obsidian.md/Files+and+folders/How+Obsidian+stores+data
	
Embedding 
	Files
	https://help.obsidian.md/Linking+notes+and+files/Embed+files 
	Webpages
	https://help.obsidian.md/Editing+and+formatting/Embed+web+pages
Git Repos
	https://forum.obsidian.md/t/what-exactly-is-a-vault/4369/2


"You do not rise to the level of your goals. You fall to the level of your systems." by James Clear ^quote-of-the-day

[[#^quote-of-the-day]]

Forums
	https://forum.obsidian.md/
Publish
	https://help.obsidian.md/Obsidian+Publish/Introduction+to+Obsidian+Publish
Plugins
	https://obsidian.md/plugins

Community Plugins
https://github.com/nguyenvanduocit/obsidian-open-gate
https://github.com/Vinzent03/obsidian-git


---
#### CSS Snippets
Obsidian: Settings > Appearance 
Loc: cd Vaults/ReactMapVault/.obsidian/snippets

margins.css [src](https://forum.obsidian.md/t/how-can-i-make-editor-width-wider/58157/2)
```css
/*- editor line width - default 700px -*/
body {
  --file-line-width: 800px;
}

/*- editor line width - fills entire editor -*/
body {
  --file-line-width: 100%;
}
```


dashboard.css [src](https://github.com/TfTHacker/DashboardPlusPlus/tree/master/.obsidian/snippets )
```css
/*
Last updated 2024-11-18
This solution is provided by TfT Hacker. Learn more at https://tfthacker.com
*/

.dashboard {
    font-family: inherit;
    padding-left: 25px !important;
    padding-right: 25px !important;
    padding-top: 20px !important;
}

.dashboard .markdown-preview-section {
    max-width: 100%;
}

/* Title at top of the document */
.dashboard .markdown-preview-section .title {
    top: 60px;
    position: absolute;
    font-size: 26pt !important;
    font-weight: bolder;
    letter-spacing: 8px;
}

.dashboard h1 {
    border-bottom-style: dotted !important;
    border-width: 1px !important;
    padding-bottom: 3px !important;
}


/* Get rid of the parent bullet */
.dashboard div.markdown-preview-section>div>ul>li>.list-bullet {
    display: none !important;
}

/* Remove the indentation guide lines */
.dashboard.markdown-rendered.show-indentation-guide li>ul::before,
.dashboard.markdown-rendered.show-indentation-guide li>ol::before {
    display: none;
}

div.markdown-preview-section>div>ul.has-list-bullet>li {
    padding-left: 0p !important;
}

.dashboard div>ul {
    list-style: none;
    display: flex;
    column-gap: 50px;
    flex-flow: row wrap;
}

.dashboard div>ul>li {
    min-width: 250px;
    width: 15%;
}

/* Dataview support */
.dashboard ul.dataview {
    row-gap: 0px !important;
    list-style-type: disc !important;
}
```


dashboard-ReadLineLength.css [src](https://github.com/TfTHacker/DashboardPlusPlus/tree/master/.obsidian/snippets )
```css 
/*
Optional css that can be added to make dashboards use wide margin
if "Readable line length" is enabled in Editor

Last updated 2024-11-18
This solution is provided by TfT Hacker. Learn more at https://tfthacker.com
*/

.dashboard.markdown-preview-view.is-readable-line-width .markdown-preview-section,
.dashboard.markdown-preview-view.is-readable-line-width .markdown-preview-section>div {
  width: 100% !important;
  max-width: 100% !important;
}
```


Other snippet sources:
- obsidian docs [CSS snippets overview](https://help.obsidian.md/snippets)
- obsidian docs [CSS Variables](https://docs.obsidian.md/Reference/CSS+variables/CSS+variables)
- obsidian docs [CSS Properties](https://help.obsidian.md/properties#Property+types)
- obsidian forum [adjusting margins for different views ](https://forum.obsidian.md/t/how-to-get-a-lager-page-width-in-both-editing-mode-and-preview-mode/7555)
- github [codeblock snippets](https://github.com/gsarig/obsidian-css-snippets/blob/main/code.css)
- github [more snippets - incl codeblock styles](https://github.com/r-u-s-h-i-k-e-s-h/Obsidian-CSS-Snippets/tree/Collection/Snippets)
- codeblock styling [code styler/Style Settings plugin](https://obsidian.rocks/the-obsidian-code-block/)
- snippets [card view, media grids, pretty highlights/tables, tasks customization](https://prakashjoshipax.com/obsidian-css-snippets/)
