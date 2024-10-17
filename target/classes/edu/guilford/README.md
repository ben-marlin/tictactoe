# Tic Tac Toe

I'm pretty sure we've all played this game as kids, but we don't play it much any more. That's because the strategy to not lose is so obvious it becomes very hard to win, and most games are a draw. Programming this game will be a bit of a challenge because there are a lot of moving parts.

Just for the text-based version, we use a class, constructors, arrays, multiple methods, and complex logic.

## Sample Code

You've been provided with sample code in `TicTacToe.java` that lets two players alternately choose empty locations on the board. When someone has three in a row, they win! I took the simple expedient of numbering the spaces 1 to 9 and filling in an X or O as they're played.

### display()
> This method prints the current state of the board as text.

### isFull()
> This method returns true if all 9 spaces have been chose and false otherwise.

### checkWin(player)
> This method checks whether any row, column, or diagonal is filled with the current player's symbol, X or O. Game halts when this happens, so this is only checked when the current player has just claimed a space.

### makeMove(player, position)
> Checks to see whether the space corresponding to position is occupied. If not, then it assigns it to the current player and returns true. If it is occupied, then it returns false. 

As we are doing with a lot of games, we will now convert this text-based game into a more visual game where the players click on squares to choose them.

In this, we'll use frames, panels, classes, constructors, fields, and a mouse listener.

## Create the Class

You've done this before. `File > New File > Java File > Class`. I named mine TicTacToeBoard in order to distinguish it from the version played with text. Add the package and make it extend JFrame. Don't forget to put your name in comments here at the top.

Add a field to the class as follows. 
```
char player = 'X';

```
We'll use that to track who the current player is, and X goes first. 

Create a blank constructor. This follows our philosophy of creating minimal working examples - a blank constructor is just a placeholder for what we'll add later.

You'll need the following imports.
```
import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import javax.swing.border.Border;
import javax.swing.border.LineBorder;
```
The only ones you haven't seen are the Border and LineBorder classes. They're going to help us draw the lines between the cells.

## Code the main Method

Add a main method. All it needs is this.
```
public static void main(String[] args) {
    // Instantiate the TicTacToeBoard
    new TicTacToeBoard();
}
```
The real action will take place in the private classes. This just makes an instance of the class appear.

## Code the Constructor

We just need very basic things to get the constructor started. In fact, you may be able to test run it already! Whether you can or not, let's add this code.
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

## Adding CellPanels

Because each of the nine cell panels needs the same thing done to them, we're going to create another private class that sets them up. First of all, let's make this thing look like a tic-tac-toe board.
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

With this little bit, change the for loop in the TicTacToeBoard constructor to add CellPanel objects, not JPanel objects. You should now be able to test it and see a tic-tac-toe board.

## Making CellPanel Display X or O

Since each cell needs to hold a single character, but also display it, let's add the following fields to the CellPanel class.
```
private char symbol; // Field to hold the character symbol
private JLabel label; // JLabel to display the character
```
We could, in fact, get away with just the label, but it would make some of the upcoming code really cumbersome.

Now, inside the CellPanel constructor, add this.
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
This should be another good point to make sure your program works. We haven't added anything visible, but if you want to check on it, you could change the line that initializes label so it's X or something. Once you see that it works, you can adjust the size of the font to bigger or smaller. You might try another font if you have a favorite - installed fonts vary by machine, though.

When you're done testing, though, change the label back to a blank space.

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
Again, a minimal working example. Run your code and make sure this puts a question mark on any cell you click on. That's not our final product, of course.

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

## Adapting checkWin(player)

Originally, I meant to have you adapt the checkWin method from TicTacToe to TicTacToeBoard. I still think it's doable, but I think it's a little ambitious for one day. Let's come back to it later. 

But I want you to think on it. When would you check to see if a player won? In the original, it appeared after makeMove, and the program only got to that point if makeMove was legal. So for us, we would want to check for a win right after marking the space for a player.

The method should then go through the winning combos... but that logic required that the cells be checked, which will require an array for cells! So... let's come back to this another day.

## Wrapping Up

As usual, save it, stage it, commit message, commit button, sync button. On Canvas, paste a screenshot of your tic-tac-toe board working. Also paste the URL of your repo in case I have problems finding it later.