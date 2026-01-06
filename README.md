# spectra

Two small Jupyter notebooks:

- `skydome.ipynb`: overlays targets (from a CSV with RA/Dec) on the **AAT skycam** view, with an optional **RA/Dec grid**.
- `fix_csv.ipynb`: fixes a specific CSV export format where RA/Dec are split across multiple columns.

## Requirements

- Python 3.10+ (3.11 OK)
- Jupyter Lab / Notebook
- Packages used by `skydome.ipynb`:
  - `numpy`, `pandas`, `matplotlib`, `ipympl`
  - `requests`, `Pillow`
  - `opencv-python`
  - `astropy`
  - `mplcursors`
  - `cmasher`

Example install:

```bash
python -m pip install -U numpy pandas matplotlib ipympl requests pillow opencv-python astropy mplcursors cmasher
```

## `fix_csv.ipynb` (create `*_fixed.csv`)

This notebook is for CSVs where RA/Dec are split across multiple columns (a common spreadsheet export pattern).

What it does:

- Combines RA pieces into one string: `RA = "hh:mm:ss"` using columns `RA`, `Unnamed: 2`, `Unnamed: 3`
- Combines Dec pieces into one string: `Dec = "±dd:mm:ss"` using columns `Dec`, `Unnamed: 5`, `Unnamed: 6`
- Drops the `Unnamed:*` columns and writes a new CSV

How to use:

- Open `fix_csv.ipynb`
- Edit the filenames in the first two cells:
  - Input: `pd.read_csv("...original.csv")`
  - Output: `df.to_csv("..._fixed.csv", index=False)`
- Run all cells

## `skydome.ipynb` (skycam overlay)

What it shows:

- The live AAT skycam image (optional)
- Your targets projected onto the sky (only targets **above the horizon** are plotted)
- Optional **RA/Dec grid** overlay
- Optional planet markers (Moon + planets when above horizon)
- Hover tooltips (alt/az/airmass + CCD values when present)

### Input CSV expectations

Your CSV must contain either:

- `RA_deg` and `Dec_deg` (in degrees), **or**
- `RA` and `Dec` (strings like `hh:mm:ss` and `±dd:mm:ss`, or numeric values; the notebook auto-parses)

For tooltips / filtering, these columns are used when present:

- `CCD1` (used for a simple filter in the notebook and displayed in tooltips)
- `CCD2`, `CCD3` (displayed in tooltips if present)
- A target name/id column: first found among `Object_ID`, `Object`, `Name`, `Target`, `ID`

### How to use

- Open `skydome.ipynb`
- In the “Only input you should need to change” section, set:
  - `CSV_FILE = ...` (choose one of the constants or set your own filename)
- Run all cells, then run the final plot cell:
  - `plot_static_skycam(df)`

### Plot options

`plot_static_skycam()` supports:

- **RA/Dec grid**:
  - `show_radec_grid=True`
  - `ra_grid_step_deg=30` (default: 2 hours)
  - `dec_grid_step_deg=15`
- **Background image toggle**:
  - `show_background_image=True` (default)
  - `show_background_image=False` for a plain black background

Examples:

```python
plot_static_skycam(df, show_radec_grid=True, ra_grid_step_deg=15, dec_grid_step_deg=10)
plot_static_skycam(df, show_background_image=False)  # black background
```

### Notes / troubleshooting

- The notebook uses `%matplotlib widget`. If you see backend/widget errors, install `ipympl` and restart the kernel.
- The skycam image is downloaded from `SKYCAM_URL` (network required).


