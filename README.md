# EEG-Eye-Classification# EEG-Based Eye State Classification Using Alpha Band Activity

Classifies **eyes open vs. eyes closed** from short EEG windows using alpha-band (8-13 Hz) spectral power, based on the well-established "alpha blocking" phenomenon: alpha power rises when the eyes close and drops when they open.

Originally a course project for Emory University's Machine Learning course (Fall 2025), built with teammates **Raphael Haddad** and **Chloe Guerrero**. This repo is a personal rebuild of the pipeline described in our project report, written from scratch to be runnable end-to-end and open for anyone to reproduce.

## Approach

1. **Data**: [PhysioNet EEG Motor Movement/Imagery Dataset](https://physionet.org/content/eegmmidb/1.0.0/) (Goldberger et al., 2000) — accessed via MNE-Python's built-in loader. Uses each subject's baseline eyes-open and eyes-closed resting runs.
2. **Preprocessing**: bandpass filter (1–40 Hz), segmented into non-overlapping 2-second windows.
3. **Feature extraction**: per-channel alpha-band (8–13 Hz) power via Welch's method for power spectral density.
4. **Normalization**: Z-score standardization across features.
5. **Models**: Random Forest and K-Nearest Neighbors, both tuned via grid search with 5-fold cross-validation.
6. **Evaluation**: accuracy, classification report, and confusion matrices on a held-out test split.

## Run it yourself

```bash
pip install mne scikit-learn matplotlib seaborn numpy pandas
jupyter notebook eeg_eye_state_classification.ipynb
```

Or open directly in Google Colab and run top to bottom — the first code cell downloads the PhysioNet data automatically via MNE, no manual data wrangling required.

## Notes

- Evaluation is at the **window level**: windows from the same subject can appear in both train and test splits, so results reflect within-subject generalization, not performance on entirely unseen subjects. (Same caveat noted in the original project report.)
- The notebook defaults to 10 subjects for a fast sanity-check run; increase `N_SUBJECTS` to scale up.

## References

- Amin, H. U. et al. (2017). Classification of EEG Signals Based on Pattern Recognition Approach. *Frontiers in Computational Neuroscience*, 11, 103.
- Barry, R. J. et al. (2007). EEG Differences between Eyes-Closed and Eyes-Open Resting Conditions. *Clinical Neurophysiology*, 118(12), 2765–2773.
- Goldberger, A. L. et al. (2000). PhysioBank, PhysioToolkit, and PhysioNet. *Circulation*, 101(23), e215–e220.
- Niedermeyer, E. (1997). Alpha Rhythms as Physiological and Abnormal Phenomena. *International Journal of Psychophysiology*, 26(1), 31–49.
