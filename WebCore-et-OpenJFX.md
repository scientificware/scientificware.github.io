# WebCore et OpenJFX à la frontière des deux mondes.
[Freetype definitions](https://www.freetype.org/freetype2/docs/glyphs/glyphs-3.html) :

Picture giving all the JavaFX metric details for horizontal layout
![horizontal_layout_metrics](https://user-images.githubusercontent.com/19194678/41586819-f6da96c8-73ad-11e8-80e7-55dfa975f644.png)

Picture giving all the JavaFX metric details for vertical layout
![vertical_layout_metrics](https://user-images.githubusercontent.com/19194678/41586822-f7c46f1e-73ad-11e8-8c42-5bf256ecc8e5.png)

Picture giving all the WebCore metric details for horizontal layout
![webcore_glyph_metrics](https://user-images.githubusercontent.com/19194678/41594094-7f8d1ad6-73c2-11e8-8db5-7dd0df3408ca.png)

So I understand why when using GlyphBoundingBox, I didn't succeed to perfrom right alignment. Then I used getAscent() and getDescent() which work well for usual glyphs but not for mathematical glyphs as explains in the previous comment.

The rectangle definition are in different order : 
- JavaFX box[1] contains Glyph descent position on the y axis (oriented from down to up) but WebCore box[1] contains the ascent position on the y axis (oriented from up to down). Thus It's a negative value when a part of the glyph is over the baseline.
- JavaFX box[3] contains Glyph ascent position on the y axis (oriented from down to up) but WebCore box[3] contains the height of the glyph. it's always a positive value.

Finally no more need of getAscent() and getDescent(), I used, pending best JavaFX metrics understanding. 
Modify `getGlyphBoundingBox`


 in [`WCFontImpl.java`
](https://github.com/javafxports/openjdk-jfx/blob/48fd8216450b58aa100fbacf8d070b0385f0aaf5/modules/javafx.web/src/main/java/com/sun/javafx/webkit/prism/WCFontImpl.java)