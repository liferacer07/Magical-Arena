# Magical-Arena

## Description
"Magical Arena" is a Java console game where players battle using health, strength, and attack attributes. They roll dice to attack and defend, aiming to reduce opponents' health to zero. Fast-paced and strategic, it ends when a player's health depletes.

## Overview 
The project consists of the following components:

* Player.java: Defines the attributes and behavior of a player.

* Match.java: Manages the interaction between two players during a match.

* Main.java: handles player creation, validates input, and starts a match between two players.

* PlayerTest: Unit tests for corresponding classes.

## Compile and run the source code
 1. Compile the Java files using a Java compiler using command <br>
 `javac Main.java`
 1. Run the game using command <br>
 `java Main`

 ## How to play
 1. Enter the attributes for Player A when prompted (health, strength, attack).
 2. Enter the attributes for Player B when prompted (health, strength, attack).
 3. The game will simulate the battle between the two players, with Player having less health attacking first.
 4. The attacker's attack value, multiplied by the attacking dice roll, determines the damage inflicted, while the defender's strength value, multiplied by the defending dice roll, determines the damage defended.
 5. Any excess damage from the attacker reduces the defender's health.
 6. The game continues with players exchanging attacker and defender roles until one player's health reaches zero, indicating the end of the match.
 7. The game announces the winner based on which player's health reaches zero first.
    


## Getting Started
To run the Magical Arena simulation, follow these steps:

1. Clone or download the project repository to your local machine.
2. Open the project in your preferred Java IDE, such as Eclipse or IntelliJ IDEA.

## Running the Application

1. Locate the Main.java file in the project structure.
2. Run the main() method in Main.java to start the application.
3. Follow the prompts or input required parameters to simulate matches in the Main.

## Main Class

Explanation:

* The Main class serves as the entry point for the application.
* In the main() method, it creates two players, playerA and playerB, with their respective attributes: health, strength, and attack.
* It then initiates a match between playerA and playerB using the Match class.
* After the match is completed, it checks the health of playerA to determine the winner.
* If playerA's health is less than or equal to 0, it declares playerB as the winner. Otherwise, it declares playerA as the winner.

 
      import java.util.Scanner;
  
      public class Main {
  
      public static Player createPlayer(Scanner scanner, String name) {
          System.out.println("Enter " + name + "'s attributes (Health, Strength, Attack)");
          
          //** Get valid health input in range 1-100 **//
          int health = getValidInput(scanner, "Health (1-100)", 1, 100);  // Ask for health input from the user
          int strength = getValidInput(scanner, "Strength");  // Strength input
          int attack = getValidInput(scanner, "Attack");  // Attack input
  
          return new Player(health, strength, attack, name);  // Return a new Player object with user input
      }
  
      private static int getValidInput(Scanner scanner, String attribute) {
          int value;
          while (true) {
              System.out.print(attribute + ": ");
              value = scanner.nextInt();  // Get input from the user
              if (value > 0) {  // Validate that input is positive
                  break;  // If valid, exit loop
              } else {
                  System.out.println(attribute + " must be a positive number. Please try again.");
              }
          }
          return value;  // Return valid input
      }
  
      private static int getValidInput(Scanner scanner, String attribute, int min, int max) {
          int value;
          while (true) {
              System.out.print(attribute + ": ");
              value = scanner.nextInt();  // Get input from the user
              if (value >= min && value <= max) {  // Validate that input is within range
                  break;  // If valid, exit loop
              } else {
                  System.out.println(attribute + " must be between " + min + " and " + max + ". Please try again.");
              }
          }
          return value;  // Return valid input
      }
  
      public static void main(String[] args) {
          Scanner scanner = new Scanner(System.in);  // Create Scanner object for user input
  
          // Create Player A with health, strength, and attack attributes
          Player playerA = createPlayer(scanner, "Player A");
          // Create Player B with health, strength, and attack attributes
          Player playerB = createPlayer(scanner, "Player B");
  
          // Start the match between Player A and Player B
          Match match = new Match(playerA, playerB);
          match.start();  // Start the match
  
          scanner.close();  // Close the scanner after use
      }
      }

## Player Class

Explaination:

1. Attributes:

The Player class encapsulates the attributes of a player in the Magical Arena. These attributes include:
* health: Represents the health points of the player.
* strength: Denotes the strength attribute of the player, which is used for defending against attacks.
* attack: Indicates the attack attribute of the player, used for inflicting damage on the opponent.
2. Constructor:

* The class has a constructor that initializes a Player object with specific values for health, strength, and attack.
* It sets the initial values of the player's attributes based on the parameters passed to the constructor.


      import java.util.Random;

      class Player {
      private int health;
      private int strength;
      private int attack;
      private String name;
      private Random random;
  
      // Constructor to initialize a player with given attributes
      public Player(int health, int strength, int attack, String name) {
          if (health <= 0 || health > 100) {  // Health must be between 1 and 100
              throw new IllegalArgumentException("Health must be between 1 and 100.");
          }
          
          //** Strength and Attack must be between 1 and 30 **//
          if (strength < 1 || strength > 30) {
              throw new IllegalArgumentException("Strength must be between 1 and 30.");
          }
          
          if (attack < 1 || attack > 30) {
              throw new IllegalArgumentException("Attack must be between 1 and 30.");
          }
  
          this.health = health;
          this.strength = strength;
          this.attack = attack;
          this.name = name;
          this.random = new Random();
      }
  
      // Method to get the player's current health
      public int getHealth() {
          return health;
      }
  
      // To get the name of the player
      public String getName() {
          return this.name;
      }
  
      // Method to check if the player is alive (health > 0)
      public boolean isAlive() {
          return health > 0;
      }
  
      // Method to apply damage to the player
      public void receiveDamage(int damage) {
          health -= damage;
          if (health < 0) {
              health = 0;
          }
      }
  
      // Method to simulate rolling a six-sided die
      public int rollDice() {
          return random.nextInt(6) + 1; // Roll a 6-sided die
      }
  
      // Method to calculate the damage inflicted by the player's attack
      public int calculateAttackDamage() {
          int diceValue = rollDice();
          return attack * diceValue;
      }
  
      // Method to calculate the defense strength of the player
      public int calculateDefendStrength() {
          int diceValue = rollDice();
          return strength * diceValue;
      }
      }

## Match Class

1. Constructor:

* Initializes the Match object with two players (player1 and player2).
* Determines the initial attacker and defender based on the players' health. The player with lower health becomes the attacker, and the other becomes the defender.

2. rollDice() method: Roll a 6-sided die
3. calculateAttackDamage() method: Calculate attack damage
4. calculateDefendStrength() method:  Calculate defense strength

      

       public class Match {
        private Player playerA;
        private Player playerB;
    
        // Constructor to initialize the match with two players
        public Match(Player playerA, Player playerB) {
            this.playerA = playerA;
            this.playerB = playerB;
        }
    
        // Method to start the match
        public void start() {
            // Determine the initial attacker and defender based on their health
            Player attacker = (playerA.getHealth() <= playerB.getHealth()) ? playerA : playerB;
            Player defender = (attacker == playerA) ? playerB : playerA;
    
            System.out.println("Starting the match between " + this.playerA.getName() + " - " + this.playerB.getName());
            System.out.println(attacker.getName() + " will start with attacking");
    
            int rounds = 0;
    
            while (playerA.isAlive() && playerB.isAlive()) {
                System.out.println("\nRound: " + ++rounds);
                attack(attacker, defender);
    
                // Swap attacker and defender for the next turn
                Player temp = attacker;
                attacker = defender;
                defender = temp;
            }
    
            // Determine and print the winner of the match
            Player winner = playerA.isAlive() ? playerA : playerB;
            System.out.println("\nWinner: " + winner.getName() + " in " + rounds + " rounds.");
        }
    
        // Method to handle an attack between two players
        private void attack(Player attacker, Player defender) {
            int attackDamage = attacker.calculateAttackDamage();
            int defendStrength = defender.calculateDefendStrength();
    
            int damageTaken = Math.max(0, attackDamage - defendStrength);
            defender.receiveDamage(damageTaken); // Apply damage to the defending player
    
            System.out.println(attacker.getName() + " attacks " + defender.getName() + " with " + attackDamage + " damage.");
            System.out.println(defender.getName() + " defends with " + defendStrength + " strength.");
            System.out.println(defender.getName() + "'s Remaining Health: " + defender.getHealth());
        }
       }
## Unit Testing
The project includes a suite of unit tests to ensure the correctness and reliability of the codebase.
This PlayerTest class contains unit tests for the Player class. Here's a brief overview of what each test does:

1. testPlayerAlive: Verifies that a player with health > 0 is considered alive.
2. testPlayerDead: Verifies that a player with health = 0 is considered dead.
3. testNegativeHealth: Checks that an exception is thrown when health is set to a negative value.
4. testExcessiveHealth: Checks that an exception is thrown when health exceeds 100.
5. testInvalidStrength: Verifies that an exception is thrown when strength exceeds 30.
6. testInvalidAttack: Verifies that an exception is thrown when attack exceeds 30.
7. testReceiveDamage: Tests if the player's health decreases correctly after receiving damage.
8. testCalculateAttackDamage: Ensures the attack damage is within the expected range.
9. testCalculateDefendStrength: Ensures the defense strength is within the expected range.

To run the unit tests:

* Navigate to the corresponding test files in the src/test directory.
* Run each test class or individual test methods using your IDE's testing framework.


      import org.junit.jupiter.api.Test;
      import static org.junit.jupiter.api.Assertions.*;
  
      public class PlayerTest {
  
      @Test
      public void testPlayerAlive() {
          Player playerA = new Player(50, 10, 5, "Player A");
          assertTrue(playerA.isAlive(), "Player should be alive with health greater than 0");
      }
  
      @Test
      public void testPlayerDead() {
          Player playerB = new Player(0, 10, 5, "Player B");
          assertFalse(playerB.isAlive(), "Player should be dead with health equal to 0");
      }
  
      @Test
      public void testNegativeHealth() {
          try {
              Player playerC = new Player(-5, 10, 5, "Player C");
              fail("Expected IllegalArgumentException due to negative health");
          } catch (IllegalArgumentException e) {
              assertEquals("Health must be between 1 and 100.", e.getMessage());
          }
      }
  
      @Test
      public void testExcessiveHealth() {
          try {
              Player playerD = new Player(101, 10, 5, "Player D");
              fail("Expected IllegalArgumentException due to health greater than 100");
          } catch (IllegalArgumentException e) {
              assertEquals("Health must be between 1 and 100.", e.getMessage());
          }
      }
  
      @Test
      public void testInvalidStrength() {
          try {
              Player playerE = new Player(50, 35, 5, "Player E");
              fail("Expected IllegalArgumentException due to strength greater than 30");
          } catch (IllegalArgumentException e) {
              assertEquals("Strength must be between 1 and 30.", e.getMessage());
          }
      }
  
      @Test
      public void testInvalidAttack() {
          try {
              Player playerF = new Player(50, 10, 40, "Player F");
              fail("Expected IllegalArgumentException due to attack greater than 30");
          } catch (IllegalArgumentException e) {
              assertEquals("Attack must be between 1 and 30.", e.getMessage());
          }
      }
  
      @Test
      public void testReceiveDamage() {
          Player playerG = new Player(50, 10, 5, "Player G");
          playerG.receiveDamage(10);
          assertEquals(40, playerG.getHealth(), "Player's health should decrease by 10 after taking damage");
  
          playerG.receiveDamage(100);
          assertEquals(0, playerG.getHealth(), "Player's health should not be less than 0");
      }
  
      @Test
      public void testCalculateAttackDamage() {
          Player playerH = new Player(50, 10, 5, "Player H");
          int damage = playerH.calculateAttackDamage();
          assertTrue(damage >= 5 && damage <= 30, "Attack damage should be between 5 and 30");
      }
  
      @Test
      public void testCalculateDefendStrength() {
          Player playerI = new Player(50, 10, 5, "Player I");
          int defendStrength = playerI.calculateDefendStrength();
          assertTrue(defendStrength >= 10 && defendStrength <= 60, "Defend strength should be between 10 and 60");
      }
      }










    


