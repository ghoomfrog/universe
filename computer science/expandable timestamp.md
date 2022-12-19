# Expandable Timestamp

An expandable timestamp is a variable-sized millisecond-precise timestamp.

### Structure

Field    |Size      |Description
---------|----------|-----------
End      |64b       |last bit index of timestamp (timestamp size minus 1)
Timestamp|*variable*|
