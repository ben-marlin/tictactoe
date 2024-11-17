# Tic Tac Toe

I'm pretty sure we've all played this game as kids, but we don't play it much any more. That's because the strategy to not lose is so obvious it becomes very hard to win, and most games are a draw. Programming this game will be a bit of a challenge because there are a lot of moving parts.

Just for the text-based version, we use 

- class constructor
- arrays
- multiple methods
- complex logic.

## Sample Code

You've been provided with sample code in `TicTacToe.java` that lets two players alternately choose empty locations on the board. When someone has three in a row, they win! I took the simple expedient of numbering the spaces 1 to 9 and filling in an X or O as they're played.

### display()
> This method prints the current state of the board as text.

### isFull()
> This method returns true if all 9 spaces have been chosen and false otherwise.

### checkWin(player)
> This method checks whether any row, column, or diagonal is filled with the current player's symbol, X or O. Game halts when this happens, so this is only checked when the current player has just claimed a space.

### makeMove(player, position)
> Checks to see whether the space corresponding to position is occupied. If not, then it assigns it to the current player and returns true. If it is occupied, then it returns false. 

As we are doing with a lot of games, we will now convert this text-based game into a more visual game where the players click on squares to choose them.

In this, we'll use 
- frame
- panels
- class constructors
- global fields
- mouse listener.

## Create the Class

You've done this before. `File > New File > Java File > Class`. I named mine TicTacToeBoard in order to distinguish it from the version played with text. Add the package and make it extend JFrame. Don't forget to put your name in comments here at the top.

Add a field to the class as follows. 
```
char player = 'X';
```
We'll use that to track who the current player is, and X goes first. 

Create a blank constructor. This follows our philosophy of creating minimal working examples - a blank constructor is just a placeholder for what we'll add later.

## Imports
You'll need the following classes imported.
```
import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import javax.swing.border.Border;
import javax.swing.border.LineBorder;
```
The only ones you haven't seen are the Border and LineBorder classes. They're going to help us draw the lines between the cells.

## Code the Constructor

We just need very basic things to get the constructor started. In fact, you may be able to test run it already! Whether you can or not, let's add this code to the blank constructor.
```
// Set the title of the JFrame
setTitle("Tic-Tac-Toe Board");

// Set the default close operation
setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

// Set the size of the JFrame
setSize(300, 300);

// Set the layout of the JFrame to a 3x3 GridLayout
setLayout(new GridLayout(3, 3));

// Create and add 9 cells to the JFrame
for (int i = 0; i < 9; i++) {
    JPanel cell = new JPanel();
    add(cell);
}

// Make the JFrame visible
setVisible(true);
```
After you add this to the constructor, you should be able to get a... blank gray window. Doesn't seem like we've accomplished much. It doesn't even look like a tic-tac-toe board - where are the lines?

## Code the main Method

Add a main method. All it needs is this.
```
public static void main(String[] args) {
    // Instantiate the TicTacToeBoard
    new TicTacToeBoard();
}
```
The real action will take place in the private classes. This just makes an instance of the class appear. Go ahead and test your code at this point to make sure it does something. Unfortunately, since the panels are the same color, it doesn't look much like what we want yet.

## Adding CellPanels

Because each of the nine cell panels needs the same thing done to them, we're going to create another private class that sets them up. First of all, let's make this thing look like a tic-tac-toe board. Insert the following class after the constructor but before the main method.
```
private class CellPanel extends JPanel {
    public CellPanel() {
        // Set the background color and border
        setBackground(Color.LIGHT_GRAY);
        Border border = new LineBorder(Color.BLACK, 2);
        setBorder(border); // Set the border for the panel
    }
}
```
A LineBorder allows you to specify the color and size of the border for each cell. I chose 2 pixels after experimenting for a while - feel free to personalize it. I also chose LIGHT_GRAY and BLACK for colors, which is kind of boring.

Once you have this little class, change the for loop in the TicTacToeBoard constructor to add CellPanel objects, not JPanel objects. You should now test it to make sure you see a tic-tac-toe board.

## Making CellPanel Display X or O

Since each cell needs to hold a single character, but also display it, let's add the following fields to the CellPanel class.
```
private char symbol; // Field to hold the character symbol
private JLabel label; // JLabel to display the character
```
We could, in fact, get away with just the label, but it would make some of the upcoming code really cumbersome to type and keep track of.

Next, inside the CellPanel constructor, add this.
```
this.symbol = ' '; // init symbol to empty space
this.label = new JLabel(" "); // init label to empty space
label.setFont(new Font("Arial", Font.PLAIN, 48)); 
    // Set font size for readability & visibility
label.setHorizontalAlignment(SwingConstants.CENTER); 
    // Center text in label

setLayout(new BorderLayout());  // lets us center label
// Add the label to the panel
add(label,BorderLayout.CENTER);
```
This should be another good point to make sure your program works. We haven't added anything visible, but if you want to check on it, you could change the line that initializes label so it's X or something. Once you see that it works, you can adjust the size of the font to bigger or smaller. You might try another font if you have a favorite - installed fonts vary by machine, though. As we've discovered, Arial is a common one to have, though.

When you're done testing, change the label back to a blank space!

## Attaching a MouseListener

We're going to attach a MouseListener to each panel, and each one should do exactly the same thing. This is a little weird! When dealing with buttons, you expected each button to do a different job, so you made a separate ActionListener for each one. But this fits our situation well. So add this code at the end of your constructor for CellPanel.
```
// Add a mouse listener to handle clicks
addMouseListener(new MouseAdapter() {
    @Override
    public void mouseClicked(MouseEvent e) {
        label.setText("?");
    }
});
```
Again, this is a minimal working example. Run your code and make sure this puts a question mark on any cell you click on. That's not our final product, of course!

Replace `label.setText("?");` with the following.
```
if (symbol == ' ') {         // check if unoccupied
    symbol = player;         // claim by player
    label.setText("" + player); // mark for player

    if (player == 'X') {     // switch players
        player = 'O';
    } else {
        player = 'X';
    }
}
```
Test your code. Believe it or not, this should be a playable game of tic-tac-toe! Of course, it doesn't reset, it doesn't show who won, and it doesn't stop the game when someone wins.

## Embellishments

You should personalize the colors, title, font, line border width, etc. But that's enough for a one-day project. If you happen to be working on a tic-tac-toe game for a final project, the following is for you! If you're not, then you can skip to "wrapping up" and turn this in.

## Adapting checkWin

Recall the following method from the sample TicTacToe.java code.

```
public boolean checkWin(char player) {

    // winning combinations are arrays of arrays...
    int[][] winCombinations = {
        {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // Rows
        {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // Columns
        {0, 4, 8}, {2, 4, 6}             // Diagonals
    };

    for (int[] combo : winCombinations) {
        if (cells[combo[0]] == player &&      // whole line must match
            cells[combo[1]] == player &&      // player for this to be
            cells[combo[2]] == player) {      // true
            return true;
        }
    }
    return false;   // only gets here if player has no win combo
}
```
This depended on having cells[0]-cells[8] holding either X or 0. Since we don't have an array of cells, we'll need to modify our constructor to be able to identify the individual cells.

Right after the line that says `char player = 'X'` - the field before the constructor - add the following line.
```
CellPanel[] cells;
```
This declares the array, but does not instantiate it. It shouldn't create any conflicts, as Java sees cells and cell as completely different variables. To instantiate it, change
```
// Create and add 9 cells to the JFrame
for (int i = 0; i < 9; i++) {
    JPanel cell = new JPanel();
    add(cell);
}
```
to instead say
```
// Create and add cells array to the JFrame
cells = CellPanel[9];

for (int i = 0; i < 9; i++) {
    cells[i] = new CellPanel();
    add(cells[i]);
}
```
This should not change the functionality of your program at all - it's just an organizational change. But test your program to make sure.

Once you know that works, we'll make a small change to CellPanel. Add a method to that private class.
```
public char getSymbol() {
    return symbol;
}
```
We've seen getters & setters before - this is just one of those. We'll use it in the following as we use a modified version of checkWin.
```
public boolean checkWin(char player) {

    // winning combinations are arrays of arrays...
    int[][] winCombinations = {
        {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // Rows
        {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // Columns
        {0, 4, 8}, {2, 4, 6}             // Diagonals
    };

    for (int[] combo : winCombinations) {
        if (cells[combo[0]].getSymbol() == player &&      // whole line must match
            cells[combo[1]].getSymbol() == player &&      // player for this to be
            cells[combo[2]].getSymbol() == player) {      // true
            return true;
        }
    }
    return false;   // only gets here if player has no win combo
}
```
Notice that all we did here was to use getSymbol() for the cells array element! We re-used that code... but we don't have a use for it, as we weren't checking to see if the player won. So now we have to modify the mouseListener.
```
// Add a mouse listener to handle clicks
addMouseListener(new MouseAdapter() {
    @Override
    public void mouseClicked(MouseEvent e) {
        if (symbol == ' ') {         // check if unoccupied
            symbol = player;         // claim by player
            label.setText("" + player); // mark for player

            if (player == 'X') {     // switch players
                player = 'O';
            } else {
                player = 'X';
            }
        }

        // Check whether this was a winning move
        if checkWin(symbol) {
            System.out.println("Player " + symbol + " wins!);

            // Disable play
            player = ' ';
        }
    }
});
```
This will print the win message to the command line (we can expand on that in a minute). Then setting player to a blank space means that for future clicks on a blank panel, the symbol won't be changed, and since the player is no longer X or O, the player won't get changed to X or O again.

## Embellishing the Embellishment

It sure would be nice if the message were in the frame, right? And maybe visual?

Modify your checkWin() method.
```
public boolean checkWin(char player) {

    // winning combinations are arrays of arrays...
    int[][] winCombinations = {
        {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // Rows
        {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // Columns
        {0, 4, 8}, {2, 4, 6}             // Diagonals
    };

    for (int[] combo : winCombinations) {
        if (cells[combo[0]].getSymbol() == player &&      // whole line must match
            cells[combo[1]].getSymbol() == player &&      // player for this to be
            cells[combo[2]].getSymbol() == player) {   for (int square : combo) {
                cells[square].setBackground(Color.RED);
            }   
                // true
            return true;
        }
    }
    return false;   // only gets here if player has no win combo
}
```
This should make the winning squares red - feel free to find a better color! 

Finally, look back at the project where we used JOptionPane to display a message. Replace the print statement to use that, instead.

If you got all that to work, you've done something pretty neat. You've re-used code. You've managed a tricky win condition with complex logic and used a display that was split across multiple methods. That's pretty cool!

## Wrapping Up

As usual, save it, stage it, commit message, commit button, sync button. On Canvas, paste a screenshot of your tic-tac-toe board working. Also paste the URL of your repo in case I have problems finding it later.