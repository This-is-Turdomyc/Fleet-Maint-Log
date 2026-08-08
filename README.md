from datetime import datetime
import json
import os

DATA_FILE = "fleet_data.json"

def load_data():
    if not os.path.exists(DATA_FILE):
        return {
            "vessels": [
                {"id": "Vessel 01", "name": "Boston Whaler Outrage", "year": "2021", "type": "Outboard", "status": "Active"},
                {"id": "Vessel 02", "name": "Grady-White Freedom", "year": "2020", "type": "Twin Outboard", "status": "Active"}
            ],
            "logs": []
        }
    with open(DATA_FILE, "r") as f:
        return json.load(f)

def save_data(data):
    with open(DATA_FILE, "w") as f:
        json.dump(data, f, indent=4)

def display_fleet(data):
    print("\n--- Active Fleet ---")
    for v in data["vessels"]:
        print(f"ID: {v['id']} | Name: {v['name']} ({v['year']}) | Type: {v['type']} | Status: {v['status']}")

def display_logs(data):
    print("\n--- Maintenance Logs ---")
    if not data["logs"]:
        print("No logs recorded yet.")
        return
    for log in data["logs"]:
        print(f"\n[{log['date']}] {log['vessel']} - {log['type']}")
        print(f"  Engine Hours: {log['hours']}")
        print(f"  Performed By: {log['technician']}")
        print(f"  Work Done: {log['work']}")
        print(f"  Parts Used: {log['parts']}")
        print(f"  Notes/Cost: {log['notes']}")

def add_log(data):
    print("\n--- Add Maintenance Log ---")
    display_fleet(data)
    vessel_id = input("Enter Vessel ID: ").strip()
    date = input(f"Enter Date (YYYY-MM-DD) [default: {datetime.today().strftime('%Y-%m-%d')}]: ").strip() or datetime.today().strftime("%Y-%m-%d")
    hours = input("Enter Current Engine Hours: ").strip()
    m_type = input("Maintenance Type (Routine / Repair / Inspection / Emergency): ").strip()
    technician = input("Performed By: ").strip()
    work = input("Work Completed Summary: ").strip()
    parts = input("Parts Used: ").strip()
    notes = input("Notes / Cost: ").strip()

    new_log = {
        "vessel": vessel_id,
        "date": date,
        "hours": hours,
        "type": m_type,
        "technician": technician,
        "work": work,
        "parts": parts,
        "notes": notes
    }

    data["logs"].append(new_log)
    save_data(data)
    print("✅ Maintenance log added successfully!")

def main():
    data = load_data()
    while True:
        print("\n=== 🛥️ FLEET MAINTENANCE MANAGER ===")
        print("1. View Active Fleet")
        print("2. View Maintenance Logs")
        print("3. Add New Log Entry")
        print("4. Exit")

        choice = input("Select an option (1-4): ").strip()

        if choice == "1":
            display_fleet(data)
        elif choice == "2":
            display_logs(data)
        elif choice == "3":
            add_log(data)
        elif choice == "4":
            print("Exiting.")
            break
        else:
            print("❌ Invalid choice.")

if __name__ == "__main__":
    main()
