# vbox.sh
these are my personal notes

- This is a bash script i use to start and stop Oracle VirtualBox machines on my homeserver.
- If it's useful to someone, please enjoy!

![vbox](https://github.com/tommyh1/vbox.sh/blob/346191806befeb7017896e585bd3632a9a534c29/vbox.png)

```bash
#!/bin/bash

###########################
# VirtualBox Management Script
###########################

# Function to display the menu
display_menu() {
    clear
    echo "VirtualBox Management Menu"
    echo "--------------------------"
    echo "1. List all VMs"
    echo "2. Start a VM"
    echo "3. Stop a VM"
    echo "4. Restart a VM"
    echo "5. Show Memory Consumption of Running VMs"
    echo "6. Exit"
    echo "--------------------------"
}

# Function to list all VMs
list_vms() {

    all_vms=$(vboxmanage list vms)
    running_vms=$(vboxmanage list runningvms)
    echo " "    
    echo "All VMs:"
    echo " "
    num=1
    while read -r vm; do
        vm_name=$(echo "$vm" | awk -F '"' '{print $2}')
        if echo "$running_vms" | grep -q "$vm_name"; then
            echo -e "\e[1;32m$num. $vm_name (Running)\e[0m"
        else
            echo "$num. $vm_name"
        fi
        ((num++))
    done <<< "$all_vms"
}

# Function to start a VM
start_vm() {
    list_vms
    echo " "
    read -p "Enter the number of the VM to start (or press Enter to go back to the main menu): " vm_num

    # If no input is provided, return to the main menu
    if [ -z "$vm_num" ]; then
        return
    fi

    # Check if the input is a number
    if ! [[ "$vm_num" =~ ^[0-9]+$ ]]; then
    echo " "
        echo "Invalid input. Please enter a valid number."
    echo " "
        return
    fi

    vm_count=$(vboxmanage list vms | wc -l)

    # Check if the input is within the valid range of VMs
    if [ "$vm_num" -le 0 ] || [ "$vm_num" -gt "$vm_count" ]; then
    echo " "
        echo "Invalid input. Please enter a valid number."
    echo " "
        return
    fi

    vm_name=$(vboxmanage list vms | sed -n "${vm_num}p" | awk -F '"' '{print $2}')
    vboxmanage startvm "$vm_name" --type headless
}

# Function to stop a VM
stop_vm() {
    
    list_vms
    echo " "
    read -p "Enter the number of the VM to stop (or press Enter to go back to the main menu): " vm_num
    echo " "
    # If no input is provided, return to the main menu
    if [ -z "$vm_num" ]; then
        return
    fi

    # Check if the input is a number
    if ! [[ "$vm_num" =~ ^[0-9]+$ ]]; then
    echo " "
        echo "Invalid input. Please enter a valid number."
    echo " "
        return
    fi

    vm_count=$(vboxmanage list vms | wc -l)

    # Check if the input is within the valid range of VMs
    
    if [ "$vm_num" -le 0 ] || [ "$vm_num" -gt "$vm_count" ]; then
    
        echo "Invalid input. Please enter a valid number."
    echo " "
        return
    fi

    vm_name=$(vboxmanage list vms | sed -n "${vm_num}p" | awk -F '"' '{print $2}')
    vboxmanage controlvm "$vm_name" poweroff
}

# Function to restart a VM
restart_vm() {
    echo " "
    list_vms

    echo " "
        read -p "Enter the number of the VM to restart (or press Enter to go back to the main menu): " vm_num
    echo " "
    # If no input is provided, return to the main menu
    if [ -z "$vm_num" ]; then
        return
    fi

    # Check if the input is a number
    echo " "
    if ! [[ "$vm_num" =~ ^[0-9]+$ ]]; then
    echo " "
        echo "Invalid input. Please enter a valid number."
    echo " "
        return
    fi

    vm_count=$(vboxmanage list vms | wc -l)

    # Check if the input is within the valid range of VMs
    echo " "
    if [ "$vm_num" -le 0 ] || [ "$vm_num" -gt "$vm_count" ]; then
    echo " "
        echo "Invalid input. Please enter a valid number."
    echo " "
        return
    fi

    vm_name=$(vboxmanage list vms | sed -n "${vm_num}p" | awk -F '"' '{print $2}')
    vboxmanage controlvm "$vm_name" reset
}

# Function to show total memory consumption of running VMs
show_total_memory_consumption() {
    echo " "
    total_memory=$(vboxmanage list runningvms --long | grep -E '^Name:|^Memory size' | awk '/Name:/ {name=$NF} /Memory size/ {sub(/[^0-9]+/, "", $NF); memory[name]+=$NF} END {for (vm in memory) print vm, memory[vm] " MB"}')
    if [ -z "$total_memory" ]; then
        echo "No running VMs."
    else
        echo "Total Memory Consumption of Running VMs:"
        echo "$total_memory"
    fi
}

# Main script
while true; do
    display_menu
    echo " "
    read -p "Enter your choice: " choice
    echo " "
    case $choice in
        1)
            list_vms
            ;;
        2)
            start_vm
            ;;
        3)
            stop_vm
            ;;
        4)
            restart_vm
            ;;
        5)
            show_total_memory_consumption
            ;;
        6)
            echo "Exiting..."
            exit 0
            ;;
        *)
            echo "Invalid choice. Please enter a number between 1 and 6."
            ;;
    esac
    echo " "
    read -p "Press Enter to continue..."
done

```
