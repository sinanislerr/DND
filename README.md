import random

# Karakter sınıfları
classes = ["Barbarian", "Wizard", "Rogue", "Cleric"]

# Canavarlar ve özellikleri
monsters = {
    "Goblin": {"health": 15, "attack": 5},
    "Orc": {"health": 20, "attack": 8},
    "Dragon": {"health": 50, "attack": 15}
}

# Karakterin başlangıç durumu
player = {
    "name": "",
    "class": "",
    "health": 50,
    "attack": 10,
    "gold": 0,
    "inventory": []
}

def main():
    print("Welcome to the Dungeon!")
    print("=======================")
    
    create_character()
    
    while player["health"] > 0:
        print("\n{} (Health: {}, Attack: {}, Gold: {})".format(player["name"], player["health"], player["attack"], player["gold"]))
        print("1. Explore the dungeon")
        print("2. Check inventory")
        print("3. Rest")
        print("4. Exit game")
        
        choice = input("What would you like to do? ")
        
        if choice == "1":
            explore_dungeon()
        elif choice == "2":
            check_inventory()
        elif choice == "3":
            rest()
        elif choice == "4":
            print("Exiting the game...")
            break
        else:
            print("Invalid choice. Please try again.")

    print("\nGame over. You are dead!")

def create_character():
    global player
    
    player["name"] = input("Enter your character's name: ")
    print("Choose your class:")
    for i, class_name in enumerate(classes, 1):
        print("{}. {}".format(i, class_name))
    class_choice = int(input("Enter the number of your class: "))
    player["class"] = classes[class_choice - 1]
    
    if player["class"] == "Barbarian":
        player["health"] += 10
        player["attack"] += 5
    elif player["class"] == "Wizard":
        player["health"] -= 10
        player["attack"] += 15
    elif player["class"] == "Rogue":
        player["attack"] += 10
    elif player["class"] == "Cleric":
        player["health"] += 20

    print("\nCharacter created!")
    print("Name: {}, Class: {}, Health: {}, Attack: {}".format(player["name"], player["class"], player["health"], player["attack"]))

def explore_dungeon():
    global player
    
    monster_name, monster_stats = random.choice(list(monsters.items()))
    print("\nYou encountered a {} with {} health and {} attack!".format(monster_name, monster_stats["health"], monster_stats["attack"]))
    
    while player["health"] > 0 and monster_stats["health"] > 0:
        print("\n1. Attack")
        print("2. Run away")
        attack_choice = input("What will you do? ")
        
        if attack_choice == "1":
            attack_damage = random.randint(1, 10) + player["attack"]
            print("You attack the {} and deal {} damage!".format(monster_name, attack_damage))
            monster_stats["health"] -= attack_damage
            
            if monster_stats["health"] > 0:
                player_damage = random.randint(1, 10) + monster_stats["attack"]
                print("The {} attacks you back and deals {} damage!".format(monster_name, player_damage))
                player["health"] -= player_damage
        elif attack_choice == "2":
            print("You ran away from the {}!".format(monster_name))
            break
        else:
            print("Invalid choice. Please try again.")
    
    if player["health"] <= 0:
        print("You died!")
    elif monster_stats["health"] <= 0:
        print("You defeated the {} and gained 10 gold!".format(monster_name))
        player["gold"] += 10
        loot = random.choice(["Health potion", "Mana potion", "Gold coin"])
        print("You found a {}!".format(loot))
        player["inventory"].append(loot)

def check_inventory():
    print("\nInventory:")
    for item in player["inventory"]:
        print("-", item)

def rest():
    global player
    
    heal_amount = random.randint(10, 20)
    print("\nYou rest and regain {} health.".format(heal_amount))
    player["health"] += heal_amount

# Oyunu başlat
if __name__ == "__main__":
    main()
