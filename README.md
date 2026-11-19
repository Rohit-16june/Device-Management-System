"""
DEVICE MANAGEMENT SYSTEM
Operating Systems Mini Project
Language: Python

This program simulates basic I/O device management:
- Add and remove devices
- View and search devices
- Allocate an available device to a user/process
- Release an allocated device
"""

class Device:
    def __init__(self, device_id, name, device_type):
        self.device_id = device_id
        self.name = name
        self.device_type = device_type
        self.status = "Available"
        self.allocated_to = None

    def display(self):
        owner = self.allocated_to if self.allocated_to else "-"
        print(f"{self.device_id:<10} {self.name:<18} "
              f"{self.device_type:<15} {self.status:<12} {owner}")


class DeviceManagementSystem:
    def __init__(self):
        self.devices = []

    def find_device(self, device_id):
        for device in self.devices:
            if device.device_id.lower() == device_id.lower():
                return device
        return None

    def add_device(self):
        print("\n--- Add Device ---")
        device_id = input("Enter device ID: ").strip()
        if not device_id:
            print("Device ID cannot be empty.")
            return
        if self.find_device(device_id):
            print("A device with this ID already exists.")
            return

        name = input("Enter device name: ").strip()
        device_type = input("Enter device type (Input/Output): ").strip().title()

        if not name or device_type not in ("Input", "Output"):
            print("Enter a valid name and type (Input or Output).")
            return

        self.devices.append(Device(device_id, name, device_type))
        print("Device added successfully.")

    def remove_device(self):
        print("\n--- Remove Device ---")
        device_id = input("Enter device ID to remove: ").strip()
        device = self.find_device(device_id)

        if device is None:
            print("Device not found.")
        elif device.status == "Allocated":
            print("Cannot remove an allocated device. Release it first.")
        else:
            self.devices.remove(device)
            print("Device removed successfully.")

    def view_devices(self):
        print("\n--- Device List ---")
        if not self.devices:
            print("No devices are registered.")
            return

        print(f"{'ID':<10} {'Name':<18} {'Type':<15} {'Status':<12} Allocated To")
        print("-" * 75)
        for device in self.devices:
            device.display()

    def search_device(self):
        print("\n--- Search Device ---")
        device_id = input("Enter device ID: ").strip()
        device = self.find_device(device_id)

        if device:
            print(f"ID: {device.device_id}")
            print(f"Name: {device.name}")
            print(f"Type: {device.device_type}")
            print(f"Status: {device.status}")
            print(f"Allocated to: {device.allocated_to or '-'}")
        else:
            print("Device not found.")

    def allocate_device(self):
        print("\n--- Allocate Device ---")
        device_id = input("Enter device ID: ").strip()
        device = self.find_device(device_id)

        if device is None:
            print("Device not found.")
            return
        if device.status == "Allocated":
            print(f"Device is already allocated to {device.allocated_to}.")
            return

        process_name = input("Enter user/process name: ").strip()
        if not process_name:
            print("User/process name cannot be empty.")
            return

        device.status = "Allocated"
        device.allocated_to = process_name
        print(f"{device.name} allocated successfully to {process_name}.")

    def release_device(self):
        print("\n--- Release Device ---")
        device_id = input("Enter device ID to release: ").strip()
        device = self.find_device(device_id)

        if device is None:
            print("Device not found.")
        elif device.status == "Available":
            print("This device is already available.")
        else:
            previous_owner = device.allocated_to
            device.status = "Available"
            device.allocated_to = None
            print(f"Device released successfully (was allocated to {previous_owner}).")

    def run(self):
        while True:
            print("\n========== DEVICE MANAGEMENT SYSTEM ==========")
            print("1. Add Device")
            print("2. Remove Device")
            print("3. View All Devices")
            print("4. Search Device")
            print("5. Allocate Device")
            print("6. Release Device")
            print("7. Exit")

            choice = input("Enter your choice (1-7): ").strip()

            if choice == "1":
                self.add_device()
            elif choice == "2":
                self.remove_device()
            elif choice == "3":
                self.view_devices()
            elif choice == "4":
                self.search_device()
            elif choice == "5":
                self.allocate_device()
            elif choice == "6":
                self.release_device()
            elif choice == "7":
                print("Thank you for using Device Management System.")
                break
            else:
                print("Invalid choice. Please enter a number from 1 to 7.")


if __name__ == "__main__":
    system = DeviceManagementSystem()

    # Sample devices are added so the project can be tested immediately.
    system.devices.append(Device("D101", "Keyboard", "Input"))
    system.devices.append(Device("D102", "Mouse", "Input"))
    system.devices.append(Device("D103", "Printer", "Output"))
    system.devices.append(Device("D104", "Monitor", "Output"))

    system.run()# Device-Management-System
Operating systems mini project using python
