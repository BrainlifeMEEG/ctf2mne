<h1 align="center">CTF to mne/raw</h1>

<p align="center">
  Converts MEG CTF data to MNE Raw (fif)
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.634"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.634-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-see_LICENSE-lightgrey.svg" alt="License"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Converts MEG CTF data to MNE Raw (fif)**

Copies the CTF .ds folder to a temporary location, reads it with mne.io.read_raw_ctf, detects events, marks strictly flat channels and user-listed channels as bad, sets EOG/ECG/MISC channel types, and saves raw.fif with an HTML QC report and a PSD plot. Use it as the first step to bring CTF MEG recordings into MNE-based brainlife pipelines. The eeg parameter is not read by the code.

| | |
|---|---|
| **Inputs** | `neuro/meg/ctf` |
| **Outputs** | `neuro/meeg/mne/raw` |
| **Container** | `brainlifemeeg/mne:1.12.1` |
| **Resources** | 8 CPUs · 16 GB memory · 0.17 h walltime |
| **Run it** | [doi.org/10.25663/brainlife.app.634](https://doi.org/10.25663/brainlife.app.634) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `ctf1` | `neuro/meg/ctf` | yes | CTF MEG .ds recording folder with its BIDS headshape, channels, coordsystem and events sidecars. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir` | `neuro/meeg/mne/raw` | MNE Raw data in FIF format |

<details>
<summary><b>Parameters</b> (6)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `eog` | string |  | EOG channels if any. Default is None. |
| `ecg` | string |  | ECG channels if any. default is None. |
| `bads` | string |  | Bad channels (comma separated list). |
| `misc` | string |  | Names of channels or list of indices that should be designated MISC channels. Default is None. |
| `rm_flat` | boolean | `true` | Mark flat channels as bad if True (default). |
| `eeg` | string |  | Not read by the code (main.py handles eog, ecg, misc, bads and rm_flat only). |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.634](https://doi.org/10.25663/brainlife.app.634), choose `neuro/meg/ctf` from your project, set the parameters and press **Execute**. The results are saved to your project as `neuro/meeg/mne/raw`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `brainlifemeeg/mne:1.12.1`), [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository.
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "ds": "/path/to/ctf1/ds",
       "headshape": "/path/to/ctf1/headshape",
       "channels": "/path/to/ctf1/channels",
       "coordsystem": "/path/to/ctf1/coordsystem",
       "events": "/path/to/ctf1/events",
       "events_json": "/path/to/ctf1/events_json",
       "eog": "",
       "ecg": "",
       "bads": "",
       "misc": "",
       "rm_flat": true,
       "eeg": ""
   }
   ```

3. Run `./main`.

## Configuration Parameters

| key | type | default | description |
|---|---|---|---|
| `eog` | string | `""` | Comma-separated list of EOG channel names, if any (empty means none) |
| `ecg` | string | `""` | Comma-separated list of ECG channel names, if any (empty means none) |
| `misc` | string | `""` | Comma-separated list of channel names to designate as MISC, if any (empty means none) |
| `bads` | string | `""` | Comma-separated list of additional channel names to mark as bad |
| `rm_flat` | boolean | `true` | Automatically detect and mark strictly flat channels as bad |

## Local Testing

```bash
# Edit config.json with your own input file and options, then run:
python3 main.py
```

## Technical Details

Before conversion, the app copies the input CTF `.ds` folder to a temporary working copy (via `mne_bids.copyfiles.copyfile_ctf`) so the original input is never modified in place; the temporary copy is removed when the app exits, whether or not conversion succeeds.

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- CTF to mne/raw. brainlife.io App. [doi:10.25663/brainlife.app.634](https://doi.org/10.25663/brainlife.app.634)
- Gramfort A, et al. MEG and EEG data analysis with MNE-Python. Front Neurosci 7:267 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Guiomar Niso, Instituto Cajal, CSIC, Spain

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io, which runs this App, is publicly funded by Wellcome, the U.S. National Institutes of Health and the U.S. National Science Foundation; acknowledging that support helps keep it running.</sub>

## License

See [LICENSE](LICENSE).
