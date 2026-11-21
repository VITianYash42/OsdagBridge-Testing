# OsdagBridge Software Testing & Issue Reporting (Winter 2026 screening task)

Functional testing of the OsdagBridge short-span steel girder bridge module (plate girder, `dev` branch).

- **Tester:** Yash Singhal (GitHub: VITianYash42)
- **Environment:** Windows 11, Python 3.12, conda env `osdagbridge-env`, repository `Nidhikhare12/OsdagBridge`, branch `dev`, commit `d74f56d`, source unmodified (except the local test described in T05C, which was reverted)
- **Collaborator:** Nidhikhare12 (invited)

## Summary

18 test cases were executed: 9 pass, 8 fail, 1 observation. The full log (inputs, expected, actual, result, evidence and issue numbers) is in `test_log/Osdag_Test_Log_final.xlsx`.

| ID | What was tested | Result |
|---|---|---|
| T01 | Baseline design run and design report | Fail (known #65, #147, #114) |
| T02 | Three consecutive runs (span 30, 35, 25), saved data | Fail (stale segment_table, see #125) |
| T03 | Report dialog: Preview PDF | Fail (issue #166) |
| T04 | Report dialog: temp files left by Preview PDF | Fail (issue #167) |
| T05A | Stale-data control, fresh span 25 | Pass |
| T05B | Re-run changing only the span (30 to 25) | Fail (comment on #125) |
| T05C | Local fix for T05B, verified then reverted | Pass |
| T06 | Custom girder steel (density, Fy, Fu, E, G, Poisson, thermal) | Fail (known #20, density ignored) |
| T07a, T07b | Save 3D Model as IGS (span 30 and 20) | Fail (issue #201) |
| T07c | Save 3D Model as STEP, STL, IFC, BREP | Pass |
| T07d | Save CAD Image (PNG) | Pass |
| T07e | Content of the exported IFC file | Observation |
| T08 | Span 19 (below range) | Pass |
| T09a-d | Span 46, letters in span, empty span, skew 16 | Pass |

`test_log/Dict_vs_UI.csv` compares every entered UI value with the saved input dictionary (`.osi`) for run T06.

## GitHub activity

New issues raised:
- **#166** Preview PDF closes the report dialog and discards every field except Project Name
- **#167** Preview PDF creates a new temp folder on every use and never deletes it
- **#201** File > Save 3D Model as IGS blocks the GUI for about 13.7 s

Comment with root cause and a locally tested fix:
- **#125** Stale `segment_table` end lengths after a span change (cause in `defaults.py`, `_on_no_of_girders_changed()`): https://github.com/Nidhikhare12/OsdagBridge/issues/125#issuecomment-7039105584

Known issues reproduced independently (not re-filed): #65, #147, #114, #20.

## Folder structure

```
test_log/      test log spreadsheet and dictionary comparison
evidence/      saved input files (.osi), reports (.pdf), exported IFC, diff of the local fix
screenshots/   screenshots referenced in the test log
```

Large files are not included in this repository: the exported IGS (about 24 MB) and the screen recordings (T03, T07a). The recordings are attached to issues #166 and #201 and are also in the submission ZIP.

## Notes on method

1. Each test used the exact inputs recorded in the log. Values were saved with Save Input and compared with the UI.
2. Before any issue was filed, the open and closed issues were searched for the same symptom.
3. Observations that depend on reading the code give the file and line numbers, for example `analyser.py` L2804 (fixed steel density 78.5), `template_page.py` L1627-1631 (IGES write on the GUI thread) and `report_options.py` L545-551 and L547.
