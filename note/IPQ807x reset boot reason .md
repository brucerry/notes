```
#!/bin/sh

# Define paths for regmap interaction
REGMAP_ADDR="/sys/kernel/debug/regmap/0-00/address" 
REGMAP_COUNT="/sys/kernel/debug/regmap/0-00/count" 
REGMAP_DATA="/sys/kernel/debug/regmap/0-00/data" 

# Ensure debugfs is accessible before continuing
if [ ! -f "$REGMAP_ADDR" ]; then
    echo "Error: Regmap debugfs interface not found." 
    exit 1
fi

# Set up the regmap to read 8 registers starting at 0x8c0
echo "0x8c0" > "$REGMAP_ADDR" 
echo "8"     > "$REGMAP_COUNT" 

# Read specific registers using awk
VAL_8C0=$(head "$REGMAP_DATA" | awk '/08c0:/ {print $2}')
VAL_8C4=$(head "$REGMAP_DATA" | awk '/08c4:/ {print $2}')
VAL_8C5=$(head "$REGMAP_DATA" | awk '/08c5:/ {print $2}')
VAL_8C7=$(head "$REGMAP_DATA" | awk '/08c7:/ {print $2}')

# Differentiate the boot type based on the strict documentation matrix
if [ "$VAL_8C0" = "21" ] && [ "$VAL_8C4" = "80" ] && [ "$VAL_8C5" = "02" ] && [ "$VAL_8C7" = "80" ]; then
    echo "Boot Reason: Cold Boot" 
elif [ "$VAL_8C0" = "20" ] && [ "$VAL_8C4" = "80" ] && [ "$VAL_8C5" = "00" ] && [ "$VAL_8C7" = "04" ]; then
    echo "Boot Reason: Power Cycle" 
elif [ "$VAL_8C0" = "20" ] && [ "$VAL_8C4" = "40" ] && [ "$VAL_8C5" = "00" ]; then
    echo "Boot Reason: Warm Reset" 
else
    echo "Boot Reason: Something Else (Watchdog Timeout, etc.)" 
    echo "Debug values -> 08c0:$VAL_8C0, 08c4:$VAL_8C4, 08c5:$VAL_8C5, 08c7:$VAL_8C7" 
fi
```
