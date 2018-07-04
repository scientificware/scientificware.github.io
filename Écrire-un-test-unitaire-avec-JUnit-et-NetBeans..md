# Les outils :

## [JUnit](https://junit.org/) :

Unlike previous versions of JUnit, JUnit 5 is composed of several different modules from three different sub-projects.

JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage

The JUnit Platform serves as a foundation for launching testing frameworks on the JVM. It also defines the TestEngine API for developing a testing framework that runs on the platform. Furthermore, the platform provides a Console Launcher to launch the platform from the command line and build plugins for Gradle and Maven as well as a JUnit 4 based Runner for running any TestEngine on the platform.

JUnit Jupiter is the combination of the new programming model and extension model for writing tests and extensions in JUnit 5. The Jupiter sub-project provides a TestEngine for running Jupiter based tests on the platform.

JUnit Vintage provides a TestEngine for running JUnit 3 and JUnit 4 based tests on the platform.

Exemple de test unitaire que j'ai écrit pour Issue#71, suivi des résultats :

```
/*
 * Copyright (c) 2017, 2018, Oracle and/or its affiliates. All rights reserved.
 * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
 *
 * This code is free software; you can redistribute it and/or modify it
 * under the terms of the GNU General Public License version 2 only, as
 * published by the Free Software Foundation.  Oracle designates this
 * particular file as subject to the "Classpath" exception as provided
 * by Oracle in the LICENSE file that accompanied this code.
 *
 * This code is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
 * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
 * version 2 for more details (a copy is included in the LICENSE file that
 * accompanied this code).
 *
 * You should have received a copy of the GNU General Public License version
 * 2 along with this work; if not, write to the Free Software Foundation,
 * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
 *
 * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
 * or visit www.oracle.com if you need additional information or have any
 * questions.
 */

import java.util.concurrent.atomic.AtomicBoolean;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.TimeUnit;

import javafx.application.Application;
import javafx.application.Platform;
import javafx.scene.Scene;
import javafx.scene.layout.StackPane;
import javafx.scene.web.HTMLEditor;
import javafx.scene.web.WebEngine;
import javafx.scene.web.WebView;
import javafx.stage.Stage;

import org.junit.AfterClass;
import org.junit.BeforeClass;
import org.junit.Test;
import org.junit.Assert;

import static junit.framework.TestCase.fail;

import static test.util.Util.TIMEOUT;

public class MathMLRenderTest {

    private static final CountDownLatch launchLatch = new CountDownLatch(1);

    // Document test
    static String BodyContent
            = "<math display=\"block\">"
            + "   <mrow>"
            + "      <mi>x</mi>"
            + "      <mo>=</mo>"
            + "      <mfrac>"
            + "         <mrow>"
            + "            <mo>−</mo>"
            + "            <mi>b</mi>"
            + "            <mo>±</mo>"
            + "            <msqrt>"
            + "               <mrow>"
            + "                  <msup>"
            + "                     <mi>b</mi>"
            + "                     <mn>2</mn>"
            + "                  </msup>"
            + "                  <mo>−</mo>"
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
            + "   </mrow>"
            + "</math>";

    static String content = "<!doctype html>"
            + "<html>"
            + "   <head>"
            + "      <meta charset=\"UTF-8\">"
            + "      <title>OpenJFX and MathML</title>"
            + "   </head>"
            + "   <body>"
            + "      <p>"
            + BodyContent
            + "      </p>"
            + "   </body>"
            + "</html>";

    // Application instance
    static TestApp testApp;

    public static class TestApp extends Application {

        final HTMLEditor htmlEditor = new HTMLEditor();
        final StackPane root = new StackPane();
        final WebView webView = (WebView) htmlEditor.lookup(".web-view");

        public void init() {
            MathMLRenderTest.testApp = this;
            htmlEditor.setHtmlText(content);
            root.getChildren().add(htmlEditor);
            htmlEditor.setPrefWidth(400);
            htmlEditor.setPrefHeight(150);
        }

        @Override
        public void start(Stage primaryStage) {
            primaryStage.setTitle("OpenJFX MathML Rendering Issue Test");
            primaryStage.setScene(new Scene(root));
            primaryStage.show();
            launchLatch.countDown();
        }

        // Get height of the first token element <mo>
        public int getTokenHeight(){
            WebEngine we = webView.getEngine();
            int height = (int) we.executeScript(
                "elements = document.getElementsByTagName('mo');"
                + "element = elements[0].clientHeight;"
            );
            return height;
        }
    }

    @BeforeClass
    public static void setupOnce() {

        // Start the Test Application
        new Thread(() -> Application.launch(TestApp.class,
            (String[]) null)).start();

        try {
            if (!launchLatch.await(TIMEOUT, TimeUnit.MILLISECONDS)) {
                fail("Timeout waiting for FX runtime to start");
            }
        } catch (InterruptedException exception) {
            fail("Unexpected exception: " + exception);
        }    
    }

    @AfterClass
    public static void tearDownOnce() {
        Platform.exit();
    }

    /**
     * @test
     * @bug JDK-8147476 Rendering issues with MathML token elements
     */
    @Test
    public void testgetTokenHeight() throws Exception {

        final CountDownLatch editorStateLatch = new CountDownLatch(1);
        final AtomicBoolean rightRender = new AtomicBoolean(false);
        final AtomicInteger tokenHeight = new AtomicInteger(0);
        Platform.runLater(() -> {
            tokenHeight.set(testApp.getTokenHeight());
            rightRender.set(!(tokenHeight.get()<2));
            editorStateLatch.countDown();
        });

        try {
            editorStateLatch.await(TIMEOUT, TimeUnit.MILLISECONDS);
        } catch (InterruptedException ex) {
            throw new AssertionError(ex);
        } finally {
            Assert.assertTrue("Check MathML token height : " + tokenHeight.get() + " is much smaller than the expected size." , rightRender.get());
        }
    }
}
```
[OpenJFX unit tests](https://wiki.openjdk.java.net/display/OpenJFX/OpenJFX+unit+tests)

[Building OpenJFX and testing](https://wiki.openjdk.java.net/display/OpenJFX/Building+OpenJFX#BuildingOpenJFX-Testing)

The junit test is ready to be tested

gradle -PFULL_TEST=true :systemTests:test --tests MathMLRenderTest

# Test Succeeded !
- WebCore with the patch.

![screenshot_20180621_010700](https://user-images.githubusercontent.com/19194678/41689233-f623777a-74ef-11e8-8fa0-32f9aba8b492.png)

- WebCore without the patch.
![screenshot_20180621_005400](https://user-images.githubusercontent.com/19194678/41688822-ef072844-74ed-11e8-9623-b7c3a7b0e29e.png)

- A Headless test sample using testBase in `modules/javafx.web/src/test/java/test/javafx/scene/web` :
```
/*
 * Copyright (c) 2018, Oracle and/or its affiliates. All rights reserved.
 * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
 *
 * This code is free software; you can redistribute it and/or modify it
 * under the terms of the GNU General Public License version 2 only, as
 * published by the Free Software Foundation.  Oracle designates this
 * particular file as subject to the "Classpath" exception as provided
 * by Oracle in the LICENSE file that accompanied this code.
 *
 * This code is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
 * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
 * version 2 for more details (a copy is included in the LICENSE file that
 * accompanied this code).
 *
 * You should have received a copy of the GNU General Public License version
 * 2 along with this work; if not, write to the Free Software Foundation,
 * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
 *
 * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
 * or visit www.oracle.com if you need additional information or have any
 * questions.
 */

package test.javafx.scene.web;

import static org.junit.Assert.assertTrue;
import org.junit.Test;

public class MathMLRenderTest extends TestBase {

    @Test public void testTokenHeight() throws Exception {

        // Document test
        String mathmlContent
            = "<math display=\"block\">"
            + "   <mrow>"
            + "      <mi>x</mi>"
            + "      <mo>=</mo>"
            + "      <mfrac>"
            + "         <mrow>"
            + "            <mo>−</mo>"
            + "            <mi>b</mi>"
            + "            <mo>±</mo>"
            + "            <msqrt>"
            + "               <mrow>"
            + "                  <msup>"
            + "                     <mi>b</mi>"
            + "                     <mn>2</mn>"
            + "                  </msup>"
            + "                  <mo>−</mo>"
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
            + "   </mrow>"
            + "</math>";

        String htmlBody = "<!doctype html>"
            + "<html>"
            + "   <body>"
            + "      <p>"
            + mathmlContent
            + "      </p>"
            + "   </body>"
            + "</html>";

        loadContent(htmlBody);

        int height = (int) executeScript(
            "elements = document.getElementsByTagName('mo');"
            + "element = elements[0].clientHeight;"
        );

        assertTrue("MathML token height must be greater than " + height, height > 1);
    }
}
```

- A smaller version :+1: 
```
/*
 * Copyright (c) 2018, Oracle and/or its affiliates. All rights reserved.
 * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
 *
 * This code is free software; you can redistribute it and/or modify it
 * under the terms of the GNU General Public License version 2 only, as
 * published by the Free Software Foundation.  Oracle designates this
 * particular file as subject to the "Classpath" exception as provided
 * by Oracle in the LICENSE file that accompanied this code.
 *
 * This code is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
 * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
 * version 2 for more details (a copy is included in the LICENSE file that
 * accompanied this code).
 *
 * You should have received a copy of the GNU General Public License version
 * 2 along with this work; if not, write to the Free Software Foundation,
 * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
 *
 * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
 * or visit www.oracle.com if you need additional information or have any
 * questions.
 */

package test.javafx.scene.web;

import static org.junit.Assert.assertTrue;
import org.junit.Test;

public class MathMLRenderTest extends TestBase {

    @Test public void testTokenHeight() throws Exception {
        loadContent("<!doctype html><html><body><math><mo>=</mo></math></body></html>");
        int height = (int) executeScript("document.getElementsByTagName('mo')[0].clientHeight");
        assertTrue("MathML token height is lesser than expected " + height, height > 1);
    }
}
```

## [NetBeans](https://netbeans.apache.org)

 NetBeans provides :
- an IDE out-of-the-box code analyzers and editors for working with the latest Java 8 technologies--Java SE, Java SE Embedded, and Java ME Embedded. The IDE also has a range of new tools for HTML5/JavaScript, in particular for Node.js, KnockoutJS, and AngularJS; enhancements that further improve its support for Maven and Java EE with PrimeFaces; and improvements to PHP and C/C++ support.
- a platform i.e. a generic framework for Swing applications. It provides the "plumbing" that, before, every developer had to write themselves—saving state, connecting actions to menu items, toolbar items and keyboard shortcuts; window management, and so on.

# La procédure :

## Première étape : 
Le programme à tester.

## Deuxième étape :
Utilisation de NetBeans pour créer les tests unitaires.