# NetChess 2.1 — SEH Buffer Overflow (0-day)

Original vulnerability research and proof-of-concept exploit for a **Structured Exception Handler (SEH) based stack buffer overflow** in NetChess 2.1, a Windows peer-to-peer chess client. Discovered and disclosed by **Ugur Eminli**.

## Summary

| | |
|---|---|
| **Vulnerability** | SEH-based stack buffer overflow |
| **Affected software** | NetChess 2.1 |
| **Vendor** | [avmnetchess on SourceForge](https://sourceforge.net/projects/avmnetchess/) |
| **Impact** | Arbitrary code execution in the context of the running user |
| **Tested on** | Windows XP SP2 (Build 2600) |
| **Author** | Ugur Eminli |
| **Disclosure date** | 2022-08-01 |
| **Advisory** | [Packet Storm](https://packetstormsecurity.com/files/170612/) |

## Details

NetChess 2.1 fails to validate the length of attacker-controlled input before copying it onto the stack. A sufficiently long input overruns the buffer and overwrites the SEH chain. By overwriting the `nSEH` / `SEH` records with a `pop pop ret` gadget and chaining into shellcode, an attacker gains control of execution flow and can run arbitrary code as the user running NetChess.

The included proof-of-concept (`netchess.pl`) generates the malicious payload that triggers the overflow and demonstrates control of the exception handler.

## Proof of concept

```bash
perl netchess.pl
```

The script writes the crafted payload used to reproduce the crash and demonstrate SEH overwrite. Review the source before running — it is published for research and educational purposes.

## Repository contents

- `netchess.pl` — PoC payload generator (Perl)
- `README.md` — this document

## Responsible use & disclosure

This material is provided solely for **education, defensive research, and authorized testing**. NetChess 2.1 is legacy software and the target environment (Windows XP) is end-of-life. Do not use this against systems you do not own or have explicit written permission to test. The author accepts no liability for misuse.

## References

- Vendor: https://sourceforge.net/projects/avmnetchess/
- Packet Storm advisory: https://packetstormsecurity.com/files/170612/
