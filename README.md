# Comp-Vision

## 1. Preprocessing (Conversion from CSV and TXT to XML annotations)

- Pipeline that converts from csv to xml (Google Open Images) and Synonym Finder
- Pipeline that converts from txt to xml (Kitti)


## 2. Pre Downloader Visual Check Summary (Open Image Dataset).ipynb
Issue that the Pre-Downloader Visual Check Summary portion of the pipeline attempts to address:<br>

There is time and processing cost involved in downloading large datasets. Before downloading the entire dataset, summary analysis can be carried on csv data and through sample data attained. Such insights can better inform on the suitability and preprocessing required of the dataset.

### Data issues:
There are multiple overlapping bounding boxes due in part to numerous class categories that could technically be subsumed under others (eg. man, woman, human. girl, boy). Narrower filltering of classes also results in missing bounding boxes where only one bounding box may have been present (e.g. girl). A synonym finder has been included (that matches available OID classes with possible synomyns). It is non-exhaustive and partially addresses this issue.

### Description:
Prior to this downloader, a random sample was retrieved from the main dataset (using Fifty One). A smaller random sampling of images (200) from this larger sample was made. Filtering and preprocessing of csv dataframe was carried out before creating xml annotations on these random samples (on specific classes).
Summary analysis was made to check for:

- class imbalance
- prevalence of very small (<3% of image size) or large bounding box sizes (>85%) (signal suitability of image for preprocessing, ingrouping etc)
class location of these very small or large bounding box sizes
- correlation between csv variables
- proportion of istruncated and isoccluded in sample images
- view cropped images of very small (<3% of image size) bounding boxs


## 4. Getty Images Webscraper
Webscrapes Getty Images

### Requirements:

- Selenium
- urlib.request
- time
- os
- ChromeDriver (update the path to your downloaded chromedriver in the code). Before this, download Chromedriver at: https://sites.google.com/chromium.org/driver/ by selecting the appropriate driver based on your chrome version.<br>

### Usage
This Webscraper tool is designed to extract images from the Getty website (https://www.gettyimages.in/), by finding the image elements using the xpath locator and clicking on the next button via a pagination loop to extract images from every page.<br>
In this xpath element locator, we traverse through the whole html tree and locate the image class 'MosaicAsset-module__thumb___epLhd' for all images. For the css element locator we locate and click on the next button '.PaginationRow-module__nextButton___PYo5w' to get the images from each page.

### How to use it:

- Go to https://www.gettyimages.in/ and key in the object you are searching for in the search bar
- Copy the url link (e.g. https://www.gettyimages.com/search/2/image?family=creative&phrase=bag)
- Scroll down webpage end to view feasible number of pages that can be downloaded (do not exceed this number as looping and duplicates will occur)
- Create a destination path for scraped images to reside
- Run the code and input the information above


## 5. Data Augmentation
Using automated pre-trained models to identify and generate masks for semantic segmentation may not always lead to accurately shaped masks if the selected model is not robust or sufficiently tweaked. Binary masking using thresholding is an alternative way to generate accurately shaped masks and image cutouts. Although accuracy may not be possible for all images, we select the best ones and then use data agumentation (via Albumentations library) to generate more transformations on both the masks and cutout images. This also serves to generate more realistic or natural looking shaped image samples. (For e.g. ElasticTransform and GridDistortion can help to reproduce the look of natural compression of a standing bag or elongation due to heavy items).

### Description
- Mash Generation using Thresholding
- Foreground Segmentation and Extraction Using Grabcut (Mask as input)
- Determining BBox Rectangle (based on Mask)
- Transforming Both Images and Masks (Albumentations library)
