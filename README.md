# Bash-Scanner
Self bash scanner. 
🔍 Port ScannerA simple Bash script that scans ports 1-30 on 127.0.0.1 (localhost) to identify open ports. It uses /dev/tcp for connectivity testing and includes a timeout mechanism to avoid infinite waits.
🚀 How It Works1️⃣ The script displays a message and waits 3 seconds before starting the scan.2️⃣ It loops through ports 1 to 30, attempting to establish a connection.3️⃣ If a connection succeeds, the port is marked as open.4️⃣ If no open ports are found, the script notifies the user.

📜 Code Structure

#!/bin/bash

greenColour="\e[0;32m\033[1m"
redColour="\e[0;31m\033[1m"
yellowColour="\e[0;33m\033[1m"
endColour="\033[0m\e[0m"

function ctrl_c() {
    echo -e "\n\n[!] Timeout... \n"
    exit 1
}

trap ctrl_c INT

echo -e "\n\n ${yellowColour}[+] ${endColour} Scanning ports on 127.0.0.1 ....\n"
sleep 3 # Wait 3 seconds before starting

found_ports=0 # Counter for open ports

for port in $(seq 1 30); do
  timeout 1 bash -c "echo '' > /dev/tcp/127.0.0.1/$port" 2>/dev/null && {
    echo "[+] Port $port - OPEN"
    ((found_ports++))
  } &
done

wait

if [ "$found_ports" -eq 0 ]; then
    echo -e "\n\n ${greenColour}[!] ${endColour} No open ports found. \n"
    
fi

⚠️ Notes✅ Works on Linux/macOS with Bash.
✅ Scans localhost (127.0.0.1) only.
✅ Modify the port range by changing seq 1 30.
✅ Run as a normal user (no root required).
