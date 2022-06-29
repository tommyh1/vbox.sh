# vbox.sh

- This is a bash script i use to start and stop Oracle VirtualBox machines on my homeserver.
- It's my personal script, but if it's useful to someone, please enjoy!

![vbox](https://github.com/tommyh1/vbox.sh/blob/346191806befeb7017896e585bd3632a9a534c29/vbox.png)



```bash
#!/bin/bash
trap '' 2
while true
do
clear
echo   
    echo "  VirtualBox Menu  "
    echo "  ---------------  "
    echo 
    echo "  1. List Installed VirtualBoxes "
    echo "  2. List Running VirtualBoxes "
    echo
    echo "  3. Run Nextcloud        33. Stop Nextcloud "
    echo "  4. Run Win10 Pro        44. Stop Win10 Pro "
    echo "  5. Run Win10 Pro Work   55. Stop Win10 Pro Work "
    echo "  6. Run Kali             66. Stop Kali "
    echo "  7. Run Calendar         77. Stop Calendar "
    echo
    echo "  ~~~~~~~~~~ "
    echo "  x. to exit "
    echo "  ~~~~~~~~~~ "
    echo
    echo -e "  Your choice: \c" echo -e "\e"
    read input
    clear
    case "$input" in
        1)  VBoxManage list vms ;;
        2)  VBoxManage list runningvms ;;
        3)  VBoxManage startvm "nextcloud" --type headless ;;
        33) VBoxManage controlvm "nextcloud" poweroff --type headless ;;
        4)  VBoxManage startvm "win10pro" --type headless ;;
        44) VBoxManage controlvm "win10pro" poweroff --type headless ;;
        5)  VBoxManage startvm "work" --type headless ;;
        55) VBoxManage controlvm "work" poweroff --type headless ;;
        6)  VBoxManage startvm "Kali" --type headless ;;
        66) VBoxManage controlvm "Kali" poweroff --type headless ;;
        7)  VBoxManage startvm "calserver" --type headless ;;
        77) VBoxManage controlvm "calserver" poweroff --type headless ;;
        x) exit ;;
    esac
    echo
    echo -e "  Enter to continue \c"
    read input

done

```
