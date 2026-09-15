# ♟️ Chess Game — C++ Low-Level Design

A console-based Chess Game implemented in **C++** with a focus on **Object-Oriented Programming, SOLID principles, and Design Patterns**.

The project models a multiplayer chess system with chess piece movement, rule validation, matchmaking, player scoring, and in-game chat.

## 🚀 Features

* Complete chess board initialization
* Individual classes for all chess pieces

  * King
  * Queen
  * Rook
  * Bishop
  * Knight
  * Pawn
* Valid movement generation for each piece
* Turn-based gameplay
* Capture handling
* Check detection
* Checkmate detection
* Stalemate detection
* Player scoring system
* Score-based matchmaking
* Active match management
* In-game player chat
* Scholar's Mate demonstration
* Console-based board display

## 🏗️ System Design

The project is designed using object-oriented principles with separate responsibilities for the major components.

### Main Classes

| Class                | Responsibility                            |
| -------------------- | ----------------------------------------- |
| `Position`           | Represents a board coordinate             |
| `Move`               | Represents a chess move                   |
| `Piece`              | Abstract base class for chess pieces      |
| `King`               | Implements King movement                  |
| `Queen`              | Implements Queen movement                 |
| `Rook`               | Implements Rook movement                  |
| `Bishop`             | Implements Bishop movement                |
| `Knight`             | Implements Knight movement                |
| `Pawn`               | Implements Pawn movement                  |
| `PieceFactory`       | Creates chess pieces                      |
| `Board`              | Maintains board state and pieces          |
| `ChessRules`         | Interface for chess rules                 |
| `StandardChessRules` | Implements standard chess rule validation |
| `Message`            | Represents a chat message                 |
| `ChatMediator`       | Mediator interface for chat               |
| `User`               | Represents a player                       |
| `Match`              | Manages an individual chess match         |
| `MatchingStrategy`   | Interface for matchmaking                 |
| `ScoreBasedMatching` | Matches players based on score            |
| `GameManager`        | Manages active matches and matchmaking    |

## 🎨 Design Patterns Used

### 1. Strategy Pattern

The `Piece` class defines a common interface for calculating possible moves, while each concrete piece provides its own implementation.

```cpp
class Piece {
public:
    virtual vector<Position> getPossibleMoves(
        Position currentPos,
        Board* board
    ) = 0;
};
```

This allows movement behavior to vary between:

* `King`
* `Queen`
* `Rook`
* `Bishop`
* `Knight`
* `Pawn`

The chess rules system also uses the Strategy Pattern through `ChessRules` and `StandardChessRules`.

### 2. Factory Pattern

`PieceFactory` centralizes creation of chess pieces.

```cpp
Piece* piece = PieceFactory::createPiece(QUEEN, WHITE);
```

Instead of directly creating concrete piece objects throughout the board initialization logic, object creation is handled by the factory.

### 3. Mediator Pattern

The chat system uses the Mediator Pattern.

`User` objects communicate through `ChatMediator`, with `Match` acting as the mediator.

```text
User
  │
  ▼
ChatMediator
  │
  ▼
Match
  │
  ▼
Opponent User
```

This prevents users from directly depending on each other for communication.

### 4. Singleton Pattern

`GameManager` is implemented as a Singleton.

```cpp
GameManager* gm = GameManager::getInstance();
```

It maintains centralized information about:

* Active matches
* Waiting players
* Matchmaking strategy
* Match IDs

### 5. Strategy Pattern for Matchmaking

Matchmaking is abstracted through:

```cpp
class MatchingStrategy {
public:
    virtual User* findMatch(
        User* user,
        vector<User*>& waitingUsers
    ) = 0;
};
```

The current implementation uses `ScoreBasedMatching`, which finds opponents within a configurable score tolerance.

This allows other matchmaking strategies to be introduced without changing `GameManager`.

## ♟️ Chess Rules

The current implementation supports:

* Piece-specific movement
* Collision detection
* Capturing opponent pieces
* Turn validation
* Check detection
* Checkmate detection
* Stalemate detection
* Pawn's initial two-square move

The project also demonstrates **Scholar's Mate** as a predefined game sequence.

## 🎮 Example Flow

A match follows this general flow:

```text
User
 │
 │ Request Match
 ▼
GameManager
 │
 │ Matchmaking
 ▼
MatchingStrategy
 │
 │ Opponent Found
 ▼
Match
 │
 ├── Board
 ├── ChessRules
 ├── Move History
 └── Chat
```

During gameplay:

```text
Player
  ↓
GameManager
  ↓
Match
  ↓
ChessRules
  ↓
Piece
  ↓
Board
```

## 💬 Chat System

Players can send messages during a match.

```cpp
aditya->send(
    new Message(aditya->getId(), "Good game!")
);
```

The `Match` acts as the mediator and forwards the message to the opponent.

## 🏆 Player Scoring

Each `User` starts with a score of `1000`.

The current scoring system includes:

* Winner: **+30**
* Loser: **-20**
* Player quitting: **-50**

Players are matched using their score, with the current matchmaking tolerance set to **100 points**.

## 🧪 Demonstration

The program includes a demonstration of Scholar's Mate:

```text
1. e2-e4
1. e7-e5
2. Bf1-c4
2. Nb8-c6
3. Qd1-h5
3. Ng8-f6
4. Qh5xf7#
```

The program also demonstrates:

* Player creation
* Matchmaking
* Active match management
* Player scoring
* Chat functionality

## 🛠️ Technologies

* **Language:** C++
* **Programming Paradigm:** Object-Oriented Programming
* **Interface:** Console
* **Data Structures:** `vector`, `map`
* **Concepts:** OOP, inheritance, polymorphism, abstraction, encapsulation, design patterns

## ▶️ How to Run

### Compile

Using `g++`:

```bash
g++ chess.cpp -o chess
```

### Run

Windows:

```bash
chess.exe
```

Linux/macOS:

```bash
./chess
```

## 📁 Project Structure

```text
Chess-Game/
│
├── chess.cpp
└── README.md
```

## 🔮 Possible Future Improvements

The project can be extended with:

* Castling
* En passant
* Pawn promotion
* Threefold repetition
* Fifty-move rule
* Draw by insufficient material
* Move notation / PGN support
* Timers and clocks
* Persistent player accounts
* Online multiplayer
* GUI interface
* Database integration
* REST/WebSocket based backend
* AI opponent using a chess engine
* Unit and integration testing
* Splitting the implementation into separate `.h` and `.cpp` files

## 📌 Purpose

This project was built primarily to demonstrate **C++ Low-Level Design and Object-Oriented Design** concepts through a practical Chess Game implementation.
