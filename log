#!/bin/bash

help() {
  echo "Usage: $(basename "$0") [-v] [-n SECONDS] [-o FILE] [-t] [-h] <command>"
  echo "Run a command every N seconds."
  echo
  echo "Options:"
  echo "  -v            Print output to terminal"
  echo "  -n SECONDS    Interval in seconds (default: 1)"
  echo "  -o FILE       Log output to FILE (only if -o used)"
  echo "  -t            Prepend each line with Unix timestamp"
  echo "  -h, --help    Show this help message and exit"
  exit 0
}

# Default values
verbose=false
output_file=""
interval=1
timestamp_flag=false

# Parse flags
while getopts "vn:o:th" opt; do
  case $opt in
    v) verbose=true ;;
    n) interval="$OPTARG" ;;
    o) output_file="$OPTARG" ;;
    t) timestamp_flag=true ;;
    h) help ;;
    *) help ;;
  esac
done
shift $((OPTIND-1))

# Command to run
command="$1"
if [[ -z "$command" ]]; then
  echo "Error: No command provided." >&2
  help
fi

# Main loop
while true; do
  output=$(eval "$command")
  timestamp=""
  if [[ "$timestamp_flag" == true ]]; then
    timestamp="[$(date +%s)] "
  fi

  # Output logic
  if [[ "$verbose" == true && -n "$output_file" ]]; then
    echo "${timestamp}${output}" | tee -a "$output_file"
  elif [[ "$verbose" == true ]]; then
    echo "${timestamp}${output}"
  elif [[ -n "$output_file" ]]; then
    echo "${timestamp}${output}" >> "$output_file"
  else
    echo "${timestamp}${output}" >> "$(echo "$command" | awk '{print $1}').log"
  fi

  sleep "$interval"
done
