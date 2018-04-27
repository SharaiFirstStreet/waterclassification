# waterclassification
Determine terrestrial water, wetlands, oceans and rivers from satellite imagery 
New method for clipping under 200cm elevation in SC

Elevation:
North South= mosaic to new raster
Reclassify with 200 threshold, 1 under 200cm & 0 for over 200cm

Roads: 
Reclassify roads to be 0 for roads and 1 for everything else

NDWI:
NDWI south: D:\FloodiQ-Prod\Regions\southcarolina\Working\Water\SC_South\Scaled_Outputs\ndwi_sd.tif 
NDWI north: D:\FloodiQ-Prod\Regions\southcarolina\Working\Water\SC_North\Scaled_Outputs\ndwi_sd.tif

1. Reclassify:  reclass NDWI with a 146 threshold limit. 1 for "water" (146 and over) and 0 for "land" (under 146)
2. Raster Calculator: multiply reclassified NDWI layer with binary Roads layer (D:\FloodiQ-Prod\Regions\southcarolina\Working\roads\sc_2017_rds_buffer_rast30ft2_reclass.tif)
3. Sieve: sieve by 100,000


NIR:  
NIR south: D:\FloodiQ-Prod\Regions\southcarolina\Working\Water\SC_South\Scaled_Outputs\Band4.tif      (Maximum: 244)
NIR north: D:\FloodiQ-Prod\Regions\southcarolina\Working\Water\SC_North\Scaled_Outputs\BandNIR.tif    (Maximum: 244)

1. NIR reverse = (NIR-244)*-1
2. NIR reverse cube divide by 100k: ((NIRreverse * NIRreverse * NIRreverse) / 100000)
3. reclassify: find threshold, in case of SC 60 is good, 60+ = 1, <60 = 0
4. multiply by roads (where roads are 0 and all else is 1)
5. sieve: sieve by 2000 to remove stray pixels

Then (after NIR and NDWI north and south are complete): 
1. Mosaic to new raster the NIR and NDWI files together (2 bit, GCS NAD83, 1 band, mosaic operaror: maximum)
2. Raster calculator the binary elevation file  (D:\FloodiQ-Prod\Regions\southcarolina\Working\Water\SC_NorthSouth_Elev_Reclass200cm.tif)
3. Set water (value of 1) to NoData
4. Clip inundation files 
