# Expandable Universally Unique Identifier

An expandable universally unique identifier (XUUID) is a variable-sized label for identifying resources.

### Structure

Field|Size      |Description
-----|----------|-----------
Time |*variable*|an [expandable timestamp](https://github.com/ghoomfrog/universe/blob/main/computer%20science/expandable%20timestamp.md)
End  |64b       |last bit index of noise (noise size minus 1)
Noise|*variable*|random bits
