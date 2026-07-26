================================================================
 README
 Predictive Modeling of New Zealand Real-Time Dispatch
 Energy Prices
 Author: Ibrahim Dodo
================================================================

This file explains exactly how to set up and run the code that
produces all results, tables, and figures in the report.

The code is a single Jupyter notebook:

    project-fixes.ipynb

Running it from top to bottom reproduces every number, table, and
plot used in the report.


----------------------------------------------------------------
 1. WHAT YOU NEED
----------------------------------------------------------------

- A computer with Python 3.9 or newer (3.10 or 3.11 recommended).
- About 1 GB of free disk space.
- No internet connection is needed once the steps below are done.
- The data file (see Section 4).

Python libraries used by the notebook:

    pandas
    numpy
    scipy
    scikit-learn
    xgboost
    matplotlib
    seaborn
    joblib

(The notebook also uses datetime, itertools, os, platform, and
warnings, but these come built in with Python and need no install.)


----------------------------------------------------------------
 2. FOLDER LAYOUT
----------------------------------------------------------------

Place the files like this BEFORE running:

    project-folder/
    |-- project-fixes.ipynb            (the code)
    |-- README.TXT                     (this file)
    |-- data/
    |     |-- EMI_Filtered_2023_2025.csv   (the input data)

The notebook reads the data from:  ./data/EMI_Filtered_2023_2025.csv

So the "data" folder must sit next to the notebook, and the CSV
must be inside it with that exact name.

The notebook creates its own output folder automatically:

    project-folder/
    |-- model_artifacts/               (saved models, made for you)


----------------------------------------------------------------
 3. SETUP (one time)
----------------------------------------------------------------

Open a terminal (Command Prompt, PowerShell, or Terminal) in the
project folder, then run the commands below.

Step 3.1 - (Recommended) Create and activate a virtual environment

    Windows:
        python -m venv venv
        venv\Scripts\activate

    macOS / Linux:
        python3 -m venv venv
        source venv/bin/activate

Step 3.2 - Install the libraries

        pip install pandas numpy scipy scikit-learn xgboost matplotlib seaborn joblib jupyter

    (If "pip" is not found, try "pip3".)

That is the full setup. You only do this once.


----------------------------------------------------------------
 4. THE DATA
----------------------------------------------------------------

Input file:  data/EMI_Filtered_2023_2025.csv

This is the filtered Electricity Authority (EMI) "Dispatch Energy
Prices" dataset, covering January 2023 to December 2025, for the
two North Island nodes used in the study: HLY2201 (Huntly) and
OTA2201 (Otahuhu). Prices are at 30-minute trading periods and the
file contains a Datetime column plus the node prices.

If the CSV is included with this submission, just leave it in the
"data" folder.

If you need to rebuild it, the source data is available from the
EMI portal:
    https://www.emi.ea.govt.nz/Wholesale/Datasets/DispatchAndPricing/DispatchEnergyPrices/
Download the DispatchEnergyPrices data for 2023-2025, keep the two
nodes above, and save the result as
    data/EMI_Filtered_2023_2025.csv


----------------------------------------------------------------
 5. HOW TO RUN
----------------------------------------------------------------

Option A - Jupyter Notebook (simplest)

    1. In the terminal (with the virtual environment active), run:
           jupyter notebook
    2. Your browser opens. Click on  project-fixes.ipynb
    3. In the menu choose:  Kernel  ->  Restart & Run All
    4. Wait for all cells to finish (see runtime note below).

Option B - VS Code

    1. Open the project folder in VS Code.
    2. Open project-fixes.ipynb.
    3. Select the Python interpreter (the "venv" you made).
    4. Click "Run All" at the top of the notebook.

Option C - Google Colab

    1. Upload project-fixes.ipynb to Google Colab.
    2. Upload EMI_Filtered_2023_2025.csv and put it in a folder
       named "data" (or change DATA_PATH in the first code cell).
    3. Run all cells (Runtime -> Run all).

Run the cells IN ORDER from top to bottom. Each cell depends on
the ones before it. The first code cell sets things up; the later
cells build features, train the models, and produce the figures.


----------------------------------------------------------------
 6. WHAT THE CODE PRODUCES
----------------------------------------------------------------

When the notebook finishes, you will find:

In the "data" folder:
    - All result figures as PNG files (plot...png), including the
      model-comparison bars, predicted-vs-actual plots, the full
      test-series plot, the confusion matrices, the feature
      importance plots, and the Diebold-Mariano plots.
    - diebold_mariano_results.csv   (significance test results)
    - rolling_window_sweep.csv      (retraining experiment results)

In the "model_artifacts" folder (created automatically):
    - The trained Huber models and scalers (.joblib files)
    - metadata.json

The tables and numbers in the report are printed in the notebook
output as the cells run.


----------------------------------------------------------------
 7. NOTES
----------------------------------------------------------------

- RUNTIME: Expect a few minutes on a normal laptop, up to about
  15-20 minutes if your machine is slow. Most of the time is spent
  training the Random Forest and XGBoost models.

- REPRODUCIBILITY: All models use a fixed random seed
  (random_state = 42), so re-running gives the same results.

- IF A LIBRARY IS MISSING: the notebook will stop with a message
  like "ModuleNotFoundError: No module named X". Fix it by running
  "pip install X" and then run the notebook again.

- IF THE DATA IS NOT FOUND: you will see "FileNotFoundError" for
  ./data/EMI_Filtered_2023_2025.csv. Check that the CSV is inside
  a folder named "data" next to the notebook, with that exact name.

- ORDER MATTERS: always run the cells from top to bottom. If you
  jump ahead, later cells may fail because earlier variables were
  not created. The safe option is "Restart & Run All".


----------------------------------------------------------------
 8. OPTIONAL: INTERACTIVE WEB APP
----------------------------------------------------------------

A separate, optional Hugging Face (Gradio) web app version of this
project is provided in its own package (nz-dispatch-space). It has
its own README with setup and run steps and is NOT required to
reproduce the report results. The notebook above is the main code.
