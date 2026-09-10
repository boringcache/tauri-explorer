# BoringCache qualification

Reviewed 10 September 2026. This fork retains a watchlist assessment for [upstream issue 694](https://github.com/xnmp/tauri-explorer/issues/694). The issue is closed by [PR 695](https://github.com/xnmp/tauri-explorer/pull/695), merged on 9 September at 12:04:54 UTC. Subsequent runs now demonstrate target reuse with the existing GitHub cache.

The repaired [qualification run](https://github.com/xnmp/tauri-explorer/actions/runs/34348989427) passed Windows and Linux. Windows took 21m26s overall: the repaired target key missed, the Tauri build step took 17m37s, native contracts took 2s, the GUI checks passed, and target saving took 18s. Linux passed in 20m18s with a cold target, a 10m21s build step and an 8m01s GUI step. These are upstream measurements, not BoringCache results.

The repair keys Cargo target artifacts by compiler identity and relevant build inputs and saves them after a successful native outcome. The first cold run did not establish warm reuse. The subsequent observations below confirm that the existing cache restores compatible targets. A storage backend does not correct Cargo fingerprint incompatibility by itself.

## Subsequent upstream observations

All four jobs below passed. These runs use different source revisions; they are observations of the repaired upstream configuration, not a matched cold/warm experiment or a BoringCache comparison.

| Run / platform | Whole job | Tauri build step | Cargo dev compilation | Target restore step | Target archive bytes |
| --- | ---: | ---: | ---: | ---: | ---: |
| [34363422384 / Windows](https://github.com/xnmp/tauri-explorer/actions/runs/34363422384/job/102505937899) | 352 s | 73 s | 49.38 s | 17 s | 912,760,717 |
| [34363422384 / Linux](https://github.com/xnmp/tauri-explorer/actions/runs/34363422384/job/102505938246) | 623 s | 36 s | 26.54 s | 18 s | 1,299,824,862 |
| [34425075852 / Windows](https://github.com/xnmp/tauri-explorer/actions/runs/34425075852/job/102708478977) | 923 s | 167 s | 123 s | 51 s | 1,383,184,390 |
| [34425075852 / Linux](https://github.com/xnmp/tauri-explorer/actions/runs/34425075852/job/102708479558) | 462 s | 46 s | 37.66 s | 37 s | 1,372,074,970 |

Run 34363422384 used `e551e5426ac502cfec63cbb548f2175877143ec1`; run 34425075852 used `568f0bb7d983861fe3d2b6e07d3d747e2f8066ac`. Each job logged a restored target key. Archive bytes are the cache action's reported archive size, not unique retained storage or BoringCache transfer. The latest Windows job includes additional tests, so its whole-job duration is not directly comparable with the earlier run.

The original GUI/cache issue is repaired and target reuse is demonstrated. Keep this prospect on hold unless the maintainer reports a distinct remaining storage, retention or cross-runner reuse problem. There is no measured BoringCache benefit or confirmed commercial interest. [Machine-readable upstream measurements](boringcache-upstream-measurements.json) retain the source hashes, step timings and supporting log lines.

The validation branch is based at `3c1a121adc9e2f2cd01c780c321eebed5e8bd355`, five first-parent commits behind captured head `f6ddb8da1fbd3c468eb2d80a716c7c7fe137a243`. This branch contains no benchmark timing series or assertion of Windows BoringCache support. Public contact: [xnmp](https://github.com/xnmp). No outreach, upstream PR or comment was sent.
