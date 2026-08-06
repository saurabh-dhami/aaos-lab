# 01 — Android Versions Timeline

Source: aospinsider.com — AOSP Foundations

## Core idea
Android's evolution is a story of solving bottlenecks.
Each architectural change came from the previous design's limit.

## Bottleneck to fix
| Problem | Solution | Version |
|---|---|---|
| CPU architecture unknown at build time | Dalvik + JIT | 1.0 |
| JIT burns CPU at launch, drains battery | ART + AOT (dex2oat) | 5.0 |
| OEM driver rewrites blocked every update | Treble — VINTF boundary | 8.0 |
| Security patches stuck behind OEM OTA | Mainline — APEX modules | 10 |

## In my own words
TODO

## Verified on device
TODO
