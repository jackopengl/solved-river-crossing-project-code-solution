Download Link: https://assignmentchef.com/product/solved-river-crossing-project-code-solution
<br>
<h2><strong>Overview</strong></h2>

<p style="font-weight: 400;">In this project, we will redesign an existing RiverCrossing application. The application includes a test, which currently passes. The application also includes a GUI component, which does not work correctly with the existing game engine. It would be nice if we were able to implement different games like those found here:

<p style="font-weight: 400;"><strong><a href="https://www.transum.org/software/River_Crossing/" data-saferedirecturl="https://www.google.com/url?q=https://www.transum.org/software/River_Crossing/&amp;source=gmail&amp;ust=1550714747663000&amp;usg=AFQjCNFp5pPi9PxRa5Q82wCE9LyR5NjTnQ">https://www.transum.org/software/River_Crossing/ (Links to an external site.)Links to an external site.</a></strong>

<p style="font-weight: 400;">In order to do this, we have to tackle a few problems.

<ul>

 <li style="font-weight: 400;">First, although the code works for a specific case (it passes the test case), the code is not in very good shape. There is a lot of duplicate code in the game-engine class and there is some unnecessary code in the game-object class. The GUI is also in need of some serious refactoring.</li>

 <li style="font-weight: 400;">Second, the code (game-engine) does not work properly with the GUI. We need to get those two components to communicate with one another.</li>

 <li style="font-weight: 400;">Third, how do we generalize the code so that we can not only handle the farmer, goose, and beans, but also handle the other scenarios, like the big and small robots, and the monsters and munchkins?</li>

</ul>

<h2><strong>Part 1 – Get Familiar with the Current Code</strong></h2>

<h3><strong>Getting the GitHub Project</strong></h3>

<p style="font-weight: 400;">Go to the <strong><a href="https://github.com/gregwk/RiverCrossing" data-saferedirecturl="https://www.google.com/url?q=https://github.com/gregwk/RiverCrossing&amp;source=gmail&amp;ust=1550714747663000&amp;usg=AFQjCNHhVTiSDaa13CuvsOa6M2SEzaZURQ">RiverCrossing project (Links to an external site.)Links to an external site.</a></strong> on my GitHub site and download the ZIP file of the current code. Move it to the workspace of your favorite IDE and create a project using that code. For example, in Eclipse you just create a new Java project and give it the name “RiverCrossing” and as long as the RiverCrossing folder is in your workspace, Eclipse will detect that and use it for the project.

<p style="font-weight: 400;">Once you have the files working in your IDE, do the following (note: you do not need to turn this in):

<ul>

 <li style="font-weight: 400;">Sketch out a class diagram for the application that includes all the classes in the “river” package and the relationships between them.</li>

 <li style="font-weight: 400;">Look at the game-engine test class and try to understand how the engine gets to a winning state in the test-winning-game test case.</li>

 <li style="font-weight: 400;">Look at Wikipedia’s entry on <strong><a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller" data-saferedirecturl="https://www.google.com/url?q=https://en.wikipedia.org/wiki/Model%25E2%2580%2593view%25E2%2580%2593controller&amp;source=gmail&amp;ust=1550714747663000&amp;usg=AFQjCNG7wY9hXtxHJEcuIHBfi-_4xZRscw">Model-View-Controller (Links to an external site.)Links to an external site.</a></strong>. Can you identify each of the 3 components in the current application?</li>

</ul>

<p style="font-weight: 400;">Try to answer the following questions about the project and how we need to redesign it:

<ul>

 <li style="font-weight: 400;">What is the relationship between the farmer’s location and the current location? How does the farmer’s location differ from other game object locations?</li>

 <li style="font-weight: 400;">How can we get the GUI to communicate better with the game engine? Why doesn’t the GUI work with the game engine?</li>

 <li style="font-weight: 400;">What do we need to do if we want to create 3 different scenarios like they did in the web link above?</li>

 <li style="font-weight: 400;">What are the implications of creating multiple scenarios on game-object and its subclasses?</li>

</ul>

<h2><strong>Part 2 – Refactor the Code</strong></h2>

<h4><strong>Refactor GameObject</strong></h4>

<ul>

 <li style="font-weight: 400;">Get rid of the GameObject subclasses – we don’t want to limit ourselves to the farmer-oriented river-crossing game. Therefore, it doesn’t make much sense to have specific subclasses devoted to these objects. Furthermore, the subclasses don’t serve a lot of purpose – they are essentially just there to initialize fields in the game-object.</li>

 <li style="font-weight: 400;">Change the getter method “getSound” in GameObject so that it is based on a field, just like the “getName” and “getLocation” getter methods.</li>

 <li style="font-weight: 400;">We are not going to modify the name field of a game object after it is created. Therefore, get rid of the setter method for it. Remember to check that there are no references to a method (or field or constructor) before deleting it.</li>

 <li style="font-weight: 400;">Clean up the class. Get rid of any unused constructors. Make all fields private. Make the fields that are never assigned to final.</li>

</ul>

<h4><strong>Refactor GameEngine</strong></h4>

<ul>

 <li style="font-weight: 400;">Put the Location class into its own file. Note that there is an Eclipse refactoring that will do this for you.</li>

 <li style="font-weight: 400;">Change all variables named top, mid, bottom, and player to wolf, goose, beans, and farmer. This includes the fields in GameEngine and the constants in Item. Make sure you keep the proper style (for example, constants are always capitalized).</li>

 <li style="font-weight: 400;">Declare a instance variable of type Map that maps Items to GameObjects. Initialize the map in the constructor using a HashMap or an EnumMap (your choice) and put the four game-objects in it. Use your map to simplify the methods getName, getLocation, getItem, and any other methods that can be simplified.</li>

 <li style="font-weight: 400;">Once you have created the map and simplified the code, you should no longer need the fields wolf, goose, beans, and farmer (since you can easily access them using the map). Make sure there are no references to them and then delete them. Note that you will still have to create appropriate game objects in the constructor so that you can add them to the map, but fix things so that you won’t need the fields anymore.</li>

 <li style="font-weight: 400;">Change the method names getName, getLocation, and getSound to getItemName, getItemLocation, and getItemSound. This will make it clear that these methods just call through to the game objects getters.</li>

 <li style="font-weight: 400;">The “current location” is basically just the boat location. Rename the field and the getter to use boat-location rather than current-location.</li>

 <li style="font-weight: 400;">Once we extract the GameEngine interface in Part 6, the passenger field will no longer be needed, so feel free to skip this refactoring.</li>

 <li style="font-weight: 400;">Clean up the class. Make sure all fields and helper methods have private access. And make sure all fields that can be declared final <em>are</em> declared final.</li>

</ul>

<h4><strong>Refactor GameEngineTest</strong></h4>

<ul>

 <li style="font-weight: 400;">The class under test is GameEngine, so declare a private field “engine” of type GameEngine and initialize it in the @Before method (setUp). Use it to replace the local instances of the game engine.</li>

 <li style="font-weight: 400;">Rename the testObject method to testObjectCallThroughs. Instead of creating game-objects, test the call through methods in the game-engine class. For example, instead of testing whether farmer.getName is “Farmer”, test whether engine.getItemName(Item.FARMER) is “Farmer”.</li>

 <li style="font-weight: 400;">Write a helper method called transport that takes an item. The method should transport the item from one side of the river to the other. Use it to simplify some of the test cases in which an item is transported.</li>

 <li style="font-weight: 400;">Write a helper method called goBackAlone that just calls engine.rowBoat, and use it whenever the farmer is going back alone in the test cases.</li>

</ul>

<p style="font-weight: 400;">Remember that through all of the above refactorings, you are only changing the design of the code, not its functionality. Therefore, your test cases should continue to pass after each refactoring and — if you do them carefully — during each step of your refactoring.

<h2><strong>Part 3 – Modify the Functionality</strong></h2>

<p style="font-weight: 400;">In the original GameEngine class, the farmer’s location is never “BOAT”. This is not what the GUI class expects. We’re going to enhance the GameEngine class so that the farmer is explicitly loaded onto the boat. But before we do this, we’re going to make some changes to GameEngineTest so that we still pass the tests.

<ul>

 <li style="font-weight: 400;">Modify the test class by explicitly loading and unloading the farmer in methods “transport” and “goBackAlone”.</li>

</ul>

<p style="font-weight: 400;">Recall the helper methods for the test class that you created in Part 2. These methods transport the farmer and (possibly) a passenger to the other side of the river. Make sure both methods explicitly load the farmer into the boat before rowing the boat, and make sure both methods unload the boat after rowing the boat (the transport method already will). Run the tests. Notice that the test cases still pass! That’s because currently, calling the loadBoat method with the farmer item does nothing, and unloading the boat when there is no passenger also does nothing.

<p style="font-weight: 400;">Now let’s update the game-engine class.

<ul>

 <li style="font-weight: 400;">Change loadBoat so that if called with the farmer, the farmer is loaded onto the boat.</li>

 <li style="font-weight: 400;">Change the implementation of rowBoat, so that the farmer stays on the boat.</li>

 <li style="font-weight: 400;">Change the implementation of unloadBoat so that the farmer is unloaded from the boat.</li>

</ul>

<p style="font-weight: 400;">Run the tests, they should still work because of the modifications you made. Now try running the application, it should work also. However, notice that (1) the application does not tell you if you win or lose, it just lets you keep playing, and (2) the application unloads the farmer and the passenger at the same time. We’re not going to worry about (1) yet, but we do need to fix (2). We want to unload the boat one passenger at a time.

<ul>

 <li style="font-weight: 400;">Overload the unloadBoat method in game-engine with a method by the same name that takes an Item object as a parameter. It should only unload the specified item and keep the other items where they are.</li>

 <li style="font-weight: 400;">Modify your tests so that they use the new unloadBoat method instead of the original. Run your tests to make sure they work.</li>

</ul>

<p style="font-weight: 400;">Finally, once you have added the new unloadBoat method to GameEngine, tweaked the test cases, and made sure they work, you will have to modify the RiverGUI class. Look for calls to unloadBoat() in the RiverGUI class and figure out how to change the code with the help of unloadBoat(Item) in such a way that one item is unloaded at a time when you run the application.

<ul>

 <li style="font-weight: 400;">Modify RiverGUI to use unloadBoat(Item) instead of unloadBoat() so that one item at a time is unloaded when you run the application.</li>

</ul>

<h2><strong>Part 4 – Prepare to Create the GameEngine Interface</strong></h2>

<p style="font-weight: 400;">The refactorings in this section have to do with the fact that we don’t want the GUI to depend on anything that has to do with this particular implementation of the game engine. Recall that the web site we looked at had three different river-crossing puzzles: one with the farmer (like the one we have here), one with robots, and one with monsters. Now take a look at the GUI. We see:

<ul>

 <li style="font-weight: 400;">Lots of direct references to the farmer, wolf, goose, and beans through the use of Item.FARMER, Item.WOLF, etc.</li>

 <li style="font-weight: 400;">Lots of indirect references to farmer, wolf, goose, and beans through the colors we choose for the rectangles and the letters we place in the rectangles.</li>

 <li style="font-weight: 400;">Indirect references to farmer, wolf, goose, and beans through the variables names we choose.</li>

</ul>

<p style="font-weight: 400;">Unfortunately, all of this will have to change. Fortunately, we can do much of this through fairly straightforward refactorings.

<ul>

 <li style="font-weight: 400;">Get rid of references to farmer, wolf, goose, and beans game-objects. The only place these are used anymore are in the constructor and in the gameIsWon and gameIsLost methods. For the constructor, you can use local variables to help you create the game objects, or just inline them. For the gameIsLost/Won methods, replace goose.getLocation() with getItemLocation(Item.GOOSE) and do the same for the other game objects. Run your tests to make sure they work. Check that there are no other references to wolf, goose, beans, and farmer, and then delete those fields.</li>

 <li style="font-weight: 400;">Move the Item class out of GameEngine and refactor the names as follows:</li>

</ul>

<p style="font-weight: 400;">o   BEANS =&gt; ITEM_0

<p style="font-weight: 400;">o   GOOSE =&gt; ITEM_1

<p style="font-weight: 400;">o   WOLF =&gt; ITEM_2

<p style="font-weight: 400;">o   FARMER =&gt; ITEM_3

<ul>

 <li style="font-weight: 400;">After you do this, the constants in the Item class will not be in numerical order. Move them around so that they are – this will become important when refactoring the GUI. In the game-engine class, <strong>extract constants</strong> for Item.ITEM_0, Item.ITEM_1, etc. Give them their former names of BEANS, GOOSE, etc. Do the same for the GameEngineTest. Run you tests and the app to make sure everything still works.</li>

 <li style="font-weight: 400;">Replace properties name and sound in GameObject (which are not used except in tests) with label and color. The label property is a string and the color property is a color. These properties will be used by the GUI to draw the rectangles. In the GameEngine, make the labels and colors the same colors that currently appear in the GUI. Much of this can be done with the renaming of fields and methods, but at some point you are going to have to change a String type to a Color and this will break things. Make sure you have an idea of what it will break before you do it. Then fix those items. Run the test and the GUI again to see if they still work.</li>

</ul>

<p style="font-weight: 400;">As always, tidy up your code and make sure it is well-formatted.

<h2><strong>Part 5 – Modify RiverGUI</strong></h2>

<p style="font-weight: 400;">We’re going to completely redesign RiverGUI so that the following are true:

<ul>

 <li style="font-weight: 400;">There are no signs of beans, goose, wolf, or farmer anywhere!</li>

 <li style="font-weight: 400;">In the View portion of the class, we’re going to map items to their new rectangles, and then we’ll draw the rectangles.</li>

 <li style="font-weight: 400;">In the Controller portion of the class, we’re going to check if a click occurred in an items rectangle, and handle it if it did.</li>

 <li style="font-weight: 400;">We’re going to use offsets and maps so that we can handle different items using the same method.</li>

</ul>

<p style="font-weight: 400;">If you have a different idea for refactoring and updating the RiverGUI *and* you’re very comfortable with Java, let me know on Piazza and I will let you try it. But consider working with the outline I give here since I tested it and I know it works (and I know *how* it works). First, let’s look at some of the fields you are going to need.

<p style="font-weight: 400;">        private final Rectangle leftBoatRectangle = …private final Rectangle rightBoatRectangle = …private final Rectangle baseLeftRectangle = …private final Rectangle baseRightRectangle = …private final int[] dx = { .., .., .., .. };private final int[] dy = { .., .., .., .. };

private GameEngine engine; // Modelprivate Map&lt;Item, Rectangle&gt; rectMap;private Rectangle boatRectangle;

<p style="font-weight: 400;">The left boat rectangle and right boat rectangle have not changed. The base left rectangle is the old left goose rectangle and the base right rectangle is the old right goose rectangle. Note that in the old GUI, the goose was always at the bottom left of the item group, while in this GUI, the beans will always be at the bottom left (we can change this depending on how we implement the game engine). We do not need the other rectangles because we’re going to use offsets. The dx and dy array represents the offsets of the four items. ITEM_0 is the bottom-left item, so it’s offset is 0, 0. ITEM_3 is the top-right item so it’s offset is 60, -60 (that’s 60 pixels right along the x-axis and 60 pixel up along the y-axis). We’re also going to change the way items are loaded onto the boat. We will only allow at most two items, but those items will be in their offset positions. This doesn’t look good, but we’ll do it this way for now and try to get things looking better later. The image below shows how offsets are used.

<p style="font-weight: 400;">We also introduce a map field that maps items to rectangles. This is going to help us decide if a click event takes place inside an item or boat. The boatRectangle field will hold what you expect – the rectangle for the boat.

<p style="font-weight: 400;">The code below indicates how the view will change.

<p style="font-weight: 400;">@Overridepublic void paintComponent(Graphics g) {g.setColor(Color.GRAY);g.fillRect(0, 0, this.getWidth(), this.getHeight());paintItem(g, Item.ITEM_0);paintItem(g, Item.ITEM_1);paintItem(g, Item.ITEM_2);paintItem(g, Item.ITEM_3);paintBoat(g);}

private void paintItem(Graphics g, Item item) { … }private void paintBoat(Graphics g) { … }private Rectangle getItemRectangle(Item item) {// associate the item with its new value (rectangle) in the mapreturn rectMap.get(item);}private Rectangle getBoatRectangle() {// set boatRectangle to its new valuereturn boatRectangle;}

private Rectangle getRectangleOffsetBy(Rectangle rect, int dx, int dy) { … }

private void paintRectangle(Graphics g, Color color, String label, Rectangle rect) {// similar to paintStringInRectangle but now it creates// the rectangle with the specified color first// use rect.x, rect.y, rect.width, rect.height to get those values if needed}

<p style="font-weight: 400;">The main method simply paints 4 items and the boat. To paint a rectangular object, you will need (1) the color, (2) the label, and (3) the rectangle. Color and label are easy to get from the game engine. For the rectangle of an item, you have to change where the item is (start, finish, or boat) and then determine what the rectangle is. Once you have the rectangle, use it to update the map. The rectangle in the map is what you should return. For the item rectangles, you will need to work with offsets, that’s why we included the method getRectangleOffsetBy(…). It is very important that when you implement this method the first thing you do is make a *copy* of the rectangle that is passed in using the copy constructor. Unfortunately (in my opinion), Java rectangles are *mutable*, which means its easy to inadvertently modify their values even if you have declared them to be *final*. The body of the paint-rectangle method can be fairly easily deduced from the paint-string-in-rectangle method from the original code.

<p style="font-weight: 400;">Finally, because we mapped items to rectangles in the View, the Controller is not that difficult:

<p style="font-weight: 400;">@Overridepublic void mouseClicked(MouseEvent e) {if (rectMap.get(Item.ITEM_0).contains(e.getPoint())) {respondToItemClick(Item.ITEM_0);} else if (rectMap.get(Item.ITEM_1).contains(e.getPoint())) {respondToItemClick(Item.ITEM_1);} else if (rectMap.get(Item.ITEM_2).contains(e.getPoint())) {respondToItemClick(Item.ITEM_2);} else if (rectMap.get(Item.ITEM_3).contains(e.getPoint())) {respondToItemClick(Item.ITEM_3);} else if (boatRectangle.contains(e.getPoint())) {engine.rowBoat();}repaint();}

<p style="font-weight: 400;">The respond to item click method is fairly simple.

<h2><strong>Part 6 – The GameEngine Interface</strong></h2>

<ul>

 <li style="font-weight: 400;">Rename getCurrentLocation to getBoatLocation.</li>

 <li style="font-weight: 400;">Rename GameEngine to FarmerGameEngine.</li>

 <li style="font-weight: 400;">Extract a GameEngine interface from FarmerGameEngine. Select all public methods *except* the no-argument unloadBoat.</li>

 <li style="font-weight: 400;">Rename your GameEngineTest to FarmerGameEngineTest.</li>

 <li style="font-weight: 400;">Make sure your GameEngineTest and your RiverGUI classes *declare* GameEngines but initialize using FarmerGameEngines.</li>

 <li style="font-weight: 400;">Make any changes necessary to get your tests and your application to work (like replacing occurrences of unloadBoat() with unloadBoat(Item)).</li>

 <li style="font-weight: 400;">Take a look at the GameEngine interface below. Specifically, look at the Javadocs and ensure that your FarmerGameEngine class behaves as specified in the Javadocs.</li>

</ul>

<p style="font-weight: 400;">package river; import java.awt.Color; public interface GameEngine {         /**        * Returns the label of the specified item. This method may be used by a GUI        * (for example) to put the label string inside of a rectangle. A label is        * typically one or two characters long.        *          * @param item the item with the desired label          * @return the label of the specified item        */        String getItemLabel(Item item);         /**        * Returns the color of the specified item. This method may be used by a GUI        * (for example) to color a rectangle that represents the item.        *          * @param item the item with the desired color          * @return the color of the specified item        */        Color getItemColor(Item item);         /**        * Returns the location of the specified item. The location may be START,        * FINISH, or BOAT.        *          * @param item the item with the desired location          * @return the location of the specified item        */        Location getItemLocation(Item item);         /**        * Returns the location of the boat.        *          * @return the location of the boat        */        Location getBoatLocation();         /**        * Loads the specified item onto the boat. Assuming that all the        * required conditions are met, this method will change the location        * of the specified item to BOAT. Typically, the following conditions        * must be met: (1) the item’s location and the boat’s location        * must be the same, and (2) there must be room on the boat for the        * item. If any condition is not met, this method does nothing.        *          * @param item the item to load onto the boat        */        void loadBoat(Item item);         /**        * Unloads the specified item from the boat. If the item is on the boat        * (the item’s location is BOAT), then the item’s location is changed to        * the boat’s location. If the item is not on the boat, then this method        * does nothing.        *          * @param item the item to be unloaded        */        void unloadBoat(Item item);         /**        * Rows the boat to the other shore. This method will only change the        * location of the boat if the boat has a passenger that can drive the boat.        */        void rowBoat();         /**        * True when the location of all the game items is FINISH.        *          * @return true if all game items of a location of FINISH, false otherwise        */        boolean gameIsWon();         /**        * True when one or more implementation-specific conditions are met.        * The conditions have to do with which items are on which side of the        * river. If an item is in the boat, it is typically still considered        * to be on the same side of the river as the boat.        *          * @return true when one or more game-specific conditions are met, false        * otherwise        */        boolean gameIsLost();}

<p style="font-weight: 400;">The requirements of this interface may change some functionality. For example, now you can load 2 non-farmer items into the boat (but you can’t row it since only the farmer can drive). Therefore, if you created a passenger field from a previous refactoring, remove it. Instead, create a method that checks how many items are in the boat. When you load an item into the boat, see if there are already two items in the boat. If there are, don’t load the item.

<p style="font-weight: 400;">At this stage of the project, your RiverGUI application should behave as mine does in this video: <strong><a href="https://drive.google.com/file/d/151Z0Vy2MnYHyaTJFtMwbI25xwh6c6Ie1/view?usp=sharing" data-saferedirecturl="https://www.google.com/url?q=https://drive.google.com/file/d/151Z0Vy2MnYHyaTJFtMwbI25xwh6c6Ie1/view?usp%3Dsharing&amp;source=gmail&amp;ust=1550714747664000&amp;usg=AFQjCNEuNJWgaVY_L7IuhEHrQERvM7t1qQ">River-GUI-Relative-Positioning (Links to an external site.)Links to an external site.</a></strong>. The relative positioning of items is maintained not only on the left and right banks of the river, but also on the boat.