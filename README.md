# GeoPOINT – Synthetic Point Generator for Geospatial Applications

**GeoPOINT** is a Python-based tool for generating **synthetic 3D points** to support geospatial, geodetic, and computational experiments.  
It allows users to simulate ideal point distributions, add measurement noise, and apply geodetic distortion models, followed by transformations into local reference frames.  
The tool is implemented as a Jupyter Notebook (`GeoPOINT_generator.ipynb`).

---

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

---

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

---

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

---

## Repository Structure

```
GeoPOINT/
├─ GeoPOINT_generator.ipynb   # main notebook with generator code
├─ environment.yml            # conda environment file
├─ exports_excel/             # results in XLSX format
├─ exports_img/               # results in jpg format
└─ README.md                  # project documentation
```

---

## Documentation and Instructions

All instructions, detailed explanations, and usage notes are provided **directly in the Jupyter Notebook (`GeoPOINT_generator.ipynb`)**.  
Code comments further clarify each computational step, configuration parameter, and transformation method.  
Users are encouraged to explore the notebook to fully understand the workflow and adapt it to their specific applications.

---

## Example Applications

- Testing geodetic instrument error models (EDM, angular precision).  
- Benchmarking coordinate transformation workflows.  
- Teaching and demonstration of geodetic concepts (roll/pitch/yaw, local frames).  
- Producing synthetic datasets for algorithm validation.  

---

## Contributing

Contributions are welcome:  
- Bug fixes and improvements.  
- Additional error models or transformation options.  
- Examples of usage in geodetic or geospatial projects.  

---

## Citation

If you use GeoPOINT in your research or publications, please cite:

soon

---

## License

MIT License – see `LICENSE` file.

---
