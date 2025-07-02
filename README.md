# tblastn_merge_HSPs_from_same_alignment.py

## Overview

This script merges High-scoring Segment Pairs (HSPs) from tblastn output that belong to the same alignment, typically due to frameshifts in the subject sequence. When BLAST considers two or more HSPs to be part of the same alignment, it recomputes a new e-value with sum-statistics. All HSPs of the same alignment thus share the same e-value but keep their own bitscore.

This script identifies such HSPs (by matching query, subject, e-value, strand, and different bitscore) and merges them into a single output line, extending the coordinates and updating the output subject identifier to include the merged region.

## Features

- Merges HSPs from tblastn output that belong to the same alignment (frameshift-aware)
- Works with any column order or extra columns
- Only requires a minimal set of fields
- Preserves all other columns if present

## Required Fields

The input file must contain the following columns (case-sensitive):

- `query acc.ver`
- `subject acc.ver`
- `evalue`
- `sbjct frame`
- `bit score`
- `q. start`
- `q. end`
- `s. start`
- `s. end`

All other columns are optional and will be preserved in the output.

## Usage

```bash
python tblastn_merge_HSPs_from_same_alignment.py <inputfile> [-o <outputfile>]
```
<inputfile>: tblastn output file in format 7 (with header line starting with # Fields:)
-o <outputfile>: (optional) output file (default: stdout)
```
Example
```bash
python tblastn_merge_HSPs_from_same_alignment.py my_tblastn.out -o merged.out
```

## Input Format
The script expects a tblastn output file in format 7, with a header line like:

The order of columns can vary, and extra columns are allowed.

## Output
The output is the same as the input, but merged HSPs are output as a single line with subject acc.ver extended as subject_start_end.
All columns present in the input are preserved in the output.

## How it works
HSPs are merged if they have the same query, subject, e-value, and strand, but different bitscores.
Coordinates are extended to cover the merged region.
Some fields (e.g., % identity, alignment length, mismatches, gap opens, bit score) are set to NaN for merged HSPs.

## Limitations
Only the first # Fields: header is used to determine column names. If your file contains multiple headers, only the first is used.
The script expects tab-delimited files.

## License
MIT License