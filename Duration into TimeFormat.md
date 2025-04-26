# Converting Duration into TimeFormat

Method 1:
Input Must be in seconds or you may multiply it by 60 if it is in minutes, Final Result Would be in HH:mm:ss Format

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
