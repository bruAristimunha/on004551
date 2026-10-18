[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on004551-blue)](https://doi.org/10.82901/nemar.on004551)

This dataset was curated for publication as part of the manuscript in Sakakura et al. (in preparation). 
It contains iEEGs collected from 114 individuals during slow wave sleep. 
The available Matlab code can be found at https://github.com/kaz1126/MI_HFO.
The iEEG coordinate system employed in this dataset is MNI305.

## Channel names follow the data headers (18 Oct 2026)

In 125 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count). In the 125 with a different channel count, `mne_bids.read_raw_bids` warned and kept the header names, so the channels.tsv types and bad-channel status reached only the channels whose names happen to be identical; a reader that applies the channels.tsv rows by position gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 14476 rows took the header name (the header labels carry 'POL ' prefix, 'EEG ' prefix + '-Ref' suffix).

2043 header channels had no channels.tsv row (125 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-001_ses-01_task-sleep_run-01_ieeg.edf`: POL E, POL DC01, POL DC02, POL DC03; `sub-002_ses-01_task-sleep_run-01_ieeg.edf`: POL E, POL DC01, POL DC02, POL DC03; `sub-003_ses-01_task-sleep_run-01_ieeg.edf`: POL E, POL DC01, POL DC02, POL DC03.

1203 channels.tsv rows had no channel in the data file (56 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-001_ses-01_task-sleep_run-01_ieeg.edf`: B43, B44, B45, B46, B47; `sub-003_ses-01_task-sleep_run-01_ieeg.edf`: B15, B16, B17, B18, B19; `sub-007_ses-01_task-sleep_run-01_ieeg.edf`: B41, B42, B43, B44, B45.

125 files listed the channels in another order than the header; rows now follow the header order.

Applied by position, the former rows would have given wrong labels to 13719 channels in 125 files (rows missing, extra or out of order before that position).

electrodes.tsv is unchanged: it names the contacts (e.g. `A1`), while channel names follow the recorder (e.g. `A1-AV`); to attach positions in MNE, map channel names to contact names first, e.g. `raw.rename_channels(lambda n: re.sub(r'-[A-Za-z0-9]+$', '', n))`.
