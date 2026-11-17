import json
from pathlib import Path

FILE = Path("inventory.json")

def load_inventory():
    if FILE.exists():
        return json.loads(FILE.read_text(encoding="utf-8"))
    return {}

def save_inventory(inventory):
    FILE.write_text(
        json.dumps(inventory, indent=4, ensure_ascii=False),
        encoding="utf-8"
    )

def add_product(inventory):
    name = input("Product name: ").strip()

    try:
        quantity = int(input("Quantity: "))
        minimum = int(input("Minimum stock: "))

        if not name or quantity < 0 or minimum < 0:
            raise ValueError

        inventory[name] = {
            "quantity": quantity,
            "minimum": minimum
        }

        save_inventory(inventory)
        print("Product saved.")

    except ValueError:
        print("Please enter valid values.")

def show_inventory(inventory):
    if not inventory:
        print("Inventory is empty.")
        return

    for name, data in inventory.items():
        status = (
            "LOW STOCK"
            if data["quantity"] <= data["minimum"]
            else "OK"
        )

        print(
            f"{name}: {data['quantity']} units "
            f"(Minimum: {data['minimum']}) - {status}"
        )

def main():
    inventory = load_inventory()

    while True:
        print("\n1. Add product")
        print("2. Show inventory")
        print("3. Exit")

        choice = input("Choose: ")

        if choice == "1":
            add_product(inventory)
        elif choice == "2":
            show_inventory(inventory)
        elif choice == "3":
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
