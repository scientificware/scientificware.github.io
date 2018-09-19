The following code can help you reach your quest.
If you only want to display MathML you can simply use a WebView control.

If you want to edit it ;-) ... you can use a HTMLEditor control.
Beware, MathML editing doesn't work properly.
The place where I discuss this : https://github.com/javafxports/openjdk-jfx/issues/118.

If you have Java, C++ or JavaScript skills then you can try to improve it, too.
If you are interested in building a community around that, then I'm your Guy.

JavaFX/OpenJFX has been supporting MathML since 2011. But a small bug broke it.

This issue was of low priority to the maintainers until 2018 because of very little user interest or feedback. However, thanks new Oracle roadmap about JavaFX, OpenJFX team gave me some help and a review assistance to integrate my patch (https://github.com/javafxports/openjdk-jfx/pull/117)

Now, it seems that Oracle will give JavaFX to Gluon (https://gluonhq.com/), so I encourage you to give them feedback to show your interest in MathML support or Math editing and preserve MathML support.

[![enter image description here][2]][2]

    import javafx.application.Application;
    import javafx.scene.Scene;
    import javafx.stage.Stage;
    import javafx.scene.layout.VBox;
    import javafx.scene.web.WebView;
    
    public class TestRM extends Application {
    
        private Stage window;
        private Scene scene;
        
        
        private WebView webView = new WebView();
        
        String javaversion = "Version de Java.. : " + System.getProperty("java.runtime.version");
        String javafxversion = "Version de JavaFX : " + System.getProperty("javafx.runtime.version");
        String osinfo = "OS .............. : " + System.getProperty("os.name");
        String osarch = "CPU ............. : " + System.getProperty("os.arch");
        String agent = "User Agent ...... : " + webView.getEngine().getUserAgent();
        
        private String string2 = new String(""
                + "<p>"
                + "<math xmlns=\"http://www.w3.or/1998/Math/MathML\" display=\"block\">"
                + "   <semantics>"
                + "      <mfrac>"
                + "         <mrow>"
                + "            <mo>-</mo>"
                + "            <mi>b</mi>"
                + "            <mo>&pm;</mo>"
                + "            <msqrt>"
                + "               <mrow>"
                + "                  <msup>"
                + "                     <mi>b</mi>"
                + "                     <mn>2</mn>"
                + "                  </msup>"
                + "                  <mo>-</mo>"
                + "                  <mn>4</mn>"
                + "                  <mi>a</mi>"
                + "                  <mi>c</mi>"
                + "               </mrow>"
                + "            </msqrt>"
                + "         </mrow>"
                + "         <mrow>"
                + "            <mn>2</mn>"
                + "            <mi>a</mi>"
                + "         </mrow>"
                + "      </mfrac>"
                + "      <annotation encoding=\"SnuggleTeX\">$$ \\frac{-b \\pm \\sqrt{b^2-4ac}}{2a} $$</annotation>"
                + "   </semantics>"
                + "</math>"
                + "</p>"
                + "<p>" + javafxversion + "</p>"
                + "<p>" + javaversion + "</p>"
                + "<p>" + osinfo + "</p>"
                + "<p>" + osarch + "</p>"
                + "<p>" + agent + "</p>");
        
        VBox vbox = new VBox(webView);
    
        public static void main(String[] args) {
            launch(args);
        }
    
        public void start(Stage primaryStage) {
            webView.getEngine().loadContent(string2);
            window = primaryStage;
            scene = new Scene(vbox, 400, 300);
            window.setScene(scene);
            window.show();
        }
    }

As I said before you can use the JavaFX `HTMLEditor`, but in fact your TestRun is already a 'MathMLeditor'. In the previous code, simply add `contenteditable="true"` to the element you want to edit : I add it to the paragraph which contains your formula, but you can add it to the body element itself, I let you try ...

`HTMLEditor` is based on `WebView`, it only adds a toolbar to `WebView`. This works with many web browsers and WebView is "just" a WebKit port.

    import javafx.application.Application;
    import javafx.scene.Scene;
    import javafx.stage.Stage;
    import javafx.scene.layout.VBox;
    import javafx.scene.web.WebView;
    
    public class TestRM extends Application {
    
        private Stage window;
        private Scene scene;
        
        
        private WebView webView = new WebView();
        
        String javaversion = "Version de Java.. : " + System.getProperty("java.runtime.version");
        String javafxversion = "Version de JavaFX : " + System.getProperty("javafx.runtime.version");
        String osinfo = "OS .............. : " + System.getProperty("os.name");
        String osarch = "CPU ............. : " + System.getProperty("os.arch");
        String agent = "User Agent ...... : " + webView.getEngine().getUserAgent();
        
        private String string2 = new String(""
                + "<html>"
                + "<head>"
                + "<title>TestRM</title>"
                + "</head>"
                + "<body>"
                + "<p contenteditable=\"true\">"
                + "<math xmlns=\"http://www.w3.or/1998/Math/MathML\" display=\"block\">"
                + "   <semantics>"
                + "      <mfrac>"
                + "         <mrow>"
                + "            <mo>-</mo>"
                + "            <mi>b</mi>"
                + "            <mo>&pm;</mo>"
                + "            <msqrt>"
                + "               <mrow>"
                + "                  <msup>"
                + "                     <mi>b</mi>"
                + "                     <mn>2</mn>"
                + "                  </msup>"
                + "                  <mo>-</mo>"
                + "                  <mn>4</mn>"
                + "                  <mi>a</mi>"
                + "                  <mi>c</mi>"
                + "               </mrow>"
                + "            </msqrt>"
                + "         </mrow>"
                + "         <mrow>"
                + "            <mn>2</mn>"
                + "            <mi>a</mi>"
                + "         </mrow>"
                + "      </mfrac>"
                + "      <annotation encoding=\"SnuggleTeX\">$$ \\frac{-b \\pm \\sqrt{b^2-4ac}}{2a} $$</annotation>"
                + "   </semantics>"
                + "</math>"
                + "</p>"
                + "<p>" + javafxversion + "</p>"
                + "<p>" + javaversion + "</p>"
                + "<p>" + osinfo + "</p>"
                + "<p>" + osarch + "</p>"
                + "<p>" + agent + "</p>")
                + "</body>"
                + "</html> ";
        
        VBox vbox = new VBox(webView);
    
        public static void main(String[] args) {
            launch(args);
        }
    
        public void start(Stage primaryStage) {
            webView.getEngine().loadContent(string2);
            window = primaryStage;
            scene = new Scene(vbox, 400, 300);
            window.setScene(scene);
            window.show();
        }
    }

After few minutes you will see that there are some problems with MathML editing.

- How insert a formula from scratch ?
- How modify it properly ?
- Why cursor disappears !
- ...

I listed all problems, follow the link I posted above about issue #118.

  [1]: https://i.stack.imgur.com/NHU1J.png
  [2]: https://i.stack.imgur.com/6DYBV.png