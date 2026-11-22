# TKT-XXX: [Short title describing the user's complaint]

| Field | Value |
|-------|-------|
| **Reported by** | [e.g. Front desk user, PC1] |
| **Priority** | [Low / Medium / High / Critical] |
| **Affected** | [One user / Several users / Entire site] |
| **Layer** | [OSI layer of the root cause] |
| **Status** | Resolved |

## 1. Reported Issue

> "[The complaint in the user's own words, e.g. 'Nobody in the office has internet.']"

## 2. Initial Observations

What I checked first and what I saw.

- [Observation 1]
- [Observation 2]

```
[Paste command output here, e.g. ipconfig or ping result]
```

## 3. Troubleshooting Steps

| # | Action / Command | Result | Conclusion |
|---|------------------|--------|------------|
| 1 | `ipconfig` | [result] | [what this rules in/out] |
| 2 | `ping 192.168.1.1` | [result] | [what this rules in/out] |
| 3 | `arp -a` | [result] | [what this rules in/out] |
| 4 | [command on router/switch] | [result] | [what this rules in/out] |

Key output:

```
[Paste the output that revealed the problem]
```

## 4. Root Cause

[One or two sentences: what was wrong and why it caused the symptom.]

## 5. Resolution

```
[Exact configuration commands used to fix it]
```

## 6. Verification

How I confirmed the fix worked.

```
[Successful ping / ipconfig / show output after the fix]
```

![Verification screenshot](screenshots/verification.png)

## 7. Lessons Learned

- **How to recognize it:** [the symptom that points to this cause]
- **How it differs from similar issues:** [e.g. "host unreachable" vs "request timed out"]
- **Prevention:** [what would stop this happening in a real network]

## Files

- `lab.pkt` – Packet Tracer file with the fault
- `screenshots/` – evidence screenshots
- `capture.pcapng` – Wireshark capture (if applicable)
