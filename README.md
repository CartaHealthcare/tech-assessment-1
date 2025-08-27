# Mock Export Challenge

## Overview

You are provided with a mock server that simulates healthcare data exports. Each export
consists of one or more downloadable datasets in CSV format. Your task is to write a
program that processes these datasets and computes summary statistics.

The goal of this exercise is not only correctness but also clarity, reasoning about
trade-offs, and handling performance constraints. You are encouraged to use internet and
AI resources as part of your process. Be ready to explain and justify your approach in
the follow-up discussion.

## Setup

* Install dependencies using [uv](https://github.com/astral-sh/uv).
* Sync dependencies:

  ```bash
  uv sync
  ```
* Run the server locally:

  ```bash
  uv run server
  ```
* Add any additional dependencies with:

  ```bash
  uv add <package>
  ```

## Problem Statement

Each export contains multiple downloadable CSV files. Each row represents a simulated
patient event, with the following columns:

* `patient_id`
* `event_time`
* `event_type`
* `value`

Your task is to build a program that:

1. **Discovers exports and their downloads** using the server API.
2. **Processes CSV files** efficiently, taking into account file size and multiple
   downloads.
3. **Produces summary statistics** across patients and totals, output as formatted JSON
   printed to stdout.

The expected JSON structure should look like this (aggregated across *all* downloads of
an export):

```json
{
  "patients": {
    "P001": {
      "heart_rate": {
        "count": 1520,
        "mean": 74.2,
        "min": 55,
        "max": 120,
        "anomalies": 8,
        "latency": 9.8
      },
      "spo2": {
        "count": 1470,
        "mean": 96.8,
        "min": 85,
        "max": 100,
        "anomalies": 3,
        "latency": 10.0
      }
    }
  },
  "totals": {
    "heart_rate": {
      "count": 8000,
      "mean": 75.0,
      "min": 50,
      "max": 190,
      "anomalies": 42,
      "latency": 10.0
    },
    "spo2": {
      "count": 6000,
      "mean": 97.1,
      "min": 70,
      "max": 100,
      "anomalies": 15,
      "latency": 10.1
    }
  }
}
```

### Anomaly Definitions

* **heart\_rate**: anomalies are values `<40` or `>180`
* **spo2**: anomalies are values `<85` or `>100`
* **bp\_sys**: anomalies are values `<90` or `>200`
* **bp\_dia**: anomalies are values `<60` or `>120`

### Notes

* Your CLI should accept an **export ID** (`demo`, `small`, or `large`) as an argument
and run the analysis for that export.
* `latency` is the average gap in seconds between consecutive events for a patient/event
type, and for totals across all patients.
* All statistics must be aggregated across *all downloads* belonging to the chosen
export.
* Download time ranges are guaranteed to be non-overlapping.

## Constraints

* Do not use Pandas.
* This exercise is designed for roughly 1-2 hours of focused work.
* The full dataset may be large (millions of rows per download).
* Your solution should be mindful of performance and memory usage.
* Aim for readability and maintainability of code.
* Include tests for your implementation.

## Deliverables

* Your source code in a format that can be run locally.
* A JSON output file with your computed statistics.
* Instructions for running your program.

## Conclusion

The goal of this challenge is to demonstrate how you approach practical data processing:
discovering data, handling performance trade-offs, producing accurate results, and
presenting them clearly. There is no single “correct” solution-what matters is the
reasoning behind your choices and how you communicate them. We will review and discuss
your results together over a video call, so be prepared to explain and justify your
decisions.
