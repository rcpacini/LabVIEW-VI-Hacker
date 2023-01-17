# LabVIEW-VI-Hacker

Unlocks password protected LabVIEW VIs

***Disclaimer: This is for demonstration purposes only, not to be used for malicious intent.***

*Note: This does not support VI's in LLB's, copy the VI out of the LLB first.*

## Getting Started

Open the `/src/VI-Hacker.vi`, enter a password protected VI Path (*that's not in an llb*).

Run the VI to unlock the password protected VI and preview the content in new VI.

*Note: The original source VI remains unmodified.*

![Demo](/docs/imgs/Demo.png)

You are now able to see what's on the block diagram.

## Downloads

This project is saved in LabVIEW 2022. Older LabVIEW version zips are located in `/builds/`:

- [LabVIEW-VI-Hacker (LabVIEW 2018)](/builds/LabVIEW-VI-Hacker-LV2018.zip)
- [LabVIEW-VI-Hacker (LabVIEW 2014)](/builds/LabVIEW-VI-Hacker-LV2014.zip)
- [LabVIEW-VI-Hacker (Legacy 1.0)](/builds/LabVIEW-VI-Hacker-1.0.zip) - Older VI-Hacker 1.0


## How does it work?

The VI-Hacker library uses a simplified variation of the [VI-Explorer-VI](https://github.com/tomsoftware/VI-Explorer-VI) 
brute force algorithm to calculate the MD5 password salt to regenerate and 
replace the Block Diagram Password (BDPW) MD5 password hashes.
The MD5 of the original password is concatenated with the owning libraries (LIBN)
block (as colon delimited qualified library path) and LabVIEW Source (LVSR) data block.
The password salt is then calculated by brute force and checked against the first password hash (`Hash1`). 
Once the salt if found, the `BDPW.Password(MD5)` + `LIBN.QualifiedPath` + `LVSR.Content` + `Salt`
are concatenated with the uncompressed block diagram heap (BDHc, BDHb, or BDHP) to generate the final
hash (`Hash2`). [ZLib](https://www.zlib.net/) is used to uncompress the block diagram heap (BDHc, BDHb) 
for the final password hash. The Block Diagram Password (BDPW) block is then replaced within the VI file:

```
BDPW:
  Password (MD5) - 16 bytes
  Hash1 (MD5) - 16 bytes
  Hash2 (MD5) - 16 bytes
```

The VI is saved to a new path and then loaded as a VI scripting template to avoid application content switching.

---

 
### Credit

Ryan Pacini (c) 2023

[ZLib](https://www.zlib.net/) - Defalte to uncompress block diagram heap [LICENSE](/docs/zlib_license.txt)

[VI-Explorer-VI](https://github.com/tomsoftware/VI-Explorer-VI) - Salt cracking [LICENSE](/docs/vi-exploer-vi_license.txt)
