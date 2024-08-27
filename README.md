Description
This script is designed to perform a comprehensive enumeration of a remote Windows machine using rpcclient. It queries server information, domain users, network shares, users by RIDs, groups, password policies, logged-on users, active sessions, and LSA policy information.

It allows you to specify the username, password, and target IP address via command-line arguments, providing a flexible way to perform enumerations without needing to modify the script directly.

Options
-U: username
-P: password
-t: target Ip address

Usage: 
./rpc_enum.sh -U <username> -P <password> -t <target_ip>
