Presently, the patch for JDK-8147476 solves MathML display issues in WebView. Consequently, it solves MathML display in HTMLEditor too.

The MathML rendering is pretty good and equivalent to MathML Rendering with Safari.
I think that JDK-8089878, which describe only rendering issues, is also solved.

Set HTMLEditor content with the following code displays the quadratic formula as expected in JDK-8089878.

<math display="block"> 
   <mrow> 
      <mi>x</mi> 
      <mo>=</mo> 
      <mfrac> 
         <mrow> 
            <mo>−</mo> 
            <mi>b </mi> 
            <mo>±</mo> 
            <msqrt> 
               <mrow> 
                  <msup> 
                     <mi>b</mi> 
                     <mn>2</mn> 
                  </msup> 
                  <mo>−</mo> 
                  <mn>4</mn> 
                  <mi>a</mi> 
                  <mi>c</mi> 
               </mrow> 
            </msqrt> 
         </mrow> 
         <mrow> 
            <mn>2</mn> 
            <mi>a</mi> 
         </mrow> 
      </mfrac> 
   </mrow>
</math>

The remaining problems are related to editing capabilities.

Bugs : 
-Trying to modify a formula will occurate some trouble in your document. For example, try to modifiy the previous formula, by removing 2a from the fraction, this will break your formula.
- MathML content is not updated after an insertion. This only occurs when MathML Token are in a table like in the Mozilla MathML Torture Test.
- Cursor disappears in <mi> token when the identifier name has only one character. If the name of the identifier is longer than 1 character, the cursor appears normaly. This not occurs in <mn> and <mo>.
- In <mo>, cursor disappears too.
- Bad selection logic. Some parts of the formula disappear and selection should be only one rectangle not many rectangles.


Enhancements : A common user can not easily and directly use MathML to insert a mathematic formula in HTMLEditor.
-  MathML language is too verbose, an interface like AsciiMath for editing would be appreciate.
- Correct caret deplacements. Without other specifications caret uses the default implementation. For example : in a fraction, when the caret is at the end of the numerator, if we press right arrow, the caret goes to the beginning of the denominator. But its right position should be rather immediately after the fraction.


Each test could be made with the previous MathML code.