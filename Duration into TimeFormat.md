---
title: "DAX Duration to Time Format Conversion"
description: "Convert numeric durations to HH:mm:ss or d:HH:mm:ss formats in Power BI"
---

# Duration to Time Format Conversion


## Method 1: Seconds to HH:mm:ss
**Input**: Duration in seconds  
**Output**: `HH:mm:ss`

```dax
Your Measure Name =
   VAR MeasureCalculation =      
       [Calculated Metric Duration in Seconds]

   VAR InHours =
       INT(
           DIVIDE(
               MeasureCalculation,
               3600,
               0
           )
       )

   VAR InMinutes =
       INT(
           DIVIDE(
           MOD(
               MeasureCalculation,
               3600
               )
               ,60
           )
       )

   VAR InSeconds =
       INT(
           MOD(
               MOD(
                   MeasureCalculation,
                   3600
               ),
               60
           )
       )
   VAR TimeFormat =
       RIGHT("0" & InHours, 2)
       & ":" &
       RIGHT("0" & InMinutes, 2)
       & ":" &
       RIGHT("0" & InSeconds, 2)
RETURN TimeFormat
```

## Method 2: Minutes to d:HH:mm:ss
**Input**: Duration in minutes  
**Output**: `d:HH:mm:ss`

```dax
Your Measure Name=
   VAR Calculated_Measure =
       [Calculated Metric Duration in Minutes]
   VAR inDays =
       Calculated_Measure /1440
   VAR InHours =
       (inDays - INT(inDays)) * 24

   VAR InMinutes =
       (InHours - INT(InHours)) * 60
  
   VAR IntHours = INT(InHours)
   VAR INTMinutes = INT(InMinutes)
   VAR RemainingMinutes =
       INT(
           MOD(
               inMinutes,
               60
           )
       )
   VAR InSeconds =
   INT((inMinutes - INT(inMinutes)) * 60)

   VAR HoursTimeFormat =
       SWITCH(
           TRUE(),
           IntHours < 10,
           "0" & IntHours,
           IntHours >= 10,
           IntHours
       )
   VAR MinutesTimeFormat =
       SWITCH(
           TRUE(),
           INTMinutes < 10,
           "0" & INTMinutes,
           INTMinutes >= 10,
           INTMinutes
       )
   VAR SecondsTimeFormat =
       SWITCH(
           TRUE(),
           InSeconds < 10,
           "0" & InSeconds,
           InSeconds >= 10,
           InSeconds
       )

   VAR TimeFormat=
       INT(inDays)
       & ":"
       & HoursTimeFormat
       & ":"
       & MinutesTimeFormat
       & ":"
       & SecondsTimeFormat

RETURN TimeFormat

```