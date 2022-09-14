# R_Image_analysis
Automated image analysis

This R code allows for the automated pixel luminance extraction from .BMP images within the specified folder.
Built under RStudio version 2022.07.01. 554 and R version 4.2.1

Note: As written, the code is designed to work with .BMP images with the same image dimensions

Instructions:

1 Ensure working directory is set correctly

2 Ensure required libraries are installed and loaded (Imager, dplyr, bmp, tidyverse, readr, tibble)

3 Set 'PATH' to the desired directory, containing .BMP files in subdirectories

4 Change the bounding box for the region of interest

5 Run through the code

Code may take a while if there are alot of images to analyse, but will eventually save the mean and SD luminance values for every image in 'PATH' in the file "Output.csv" within 'PATH'


The lapply code was adapted from code found in the last comment here: https://stackoverflow.com/questions/68386626/lapply-for-multiple-files

Saving the output of the lapply code was adapted from the code in the last comment here: https://stackoverflow.com/questions/10590904/extracting-outputs-from-lapply-to-a-
