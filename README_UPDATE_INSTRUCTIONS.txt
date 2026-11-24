Add this section to your existing README.md (recommended location: right after the Executive summary section):

## Hardware Comparison

A detailed production hardware comparison across XRPL, XRPL EVM, and Vitruveo (including published recommendations vs. actual Linode and AWS experiments) is available in:

**[docs/hardware-comparison.md](docs/hardware-comparison.md)**

It covers node role color coding, RAM/CPU/storage charts, archive vs validator distinctions, and proposed planning targets.

---

Then commit the following structure:

docs/
  hardware-comparison.md
  images/
    ram_comparison.png
    cpu_comparison.png
    storage_comparison.png
