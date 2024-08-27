#!/bin/bash

# Function to print a section header
print_header() {
    echo "=================================================="
    echo "$1"
    echo "=================================================="
    echo ""
}

# Function to show usage information
usage() {
    echo "Usage: $0 -U <username> -P <password> -t <target>"
    exit 1
}

# Parse command-line arguments
while getopts "U:P:t:" opt; do
    case "$opt" in
        U) USER=$OPTARG ;;
        P) PASSWORD=$OPTARG ;;
        t) TARGET_IP=$OPTARG ;;
        *) usage ;;
    esac
done

# Check if all arguments are provided
if [ -z "$USER" ] || [ -z "$PASSWORD" ] || [ -z "$TARGET_IP" ]; then
    usage
fi

# Confirm target and user
echo "You have entered the following details:"
echo "Target IP: $TARGET_IP"
echo "Username: $USER"
read -p "Is this information correct? (y/n): " confirm

if [ "$confirm" != "y" ]; then
    echo "Exiting script. Please rerun with the correct information."
    exit 1
fi

# Enumerate Server Info
print_header "Enumerating Server Information"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "srvinfo" 2>/dev/null

# Enumerate Domains
print_header "Enumerating Domains"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "enumdomusers" 2>/dev/null

# Enumerate Shares
print_header "Enumerating Network Shares"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "netshareenumall" 2>/dev/null

# Enumerate Users with RIDs
print_header "Enumerating Users with RIDs"
for i in $(seq 500 1100); do
    rid_hex=$(printf '0x%x\n' $i)
    result=$(rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "queryuser $rid_hex" 2>/dev/null)

    if echo "$result" | grep -q "User Name"; then
        echo "RID: $rid_hex"
        echo "$result" | grep -E "User Name|user_rid|group_rid"
        echo ""
    fi
done

# Enumerate Groups
print_header "Enumerating Groups"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "enumdomgroups" 2>/dev/null

# Enumerate Policies
print_header "Enumerating Password Policy Information"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "getdompwinfo" 2>/dev/null

# Enumerate Logged on Users
print_header "Enumerating Logged On Users"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "enumloggedonusers" 2>/dev/null

# Enumerate Sessions
print_header "Enumerating Active Sessions"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "net sess" 2>/dev/null

# Enumerate LSA Policy Information (may require credentials)
print_header "Enumerating LSA Policy Information"
rpcclient -N -U "$USER%$PASSWORD" $TARGET_IP -c "lsaquery" 2>/dev/null
