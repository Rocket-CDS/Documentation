# Shared or Partial Templates

To create high-performance, uncomplicated modules, there are shared templates.   
These are templates used for specific functionalities and stop duplication of common requirements.

To use a shared template you use a INJECT token (recommended) or the @RenderTemplate() token.

---

## Admin Detail: RocketDirectory
Standard edit buttons.
```
[INJECT:appthemedirectory,EditButtonBar.cshtml]
```
```
[INJECT:appthemedirectory,AdminPaging.cshtml]
```
```
[INJECT:appthemedirectory,AdminSearch.cshtml]
```
```
[INJECT:appthemedirectory,ArticleCategoryList.cshtml]
```
```
[INJECT:appthemedirectory,ArticleCategoryListBlock.cshtml]
```
```
[INJECT:appthemedirectory,ArticleDocuments.cshtml]
```
```
[INJECT:appthemedirectory,ArticleDocumentsBlock.cshtml]
```
```
[INJECT:appthemedirectory,ArticleImages.cshtml]
```
```
[INJECT:appthemedirectory,ArticleImagesBlock.cshtml]
```
```
[INJECT:appthemedirectory,ArticleLinks.cshtml]
```
```
[INJECT:appthemedirectory,ArticleLinksBlock.cshtml]
```
```
[INJECT:appthemedirectory,ArticleModels.cshtml]
```
```
[INJECT:appthemedirectory,ArticleModelsBlock.cshtml]
```
```
[INJECT:appthemedirectory,ArticlePropertyList.cshtml]
```
```
[INJECT:appthemedirectory,ArticlePropertyListBlock.cshtml]
```
```
[INJECT:appthemedirectory,ArticleReviews.cshtml]
```
```
[INJECT:appthemedirectory,ArticleReviewsBlock.cshtml]
```
```
[INJECT:appthemedirectory,ArticleSocialMedia.cshtml]
```
```
[INJECT:appthemedirectory,ArticleSocialMediaBlock.cshtml]
```

### Admin: RocketBlog
General Admin Detail fields.
```
[INJECT:appthemesystem,AdminGeneral.cshtml]
```
```
[INJECT:appthemesystem,AdminGeneralBlock.cshtml]
```
### Admin: RocketEvents
General Admin Detail fields.
```
[INJECT:appthemesystem,AdminGeneral.cshtml]
```
```
[INJECT:appthemesystem,AdminGeneralBlock.cshtml]
```
```
[INJECT:appthemesystem,SystemSettings.cshtml]
```

### Admin: RocketBlog
General Admin Detail fields.
```
[INJECT:appthemesystem,AdminGeneral.cshtml]
```
```
[INJECT:appthemesystem,AdminGeneralBlock.cshtml]
```
```
[INJECT:appthemesystem,SystemSettings.cshtml]
```

## View: RocketDirectory
```
[INJECT:appthemedirectorydefault,SearchBanner.cshtml]
```
```
[INJECT:appthemedirectorydefault,ArticlePaging.cshtml]
```
```
[INJECT:appthemedirectorydefault,Loader.cshtml]
```
### View: RocketEvents
```
[INJECT:appthemesystem,ArticlePaging.cshtml]
```

--------------------------
--------------------
---------------------------








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