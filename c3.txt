#!/bin/bash

# Initialize variables
SECONDS=0
START_TIME=0
RUNNING=false

# Function to start the stopwatch
start() {
    if [ "$RUNNING" = true ]; then
        echo "Stopwatch is already running."
    else
        START_TIME=$(date +%s)
        RUNNING=true
        echo "Stopwatch started."
        while $RUNNING; do
            # Calculate elapsed time
            ELAPSED_TIME=$((SECONDS + $(date +%s) - START_TIME))
            # Format time in HH:MM:SS
            FORMATTED_TIME=$(printf "%02d:%02d:%02d\n" $(($ELAPSED_TIME / 3600)) $(($ELAPSED_TIME % 3600 / 60)) $(($ELAPSED_TIME % 60)))
            echo -ne "\r$FORMATTED_TIME"
            sleep 1
            # Prompt for Stop, Reset, or Exit options while stopwatch is running
            echo -e "\nOptions: Stop (s) | Reset (r) | Exit (e)"
            read -t 1 -n 1 user_input
            case $user_input in
                s) stop ;;
                r) reset ;;
                e) stop; echo "Exiting..."; exit 0 ;;
            esac
        done
    fi
}

# Function to stop the stopwatch
stop() {
    if [ "$RUNNING" = true ]; then
        RUNNING=false
        SECONDS=$((SECONDS + $(date +%s) - START_TIME))
        echo -e "\nStopwatch stopped."
    else
        echo "Stopwatch is not running."
    fi
}

# Function to reset the stopwatch
reset() {
    SECONDS=0
    RUNNING=false
    echo -e "\nStopwatch reset."
}

# Main menu loop
while true; do
    echo "Stopwatch - Choose an option:"
    echo "1) Start"
    echo "2) Exit"
    read -p "Enter your choice: " choice

    case $choice in
        1)
            start
            ;;
        2)
            stop
            echo "Exiting..."
            exit 0
            ;;
        *)
            echo "Invalid option. Please choose again."
            ;;
    esac
done
