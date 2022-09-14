#set wd, change as needed
setwd("Working directory")

#ensure libraries are loaded
library(magick)
library(dplyr)
library(ggplot2)
library(imager)
library(bmp)
library(OpenImageR)
library(tidyverse)
library(readr)
library(tibble)


#note, change 'PATH' variable to folder where images are from first
#seperate directory into sections e.g. C:/Users/User/Desktop would become "C:", "Users", "User" "Desktop", fsep ="/"
PATH <- file.path("C:", "Users", "...", 
                  fsep = "/")

FOLDERS <- list.dirs(PATH, full.names = T, recursive = F) #list folders in PATH

#loads all .BMP files within 'Folders'
files<-list.files(FOLDERS, pattern = ".BMP", full.names = T, ) %>%
  lapply(function(file) {
    Target <- load.image(file)
    # select sampling window - x and y origin is top left corner of image.
    #x is horizontal pixel dimension, y is vertical pixel dimensions
    Crop_Target <- imsub(Target, x %inr% c(160, 659), y %inr% c(502, 536)) 
    Crop_data <- as.data.frame(Crop_Target, wide = "c")
    #renames columns to r,g,b
    names(Crop_data)[3] <-paste("r")
    names(Crop_data)[4] <-paste("g")
    names(Crop_data)[5] <-paste("b")
    
    #converting rgb to luminance according to CIE XYZ colour
    Crop_data$Lum <- 0.2126*(Crop_data$r)^2.2 + 0.7152 * (Crop_data$g)^2.2 + 0.0722* (Crop_data$b)^2.2
    #stats of luminance
    Mean_Lum <- mean(Crop_data$Lum)
    SD_Lum <- sd(Crop_data$Lum)
    Data <- data.frame(Mean_Lum, SD_Lum)
  })

#output file information as data table
Files_output <-  files[[1]]
lapply(2:length(files), function(i) {Files_output <<- rbind(Files_output, files[[i]])}) 
Files_output$Name <- list.files(FOLDERS, pattern = ".BMP") # add file names
Files_output <- Files_output %>% relocate (Name, .before = Mean_Lum) #move file names to front

#this will save table as Output.CSV to the folder specified by 'PATH'
#NOTE: COLUMN ORDER WILL BE DETERMINED BY FILE NAME alphabetically e.g. Test 1, Test 10, Test 2, Test 3
write.table(Files_output, paste(PATH, "Output.csv", sep = "/"), row.names = F, col.names = T, sep = ',')
