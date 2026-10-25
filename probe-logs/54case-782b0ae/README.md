# Extended 54-case drain probe - raw logs at 782b0ae

Probe and sink built from PR #11 commit `782b0ae` (Release, VS 2022 / MSVC x64).
Buffer period for every run: 30 ms event-driven exclusive
(`config.periodFrames = rate * 30 / 1000`, i.e. 5760 frames at 192 kHz,
2880 at 96 kHz, 1323 at 44.1 kHz), the same period as all earlier 36-case runs.
Probe stdout only; silence on the endpoint; `--run-on-default-device`.

These logs are fresh captures at the same commit as the pass counts reported on
the PR: the consoles of the first runs were not preserved in full, so the matrix
was re-run for the record. Results are identical (54/54 per run,
`earlyStarts=0` everywhere, i.e. `startsBeforeFirstBuffer == 0`).

| File | Device | Runs | Result |
|---|---|---|---|
| `fosi-run1.log`, `fosi-run2.log` | Fosi Audio DA2120C | 2 | 54/54, 54/54 |
| `chord-run1..3.log` | Chord Mojo | 3 | pending reconnect - the DAC is currently powered off; logs will be added in a follow-up commit |

Device, driver and OS details are unchanged from the table in issue #10:

| | Fosi Audio DA2120C | Chord Mojo |
|---|---|---|
| Hardware ID | `USB\VID_0D8C&PID_0171&REV_0101` (composite; audio at `MI_00`) | `USB\VID_245F&PID_0815&REV_0197` |
| Driver | Microsoft inbox `usbaudio.sys`, `wdma_usb.inf`, 10.0.19041.7802 | Chord Electronics `cdh768.inf` (published `oem119.inf`), 1.0.70.0 (2026-07) |
| OS | Windows 10 Pro for Workstations 19045.7807 | same |

Per-case stats fields: `starts` = `clientStarts` (successful `IAudioClient::Start`
calls this stream), `earlyStarts` = `startsBeforeFirstBuffer` (Starts issued
before the first `ReleaseBuffer`; must be 0 in every case).
