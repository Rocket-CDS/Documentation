# Shared or Partial Templates

To create high-performance, uncomplicated modules, there are shared templates.   
These are templates used for specific functionalities and stop duplication of common requirements.

To use a shared template you use a INJECT token (recommended) or the @RenderTemplate() token.

---

Standard input fields for a row heading.  
```
[INJECT:appthemesystem,ArticleRowHeader.cshtml]
```
Displays standard fields for a row heading.
```
[INJECT:appthemesystem,ArticleRowHeaderView.cshtml]
```
Add multiple links 
```
[INJECT:appthemesystem,ArticleLinks.cshtml]
```
Add a single link
```
[INJECT:appthemesystem,ArticleLink.cshtml]
```
Add multiple images 
```
[INJECT:appthemesystem,ArticleImages.cshtml]
```
Add a single image
```
[INJECT:appthemesystem,ArticleImage.cshtml]
```
Add multiple images with fields to define size
```
[INJECT:appthemesystem,ArticleImagesSize.cshtml]
```
Add multiple documents 
```
[INJECT:appthemesystem,ArticleDocuments.cshtml]
```
Add a single document
```
[INJECT:appthemesystem,ArticleDocument.cshtml]
```
Add a title, its size and alignment, for the headerData.
```
[INJECT:appthemesystem,ArticleHeader.cshtml]
```
Display headerData.
```
[INJECT:appthemesystem,ArticleHeaderView.cshtml]
```
Adds a CDN for CKEditor. This token is to standardize across AppThemes.
```
[INJECT:appthemesystem,CKEditor4.cshtml]
```


## Shared Repo AppTheme
Each repo can have a shared template AppTheme. 
The shared AppTheme must be called: rocketcontentapi.01shared

Use "appthemeshared" to inject the repo template.

```
[INJECT:appthemeshared,ArticleRowHeader.cshtml]
```