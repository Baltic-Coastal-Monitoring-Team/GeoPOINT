<img src="http://c5studio.pl/geopoint/geopoint-logo.png" width="500px">

# GeoPOINT – Synthetic Point Generator for Geospatial Applications

**GeoPOINT** is a Python-based tool for generating **synthetic 3D points** to support geospatial, geodetic, and computational experiments.  
It allows users to simulate ideal point distributions, add measurement noise, and apply geodetic distortion models, followed by transformations into local reference frames.  
The tool is implemented as a Jupyter Notebook (`GeoPOINT_generator.ipynb`).



## Key Features

- **Synthetic point generation** on a spherical surface in a fixed (global) coordinate frame.  
  Three modes are supported:
  1. **Ideal points (noise-free)** – purely mathematical coordinates uniformly distributed on a sphere.  
  2. **Points with Gaussian noise** – adds random errors to simulate stochastic measurement noise.  
  3. **Points with geodetic distortion** – combines Gaussian noise with systematic EDM (Electronic Distance Meter) and angular errors, simulating real Total Station observations.  

- **Transformation of coordinates** into a local (body) frame:  
  - Translation along X, Y, Z axes.  
  - Rotation (roll, pitch, yaw) specified in **gradians** and internally converted to radians.  
  - Optional scaling for dimensional adjustments.  

- **Configurable error and transformation parameters** via a central configuration dictionary.

- **Export to Excel (`.xlsx`)** using `pandas` and `openpyxl` – both fixed and transformed coordinate sets are saved.  

- **Visualization and statistics** of generated and transformed datasets:  
  - 3D scatter plots and wireframe reference sphere.  
  - Comparison between datasets (ideal vs. noisy vs. distorted).  
  - Error vectors illustrating displacements between point sets.  
  - Export of figures as `.jpg` images.  



## Installation

The repository provides an **`environment.yml`** file for Conda.  
To set up the environment:

```bash
# Clone the repository
git clone https://github.com/Baltic-Coastal-Monitoring-Team/GeoPOINT.git
cd GeoPOINT

# Create environment
conda env create -f environment.yml

# Activate environment
conda activate geopoint
```

This environment includes:
- Python 3.11
- numpy, pandas, matplotlib, scipy, sympy
- openpyxl (Excel export)
- jupyterlab (for running the notebook)
- torch (optional numerical extensions)



## Quick Start

1. Launch JupyterLab:

```bash
jupyter lab
```

2. Open the **`GeoPOINT_generator.ipynb`** notebook.  
3. Configure parameters in the `config` dictionary, e.g.:

```python
config = {
    "npoints": 50,          # Number of synthetic points
    "radius": 50.0,         # Sphere radius [m]

    # Geodetic error model
    "A": 0.005,             # Constant EDM error [m]
    "B": 7.0,               # Distance-dependent EDM error [ppm]
    "angle_noise_arcsec": 1.0,  # Angular precision [arcseconds]

    # Rotation parameters [grads]
    "roll": 5,
    "pitch": 5,
    "yaw": 40,

    # Translation parameters [m]
    "dx": 10.0,
    "dy": 10.0,
    "dz": 2.0,

    # Noise added to transformed points [m]
    "gaussian_std": 0.01,
    "bias": 0.002
}
```

4. Run the generation cells to create ideal, noisy, or distorted points.  
5. Transform coordinates into the local body frame.  
6. Export results to **Excel** and plots to **.jpg** images.



## Repository Structure

```
GeoPOINT/
├─ GeoPOINT_generator.ipynb   # main notebook with generator code
├─ environment.yml            # conda environment file
├─ exports_excel/             # results in XLSX format
├─ exports_img/               # results in jpg format
└─ README.md                  # project documentation
```



## Documentation and Instructions

All instructions, detailed explanations, and usage notes are provided **directly in the Jupyter Notebook (`GeoPOINT_generator.ipynb`)**.  
Code comments further clarify each computational step, configuration parameter, and transformation method.  
Users are encouraged to explore the notebook to fully understand the workflow and adapt it to their specific applications.



## Functions and Parameters

### Data Input
- `INPUT_CFG["source"]` – `"synthetic" | "pcd" | "csv"`
- `INPUT_CFG["path"]` – `"auto"` (Open3D demo cloud) or path to file
- `INPUT_CFG["csv"]` – delimiter, usecols, skiprows (for CSV input)

### Synthetic Data Generation
- `uniform_spherical(config)` – ideal points on a sphere
- `uniform_spherical_with_noise(config)` – adds Gaussian angular noise
- `uniform_spherical_with_geodetic_distortion(config)` – adds EDM-based errors

### Transformation Parameters
- `config["roll"], ["pitch"], ["yaw"]` – rotations (grads)
- `config["dx"], ["dy"], ["dz"]` – translations [m]

### Noise Parameters
- `config["gaussian_std"]` – Gaussian noise σ [m]
- `config["bias"]` – constant bias [m]
- `config["A"], ["B"]` – EDM error model (A in m, B in ppm)

### Visualization Utilities
- `plot_on_ax(ax, points, title, color)` – scatter + reference sphere
- `plot_uncertainty_suite(...)` – histograms, CDF, Q–Q plots, |Δ| vs range



## Short functions description

Below is a list of the main functions provided in the GeoPOINT notebook, with short descriptions of their purpose.  
All functions include inline comments and docstrings for further detail.

### Data Input and Configuration
- **`load_points_from_source(cfg, synthetic_xyz=None)`**  
  Unified entry point for loading point data.  
  Depending on `cfg["source"]`, this will:
  - generate synthetic points (`"synthetic"`),
  - load a `.pcd` file (direct path or `"auto"` via Open3D demo),
  - load a `.csv` table with XYZ columns.  

- **`_load_pcd(path_str)`**  
  Reads `.pcd` point clouds using Open3D.  

- **`_load_csv_xyz(path_str, cfg_csv)`**  
  Reads XYZ data from CSV files with user-defined delimiter and columns.  

### Synthetic Point Generation
- **`uniform_spherical(config)`**  
  Generates ideal points on a sphere.  

- **`uniform_spherical_with_noise(config, theta=None, phi=None)`**  
  Adds Gaussian angular noise to spherical directions.  

- **`uniform_spherical_with_geodetic_distortion(config, theta=None, phi=None)`**  
  Adds EDM-based range errors (A, B) and angular noise.  

### Transformation and Error Modelling
- **Rotation/translation pipeline (matrix `R`, vector `T`)**  
  Applies user-defined rigid-body transformations (roll, pitch, yaw, dx, dy, dz).  

- **`add_real_noise_pair(transformed_sets, base_label="real_input", ...)`**  
  Creates a *clean/noisy* pair of real-data clouds by injecting Gaussian noise, bias, or EDM errors.  

- **`noise_only_residuals(transformed_sets, clean_label, noisy_label)`**  
  Computes per-axis errors (Δx, Δy, Δz), error magnitudes |Δ|, and ranges r.  

### Visualization
- **`plot_on_ax(ax, points, title, color)`**  
  Plots 3D scatter points with a reference wireframe sphere.  

- **`plot_uncertainty_suite(transformed_sets, clean_label, noisy_label, ...)`**  
  Produces per-axis histograms, cumulative distribution, Q–Q plots, and error vs range visualizations.  

- **Alternative visualizations (2D projections, PCA hexbin, displacement histograms)**  
  Provide compact views for both synthetic and real datasets.  



## Tested Environments

GeoPOINT has been successfully tested on:
- Python 3.11 (Conda, environment.yml provided)
- Ubuntu 22.04 LTS
- macOS 14.5 (Intel and Apple Silicon)
- Windows 11 (via WSL2)



## Example Applications

- Testing geodetic instrument error models (EDM, angular precision).  
- Benchmarking coordinate transformation workflows.  
- Teaching and demonstration of geodetic concepts (roll/pitch/yaw, local frames).  
- Producing synthetic datasets for algorithm validation.  



## Contributing

Contributions are welcome:  
- Bug fixes and improvements.  
- Additional error models or transformation options.  
- Examples of usage in geodetic or geospatial projects.  



## Citation

If you use GeoPOINT in your research or publications, please cite:

[![DOI](https://zenodo.org/badge/1023774805.svg)](https://doi.org/10.5281/zenodo.17065538)



## License

MIT License – see `LICENSE` file.

