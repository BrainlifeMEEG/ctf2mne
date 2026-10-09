# CTF to MNE Raw

[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.634-blue.svg)](https://doi.org/10.25663/brainlife.app.634)

## Description

This app converts CTF MEG `.ds` folder recordings to MNE-Python's raw FIF format using [`mne.io.read_raw_ctf`](https://mne.tools/stable/generated/mne.io.read_raw_ctf.html). It copies the CTF dataset to a temporary working folder, detects events from the trigger channels, identifies and marks strictly flat channels and any user-specified bad channels, and generates a QC report with channel information and a power spectral density (PSD) plot.

The app generates:
- A converted MNE raw `.fif` file
- An HTML QC report with channel information
- A PSD plot
- A `product.json` summary with bad/flat channel info and channel positions

## Inputs

- **`ds`** (`neuro/meg/ctf`): CTF MEG `.ds` dataset folder to convert, bundled with its associated headshape, channels, coordsystem and events files (required)

## Outputs

- `out_dir/raw.fif` (`neuro/meeg/mne/raw`): converted MNE raw data file, with flat/bad channels marked in `raw.info['bads']`
- `out_report/report.html`: QC report with raw data summary and channel listing
- `out_figs/psd.png`: power spectral density plot of the raw data

## Configuration Parameters

| key | type | default | description |
|---|---|---|---|
| `eog` | string | `""` | Comma-separated list of EOG channel names, if any (empty means none) |
| `ecg` | string | `""` | Comma-separated list of ECG channel names, if any (empty means none) |
| `misc` | string | `""` | Comma-separated list of channel names to designate as MISC, if any (empty means none) |
| `bads` | string | `""` | Comma-separated list of additional channel names to mark as bad |
| `rm_flat` | boolean | `true` | Automatically detect and mark strictly flat channels as bad |

## Usage

### Running on Brainlife.io

1. Go to the [CTF to MNE Raw app page](https://brainlife.io/app/628b6054d0697cf1eaeb1972) on Brainlife.io.
2. Select your project and the CTF `.ds` dataset as input.
3. Optionally set `eog`, `ecg`, `misc`, `bads`, and/or `rm_flat` in the configuration.
4. Submit the process, then download `out_dir/raw.fif` and `out_report/report.html` once it completes.

### Local Testing

```bash
# Edit config.json with your own input file and options, then run:
python3 main.py
```

## Technical Details

Before conversion, the app copies the input CTF `.ds` folder to a temporary working copy (via `mne_bids.copyfiles.copyfile_ctf`) so the original input is never modified in place; the temporary copy is removed when the app exits, whether or not conversion succeeds.

## Authors
- [Guiomar Niso](guiomar.niso@ctb.upm.es), Instituto Cajal, CSIC, Spain

## Citations
We kindly ask that you cite the following articles when publishing papers and code using this app.

1. Hayashi, S., Caron, B.A., Heinsfeld, A.S. et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. Nat Methods 21, 809–813 (2024). https://doi.org/10.1038/s41592-024-02237-2
2. Gramfort, A. et al. MEG and EEG data analysis with MNE-Python. Front. Neurosci. 7, 267 (2013). https://doi.org/10.3389/fnins.2013.00267

## Funding Acknowledgement
brainlife.io is publicly funded and for the sustainability of the project we kindly ask that you acknowledge the following funding sources:

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://grantome.com/grant/NIH/R01-EB030896-01)

## License

Copyright (c) 2026 MEEG Brainlife team. Licensed under AGPL-3.0, see [license.txt](license.txt) for details.
