## force-emg-task

This repository contains the code for the study described in: [Comparing Upper Limb Intermuscular Coherence between Force- and Myoelectric-Control Tasks](https://repository.tudelft.nl/record/uuid:f614c737-56e7-48e1-9ad6-739a68ab933e).

### Overview
This study investigates arm muscle coordination through force- and EMG-control tasks. In the force-control task, 2D forces applied to a handle were used to control a computer cursor, while surface EMG signals from pairs of muscles were used in the EMG-control task. Participants performed both tasks in separate blocks. In each block, multiple repetitions or trials were collected in which the cursor had to be moved to a target and held within it (center-out-hold).

The main analysis focused on assessing changes in shared neural input to pairs of muscles between control tasks. As a measure of this shared input, we quantified intermuscular coherence between EMG signals of pairs of muscles during the hold phase of the task. EMG signals were processed by filtering and rectification, and coherence was computed in the frequency domain.


The repository contains the following main components:
- `run_experiment.m`: Script for running the experiment and collecting data.
- `analysis_*.m`: Scripts for processing and analyzing the collected data, including intermuscular coherence computation.
- `figs_*.m`: Scripts for generating plots to visualize the results.

