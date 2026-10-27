# PIC18 Access-Control Module

A compact, production-ready access-control system built on the **PIC18F45K22** using XC8. Three operating modes (Disabled, User, Admin) are managed via hardware switches and a 7-segment display.

## Why it stands out
- **Robust FSM** with debounced button handling and non-blocking timers.
- **Secure admin flow** - master code can be updated on-the-fly.
- **Complete documentation** in *Actividad 6 Electronica.pdf* (see `docs/`).

## Quick start
```bash
git clone https://github.com/Alexis-c-d/pic18-access-control.git
cd pic18-access-control/src
# Open src/main.c in MPLAB X, build, and flash to the PIC.
```

🗂️ **Docs** - `docs/Actividad 6 Electronica.pdf` contains the full spec and FSM diagrams.

---

## Repository layout
```
pic18-access-control/
├─ src/        # src/main.c - complete FSM + drivers
├─ sim/        # Proteus simulation (to be added)
└─ docs/       # Actividad 6 Electronica.pdf (project report)
```

## License
MIT - feel free to use for learning or as a base for your own access-control projects.
