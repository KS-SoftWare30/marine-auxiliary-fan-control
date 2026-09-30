# Architecture Notes

```mermaid
flowchart TD
    I[Physical / Simulated Inputs] --> P[Permissive Logic]
    H[HMI Remote Commands] --> S[Local / Remote Command Selection]
    S --> L[Run Latch]
    P --> L
    L --> R[Run Command]

    R --> SF[Start Feedback Timer]
    FB[Run Feedback] --> SF
    SF --> SFF[Start Fail Latch]

    R --> RC[Run Confirmed Memory]
    FB --> RC
    RC --> FL[Feedback Loss Timer]
    R --> FL
    FB --> FL
    FL --> FF[Feedback Fault Latch]

    T[Thermal OK] --> TF[Thermal Fault Latch]

    SFF --> P
    FF --> P
    TF --> HMI[HMI / Alarm System]
```

## Key Engineering Decisions

- One reusable Function Block for three motors.
- Independent instance DBs preserve timers and latches.
- Remote commands use dedicated PLC tags rather than writing physical input addresses.
- Start failure and running feedback loss are separate fault modes.
- Thermal reset requires a healthy thermal input.
- Overview + detail HMI structure keeps navigation simple.
