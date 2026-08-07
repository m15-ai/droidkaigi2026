# Demo-day runbook

> **TODO before the talk** — fill in as rehearsals happen.

## Pre-flight (night before)

- [ ] Server box charged / powered, on the demo network
- [ ] `systemctl --user status homer-pipecat.service` → active
- [ ] `curl http://<server-ip>:7864/health` → `{"status":"ok"}` from the phone's network
- [ ] Phone: Pipecat client installed, server URL set, mic permission granted
- [ ] Test one full voice turn end-to-end
- [ ] API balances: OpenAI / Deepgram / Cartesia have headroom
- [ ] Flip repos public (or right after the talk)

## On stage

- [ ] Opening question that always lands: "How did the Dodgers do yesterday?"
- [ ] Ohtani question shows the two-way stats: "How's Ohtani doing this season?"
- [ ] Barge-in demo: interrupt Homer mid-answer

## Fallbacks

- [ ] Venue Wi-Fi fails → phone hotspot with server box joined to it (pre-paired)
- [ ] MLB API unreachable → Homer says so honestly; have a canned recording of a good run
- [ ] Server dies → `systemctl --user restart homer-pipecat.service` (~15 s to healthy)
