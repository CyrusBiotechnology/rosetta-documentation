<!-- THIS IS AN AUTOGENERATED FILE: Don't edit it directly, instead change the schema definition in the code itself. -->

_Autogenerated Tag Syntax Documentation:_

---
Computes the angle (in degrees) with the membrane normal for each TM span, and its relevant score.

```xml
<MPSpanAngle name="(&string;)" threshold="(5 &real;)"
        tm="(0 &non_negative_integer;)" ang_max="(90 &real;)"
        ang_min="(0 &real;)" output_file="(TR &string;)"
        angle_or_score="(angle &string;)" confidence="(1.0 &real;)" />
```

-   **threshold**: if MPSpanAngle score is lower than threshold, than pass.
-   **tm**: which TM to calculate angle for. if zero will test all tms
-   **ang_max**: maxiaml angle to pass
-   **ang_min**: minimal angle to pass
-   **output_file**: where to print the result to. default to TRACER. use auto for job_name_MPSpanAngle.txt
-   **angle_or_score**: whether to return the angle or the score. either angle or score
-   **confidence**: Probability that the pose will be filtered out if it does not pass this Filter

---