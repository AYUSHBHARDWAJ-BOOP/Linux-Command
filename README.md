# Linux-Command
Linux custom commands 
1.
Task is-  I want a manual page of command so that I can see the full documentation of the command.
For example if you execute the command
man ls
as output we get the doc and usage guidelines. Similarly if I execute man internsctl I want
to see the manual of my command.

solution-
# Use a text editor to create/edit the manual page file
nano internsctl.1

.TH INTERNSCTL 1 "January 2024" "Internsctl Manual"
.SH NAME
internsctl \- your command's description

.SH SYNOPSIS
.B internsctl
[\fIOPTIONS\fP]

.SH DESCRIPTION
This is a brief description of your command.

.SH OPTIONS
.TP
.B --option1
Description of option 1.

.TP
.B --option2
Description of option 2.

.SH SEE ALSO
More information or references.

.SH AUTHOR
Your name or contact information.

manual page-
sudo gzip -c internsctl.1 > /usr/share/man/man1/internsctl.1.gz

python internsctl --help


2.

#!/usr/bin/env python

import sys
import os
import argparse
from datetime import datetime

def get_cpu_info():
    # Implement logic to get CPU information (similar to lscpu command)
    # Example: You can use subprocess or other Python modules to fetch CPU information
    print("CPU Information (Similar to lscpu)")

def get_memory_info():
    # Implement logic to get memory information (similar to free command)
    # Example: You can use subprocess or other Python modules to fetch memory information
    print("Memory Information (Similar to free command)")

def create_user(username):
    # Implement logic to create a new user
    # Example: You can use subprocess or other Python modules to create a new user
    print(f"Creating user: {username}")

def list_users(sudo_only):
    # Implement logic to list users (with or without sudo permissions)
    # Example: You can use subprocess or other Python modules to list users
    if sudo_only:
        print("Listing users with sudo permissions")
    else:
        print("Listing all regular users")

def get_file_info(filename, options):
    # Implement logic to get file information with optional details
    # Example: You can use os.path or other Python modules to get file information
    file_info = {
        "File": filename,
        "Access": os.access(filename, os.R_OK) and "r" or "-",
        # Add other file information fields as needed
    }

    if "s" in options or "size" in options:
        file_info["Size(B)"] = os.path.getsize(filename)

    if "p" in options or "permissions" in options:
        file_info["Access"] = os.access(filename, os.R_OK) and "r" or "-"
        # Add other permission-related information as needed

    if "o" in options or "owner" in options:
        file_info["Owner"] = os.path.getlogin()

    if "m" in options or "last-modified" in options:
        file_info["Modify"] = datetime.fromtimestamp(os.path.getmtime(filename)).strftime('%Y-%m-%d %H:%M:%S.%f %z')

    # Print the file information
    for key, value in file_info.items():
        print(f"{key}: {value}")

def main():
    parser = argparse.ArgumentParser(description="Custom command-line tool - internsctl")
    subparsers = parser.add_subparsers(title="Commands", dest="command")

    # Part 1
    cpu_parser = subparsers.add_parser("cpu", help="Get CPU information")
    cpu_parser.set_defaults(func=get_cpu_info)

    memory_parser = subparsers.add_parser("memory", help="Get memory information")
    memory_parser.set_defaults(func=get_memory_info)

    # Part 2
    user_parser = subparsers.add_parser("user", help="User-related operations")
    user_subparsers = user_parser.add_subparsers(title="User Commands", dest="user_command")

    create_user_parser = user_subparsers.add_parser("create", help="Create a new user")
    create_user_parser.add_argument("username", help="Username to create")
    create_user_parser.set_defaults(func=create_user)

    list_users_parser = user_subparsers.add_parser("list", help="List users")
    list_users_parser.add_argument("--sudo-only", action="store_true", help="List users with sudo permissions only")
    list_users_parser.set_defaults(func=list_users)

    # Part 3
    file_parser = subparsers.add_parser("file", help="File-related operations")
    file_parser.add_argument("file_name", help="File name")
    file_parser.add_argument("options", nargs="*", help="File information options")
    file_parser.set_defaults(func=get_file_info)

    args = parser.parse_args()
    if not vars(args):
        parser.print_help()
        sys.exit(1)

    args.func(**vars(args))

if __name__ == "__main__":
    main()


