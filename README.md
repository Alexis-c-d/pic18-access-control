# PIC18 Access Control Module

Access-control system on **PIC18F45K22** (XC8) with three operating modes governed by hardware switches.

## Overview
| Switch State | Mode | Behaviour |
|--------------|------|-----------|
| `SW_ENABLE=0` | **DISABLED** | Red LED solid, keypad ignored, access denied |
| `SW_ENABLE=1, SW_ADMIN=0` | **USER** | Enter code 0–9 via `BTN_UP`, validate with `BTN_OK`; ✅ green 2 s if match, ❌ red 2 s if wrong; `BTN_CLR` clears entry |
| `SW_ENABLE=1, SW_ADMIN=1` | **ADMIN** | Change master code: `BTN_OK`→enter new code→`BTN_OK`→confirm; match = update, mismatch = error |

**Default master code**: `5`

## Hardware
| Signal | Pin | Type |
|--------|-----|------|
| `SW_ENABLE` | RA0 | Input, pull-up |
| `SW_ADMIN` | RA1 | Input, pull-up |
| `BTN_UP` | RB0 | Input, pull-up |
| `BTN_OK` | RB1 | Input, pull-up |
| `BTN_CLR` | RB2 | Input, pull-up |
| `LED_VERDE` | RC0 | Output |
| `LED_ROJO` | RC1 | Output |
| 7-seg display | RD0–RD7 | Common cathode |

## Repository Layout

pic18-access-control/
└── src/
    └── main.c          # Complete FSM + drivers
└── sim/                # Proteus simulation (to be added)
└── docs/
    └── Actividad 6 Electronica.pdf  # Original lab report (Spanish)

## Building
1. **MPLAB X IDE** + **XC8**, device **PIC18F45K22**, 8 MHz INTIO.
2. Build → `main.hex`.
3. Simulate in **Proteus** or flash to hardware.

## FSM Details (from *Actividad 6 Electronica.pdf*)

DISABLED ──SW_ENABLE=1, SW_ADMIN=0──► USER
USER     ──SW_ADMIN=1───────────────► ADMIN
ADMIN    ──SW_ADMIN=0───────────────► USER
ANY      ──SW_ENABLE=0──────────────► DISABLED

**USER sub-states**:
- `INGRESO` – accumulate digit (0–9) via `BTN_UP`, show on 7-seg
- `VALIDANDO` – `BTN_OK` pressed, compare with master code
- `ACCESO_OK` – green LED 2 s, then back to `INGRESO`
- `ACCESO_NO` – red LED 2 s, then back to `INGRESO`

**ADMIN sub-states**:
- `ESPERA` – show current master code, wait for `BTN_OK`
- `NUEVO_COD` – enter new code 0–9 via `BTN_UP`
- `CONFIRMA` – re-enter same code; if match → update master, green 2 s; else red 2 s

## Timer
- **Timer0** @ 10 ms tick (8 MHz / 256 prescaler)
- Used for non-blocking 2 s LED timeouts (`timer_led` counts 10 ms ticks)

## Extracted From
Lab report: *Actividad 6 Electronica.pdf* (see `docs/`), Materia: LRT-2042 Electrónica Integrada, Periodo: Primavera 2026, Profesor: Dr. José Luis Vázquez González.

## License
MIT