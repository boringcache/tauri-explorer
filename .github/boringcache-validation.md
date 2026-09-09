# BoringCache qualification

Reviewed 9 September 2026. This fork retains a watchlist assessment for [upstream issue 694](https://github.com/xnmp/tauri-explorer/issues/694). The issue is now closed by [PR 695](https://github.com/xnmp/tauri-explorer/pull/695), merged at 12:04:54 UTC.

The repaired [qualification run](https://github.com/xnmp/tauri-explorer/actions/runs/34348989427) passed Windows and Linux. Windows took 21m26s overall: the repaired target key missed, the Tauri build step took 17m37s, native contracts took 2s, the GUI checks passed, and target saving took 18s. Linux passed in 20m18s with a cold target, a 10m21s build step and an 8m01s GUI step. These are upstream measurements, not BoringCache results.

The repair keys Cargo target artifacts by compiler identity and relevant build inputs and saves them after a successful native outcome. The first cold run does not establish warm reuse. A second equivalent Windows run with that key is needed before attributing any remaining delay to storage. A storage backend does not correct Cargo fingerprint incompatibility by itself.

The validation branch is based at `3c1a121adc9e2f2cd01c780c321eebed5e8bd355`, five first-parent commits behind captured head `f6ddb8da1fbd3c468eb2d80a716c7c7fe137a243`. This branch contains no benchmark timing series or assertion of Windows BoringCache support. Public contact: [xnmp](https://github.com/xnmp). No outreach, upstream PR or comment was sent.
