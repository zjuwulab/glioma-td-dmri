# Glioma td-dMRI analysis

Download and extract [glioma-td-dmri.zip](glioma-td-dmri.zip), then run the commands below from the extracted directory.

JOINT fitting with NLLS initialization and Bayesian estimation (MATLAB), and histology quantification after nuclear segmentation (Python).

## Requirements

- MATLAB with Optimization Toolbox; a NIfTI toolbox for parameter mapping.
- Parallel Computing Toolbox for the optional gradient helpers.
- Python 3.9 or later for histology calculations (standard library).

## JOINT

Run the synthetic example from the repository directory:

```matlab
setup_joint
example_joint
```

Fit normalized signals with your acquisition matrix:

```matlab
result = fit_joint_signals(signal, seqParams);
```

`signal` is an N-voxel × N-measurement matrix. `result.nlls` contains NLLS estimates; `result.parameters` contains posterior means of `fin, diameter, Dex, Din, kin`, in units of dimensionless, µm, µm²/ms, µm²/ms and s⁻¹.

- Acquisition matrix and units: [study_sequence.m](joint/examples/study_sequence.m).
- Fitting settings and priors: [fit_joint_signals.m](joint/fit_joint_signals.m).
- NIfTI mapping: set paths using [example_config.m](joint/examples/example_config.m), then call `run_joint_nifti(cfg)`. Input volumes follow the acquisition-matrix order.

## Histology

```sh
python histology/postprocess.py --regions histology/examples/regions.csv --output results/histology.csv
```

Calculates nuclear area fraction, its 3/2 transformation and optional weighted nuclear diameter. See [input formats and formulas](histology/README.md).

## References

- Gustafsson et al. *Magnetic Resonance in Medicine* 2018;79:1674–1683. [DOI](https://doi.org/10.1002/mrm.26783).
- Graham et al. *Medical Image Analysis* 2019;58:101563 (HoVer-Net). [DOI](https://doi.org/10.1016/j.media.2019.101563).

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
