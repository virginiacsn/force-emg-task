## force-emg-task

Code for the study: **[Comparing Upper Limb Intermuscular Coherence between Force- and Myoelectric-Control Tasks](https://repository.tudelft.nl/record/uuid:f614c737-56e7-48e1-9ad6-739a68ab933e)**

---

### Overview

This study investigates how task requirements affect coordination between arm muscles. Participants performed a **center-out-hold (COH)** cursor task under two conditions:

- **Force-control**: 2D forces applied to a handle drive cursor movement.
- **EMG-control**: Surface EMG signals from pairs of muscles drive cursor movement.

In each block, participants moved the cursor from the center to a peripheral target (arranged in a circle) and held it within the target. The main outcome measure is **intermuscular coherence**, an index of shared neural input to pairs of muscles, computed from rectified EMG signals during the hold phase, analyzed in the frequency domain (alpha: 8–12 Hz, beta: 15–30 Hz, gamma: 30–80 Hz).

---

### Requirements

- MATLAB with the **Signal Processing Toolbox** and **Statistics and Machine Learning Toolbox**
- For data collection: **TMSi Saga** EMG amplifier (Windows only; requires `TMSiSDK_thunk_pcwin64.dll`)
- Force transducer sampled via DAQ at 2048 Hz; EMG sampled at 1024 Hz

---

### Repository Structure

| File / Folder                             | Description                                              |
| ----------------------------------------- | -------------------------------------------------------- |
| `run_experiment.m`                        | End-to-end experiment script (data collection)           |
| `analysis_single_subject.m`               | Single-subject processing and coherence computation      |
| `analysis_all_subjects.m`                 | Population-level analysis and statistics                 |
| `analysis_coherence.m`                    | Detailed coherence analysis                              |
| `analysis_single_trial.m`                 | Single-trial inspection                                  |
| `analysis_task.m` / `analysis_task_app.m` | Task-level summaries                                     |
| `figs_*.m`                                | Figure generation scripts                                |
| `table_CV.m`                              | Coefficient of variation summary table                   |
| `utils/experiment/`                       | Real-time task control, calibration, and visualization   |
| `utils/analysis/`                         | Signal processing, coherence, and statistics utilities   |
| `+TMSi/`                                  | MATLAB package wrapping the TMSi SDK for EMG acquisition |
| `EMG_examples/`                           | Standalone scripts for hardware testing                  |

---

### Usage

#### Running the experiment

Open MATLAB, navigate to the repository root, and run:

```matlab
run_experiment
```

Before running, edit the `fileparams` struct at the top of the script to set the session date and subject ID. The root data path is selected automatically based on the OS (Windows or macOS); update the relevant line if your path differs:

```matlab
if strcmp(computer, 'PCWIN64')
    rootpath = 'D:\Student_experiments\Virginia\Data\Exp\';
elseif strcmp(computer, 'MACI64')
    rootpath = '/Users/virginia/Documents/MATLAB/Thesis/Data/';
end
```

The script executes the following sequence:

1. **EMG offset calibration** — establishes resting baseline (`EMGOffsettest`)
2. **Force-control blocks** — four blocks of center-out trials (`ForceControl_CO`)
3. **EMG scaling calibration** — derives per-muscle scaling from force-control data
4. **EMG-control calibration** (`EMGControl_Cal`)
5. **Force-control block** — one additional block after calibration
6. **EMG-control block** (`EMGControl_CO`)

#### Running analysis

```matlab
addpath(genpath('utils'))
analysis_single_subject   % process one subject
analysis_all_subjects     % aggregate across subjects
```

---

### Signal Processing Pipeline

**EMG** (`procEMG`):

1. High-pass filter (30 Hz, 2nd-order Butterworth)
2. Optional notch filter (50 Hz)
3. Full-wave rectification
4. Low-pass filter (60 Hz, 2nd-order Butterworth)
5. Moving average (800-sample window)

**Force** (`procForce`):

1. Low-pass filter (2–5 Hz, 2nd-order Butterworth)
2. Decomposition into magnitude and directional components
3. Downsampling to match EMG sample rate

**Coherence** (`coherence`, `cohStruct`):

- Welch's method with Hanning window, 1-second segments, 10 overlapping segments
- Computed for all muscle-pair combinations; 95% confidence limits included
