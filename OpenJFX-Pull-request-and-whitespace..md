The white-space checker that is run as part of the Travis CI build on Linux and Mac will list the files that are in error, along with the type of error, near the bottom of the log. It won't tell you which lines have the problem. Here is the output from the above Travis CI run on Linux:

`--- List of added or modified files`

`modules/javafx.base/src/main/java/com/sun/javafx/logging/PrintLogger.java`
`modules/javafx.base/src/main/java/com/sun/javafx/logging/PulseLogger.java`
`modules/javafx.base/src/main/java/com/sun/javafx/logging/jfr/JFRInputEvent.java`
`modules/javafx.base/src/main/java/com/sun/javafx/logging/jfr/JFRPulseEvent.java`
`modules/javafx.base/src/main/java/com/sun/javafx/logging/jfr/JFRPulseLogger.java`
`modules/javafx.base/src/main/java/com/sun/javafx/logging/jfr/PulseId.java`
`modules/javafx.base/src/main/java/module-info.java`

`--- Checking for white space errors`

`modules/javafx.base/src/main/java/com/sun/javafx/logging/PulseLogger.java :trailingWhitespace:`
`modules/javafx.base/src/main/java/com/sun/javafx/logging/jfr/JFRPulseLogger.java :trailingWhitespace:`

`Found 2 whitespace or executable issues`

`To correct, use`
   `bash tools/scripts/checkWhiteSpace -F -x`

`Error in white space check`

To fix them, you can either do it manually (e.g., using your IDE to remove trailing white-spaces), or you can run:

`cat files.list | bash tools/scripts/checkWhiteSpace -S -F -x`

where files.list has the names of files you want to process (the two file names with the issue)