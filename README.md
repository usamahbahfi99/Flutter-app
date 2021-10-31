Create the app
  1. Invoke View > Command Palette.
  2. Type “flutter”, and select the Flutter: New Project.
  3. Select Application.
  4. Create or select the parent directory for the new project folder.
  5. Enter a project name, such as myapp, and press Enter.
  6. Wait for project creation to complete and the main.dart file to appear.
The above commands create a Flutter project directory called myapp that contains a simple demo app that uses Material Components.

Run the app
  1. Locate the VS Code status bar (the blue bar at the bottom of the window)
  2. Select a device from the Device Selector area. For details, see Quickly switching between Flutter devices..
  3. Invoke Run > Start Debugging or press F5.
  4. Wait for the app to launch — progress is printed in the Debug Console view.
After the app build completes, you’ll see the starter app on your device.

Try hot reload

Flutter offers a fast development cycle with Stateful Hot Reload, the ability to reload the code of a live running app without restarting or losing app state. Make a change to app source, tell your IDE or command-line tool that you want to hot reload, and see the change in your simulator, emulator, or device.
  1. Open lib/main.dart.
  2. Change the string
      'You have pushed the button this many times'
      to
      'You have clicked the button this many times'
  3. Save your changes: invoke Save All, or click Hot Reload lightning bolt .
You’ll see the updated string in the running app almost immediately.

Step 1: Create the starter Flutter app
Create a simple, templated Flutter app, using the instructions in Getting Started with your first Flutter app. Name the project startup_namer (instead of flutter_app).
You’ll mostly edit lib/main.dart, where the Dart code lives.
  1. Replace the contents of lib/main.dart.
     Delete all of the code from lib/main.dart. Replace with the following code, which displays “Hello World” in the center of the screen.
      // Copyright 2018 The Flutter team. All rights reserved.
      // Use of this source code is governed by a BSD-style license that can be
      // found in the LICENSE file.

      import 'package:flutter/material.dart';

      void main() => runApp(MyApp());

      class MyApp extends StatelessWidget {
        @override
        Widget build(BuildContext context) {
          return MaterialApp(
            title: 'Welcome to Flutter',
            home: Scaffold(
              appBar: AppBar(
                title: const Text('Welcome to Flutter'),
              ),
              body: const Center(
                child: Text('Hello World'),
              ),
            ),
          );
        }
      }
  2. Run the app in the way your IDE describes. You should see either Android, iOS, or web output, depending on your device.

Step 2: Use an external package
In this step, you’ll start using an open-source package named english_words, which contains a few thousand of the most used English words plus some utility functions.
You can find the english_words package, as well as many other open source packages, on pub.dev.
  1. The pubspec.yaml file manages the assets and dependencies for a Flutter app. In pubspec.yaml, add english_words (3.1.5 or higher) to the dependencies list:
      8    dependencies:
      9	     flutter:
      10	     sdk: flutter
      11	   cupertino_icons: ^1.0.3
      12+    english_words: ^4.0.0
  2. In lib/main.dart, import the new package:
      import 'package:english_words/english_words.dart';
      import 'package:flutter/material.dart';
  3. Use the English words package to generate the text instead of using the string “Hello World”:
      10  class MyApp extends StatelessWidget {
      11	    @override
      12	    Widget build(BuildContext context) {
      13	+     final wordPair = WordPair.random();
      14	      return MaterialApp(
      15	        title: 'Welcome to Flutter',
      16	        home: Scaffold(
      17	          appBar: AppBar(
      18	            title: const Text('Welcome to Flutter'),
      19	          ),
      18	-         body: const Center(
      19	-           child: Text('Hello World'),
      20	+         body: Center(
      21	+           child: Text(wordPair.asPascalCase),
      22	          ),
      23	        ),
      24	      );
  5. If the app is running, hot reload to update the running app. Each time you click hot reload, or save the project, you should see a different word pair, chosen at random, in      the running app. This is because the word pairing is generated inside the build method, which is run each time the MaterialApp requires rendering, or when toggling the          Platform in Flutter Inspector.
 
Step 3: Add a Stateful widget
Stateless widgets are immutable, meaning that their properties can’t change—all values are final.
Stateful widgets maintain state that might change during the lifetime of the widget. Implementing a stateful widget requires at least two classes: 1) a StatefulWidget class that creates an instance of 2) a State class. The StatefulWidget class is, itself, immutable and can be thrown away and regenerated, but the State class persists over the lifetime of the widget.
In this step, you’ll add a stateful widget, RandomWords, which creates its State class, _RandomWordsState. You’ll then use RandomWords as a child inside the existing MyApp stateless widget.
  1. Create the boilerplate code for a stateful widget.
     In lib/main.dart, position your cursor after all of the code, enter Return a couple times to start on a fresh line. In your IDE, start typing stful. The editor asks if you      want to create a Stateful widget. Press Return to accept. The boilerplate code for two classes appears, and the cursor is positioned for you to enter the name of your            stateful widget.
  2. Enter RandomWords as the name of your widget.
     The RandomWords widget does little else beside creating its State class.
     Once you’ve entered RandomWords as the name of the stateful widget, the IDE automatically updates the accompanying State class, naming it _RandomWordsState. By default, the      name of the State class is prefixed with an underbar. Prefixing an identifier with an underscore enforces privacy in the Dart language and is a recommended best practice        for State objects.
     The IDE also automatically updates the state class to extend State<RandomWords>, indicating that you’re using a generic State class specialized for use with RandomWords.        Most of the app’s logic resides here—it maintains the state for the RandomWords widget. This class saves the list of generated word pairs, which grows infinitely as the          user scrolls and, in part 2 of this lab, favorites word pairs as the user adds or removes them from the list by toggling the heart icon.
     Both classes now look as follows:
      class RandomWords extends StatefulWidget {
        @override
        _RandomWordsState createState() => _RandomWordsState();
        }

        class _RandomWordsState extends State<RandomWords> {
        @override
        Widget build(BuildContext context) {
          return Container();
          }
        }
  3. Update the build() method in _RandomWordsState:
      class _RandomWordsState extends State<RandomWords> {
          @override
          Widget build(BuildContext context) {
            final wordPair = WordPair.random();
            return Text(wordPair.asPascalCase);
          }
        }
  4. Remove the word generation code from MyApp by making the changes shown in the following diff:
      10  class MyApp extends StatelessWidget {
      11	    @override
      12	    Widget build(BuildContext context) {
            -     final wordPair = WordPair.random();
      13	      return MaterialApp(
      14	        title: 'Welcome to Flutter',
      15	        home: Scaffold(
      16  @@ -18,8 +17,8 @@
      17	            title: const Text('Welcome to Flutter'),
      18	          ),
      19	          body: Center(
            -           child: Text(wordPair.asPascalCase),
      20	  +           child: RandomWords(),
      21	          ),
      22	        ),
      23	      );
      24	    }
  5. Restart the app. The app should behave as before, displaying a word pairing each time you hot reload or save the app.
  
Step 4: Create an infinite scrolling ListView
In this step, you’ll expand _RandomWordsState to generate and display a list of word pairings. As the user scrolls the list (displayed in a ListView widget) grows infinitely. ListView’s builder factory constructor allows you to build a list view lazily, on demand.
  1. Add a _suggestions list to the _RandomWordsState class for saving suggested word pairings. Also, add a _biggerFont variable for making the font size larger.
      class _RandomWordsState extends State<RandomWords> {
        final _suggestions = <WordPair>[];
        final _biggerFont = const TextStyle(fontSize: 18.0);
        // ···
      }
     Next, you’ll add a _buildSuggestions() function to the _RandomWordsState class. This method builds the ListView that displays the suggested word pairing.
     The ListView class provides a builder property, itemBuilder, that’s a factory builder and callback function specified as an anonymous function. Two parameters are passed to      the function—the BuildContext, and the row iterator, i. The iterator begins at 0 and increments each time the function is called. It increments twice for every suggested        word pairing: once for the ListTile, and once for the Divider. This model allows the suggested list to continue growing as the user scrolls.
  2. Add a _buildSuggestions() function to the _RandomWordsState class:
      Widget _buildSuggestions() {
        return ListView.builder(
            padding: const EdgeInsets.all(16.0),
            itemBuilder: /*1*/ (context, i) {
              if (i.isOdd) return const Divider(); /*2*/

              final index = i ~/ 2; /*3*/
              if (index >= _suggestions.length) {
                _suggestions.addAll(generateWordPairs().take(10)); /*4*/
              }
              return _buildRow(_suggestions[index]);
            });
      }
  3. Add a _buildRow() function to _RandomWordsState:
      Widget _buildRow(WordPair pair) {
        return ListTile(
          title: Text(
            pair.asPascalCase,
            style: _biggerFont,
          ),
        );
      }
  4. In the _RandomWordsState class, update the build() method to use _buildSuggestions(), rather than directly calling the word generation library. (Scaffold implements the          basic Material Design visual layout.) Replace the method body with the highlighted code:
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(
             title: const Text('Startup Name Generator'),
           ),
           body: _buildSuggestions(),
         );
       }
  5. In the MyApp class, update the build() method by changing the title, and changing the home to be a RandomWords widget:
      10.  class MyApp extends StatelessWidget {
      11	    @override
      12	    Widget build(BuildContext context) {
      13	      return MaterialApp(
          -       title: 'Welcome to Flutter',
          -       home: Scaffold(
          -         appBar: AppBar(
          -           title: const Text('Welcome to Flutter'),
          -         ),
          -         body: Center(
          -           child: RandomWords(),
          -         ),
          -       ),
      14	+       title: 'Startup Name Generator',
      15	+       home: RandomWords(),
      16	      );
      17	    }
  6. Restart the app. You should see a list of word pairings no matter how far you scroll.
