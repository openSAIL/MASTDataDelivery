# Documentation on GALEX Spectral Data Returned by DataDelivery

## Description of GALEX Spectra Data Formats

GALEX spectral data come in many different stages of processing and a few different formats.  For the purposes of DataDelivery, only two of these files are relevant.  Both unextracted (two dimensional) grism spectra and extracted (one dimensional) spectra are available in the Portal.

## Description of How DataDelivery Locates Data

DataDelivery requires an *observation ID*, a *filter* value, and a *URL* to identify the correct FITS file to read.  The filter must be one of two values, "FUV" or "NUV".  The URL given (ignoring the leading "https://") is for the static preview image for this observation ID.  The reason the URL is provided to DataDelivery is that the GALEX spectra are organized into complicated subdirectory paths that do not directly tie to either the observation ID or the filter value.  Since the static preview image URLs are already available to the Portal, and the FITS files containing the spectral data are in a similar location, providing the URL to the preview image is the quickest way to generate the file path to the FITS file.

If the preview URL ends with "int_2color.jpg" then the observation ID is referring to a spectral image (2-D grism).  DataDelivery currently only handles one dimensional data, so if DataDelivery finds a 2-D grism preview URL it will immediately return an error JSON with a code referencing that 2-D grism data is not supported at this time.  Otherwise it is assumed the preview URL corresponds to a 1-D extracted spectrum.  The path to the FITS file is generated by stripping the file name and last two subdirectories in the path, then appending "SSAP".  Each extracted spectrum has its own FITS file within the SSAP subdirectory whose file is named the same as its observation ID.

## Description of Data Processing

The spectral data are stored quite simply within each FITS file.  The wavelengths and fluxes are stored in a data table in the first FITS extension, in columns called "wave" and "flux", respectively.  No additional processing or selection of data points to return is performed.
