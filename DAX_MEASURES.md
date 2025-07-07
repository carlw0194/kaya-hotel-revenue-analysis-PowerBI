# Kaya Hotel Group - DAX Measures & Columns

## Table of Contents
- [Tier 1: Executive KPIs](#tier-1-executive-kpis)
- [Tier 2: Diagnostic Metrics](#tier-2-diagnostic-metrics)
- [Time Intelligence Measures](#time-intelligence-measures)
- [Calculated Columns](#calculated-columns)

## Tier 1: Executive KPIs
These measures should be in a table/folder named `_Tier 1 KPIs`

### Total Realized Revenue
Calculates the total income from completed guest stays.
```dax
Total Realized Revenue =
CALCULATE (
    SUM ( 'fact_bookings'[revenue_realized] ),
    'fact_bookings'[booking_status] = "Checked Out"
)
```

### Occupancy Rate
Calculates the overall percentage of available rooms that were sold.
```dax
Occupancy Rate =
DIVIDE (
    SUM ( 'fact_aggregated_bookings'[successful_bookings] ),
    SUM ( 'fact_aggregated_bookings'[capacity] ),
    0
)
```

### Average Daily Rate
Calculates the average rental income per paid occupied room.
```dax
Average Daily Rate =
DIVIDE (
    [Total Realized Revenue],
    SUM ( 'fact_aggregated_bookings'[successful_bookings] ),
    0
)
```

### RevPAR
Calculates the total revenue earned per available room.
```dax
RevPAR = [Average Daily Rate] * [Occupancy Rate]
```

## Tier 2: Diagnostic Metrics
These measures should be in a table/folder named `_Tier 2 Diagnostics`

### Cancellation Rate
Calculates the percentage of total bookings that were canceled.
```dax
Cancellation Rate =
VAR TotalCancellations =
    CALCULATE (
        COUNTROWS ( 'fact_bookings' ),
        'fact_bookings'[booking_status] = "Cancelled"
    )
VAR TotalBookings = COUNTROWS ( 'fact_bookings' )
RETURN
    DIVIDE ( TotalCancellations, TotalBookings, 0 )
```

### No-Show Rate
Calculates the percentage of total bookings that were a no-show.
```dax
No-Show Rate =
VAR NoShowCount =
    CALCULATE (
        COUNTROWS ( 'fact_bookings' ),
        'fact_bookings'[booking_status] = "No Show"
    )
VAR TotalBookings = COUNTROWS ( 'fact_bookings' )
RETURN
    DIVIDE ( NoShowCount, TotalBookings, 0 )
```

### Average Length of Stay
Calculates the average number of nights for completed stays.
```dax
Average Length of Stay =
AVERAGEX (
    FILTER ( 'fact_bookings', 'fact_bookings'[booking_status] = "Checked Out" ),
    DATEDIFF (
        'fact_bookings'[check_in_date],
        'fact_bookings'[checkout_date],
        DAY
    )
)
```

### Booking Lead Time
Calculates the overall average time between booking and check-in.
```dax
Booking Lead Time =
AVERAGEX (
    'fact_bookings',
    DATEDIFF (
        'fact_bookings'[booking_date],
        'fact_bookings'[check_in_date],
        DAY
    )
)
```

## Time Intelligence Measures
These measures should be in a table/folder named `_Time Intelligence`

### Revenue YTD
Calculates cumulative revenue from the start of the year.
```dax
Revenue YTD = TOTALYTD ( [Total Realized Revenue], 'dim_date'[date] )
```

### RevPAR 3-Month Rolling Avg
Calculates the rolling 3-month average for RevPAR to smooth out trends.
```dax
RevPAR 3-Month Rolling Avg =
CALCULATE (
    [RevPAR],
    DATESINPERIOD (
        'dim_date'[date],
        LASTDATE ( 'dim_date'[date] ),
        -3,
        MONTH
    )
)
```

### Previous Month Revenue
Calculates the revenue from the previous month for MoM comparison.
```dax
Previous Month Revenue =
CALCULATE (
    [Total Realized Revenue],
    DATEADD ( 'dim_date'[date], -1, MONTH )
)
```

### Revenue MoM %
Calculates the percentage change in revenue from the previous month.
```dax
Revenue MoM % =
DIVIDE (
    [Total Realized Revenue] - [Previous Month Revenue],
    [Previous Month Revenue],
    0
)
```

### Previous Month Occupancy
Calculates the occupancy rate from the previous month.
```dax
Previous Month Occupancy =
CALCULATE (
    [Occupancy Rate],
    DATEADD ( 'dim_date'[date], -1, MONTH )
)
```

### Occupancy MoM Change
Calculates the absolute change in occupancy percentage points.
```dax
Occupancy MoM Change = [Occupancy Rate] - [Previous Month Occupancy]
```

### Previous Month ADR
Calculates the ADR from the previous month.
```dax
Previous Month ADR =
CALCULATE (
    [Average Daily Rate],
    DATEADD ( 'dim_date'[date], -1, MONTH )
)
```

### ADR MoM %
Calculates the percentage change in ADR from the previous month.
```dax
ADR MoM % =
DIVIDE (
    [Average Daily Rate] - [Previous Month ADR],
    [Previous Month ADR],
    0
)
```

## Calculated Columns
These columns should be created in the `fact_bookings` table.

### Lead Time (Days)
Calculates the lead time in days for each individual booking row.
```dax
Lead Time (Days) =
DATEDIFF (
    'fact_bookings'[booking_date],
    'fact_bookings'[check_in_date],
    DAY
)
```

### Booking Window Group
Groups the lead times into user-friendly bins for analysis.
```dax
Booking Window Group =
SWITCH (
    TRUE (),
    'fact_bookings'[Lead Time (Days)] <= 7, "0-7 Days",
    'fact_bookings'[Lead Time (Days)] <= 30, "8-30 Days",
    'fact_bookings'[Lead Time (Days)] <= 60, "31-60 Days",
    'fact_bookings'[Lead Time (Days)] <= 90, "61-90 Days",
    "91+ Days"
)
```

### Booking Window Sort Order
Provides a numeric sort order for the Booking Window Group.
```dax
Booking Window Sort Order =
SWITCH (
    TRUE (), 
    'fact_bookings'[Lead Time (Days)] <= 7, 1,
    'fact_bookings'[Lead Time (Days)] <= 30, 2,
    'fact_bookings'[Lead Time (Days)] <= 60, 3,
    'fact_bookings'[Lead Time (Days)] <= 90, 4,
    5
)
```
