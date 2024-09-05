

# **Log File Analysis Script**

## **Overview**

This Python script reads a server log file, extracts IP addresses, and logs the number of successful and failed attempts. It then saves this data into a CSV file and prints the results.

## **Features**

1. **Read Log File**: Opens and reads a server log file.
2. **Extract Data**:
   - Extract IP addresses from log entries.
   - Extract the number of successful and failed attempts.
3. **Save Output**: Outputs the extracted data into a CSV file.
4. **Print Results**: Displays the DataFrame with the results.

## **Requirements**

- Python 3.x
- Pandas library
- Regular expression (re) library

You can install the required libraries using pip:
```bash
pip install pandas
```

## **Script Details**

### **Code Explanation**

1. **Reading the File**: 
   - The script opens a file named `serverlogs.log` for reading.

2. **Extract IP Addresses and Logs**:
   - **IP Addresses**: Utilizes a regular expression pattern to find and extract IP addresses from each line in the log file.
   - **Success and Failed Attempts**: Parses the number of successful and failed attempts from each log entry. Assumes these numbers are in specific positions in the log line.

3. **Save Output**:
   - Creates a DataFrame with the extracted data and saves it to `output.csv`.
   - The DataFrame includes columns for IP Addresses, Success, and Failed attempts.

4. **Send Email** (Note: This functionality is not implemented in the current script but can be added if needed).

### **Code**

```python
import re
import pandas as pd
import pprint

# Open log file
logfile = open("serverlogs.log", "r")

# Regular expression pattern for IP addresses
pattern = r"((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)"

# Lists to store extracted data
ip_addrs_lst = []
failed_lst = []
success_lst = []

# Process each line in the log file
for log in logfile:
    ip_add = re.search(pattern, log)
    if ip_add:
        ip_addrs_lst.append(ip_add.group())
    else:
        ip_addrs_lst.append("No IP Found")

    lst = log.split(" ")
    failed_lst.append(int(lst[-1]))
    success_lst.append(int(lst[-4]))

# Summarize results
total_failed = sum(failed_lst)
total_success = sum(success_lst)
ip_addrs_lst.append("Total")
success_lst.append(total_success)
failed_lst.append(total_failed)

# Create DataFrame and save to CSV
df = pd.DataFrame(columns=['IP Address', "Success", "Failed"])
df['IP Address'] = ip_addrs_lst
df["Success"] = success_lst
df["Failed"] = failed_lst

df.to_csv("output.csv", index=False)

# Print DataFrame
pprint.pprint(df)
```

## **Usage**

1. Place your server log file named `serverlogs.log` in the same directory as the script.
2. Run the script using Python:
   ```bash
   python script_name.py
   ```
3. The script will create an `output.csv` file containing the IP addresses, success counts, and failure counts.

## **Notes**

- Ensure that the log file format matches the expected structure for successful parsing.
- The script currently does not include functionality to send emails. You may add email sending features using libraries like `smtplib` if needed.

## **Future Improvements**

- Implement email notification to send the CSV file.
- Add error handling for different log formats.
- Enhance data extraction logic to handle more diverse log entries.

---

