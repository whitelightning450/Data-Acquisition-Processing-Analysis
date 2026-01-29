# HyRiver Introduction
The [HyRiver](https://github.com/hyriver/hyriver.github.io) software suite was developed as a software stack consisting of seven Python libraries that are designed to aid in hydroclimate analysis through web services.

The power of the HyRiver software stack is to bypass time wrangling of various datasets - making sure that dates align, or accounting for spatial misalignment of available data. The HyRiver suite streamlines this process, and makes acquistion of different data from various sources much more efficient. 

In this module, students will have work through demonstrations of how to easily access large amounts of data (streamflow, geophysical, and meteorological) for a basin of interest using three key libraries from the HyRiver stack: `PyGeoHydro`, `PyNHD`, and `PyDaymet`. While there are other libararies within `HyRiver`, the functionality is similar and developers will have the skillsets to utilize the resoruces. Some key elements of the modules we will cover

## PyGeohydro
[`PyGeoHydro`](https://github.com/hyriver/pygeohydro) allows for interaction with eight different online datasets, including: 
* [USGS National Water Information System (NWIS)](https://nwis.waterdata.usgs.gov/nwis) - streamflow time series data for nearly 8000 sites
* [NCAR Catchment Attributes and Meteorology for Large-sample Studies (CAMELS)](https://ral.ucar.edu/solutions/products/camels) - Streamflow, meteorolgical, and catchment data for nearly 700 minimally altered basins with CONUS.
* [USGS Water Quality Portal](https://www.waterqualitydata.us/)
* [US ACE National Invetory of Dams (NID)](https://nid.sec.usace.army.mil/#/)

## PyNHD
The [`PyNHD`](https://github.com/hyriver/pynhd) library is designed to interact with the [National Hydrography Dataset (NHD)](https://www.usgs.gov/national-hydrography/national-hydrography-dataset)and the [Hydro Network-Linked Data Index (NLDI)](https://labs.waterdata.usgs.gov/about-nldi/index.html).

### NHDPlus (National Hydrography Dataset)
The NHD defines a high-resolutioon network of stream linkages, each with a unique idenfier (ComID).  

### NLDI (Network-Linked Data Index)
The NLDI aids in the discovery of indexed information along some NHD-specified geometry (ComIDs). The NLDI essentially tranverses the linkages specified by the NHD geometry and generates data either local or basin-aggregated data relative to a specific linkage (ComID).

As will be seen later in the tutorial, the `NLDI()` function is able to retrieve over 100 different types of data for a given basin.

## PyDaymet
The [PyDaymet GirHub repository](https://github.com/hyriver/pydaymet) provides access to climate data from Daymet V4 database using NetCDF Subset Service (NCSS). Both single pixel (using `get_bycoords` function) and gridded data (using `get_bygeom`) are supported and returned as pandas.DataFrame or xarray.Dataset, respectively. In prior modules we have worked with Pandas and should be knowledgable about a Pandas DataFrame functionality. Xarray is different and can be summarized as:

* **Coordinates:** The dimensions (e.g., Latitude, Longitude, Time, Depth).

* **Data Variables:** The actual measurements (e.g., Precipitation, Temperature, Soil Moisture).

* **Attributes:** Metadata (e.g., units, CRS/projection info, author).

Pandas is designed for tabular data (rows and columns), xarray is designed for multidimensional data that is "labeled." It is the industry standard for handling NetCDF files, which are commonly used in meteorology, oceanography, and hydrology.


## Tutorial outline:
1. [PyNHD](tbd): Retrieving Geophysical (NLDI) Data, getting catchment attributes, and visualization
Retrieving USGS Water Data
2. [PyGeoHydro](tbd): Iteracting with online datasets including USGS NWIS and CAMELS, coupled with visualization
3. [DayMet](tbd): Retrieving Daymet Data, coupled with visualization
4. [HyRiver Synthesis](tbd): Connecting time-series and spatial data to create an informative dataframe - Informatics

## Let's get started!
Geospatial packages can be tricky and have several conflicts. As with other projects, it is a good habit to create a fresh virtual conda environment prior to starting. If you are starting a fresh repository and project, it is a good habit to install geospatial packages that include `GdAl (Geospatial Data Abstration Library)` into a fresh environment, and first! This will prevent potential conflicts with other packages down the line.

### Time Vampire... Mamba arrives to the rescue!

Once we start having more complex environments, `Conda's` solvers can take a really long time and be quite slow. `Mamba` address this problem! The following steps will help you get `Mamba` running on your UU CHPC system. 

    module avail miniforge

`Miniforge` contains `Mamba` and running this line in the terminal will tell us which version to load.

    module load miniforge3/25.11.0

or

    ml miniforge3/25.11.0

This functions just as we have previously done with loading miniconda3.
The difference is the next step, we need to initiate conda to link our .bashrc file to miniforge3. 

    conda init bash

 In the Miniforge distribution, mamba and conda are siblings. When you run conda init bash, it writes the necessary "hooks" into your .bashrc that enable the activate and deactivate commands for both tools. We only need to do this step once
 Lets see if its working:

    which conda

The returned filepath should have our updated miniforge3/25.11.0 path. We need to reset our workspace (i.e., close our terminal and open a new one) to use `Mamba`. In the new terminal, enter:

    bash

This sets us up in a `base` mamba environment. We can now proceed with either installing the provided environment or making our own. Notice we replace `conda` with `mamba`. To use `Mamba` in the future, we only need to load miniforge and then enter `base` into the terminal.

Install provided environment:

    mamba env create -n 310HyRiver_env -f 310HyRiver_env.yml
    mamba activate 310HyRiver_env

Advanced. If you are making a fresh conda environment. Start with the following steps and enter `y` when prompted:

    mamba create -n 310HyRiver_env python=3.10
    mamba activate 310HyRiver_env

If you are making a fresh environment, you will need to continue `mamba install` libraries of your choice
In most cases, we need to install and activate the `ipykernel` library to connect our environment to our Kernel and interactive notebook files:

    mamba install ipykernel
    python -m ipykernel install --user --name=310HyRiver_env

To install HyRiver

    mamba install -c conda-forge py3dep pynhd pygeohydro pydaymet pygridmet pynldas2 hydrosignatures pygeoogc pygeoutils async-retriever


To install additional libraries, do a mamba install:

    mamba install ...

### Using Conda
We can still use Conda to load the project environment. However, it will take much longer. Make sure to load your miniconda module and that your terminal is in the correct folder to load the environment yaml file:

    conda env create -f 310HyRiver_env.yml
    conda activate 310HyRiver_env

In most cases:

    conda install ipykernel
    python -m ipykernel install --user --name=310HyRiver_env

### Additional terminal inputs
The HyRiver package makes use of interactive mapping tools that can challenge UU firewall **trust protocols.** We can approve/trust the .ipynb module by going into the file directory and telling your HPC to trust these files. If you are in this directory:

    mamba install nbformat
    jupyter trust *.ipynb

This should set you up for success!

