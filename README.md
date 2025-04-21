sequenceDiagram
    %% ─────────  Participants  ─────────
    participant PCD as Reader (PCD)
    participant PICC as Card (PICC)

    %% ─────────  Carrier On & Power‑up  ─────────
    Note over PCD,PICC: **Carrier 13.56 MHz** – Reader field ON<br/>**t<sub>EQU</sub> ≥ 5 ms** (Field‑equilibration)  
    PCD->>PICC: ---- HF field ----

    %% ─────────  Wake‑up / REQA  ─────────
    PCD->>PICC: **REQA** 106 kbps ASK<br/>(bit = 9.44 µs)
    Note right of PICC: **T<sub>GTA</sub> = 7 000/f<sub>c</sub> ≈ 516 µs**

    %% ─────────  ATQA  ─────────
    PICC-->>PCD: **ATQA** (Load‑Mod. 847 kHz)

    %% ─────────  Anticollision  ─────────
    PCD->>PICC: SELECT (CL1)
    PICC-->>PCD: UID CL1
    PCD->>PICC: SELECT (CL2)
    PICC-->>PCD: UID CL2 & SAK

    %% ─────────  RATS / PPS etc.  ─────────
    PCD->>PICC: **RATS**
    Note right of PICC: Internal processing **t<sub>PROC</sub> ≤ 1 ms**
    PICC-->>PCD: ATS

    %% ─────────  Data Exchange  ─────────
    PCD-)PICC: I‑Block (DATA)   **FDT<sub>PCD→PICC</sub> = 1236/f<sub>c</sub> ≈ 91 µs**
    PICC-)PCD: I‑Block (RESP)   **FDT<sub>PICC→PCD</sub> = 1172/f<sub>c</sub> ≈ 86 µs**

    %% ─────────  End of Frame  ─────────
    Note over PCD,PICC: Further I‑/R‑/S‑Blocks repeat until DESELECT<br/>or card leaves field
