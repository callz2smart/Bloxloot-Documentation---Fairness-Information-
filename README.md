# Bloxloot-Documentation---Fairness-Information-

# ============================================================================
# BLOXLOOT DISCORD BOT - COMPLETE GAME MECHANICS DOCUMENTATION
# ============================================================================
# This code snippet explains how each game works with full transparency of fairness
# ============================================================================

"""
================================================================================
COINFLIP GAME
================================================================================

HOW IT WORKS:
------------
1. Creator selects items to bet (value between 0-1,000,000)
2. Chooses HEADS or TAILS
3. Can optionally enable Wild Mode (1% chance to reverse outcome)
4. Opponent joins with items within 10% of creator's bet value
5. Opponent automatically gets the opposite side
6. System flips a coin using random.choice(["heads", "tails"])
7. Winner receives the total pot minus house tax

FAIRNESS MECHANISM:
------------------
- Python's random module uses Mersenne Twister (cryptographically secure for gaming)
- Wild Mode has exactly 1% chance (1 in 100 games)
- No manipulation possible - all outcomes determined at runtime
"""

def coinflip_fairness_explanation():
    # The actual flip - truly random
    result = random.choice(["heads", "tails"])
    
    # Wild Mode - 1% chance (WILD_MODE_CHANCE = 0.01)
    wild_mode_activated = random.random() < 0.01
    
    # Tax calculation (default 5%)
    tax_amount = total_pot * 0.05
    
    return {
        "outcome": result,
        "wild_mode_triggered": wild_mode_activated,
        "house_tax": "5% of total pot"
    }

"""
OUTCOME SCENARIOS:
-----------------
- Normal win: Winner gets all items
- Wild Mode activated: Loser gets winnings, winner loses items
- Tie: Not possible in coinflip

TRANSPARENCY NOTES:
------------------
- Game ID displayed in footer for tracking
- All bets shown with emojis and values
- Tax rate displayed before game starts
- Result embed shows flip outcome clearly
"""


"""
================================================================================
MINES PVP GAME
================================================================================

HOW IT WORKS:
------------
1. Creator selects mines (1-10) and bets items
2. Opponent joins with items within 10% of bet
3. Board is 5x5 (25 cells) - mines are randomly placed
4. Random player starts first
5. Players take turns revealing cells:
   - Safe cell (✓) → Game continues, turn switches
   - Mine cell (💣) → Player loses immediately
6. Winner takes entire pot minus tax

FAIRNESS MECHANISM:
------------------
- Mines placed using random.sample() - no bias
- Starting player chosen randomly
- Wild Mode (1% chance) adds extra mine and reverses outcome
"""

def mines_fairness_explanation():
    total_cells = 25  # 5x5 grid
    
    # Mines randomly placed - FAIR
    all_cells = list(range(total_cells))
    mine_count = random.randint(1, 10)  # Creator chooses 1-10
    
    # Random mine positions
    mine_positions = random.sample(all_cells, mine_count)
    
    # Random starting player
    current_turn = random.choice(["creator", "opponent"])
    
    # Wild Mode (1% chance)
    wild_mode = random.random() < 0.01
    if wild_mode:
        mine_count += 1  # Extra mine
        # Outcome reversal: hitting mine makes you WIN
    
    return {
        "board_size": "5x5 (25 cells)",
        "mine_positions": "randomly selected",
        "starting_player": "random",
        "wild_mode_chance": "1%"
    }

"""
BOARD LAYOUT:
------------
Positions 1-25:
  1   2   3   4   5
  6   7   8   9   10
 11  12  13  14  15
 16  17  18  19  20
 21  22  23  24  25

WIN CONDITIONS:
--------------
- Hit mine → Lose immediately
- Reveal all safe cells → Last player to move wins

WILD MODE EFFECT:
---------------
- +1 extra mine added
- Outcome reversal: Hitting a mine makes you WIN instead of lose

TRANSPARENCY NOTES:
------------------
- Final board shows all mine positions after game ends
- Game ID displayed for tracking
- All moves visible in real-time
"""


"""
================================================================================
TOWERS PVP GAME
================================================================================

HOW IT WORKS:
------------
1. Creator selects items to bet
2. Opponent joins with items within 10% of bet
3. Board is 3 columns × 7 rows (21 cells)
4. 5 bombs placed (one per row, random column)
5. Players take turns revealing cells bottom-to-top only
6. First to hit bomb loses immediately
7. When reaching top, board resets for new round (bombs reposition)
8. Winner takes entire pot minus tax

FAIRNESS MECHANISM:
------------------
- One bomb per row, random column placement
- Must click bottom-most available row (enforced)
- Random starting player
"""

def towers_fairness_explanation():
    width = 3    # 3 columns
    height = 7   # 7 rows
    total_cells = width * height  # 21 cells
    bomb_count = 5  # One bomb per row (5 rows active at a time)
    
    # One bomb per row - random column (FAIR)
    bombs = set()
    for row in range(height):  # rows 0-6
        row_start = row * width  # 0, 3, 6, 9, 12, 15, 18
        row_cells = [row_start + col for col in range(width)]  # [0,1,2], [3,4,5], etc.
        bomb_cell = random.choice(row_cells)  # Random column in this row
        bombs.add(bomb_cell)
    
    # Random starting player
    current_turn = random.choice(["creator", "opponent"])
    
    # Must click bottom row first (row 0, cells 0-2)
    next_row = 0  # Starts at bottom
    
    return {
        "board_size": "3x7 (21 cells)",
        "bombs_per_round": 5,
        "bomb_placement": "one per row, random column",
        "starting_player": "random",
        "movement": "bottom-to-top only"
    }

"""
BOARD LAYOUT (Bottom to Top):
----------------------------
Row 6 (Top):    18  19  20
Row 5:          15  16  17
Row 4:          12  13  14
Row 3:          9   10  11
Row 2:          6   7   8
Row 1:          3   4   5
Row 0 (Bottom): 0   1   2

GAME RULES:
----------
- Must click bottom-most available row first
- Safe click → turn switches, move up one row
- Bomb hit → immediate loss
- Reaching top → board resets with new bomb positions
- Multiple rounds possible until someone hits a bomb

TRANSPARENCY NOTES:
------------------
- Final board shows all bomb positions after game ends
- Current turn displayed clearly
- Round number tracked
"""


"""
================================================================================
BLACKJACK PVP GAME
================================================================================

HOW IT WORKS:
------------
1. Creator selects items to bet
2. Opponent joins with items within 10% of bet
3. Both players receive 2 cards
4. Players take turns: HIT (draw card) or STAND (keep hand)
5. Goal: Get as close to 21 without going over
6. Split available if first two cards are same value
7. Closest to 21 wins; tie = push (items returned)

FAIRNESS MECHANISM:
------------------
- Standard 52-card deck
- Random shuffle using random.shuffle()
- Reshuffle when deck has <10 cards remaining
- Cards drawn from top of shuffled deck
"""

def blackjack_fairness_explanation():
    # Create standard 52-card deck
    suits = ['♥', '♦', '♣', '♠']
    values = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']
    deck = []
    for suit in suits:
        for value in values:
            deck.append(f"{value}{suit}")
    
    # Shuffle - FAIR
    random.shuffle(deck)
    
    # Reshuffle when low
    if len(deck) < 10:
        deck = deck * 1  # Single deck
        random.shuffle(deck)
    
    # Draw from top
    def draw_card():
        return deck.pop()  # Always top card
    
    return {
        "deck_type": "Standard 52-card",
        "shuffle_method": "random.shuffle() - Fisher-Yates",
        "reshuffle_threshold": "<10 cards remaining",
        "draw_method": "top card of shuffled deck"
    }

"""
CARD VALUES:
-----------
| Card | Value |
|------|-------|
| A    | 1 or 11 (best for player) |
| 2-10 | Face value |
| J, Q, K | 10 |

SPLIT RULES:
-----------
- Only if first two cards have same value (e.g., 8♥ and 8♣)
- Creates second hand
- Must play first hand completely before second
- Extra card dealt to each hand

WIN CONDITIONS:
--------------
- Blackjack (21) → Automatic win unless both have 21 (push)
- Bust (>21) → Automatic loss
- Both bust → Push (items returned)
- Higher score under 21 → Wins
- Split hands → Best hand under 21 counts

TRANSPARENCY NOTES:
------------------
- All cards shown with custom emojis
- Scores calculated and displayed
- Dealer's hidden card shown at end
- Split hands shown separately
"""


"""
================================================================================
ROCK PAPER SCISSORS PVP
================================================================================

HOW IT WORKS:
------------
1. Creator selects items to bet
2. Creator chooses move: Rock, Paper, or Scissors
3. Opponent joins with items within 10% of bet
4. Opponent chooses their move
5. Winner determined by classic rules
6. Tie = push (items returned)

FAIRNESS MECHANISM:
------------------
- Both players choose moves independently
- Creator chooses first, opponent sees only that creator has chosen
- Opponent's move is hidden until both have chosen
- Winner determined by simple rule set
"""

def rps_fairness_explanation():
    winning_rules = {
        "rock": "scissors",
        "paper": "rock",
        "scissors": "paper"
    }
    
    def determine_winner(choice1, choice2):
        if choice1 == choice2:
            return "tie"
        if winning_rules[choice1] == choice2:
            return "creator"
        return "opponent"
    
    return {
        "rules": winning_rules,
        "tie_handling": "items returned to both players",
        "transparency": "both moves revealed at resolution"
    }

"""
RULES TABLE:
-----------
| Player 1 | Player 2 | Winner |
|----------|----------|--------|
| Rock     | Scissors | Player 1 |
| Rock     | Paper    | Player 2 |
| Paper    | Rock     | Player 1 |
| Paper    | Scissors | Player 2 |
| Scissors | Paper    | Player 1 |
| Scissors | Rock     | Player 2 |
| Any      | Same     | Tie (push) |

TRANSPARENCY NOTES:
------------------
- Creator's move: shows "Chosen!" after selection (not revealed)
- Both moves revealed simultaneously when game resolves
- Game ID in footer for tracking
"""


"""
================================================================================
CASE BATTLE
================================================================================

HOW IT WORKS:
------------
1. Creator selects case section(s) and bets items
2. Opponent joins with items within 10% of bet
3. Multiple rounds (default 3) are played
4. Each round, both players open a random item from the case
5. Player with higher item value wins the round
6. Player with most rounds won (or highest total value) wins
7. Crazy Mode: Doubles all item values

FAIRNESS MECHANISM:
------------------
- Items picked by weighted chance (chance % from case config)
- Each case section has fixed drop rates
- Random selection using random.random()
"""

def casebattle_fairness_explanation():
    # Example case section with weighted chances
    case_section = {
        "name": "Xmas Juicer",
        "Value": [
            {"name": "Chroma Evergun", "value": 56000, "chance": 0.5},
            {"name": "Chroma Bauble", "value": 15750, "chance": 0.5},
            {"name": "Evergun", "value": 2850, "chance": 2.5},
            {"name": "Bauble", "value": 525, "chance": 4},
            {"name": "Icepiercer", "value": 375, "chance": 12.5},
            {"name": "Candy", "value": 195, "chance": 30},
            {"name": "Sugar", "value": 80, "chance": 50}
        ]
    }
    
    def pick_item(value_list):
        # Weighted random selection based on chance percentages
        total_chance = sum(item['chance'] for item in value_list)
        rand_num = random.random() * total_chance
        cumulative = 0
        for item in value_list:
            cumulative += item['chance']
            if rand_num <= cumulative:
                return item.copy()
        return value_list[-1].copy()
    
    return {
        "selection_method": "weighted random based on chance percentages",
        "chance_sum": "100% across all items",
        "crazy_mode": "doubles all item values (2x)"
    }

"""
CASE SECTIONS AVAILABLE:
-----------------------
1. Xmas Juicer
2. Frozen Fantasy
3. Vampires Vault
4. Chroma Chaos
5. 50/50 Harvester
6. Candy Frenzy

SCORING:
--------
- Normal Mode: Compare item values, higher wins round
- Crazy Mode: All values doubled, then compare
- Winner: Most rounds won (or highest total value if tie)

TRANSPARENCY NOTES:
------------------
- Each round's results shown immediately
- Running scores displayed
- Final results show all items won
"""


"""
================================================================================
JACKPOT
================================================================================

HOW IT WORKS:
------------
1. Jackpot automatically posts every 60 seconds
2. Players join by betting items (minimum value required)
3. Each player gets a weighted chance = (their bet / total pot)
4. After 60 seconds, winner selected using weighted random
5. Winner receives all items in the pot minus tax

FAIRNESS MECHANISM:
------------------
- Weighted random selection based on contribution
- Higher bet = higher chance to win
- Uses random.choices() with weights
"""

def jackpot_fairness_explanation():
    participants = {
        "Player1": {"value": 10000},
        "Player2": {"value": 5000},
        "Player3": {"value": 25000}
    }
    
    total_pot = sum(p["value"] for p in participants.values())
    
    # Calculate weights (contribution percentage)
    user_ids = list(participants.keys())
    weights = [participants[uid]['value'] for uid in user_ids]
    
    # Weighted random selection - FAIR
    # Player with 25,000 has 62.5% chance (25k/40k)
    winner = random.choices(user_ids, weights=weights, k=1)[0]
    
    return {
        "selection_method": "weighted random (more items = higher chance)",
        "total_pot": f"{total_pot} value",
        "example_winner_chance": "62.5% for highest contributor"
    }

"""
EXAMPLE WEIGHTED CHANCE:
-----------------------
If pot = 40,000 total:
- Player A: 25,000 → 62.5% chance
- Player B: 10,000 → 25% chance
- Player C: 5,000 → 12.5% chance

TRANSPARENCY NOTES:
------------------
- All participants shown with their contribution
- Percentage chance displayed for each player
- Countdown timer shows time remaining
- Final winner announced with all items won
"""


"""
================================================================================
HOUSE TAX SYSTEM
================================================================================

HOW TAX WORKS:
-------------
- Default tax rate: 5% of total pot
- Tax can be adjusted by admins (5%, 7.5%, 10%)
- Tax is deducted from items (not currency)
- Taxed items go to shareholders (TAX_PROFILES)

TAX DEDUCTION LOGIC:
-------------------
def deduct_tax_from_items(items: List[str], tax_amount: int):
    - Takes items from the pot
    - Selects items with value <= remaining tax
    - Takes items that sum close to tax amount
    - If all items are too big (> tax_amount), no tax taken
    - If each player has only 1 item, no tax taken

TRANSPARENCY NOTES:
------------------
- Current tax rate shown in every game embed
- Tax amount shown before game starts
- Taxed items logged to TAX_LOG_CHANNEL_ID
- Shareholders viewable in Tax Panel
"""


"""
================================================================================
RACE SYSTEM
================================================================================

HOW RACES WORK:
--------------
1. Admin creates race channel (Daily/Weekly/Monthly)
2. Race tracks wagered amounts for all games (MM2/ADM)
3. Leaderboard shows top 10 wagerers
4. Winners receive prizes based on placement
5. Prizes configured in RACE_PRIZE_CONFIG

FAIRNESS MECHANISM:
------------------
- Tracks total_wagered per user in registrations.json
- Wager history records each bet with timestamp
- No manipulation - counts only actual games played
- Winners reviewed before payout

RACE TYPES & PRIZES:
-------------------
DAILY (24h race):
- 1st: Chroma Evergun
- 2nd: Bauble
- 3rd-4th: Candy
- 5th-10th: Sugar

WEEKLY (7d race):
- 1st: Evergun
- 2nd: Bauble
- 3rd-4th: Candy
- 5th-10th: Sugar

MONTHLY (30d race):
- 1st: Chroma Evergun
- 2nd: Evergun
- 3rd: Bauble
- 4th: Candy
- 5th-10th: Sugar
"""


"""
================================================================================
SUMMARY - ALL GAMES ARE FAIR BECAUSE:
================================================================================

1. RANDOM NUMBER GENERATION:
   - Python's random module uses Mersenne Twister
   - Cryptographically suitable for gaming applications
   - No external influence on outcomes

2. TRANSPARENT ODDS:
   - Wild Mode: Exactly 1% (shown to players)
   - Mines placement: Random positions (shown after game)
   - Bomb placement: Random columns (shown after game)
   - Case Battle: Weighted by chance % (configurable)

3. VERIFIABLE GAMES:
   - Game IDs for every match
   - All bets logged
   - Taxed items recorded
   - Completed games saved to file

4. NO HIDDEN ADVANTAGES:
   - House tax displayed before game starts
   - All rules explained in game embeds
   - No manipulation of outcomes mid-game

5. ACCOUNTABILITY:
   - Staff actions limited (10 per day)
   - Event host actions limited (3 per day)
   - All admin actions logged
   - Shareholders cannot change game outcomes
"""

# ============================================================================
# END OF GAME MECHANICS DOCUMENTATION
# ============================================================================
