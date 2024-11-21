#!/bin/bash

# Function to calculate total CPU usage
cpu_usage() {
    echo "=== CPU Usage ==="
    echo "$(top -bn1 | grep "Cpu(s)" | awk '{print "CPU Usage: " $2 + $4 "%"}')"
    echo ""
}

# Function to calculate memory usage
memory_usage() {
    echo "=== Memory Usage ==="
    free -m | awk 'NR==2{printf "Memory Used: %sMB / %sMB (%.2f%%)\n", $3, $2, $3*100/$2 }'
    echo ""
}

# Function to calculate disk usage
disk_usage() {
    echo "=== Disk Usage ==="
    df -h --total | awk '/total/{printf "Disk Used: %s / %s (%s)\n", $3, $2, $5}'
    echo ""
}

# Function to get top 5 CPU-intensive processes
top_cpu_processes() {
    echo "=== Top 5 Processes by CPU Usage ==="
    ps -eo pid,comm,%cpu --sort=-%cpu | head -n 6
    echo ""
}

# Function to get top 5 memory-intensive processes
top_memory_processes() {
    echo "=== Top 5 Processes by Memory Usage ==="
    ps -eo pid,comm,%mem --sort=-%mem | head -n 6
    echo ""
}

# Stretch Goal: Additional Stats
additional_stats() {
    echo "=== Additional Stats ==="
    echo "OS Version: $(lsb_release -d | cut -f2)"
    echo "Uptime: $(uptime -p)"
    echo "Load Average: $(uptime | awk -F'load average:' '{print $2}')"
    echo "Logged in Users: $(who | wc -l)"
    echo "Failed Login Attempts: $(grep "Failed password" /var/log/auth.log | wc -l)"
    echo ""
}

# Run all functions
cpu_usage
memory_usage
disk_usage
top_cpu_processes
top_memory_processes
additional_stats
