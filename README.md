# ProjectMPR
{
    "nbformat_minor": 2, 
    "cells": [
        {
            "source": " <a href=\"https://www.bigdatauniversity.com\"><img src = \"https://ibm.box.com/shared/static/ugcqz6ohbvff804xp84y4kqnvvk3bq1g.png\" width = 300, align = \"center\"></a>\n\n<h1 align=center><font size = 5>Data Analysis with Python</font></h1>", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "# House Sales in King County, USA", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "This dataset contains house sale prices for King County, which includes Seattle. It includes homes sold between May 2014 and May 2015.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "<b>id</b> :a notation for a house\n\n<b> date</b>: Date house was sold\n\n\n<b>price</b>: Price is prediction target\n\n\n<b>bedrooms</b>: Number of Bedrooms/House\n\n\n<b>bathrooms</b>: Number of bathrooms/bedrooms\n\n<b>sqft_living</b>: square footage of the home\n\n<b>sqft_lot</b>: square footage of the lot\n\n\n<b>floors</b> :Total floors (levels) in house\n\n\n<b>waterfront</b> :House which has a view to a waterfront\n\n\n<b>view</b>: Has been viewed\n\n\n<b>condition</b> :How good the condition is  Overall\n\n<b>grade</b>: overall grade given to the housing unit, based on King County grading system\n\n\n<b>sqft_above</b> :square footage of house apart from basement\n\n\n<b>sqft_basement</b>: square footage of the basement\n\n<b>yr_built</b> :Built Year\n\n\n<b>yr_renovated</b> :Year when house was renovated\n\n<b>zipcode</b>:zip code\n\n\n<b>lat</b>: Latitude coordinate\n\n<b>long</b>: Longitude coordinate\n\n<b>sqft_living15</b> :Living room area in 2015(implies-- some renovations) This might or might not have affected the lotsize area\n\n\n<b>sqft_lot15</b> :lotSize area in 2015(implies-- some renovations)", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "You will require the following libraries ", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 1, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "import pandas as pd\nimport matplotlib.pyplot as plt\nimport numpy as np\nimport seaborn as sns\nfrom sklearn.pipeline import Pipeline\nfrom sklearn.preprocessing import StandardScaler,PolynomialFeatures\n%matplotlib inline"
        }, 
        {
            "source": "# 1.0 Importing the Data ", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": " Load the csv:  ", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 2, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "file_name='https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/coursera/project/kc_house_data_NaN.csv'\ndf=pd.read_csv(file_name)"
        }, 
        {
            "source": "\nwe use the method <code>head</code> to display the first 5 columns of the dataframe.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 3, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 3, 
                    "metadata": {}, 
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>Unnamed: 0</th>\n      <th>id</th>\n      <th>date</th>\n      <th>price</th>\n      <th>bedrooms</th>\n      <th>bathrooms</th>\n      <th>sqft_living</th>\n      <th>sqft_lot</th>\n      <th>floors</th>\n      <th>waterfront</th>\n      <th>...</th>\n      <th>grade</th>\n      <th>sqft_above</th>\n      <th>sqft_basement</th>\n      <th>yr_built</th>\n      <th>yr_renovated</th>\n      <th>zipcode</th>\n      <th>lat</th>\n      <th>long</th>\n      <th>sqft_living15</th>\n      <th>sqft_lot15</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>0</th>\n      <td>0</td>\n      <td>7129300520</td>\n      <td>20141013T000000</td>\n      <td>221900.0</td>\n      <td>3.0</td>\n      <td>1.00</td>\n      <td>1180</td>\n      <td>5650</td>\n      <td>1.0</td>\n      <td>0</td>\n      <td>...</td>\n      <td>7</td>\n      <td>1180</td>\n      <td>0</td>\n      <td>1955</td>\n      <td>0</td>\n      <td>98178</td>\n      <td>47.5112</td>\n      <td>-122.257</td>\n      <td>1340</td>\n      <td>5650</td>\n    </tr>\n    <tr>\n      <th>1</th>\n      <td>1</td>\n      <td>6414100192</td>\n      <td>20141209T000000</td>\n      <td>538000.0</td>\n      <td>3.0</td>\n      <td>2.25</td>\n      <td>2570</td>\n      <td>7242</td>\n      <td>2.0</td>\n      <td>0</td>\n      <td>...</td>\n      <td>7</td>\n      <td>2170</td>\n      <td>400</td>\n      <td>1951</td>\n      <td>1991</td>\n      <td>98125</td>\n      <td>47.7210</td>\n      <td>-122.319</td>\n      <td>1690</td>\n      <td>7639</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td>2</td>\n      <td>5631500400</td>\n      <td>20150225T000000</td>\n      <td>180000.0</td>\n      <td>2.0</td>\n      <td>1.00</td>\n      <td>770</td>\n      <td>10000</td>\n      <td>1.0</td>\n      <td>0</td>\n      <td>...</td>\n      <td>6</td>\n      <td>770</td>\n      <td>0</td>\n      <td>1933</td>\n      <td>0</td>\n      <td>98028</td>\n      <td>47.7379</td>\n      <td>-122.233</td>\n      <td>2720</td>\n      <td>8062</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>3</td>\n      <td>2487200875</td>\n      <td>20141209T000000</td>\n      <td>604000.0</td>\n      <td>4.0</td>\n      <td>3.00</td>\n      <td>1960</td>\n      <td>5000</td>\n      <td>1.0</td>\n      <td>0</td>\n      <td>...</td>\n      <td>7</td>\n      <td>1050</td>\n      <td>910</td>\n      <td>1965</td>\n      <td>0</td>\n      <td>98136</td>\n      <td>47.5208</td>\n      <td>-122.393</td>\n      <td>1360</td>\n      <td>5000</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>4</td>\n      <td>1954400510</td>\n      <td>20150218T000000</td>\n      <td>510000.0</td>\n      <td>3.0</td>\n      <td>2.00</td>\n      <td>1680</td>\n      <td>8080</td>\n      <td>1.0</td>\n      <td>0</td>\n      <td>...</td>\n      <td>8</td>\n      <td>1680</td>\n      <td>0</td>\n      <td>1987</td>\n      <td>0</td>\n      <td>98074</td>\n      <td>47.6168</td>\n      <td>-122.045</td>\n      <td>1800</td>\n      <td>7503</td>\n    </tr>\n  </tbody>\n</table>\n<p>5 rows \u00d7 22 columns</p>\n</div>", 
                        "text/plain": "   Unnamed: 0          id             date     price  bedrooms  bathrooms  \\\n0           0  7129300520  20141013T000000  221900.0       3.0       1.00   \n1           1  6414100192  20141209T000000  538000.0       3.0       2.25   \n2           2  5631500400  20150225T000000  180000.0       2.0       1.00   \n3           3  2487200875  20141209T000000  604000.0       4.0       3.00   \n4           4  1954400510  20150218T000000  510000.0       3.0       2.00   \n\n   sqft_living  sqft_lot  floors  waterfront     ...      grade  sqft_above  \\\n0         1180      5650     1.0           0     ...          7        1180   \n1         2570      7242     2.0           0     ...          7        2170   \n2          770     10000     1.0           0     ...          6         770   \n3         1960      5000     1.0           0     ...          7        1050   \n4         1680      8080     1.0           0     ...          8        1680   \n\n   sqft_basement  yr_built  yr_renovated  zipcode      lat     long  \\\n0              0      1955             0    98178  47.5112 -122.257   \n1            400      1951          1991    98125  47.7210 -122.319   \n2              0      1933             0    98028  47.7379 -122.233   \n3            910      1965             0    98136  47.5208 -122.393   \n4              0      1987             0    98074  47.6168 -122.045   \n\n   sqft_living15  sqft_lot15  \n0           1340        5650  \n1           1690        7639  \n2           2720        8062  \n3           1360        5000  \n4           1800        7503  \n\n[5 rows x 22 columns]"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "df.head()"
        }, 
        {
            "source": "#### Question 1 \nDisplay the data types of each column using the attribute dtype, then take a screenshot and submit it, include your code in the image. ", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 5, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "output_type": "stream", 
                    "name": "stdout", 
                    "text": "Unnamed: 0         int64\nid                 int64\ndate              object\nprice            float64\nbedrooms         float64\nbathrooms        float64\nsqft_living        int64\nsqft_lot           int64\nfloors           float64\nwaterfront         int64\nview               int64\ncondition          int64\ngrade              int64\nsqft_above         int64\nsqft_basement      int64\nyr_built           int64\nyr_renovated       int64\nzipcode            int64\nlat              float64\nlong             float64\nsqft_living15      int64\nsqft_lot15         int64\ndtype: object\n"
                }
            ], 
            "source": "print(df.dtypes)"
        }, 
        {
            "source": "We use the method describe to obtain a statistical summary of the dataframe.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 6, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 6, 
                    "metadata": {}, 
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>Unnamed: 0</th>\n      <th>id</th>\n      <th>price</th>\n      <th>bedrooms</th>\n      <th>bathrooms</th>\n      <th>sqft_living</th>\n      <th>sqft_lot</th>\n      <th>floors</th>\n      <th>waterfront</th>\n      <th>view</th>\n      <th>...</th>\n      <th>grade</th>\n      <th>sqft_above</th>\n      <th>sqft_basement</th>\n      <th>yr_built</th>\n      <th>yr_renovated</th>\n      <th>zipcode</th>\n      <th>lat</th>\n      <th>long</th>\n      <th>sqft_living15</th>\n      <th>sqft_lot15</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>count</th>\n      <td>21613.00000</td>\n      <td>2.161300e+04</td>\n      <td>2.161300e+04</td>\n      <td>21600.000000</td>\n      <td>21603.000000</td>\n      <td>21613.000000</td>\n      <td>2.161300e+04</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>...</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n    </tr>\n    <tr>\n      <th>mean</th>\n      <td>10806.00000</td>\n      <td>4.580302e+09</td>\n      <td>5.400881e+05</td>\n      <td>3.372870</td>\n      <td>2.115736</td>\n      <td>2079.899736</td>\n      <td>1.510697e+04</td>\n      <td>1.494309</td>\n      <td>0.007542</td>\n      <td>0.234303</td>\n      <td>...</td>\n      <td>7.656873</td>\n      <td>1788.390691</td>\n      <td>291.509045</td>\n      <td>1971.005136</td>\n      <td>84.402258</td>\n      <td>98077.939805</td>\n      <td>47.560053</td>\n      <td>-122.213896</td>\n      <td>1986.552492</td>\n      <td>12768.455652</td>\n    </tr>\n    <tr>\n      <th>std</th>\n      <td>6239.28002</td>\n      <td>2.876566e+09</td>\n      <td>3.671272e+05</td>\n      <td>0.926657</td>\n      <td>0.768996</td>\n      <td>918.440897</td>\n      <td>4.142051e+04</td>\n      <td>0.539989</td>\n      <td>0.086517</td>\n      <td>0.766318</td>\n      <td>...</td>\n      <td>1.175459</td>\n      <td>828.090978</td>\n      <td>442.575043</td>\n      <td>29.373411</td>\n      <td>401.679240</td>\n      <td>53.505026</td>\n      <td>0.138564</td>\n      <td>0.140828</td>\n      <td>685.391304</td>\n      <td>27304.179631</td>\n    </tr>\n    <tr>\n      <th>min</th>\n      <td>0.00000</td>\n      <td>1.000102e+06</td>\n      <td>7.500000e+04</td>\n      <td>1.000000</td>\n      <td>0.500000</td>\n      <td>290.000000</td>\n      <td>5.200000e+02</td>\n      <td>1.000000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>...</td>\n      <td>1.000000</td>\n      <td>290.000000</td>\n      <td>0.000000</td>\n      <td>1900.000000</td>\n      <td>0.000000</td>\n      <td>98001.000000</td>\n      <td>47.155900</td>\n      <td>-122.519000</td>\n      <td>399.000000</td>\n      <td>651.000000</td>\n    </tr>\n    <tr>\n      <th>25%</th>\n      <td>5403.00000</td>\n      <td>2.123049e+09</td>\n      <td>3.219500e+05</td>\n      <td>3.000000</td>\n      <td>1.750000</td>\n      <td>1427.000000</td>\n      <td>5.040000e+03</td>\n      <td>1.000000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>...</td>\n      <td>7.000000</td>\n      <td>1190.000000</td>\n      <td>0.000000</td>\n      <td>1951.000000</td>\n      <td>0.000000</td>\n      <td>98033.000000</td>\n      <td>47.471000</td>\n      <td>-122.328000</td>\n      <td>1490.000000</td>\n      <td>5100.000000</td>\n    </tr>\n    <tr>\n      <th>50%</th>\n      <td>10806.00000</td>\n      <td>3.904930e+09</td>\n      <td>4.500000e+05</td>\n      <td>3.000000</td>\n      <td>2.250000</td>\n      <td>1910.000000</td>\n      <td>7.618000e+03</td>\n      <td>1.500000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>...</td>\n      <td>7.000000</td>\n      <td>1560.000000</td>\n      <td>0.000000</td>\n      <td>1975.000000</td>\n      <td>0.000000</td>\n      <td>98065.000000</td>\n      <td>47.571800</td>\n      <td>-122.230000</td>\n      <td>1840.000000</td>\n      <td>7620.000000</td>\n    </tr>\n    <tr>\n      <th>75%</th>\n      <td>16209.00000</td>\n      <td>7.308900e+09</td>\n      <td>6.450000e+05</td>\n      <td>4.000000</td>\n      <td>2.500000</td>\n      <td>2550.000000</td>\n      <td>1.068800e+04</td>\n      <td>2.000000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>...</td>\n      <td>8.000000</td>\n      <td>2210.000000</td>\n      <td>560.000000</td>\n      <td>1997.000000</td>\n      <td>0.000000</td>\n      <td>98118.000000</td>\n      <td>47.678000</td>\n      <td>-122.125000</td>\n      <td>2360.000000</td>\n      <td>10083.000000</td>\n    </tr>\n    <tr>\n      <th>max</th>\n      <td>21612.00000</td>\n      <td>9.900000e+09</td>\n      <td>7.700000e+06</td>\n      <td>33.000000</td>\n      <td>8.000000</td>\n      <td>13540.000000</td>\n      <td>1.651359e+06</td>\n      <td>3.500000</td>\n      <td>1.000000</td>\n      <td>4.000000</td>\n      <td>...</td>\n      <td>13.000000</td>\n      <td>9410.000000</td>\n      <td>4820.000000</td>\n      <td>2015.000000</td>\n      <td>2015.000000</td>\n      <td>98199.000000</td>\n      <td>47.777600</td>\n      <td>-121.315000</td>\n      <td>6210.000000</td>\n      <td>871200.000000</td>\n    </tr>\n  </tbody>\n</table>\n<p>8 rows \u00d7 21 columns</p>\n</div>", 
                        "text/plain": "        Unnamed: 0            id         price      bedrooms     bathrooms  \\\ncount  21613.00000  2.161300e+04  2.161300e+04  21600.000000  21603.000000   \nmean   10806.00000  4.580302e+09  5.400881e+05      3.372870      2.115736   \nstd     6239.28002  2.876566e+09  3.671272e+05      0.926657      0.768996   \nmin        0.00000  1.000102e+06  7.500000e+04      1.000000      0.500000   \n25%     5403.00000  2.123049e+09  3.219500e+05      3.000000      1.750000   \n50%    10806.00000  3.904930e+09  4.500000e+05      3.000000      2.250000   \n75%    16209.00000  7.308900e+09  6.450000e+05      4.000000      2.500000   \nmax    21612.00000  9.900000e+09  7.700000e+06     33.000000      8.000000   \n\n        sqft_living      sqft_lot        floors    waterfront          view  \\\ncount  21613.000000  2.161300e+04  21613.000000  21613.000000  21613.000000   \nmean    2079.899736  1.510697e+04      1.494309      0.007542      0.234303   \nstd      918.440897  4.142051e+04      0.539989      0.086517      0.766318   \nmin      290.000000  5.200000e+02      1.000000      0.000000      0.000000   \n25%     1427.000000  5.040000e+03      1.000000      0.000000      0.000000   \n50%     1910.000000  7.618000e+03      1.500000      0.000000      0.000000   \n75%     2550.000000  1.068800e+04      2.000000      0.000000      0.000000   \nmax    13540.000000  1.651359e+06      3.500000      1.000000      4.000000   \n\n           ...               grade    sqft_above  sqft_basement      yr_built  \\\ncount      ...        21613.000000  21613.000000   21613.000000  21613.000000   \nmean       ...            7.656873   1788.390691     291.509045   1971.005136   \nstd        ...            1.175459    828.090978     442.575043     29.373411   \nmin        ...            1.000000    290.000000       0.000000   1900.000000   \n25%        ...            7.000000   1190.000000       0.000000   1951.000000   \n50%        ...            7.000000   1560.000000       0.000000   1975.000000   \n75%        ...            8.000000   2210.000000     560.000000   1997.000000   \nmax        ...           13.000000   9410.000000    4820.000000   2015.000000   \n\n       yr_renovated       zipcode           lat          long  sqft_living15  \\\ncount  21613.000000  21613.000000  21613.000000  21613.000000   21613.000000   \nmean      84.402258  98077.939805     47.560053   -122.213896    1986.552492   \nstd      401.679240     53.505026      0.138564      0.140828     685.391304   \nmin        0.000000  98001.000000     47.155900   -122.519000     399.000000   \n25%        0.000000  98033.000000     47.471000   -122.328000    1490.000000   \n50%        0.000000  98065.000000     47.571800   -122.230000    1840.000000   \n75%        0.000000  98118.000000     47.678000   -122.125000    2360.000000   \nmax     2015.000000  98199.000000     47.777600   -121.315000    6210.000000   \n\n          sqft_lot15  \ncount   21613.000000  \nmean    12768.455652  \nstd     27304.179631  \nmin       651.000000  \n25%      5100.000000  \n50%      7620.000000  \n75%     10083.000000  \nmax    871200.000000  \n\n[8 rows x 21 columns]"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "df.describe()"
        }, 
        {
            "source": "# 2.0 Data Wrangling", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "#### Question 2 \nDrop the columns <code>\"id\"</code>  and <code>\"Unnamed: 0\"</code> from axis 1 using the method <code>drop()</code>, then use the method <code>describe()</code> to obtain a statistical summary of the data. Take a screenshot and submit it, make sure the inplace parameter is set to <code>True</code>", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 7, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 7, 
                    "metadata": {}, 
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>price</th>\n      <th>bedrooms</th>\n      <th>bathrooms</th>\n      <th>sqft_living</th>\n      <th>sqft_lot</th>\n      <th>floors</th>\n      <th>waterfront</th>\n      <th>view</th>\n      <th>condition</th>\n      <th>grade</th>\n      <th>sqft_above</th>\n      <th>sqft_basement</th>\n      <th>yr_built</th>\n      <th>yr_renovated</th>\n      <th>zipcode</th>\n      <th>lat</th>\n      <th>long</th>\n      <th>sqft_living15</th>\n      <th>sqft_lot15</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>count</th>\n      <td>2.161300e+04</td>\n      <td>21600.000000</td>\n      <td>21603.000000</td>\n      <td>21613.000000</td>\n      <td>2.161300e+04</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n      <td>21613.000000</td>\n    </tr>\n    <tr>\n      <th>mean</th>\n      <td>5.400881e+05</td>\n      <td>3.372870</td>\n      <td>2.115736</td>\n      <td>2079.899736</td>\n      <td>1.510697e+04</td>\n      <td>1.494309</td>\n      <td>0.007542</td>\n      <td>0.234303</td>\n      <td>3.409430</td>\n      <td>7.656873</td>\n      <td>1788.390691</td>\n      <td>291.509045</td>\n      <td>1971.005136</td>\n      <td>84.402258</td>\n      <td>98077.939805</td>\n      <td>47.560053</td>\n      <td>-122.213896</td>\n      <td>1986.552492</td>\n      <td>12768.455652</td>\n    </tr>\n    <tr>\n      <th>std</th>\n      <td>3.671272e+05</td>\n      <td>0.926657</td>\n      <td>0.768996</td>\n      <td>918.440897</td>\n      <td>4.142051e+04</td>\n      <td>0.539989</td>\n      <td>0.086517</td>\n      <td>0.766318</td>\n      <td>0.650743</td>\n      <td>1.175459</td>\n      <td>828.090978</td>\n      <td>442.575043</td>\n      <td>29.373411</td>\n      <td>401.679240</td>\n      <td>53.505026</td>\n      <td>0.138564</td>\n      <td>0.140828</td>\n      <td>685.391304</td>\n      <td>27304.179631</td>\n    </tr>\n    <tr>\n      <th>min</th>\n      <td>7.500000e+04</td>\n      <td>1.000000</td>\n      <td>0.500000</td>\n      <td>290.000000</td>\n      <td>5.200000e+02</td>\n      <td>1.000000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>1.000000</td>\n      <td>1.000000</td>\n      <td>290.000000</td>\n      <td>0.000000</td>\n      <td>1900.000000</td>\n      <td>0.000000</td>\n      <td>98001.000000</td>\n      <td>47.155900</td>\n      <td>-122.519000</td>\n      <td>399.000000</td>\n      <td>651.000000</td>\n    </tr>\n    <tr>\n      <th>25%</th>\n      <td>3.219500e+05</td>\n      <td>3.000000</td>\n      <td>1.750000</td>\n      <td>1427.000000</td>\n      <td>5.040000e+03</td>\n      <td>1.000000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>3.000000</td>\n      <td>7.000000</td>\n      <td>1190.000000</td>\n      <td>0.000000</td>\n      <td>1951.000000</td>\n      <td>0.000000</td>\n      <td>98033.000000</td>\n      <td>47.471000</td>\n      <td>-122.328000</td>\n      <td>1490.000000</td>\n      <td>5100.000000</td>\n    </tr>\n    <tr>\n      <th>50%</th>\n      <td>4.500000e+05</td>\n      <td>3.000000</td>\n      <td>2.250000</td>\n      <td>1910.000000</td>\n      <td>7.618000e+03</td>\n      <td>1.500000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>3.000000</td>\n      <td>7.000000</td>\n      <td>1560.000000</td>\n      <td>0.000000</td>\n      <td>1975.000000</td>\n      <td>0.000000</td>\n      <td>98065.000000</td>\n      <td>47.571800</td>\n      <td>-122.230000</td>\n      <td>1840.000000</td>\n      <td>7620.000000</td>\n    </tr>\n    <tr>\n      <th>75%</th>\n      <td>6.450000e+05</td>\n      <td>4.000000</td>\n      <td>2.500000</td>\n      <td>2550.000000</td>\n      <td>1.068800e+04</td>\n      <td>2.000000</td>\n      <td>0.000000</td>\n      <td>0.000000</td>\n      <td>4.000000</td>\n      <td>8.000000</td>\n      <td>2210.000000</td>\n      <td>560.000000</td>\n      <td>1997.000000</td>\n      <td>0.000000</td>\n      <td>98118.000000</td>\n      <td>47.678000</td>\n      <td>-122.125000</td>\n      <td>2360.000000</td>\n      <td>10083.000000</td>\n    </tr>\n    <tr>\n      <th>max</th>\n      <td>7.700000e+06</td>\n      <td>33.000000</td>\n      <td>8.000000</td>\n      <td>13540.000000</td>\n      <td>1.651359e+06</td>\n      <td>3.500000</td>\n      <td>1.000000</td>\n      <td>4.000000</td>\n      <td>5.000000</td>\n      <td>13.000000</td>\n      <td>9410.000000</td>\n      <td>4820.000000</td>\n      <td>2015.000000</td>\n      <td>2015.000000</td>\n      <td>98199.000000</td>\n      <td>47.777600</td>\n      <td>-121.315000</td>\n      <td>6210.000000</td>\n      <td>871200.000000</td>\n    </tr>\n  </tbody>\n</table>\n</div>", 
                        "text/plain": "              price      bedrooms     bathrooms   sqft_living      sqft_lot  \\\ncount  2.161300e+04  21600.000000  21603.000000  21613.000000  2.161300e+04   \nmean   5.400881e+05      3.372870      2.115736   2079.899736  1.510697e+04   \nstd    3.671272e+05      0.926657      0.768996    918.440897  4.142051e+04   \nmin    7.500000e+04      1.000000      0.500000    290.000000  5.200000e+02   \n25%    3.219500e+05      3.000000      1.750000   1427.000000  5.040000e+03   \n50%    4.500000e+05      3.000000      2.250000   1910.000000  7.618000e+03   \n75%    6.450000e+05      4.000000      2.500000   2550.000000  1.068800e+04   \nmax    7.700000e+06     33.000000      8.000000  13540.000000  1.651359e+06   \n\n             floors    waterfront          view     condition         grade  \\\ncount  21613.000000  21613.000000  21613.000000  21613.000000  21613.000000   \nmean       1.494309      0.007542      0.234303      3.409430      7.656873   \nstd        0.539989      0.086517      0.766318      0.650743      1.175459   \nmin        1.000000      0.000000      0.000000      1.000000      1.000000   \n25%        1.000000      0.000000      0.000000      3.000000      7.000000   \n50%        1.500000      0.000000      0.000000      3.000000      7.000000   \n75%        2.000000      0.000000      0.000000      4.000000      8.000000   \nmax        3.500000      1.000000      4.000000      5.000000     13.000000   \n\n         sqft_above  sqft_basement      yr_built  yr_renovated       zipcode  \\\ncount  21613.000000   21613.000000  21613.000000  21613.000000  21613.000000   \nmean    1788.390691     291.509045   1971.005136     84.402258  98077.939805   \nstd      828.090978     442.575043     29.373411    401.679240     53.505026   \nmin      290.000000       0.000000   1900.000000      0.000000  98001.000000   \n25%     1190.000000       0.000000   1951.000000      0.000000  98033.000000   \n50%     1560.000000       0.000000   1975.000000      0.000000  98065.000000   \n75%     2210.000000     560.000000   1997.000000      0.000000  98118.000000   \nmax     9410.000000    4820.000000   2015.000000   2015.000000  98199.000000   \n\n                lat          long  sqft_living15     sqft_lot15  \ncount  21613.000000  21613.000000   21613.000000   21613.000000  \nmean      47.560053   -122.213896    1986.552492   12768.455652  \nstd        0.138564      0.140828     685.391304   27304.179631  \nmin       47.155900   -122.519000     399.000000     651.000000  \n25%       47.471000   -122.328000    1490.000000    5100.000000  \n50%       47.571800   -122.230000    1840.000000    7620.000000  \n75%       47.678000   -122.125000    2360.000000   10083.000000  \nmax       47.777600   -121.315000    6210.000000  871200.000000  "
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "df.drop(['id', 'Unnamed: 0'], axis=1, inplace=True)\ndf.describe()"
        }, 
        {
            "source": "we can see we have missing values for the columns <code> bedrooms</code>  and <code> bathrooms </code>", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 8, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "output_type": "stream", 
                    "name": "stdout", 
                    "text": "number of NaN values for the column bedrooms : 13\nnumber of NaN values for the column bathrooms : 10\n"
                }
            ], 
            "source": "print(\"number of NaN values for the column bedrooms :\", df['bedrooms'].isnull().sum())\nprint(\"number of NaN values for the column bathrooms :\", df['bathrooms'].isnull().sum())\n"
        }, 
        {
            "source": "\nWe can replace the missing values of the column <code>'bedrooms'</code> with the mean of the column  <code>'bedrooms' </code> using the method replace. Don't forget to set the <code>inplace</code> parameter top <code>True</code>", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 9, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "mean=df['bedrooms'].mean()\ndf['bedrooms'].replace(np.nan,mean, inplace=True)"
        }, 
        {
            "source": "\nWe also replace the missing values of the column <code>'bathrooms'</code> with the mean of the column  <code>'bedrooms' </codse> using the method replace.Don't forget to set the <code> inplace </code>  parameter top <code> Ture </code>", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 10, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "mean=df['bathrooms'].mean()\ndf['bathrooms'].replace(np.nan,mean, inplace=True)"
        }, 
        {
            "execution_count": 11, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "output_type": "stream", 
                    "name": "stdout", 
                    "text": "number of NaN values for the column bedrooms : 0\nnumber of NaN values for the column bathrooms : 0\n"
                }
            ], 
            "source": "print(\"number of NaN values for the column bedrooms :\", df['bedrooms'].isnull().sum())\nprint(\"number of NaN values for the column bathrooms :\", df['bathrooms'].isnull().sum())"
        }, 
        {
            "source": "# 3.0 Exploratory data analysis", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "#### Question 3\nUse the method value_counts to count the number of houses with unique floor values, use the method .to_frame() to convert it to a dataframe.\n", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 12, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 12, 
                    "metadata": {}, 
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>floors</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>1.0</th>\n      <td>10680</td>\n    </tr>\n    <tr>\n      <th>2.0</th>\n      <td>8241</td>\n    </tr>\n    <tr>\n      <th>1.5</th>\n      <td>1910</td>\n    </tr>\n    <tr>\n      <th>3.0</th>\n      <td>613</td>\n    </tr>\n    <tr>\n      <th>2.5</th>\n      <td>161</td>\n    </tr>\n    <tr>\n      <th>3.5</th>\n      <td>8</td>\n    </tr>\n  </tbody>\n</table>\n</div>", 
                        "text/plain": "     floors\n1.0   10680\n2.0    8241\n1.5    1910\n3.0     613\n2.5     161\n3.5       8"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "df['floors'].value_counts().to_frame()"
        }, 
        {
            "source": "### Question 4\nUse the function <code>boxplot</code> in the seaborn library  to  determine whether houses with a waterfront view or without a waterfront view have more price outliers .", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 14, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "output_type": "stream", 
                    "name": "stderr", 
                    "text": "/opt/conda/envs/DSX-Python35/lib/python3.5/site-packages/seaborn/categorical.py:462: FutureWarning: remove_na is deprecated and is a private function. Do not use.\n  box_data = remove_na(group_data)\n"
                }, 
                {
                    "execution_count": 14, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "<matplotlib.axes._subplots.AxesSubplot at 0x7f24f8d0b208>"
                    }, 
                    "output_type": "execute_result"
                }, 
                {
                    "output_type": "display_data", 
                    "data": {
                        "image/png": "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\n", 
                        "text/plain": "<matplotlib.figure.Figure at 0x7f253cf996d8>"
                    }, 
                    "metadata": {}
                }
            ], 
            "source": "sns.boxplot(x='waterfront', y='price', data=df)"
        }, 
        {
            "source": "### Question 5\nUse the function <code> regplot</code>  in the seaborn library  to  determine if the feature <code>sqft_above</code> is negatively or positively correlated with price.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 15, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 15, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "<matplotlib.axes._subplots.AxesSubplot at 0x7f24f8ca7d30>"
                    }, 
                    "output_type": "execute_result"
                }, 
                {
                    "output_type": "display_data", 
                    "data": {
                        "image/png": "iVBORw0KGgoAAAANSUhEUgAAAaEAAAELCAYAAABwLzlKAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzsvXuUXPV15/vZ59SrH9UPSd2S0CMgWyDAr4DGxjcsQpzEBs8EPPfaicmdaybDXHETJ87jJhc8N7GzcDIDK7PGMRmPg26cCUwSE4eJY925YIJNFJwZZCPAGGMEEpJArVe3pFZ3Vdf7nH3/OOdUV3VXd1e3urr6sT9r9aqqX/3O+Z2q7v7t89v7+9tbVBXDMAzDaAdOuy/AMAzDWLuYETIMwzDahhkhwzAMo22YETIMwzDahhkhwzAMo22YETIMwzDahhkhwzAMo22YETIMwzDaRkuNkIj8uoi8IiI/EJGviEhKRK4Qke+IyGER+SsRSYR9k+HrI+H7l9ec59Nh+2si8qGa9lvCtiMicm9N+7zHMAzDMJYeaVXGBBHZAvwjcI2q5kXkq8DjwIeBv1HVR0Xkj4GXVPVLIvJLwLtU9f8QkY8D/1xVf05ErgG+ArwXuAz4JnBlOMzrwE8DQ8BzwB2q+sNwrKbHmO1zbNiwQS+//PJF/W4MwzBWO88///w5VR2Yq1+sxdcRAzpEpAx0AqeBDwA/H77/MPC7wJeA28PnAI8B/1FEJGx/VFWLwDEROUJgkACOqOpRABF5FLhdRF6d7xg6iyW+/PLLOXjw4AI/vmEYxtpERN5spl/L3HGqehL498BbBMZnDHgeuKiqlbDbELAlfL4FOBEeWwn7r69tn3LMTO3rFzCGYRiG0QZaZoREpJ9g5XEFgRutC7i1QddoFSIzvLdY7bONUYeI7BGRgyJycGRkpMEhhmEYxmLQSmHCTwHHVHVEVcvA3wD/E9AnIpEbcCtwKnw+BGwDCN/vBS7Utk85Zqb2cwsYow5V3auqu1V198DAnC5NwzAMY4G00gi9BdwgIp1hbOcngR8Cfw98NOxzJ/D18Pm+8DXh+0+HsZp9wMdDZdsVwE7guwRChJ2hEi4BfBzYFx4z3zEMwzCMNtAyYYKqfkdEHgNeACrAi8Be4P8DHhWR3wvbvhwe8mXgv4TCgwsERgVVfSVUu/0wPM8nVdUDEJFfBp4EXOBPVfWV8Fz3zGcMwzAMoz20TKK9Wti9e7eaOs4wjHay/9AwDz1zlBOjObb1d3L3TTu4eddguy9rVkTkeVXdPVc/y5hgGIaxjNl/aJjP7HuF4UyBvo44w5kCn9n3CvsPDbf70hYFM0KGYRjLmIeeOUrcFToTMUSCx7grPPTM0XZf2qJgRsgwDGMZc2I0R0fcrWvriLsMjebadEWLixkhwzCMZcy2/k7yZa+uLV/22Nrf2aYrWlzMCBmGYSxj7r5pB2VPyZUqqAaPZU+5+6Yd7b60RcGMkGEYxjLm5l2D3HfbtQymU4zlywymU9x327XLXh3XLK1OYGoYhmFcIjfvGlw1RmcqthIyDMMw2oYZIcMwDKNtmBEyDMMw2oYZIcMwDKNtmBEyDMMw2oYZIcMwDKNtmBEyDMMw2oYZIcMwDKNtmBEyDMMw2oZlTDAMw2iSlVhcbrnTspWQiFwlIt+r+RkXkV8TkXUi8pSIHA4f+8P+IiIPisgREfm+iFxXc647w/6HReTOmvbrReTl8JgHRUTC9nmPYRiGMRurvbhcu2iZEVLV11T1Par6HuB6IAd8DbgX+Jaq7gS+Fb4GuBXYGf7sAb4EgUEBPgu8D3gv8NnIqIR99tQcd0vYPq8xDMMw5mK1F5drF0sVE/pJ4A1VfRO4HXg4bH8Y+Ej4/HbgEQ04APSJyGbgQ8BTqnpBVUeBp4Bbwvd6VPVZVVXgkSnnms8YhmEYs7Lai8u1i6WKCX0c+Er4fKOqngZQ1dMiEjlUtwAnao4ZCttmax9q0L6QMU7XXqyI7CFYKbF9+/Z5fVDDMFYn2/o7Gc4U6ExMTpvLqbjcSo1XtXwlJCIJ4Dbgr+fq2qBNF9C+kDHqG1T3qupuVd09MDAwxykNw1gLLOficis5XrUU7rhbgRdU9Wz4+mzkAgsfo29pCNhWc9xW4NQc7VsbtC9kDMMwjFlZzsXlVnK8ainccXcw6YoD2AfcCdwfPn69pv2XReRRAhHCWOhKexL4tzVihA8Cn1bVCyKSEZEbgO8AnwD+aCFjLPonNgxjVbJci8udGM3R1xGva1sp8aqWGiER6QR+Gri7pvl+4KsichfwFvCxsP1x4MPAEQIl3S8AhMbmc8BzYb/7VPVC+PwXgT8DOoAnwp95j2EYhrGSWe7xqtmQQFhmzMTu3bv14MGD7b4MwzCMGYliQnFX6Ii75MseZU/b6i4UkedVdfdc/Sxtj2EYxgpnOcer5sLS9hiGYawClmu8ai5sJWQYhmG0DTNChmEYRtswI2QYhmG0DTNChmEYRtswI2QYhmG0DTNChmEYRtswI2QYhmG0DTNChmEYRtswI2QYhmG0DTNChmEYRtswI2QYhmG0DTNChmEYRtswI2QYhmG0DTNChmEYRttoqRESkT4ReUxEDonIqyLyfhFZJyJPicjh8LE/7Csi8qCIHBGR74vIdTXnuTPsf1hE7qxpv15EXg6PeVBEJGyf9xiGYRjG0tPqldAXgG+o6i7g3cCrwL3At1R1J/Ct8DXArcDO8GcP8CUIDArwWeB9wHuBz0ZGJeyzp+a4W8L2eY1hGIZhtIeWGSER6QFuAr4MoKolVb0I3A48HHZ7GPhI+Px24BENOAD0ichm4EPAU6p6QVVHgaeAW8L3elT1WQ1qlD8y5VzzGcMwDMNoA61cCe0ARoD/LCIvisifiEgXsFFVTwOEj1EpwC3AiZrjh8K22dqHGrSzgDEMwzCMNtBKIxQDrgO+pKo/Ckww6RZrhDRo0wW0z0ZTx4jIHhE5KCIHR0ZG5jilYRiGsVBaaYSGgCFV/U74+jECo3Q2coGFj8M1/bfVHL8VODVH+9YG7SxgjDpUda+q7lbV3QMDA01/YMMwDGN+tMwIqeoZ4ISIXBU2/STwQ2AfECnc7gS+Hj7fB3wiVLDdAIyFrrQngQ+KSH8oSPgg8GT4XkZEbghVcZ+Ycq75jGEYhmG0gViLz/8rwF+ISAI4CvwCgeH7qojcBbwFfCzs+zjwYeAIkAv7oqoXRORzwHNhv/tU9UL4/BeBPwM6gCfCH4D75zOGYRiG0R4kEJYZM7F79249ePBguy/DMAxjRSEiz6vq7rn6WcYEwzAMo22YETIMwzDahhkhwzAMo220WphgGIYBwP5Dwzz0zFFOjObY1t/J3Tft4OZdg3MfaKxqbCVkGEbL2X9omM/se4XhTIG+jjjDmQKf2fcK+w8Nz32wsaoxI2QYRst56JmjxF2hMxFDJHiMu8JDzxxt96UZbcaMkGEYLefEaI6OuFvX1hF3GRrNtemKjOWCGSHDMFrOtv5O8mWvri1f9tja39mmKzKWC2aEDMNoOXfftIOyp+RKFVSDx7Kn3H3TjnZfmtFmTB1nGEbLuXnXIPcRxIaGRnNsXaA6zhR2qw8zQoaxClgJk/PNuwYv6ZoihV3clTqF3X3huY2VibnjDGOFs1bkz6awW52YETKMFc5amZxNYbc6MSNkGCuctTI5m8JudWJGyDBWOGtlcjaF3erEjJBhrHCW0+S8/9Awd+w9wI0PPM0dew8salzq5l2D3HfbtQymU4zlywymU9x327UmSljhWFG7ObCidsZKIFLHXYr8eTGuIVKvdcRd8mWPsqdmKNYozRa1a6lEW0SOAxnAAyqqultE1gF/BVwOHAd+VlVHRUSALxCU384B/1JVXwjPcyfw2+Fpf09VHw7br2eyvPfjwK+qqi5kDMNYyVyq/HkxqBVIAHQmYuRKFR565mjbr81YviyFO+4nVPU9NRbxXuBbqroT+Fb4GuBWYGf4swf4EkBoUD4LvA94L/BZEekPj/lS2Dc67paFjGEYxqWzVgQSxuLSjpjQ7cDD4fOHgY/UtD+iAQeAPhHZDHwIeEpVL6jqKPAUcEv4Xo+qPquBT/GRKeeazxiGYVwia0UgYSwurTZCCvydiDwvInvCto2qehogfIzW6VuAEzXHDoVts7UPNWhfyBiGYVwiy0kgYawcWp2258dU9ZSIDAJPicihWfpKgzZdQPtsNHVMaDD3AGzfvn2OUxqGAYuXH85YW7TUCKnqqfBxWES+RhDTOSsim1X1dOgKizScQ8C2msO3AqfC9puntO8P27c26M8Cxph63XuBvRCo4+bzmQ1jLbMcBBLGyqJl7jgR6RKRdPQc+CDwA2AfcGfY7U7g6+HzfcAnJOAGYCx0pT0JfFBE+kNBwgeBJ8P3MiJyQ6h6+8SUc81nDMMwDKMNtHIltBH4WmAfiAF/qarfEJHngK+KyF3AW8DHwv6PE0injxDIp38BQFUviMjngOfCfvep6oXw+S8yKdF+IvwBuH8+YxiGYRjtwTarzoFtVjUMw5g/y2KzqmEYi8tKqBtkGPPBcscZxgphrdQNMtYWZoQMY4WwVuoGGWsLM0KGsUKwtDjGasRiQoaxQtjW38lwplBNEAorPy2OxbgMWwkZxgphtaXFqY1xuQIvnhjlrkcOcusfPmNxrjWEGSHDWCGstqJuUYyr4imnxgqoD67AsXMTJrhYQ5g7zjBWEKspLc6J0Rx9HXGOjU3gIDiOoIDna1VwsVo+qzEzthIyDKMtRKUfSp6PhKmFVSHhOia4WEOYETIMoy1EMS7XEXxVfFVUYSCdXPGCC6N5zAgZhtEWohjX5es68VQRYHNvEteRFS24MOaHxYQMw2gbUYwrkmoPjeYYTKdMqr2GMCNkGEYd7di7s5oEF8b8MHecYRhVLD+dsdQ0bYRE5EdE5KfC5x1RwTrDMFYPlp/OWGqaMkIi8r8DjwEPhU1bgb9t1UUZhtEeLD+dsdQ0uxL6JPBjwDiAqh4GzIFrGKuMaO9OLSaXNlpJs0aoqKql6IWIxICmSrKKiCsiL4rIfwtfXyEi3xGRwyLyVyKSCNuT4esj4fuX15zj02H7ayLyoZr2W8K2IyJyb037vMcwDGPu/HT7Dw1zx94D3PjA09yx94DFioxLplkj9A8i8m+ADhH5aeCvgf+3yWN/FXi15vUDwOdVdScwCtwVtt8FjKrq24HPh/0QkWuAjwPXArcA/yk0bC7wReBW4BrgjrDvvMcwDCNgtvx0JlowWoGozr2gERGHYAL/ICDAk8Cf6BwHi8hW4GHg94HfAH4GGAE2qWpFRN4P/K6qfkhEngyfPxuutM4AA8C9AKr678JzPgn8bjjE76rqh8L2T4dt9893jNk+x+7du/XgwYNzfkeGsdq5Y++BaaUkcqUKg+kUX9lzQxuvzFiOiMjzqrp7rn7N7hPqAP5UVf+f8ORu2DZXtPIPgf8LiJR064GLqloJXw8BW8LnW4ATAKHxGAv7bwEO1Jyz9pgTU9rft8AxztVetIjsAfYAbN++fY6PaBhrgyjhaC0mWjAulWbdcd8iMDoRHcA3ZztARP4ZMKyqz9c2N+iqc7y3WO1zjT/ZoLpXVXer6u6BgYEGhxjG2mNbfyfnJ4ocHcly6Mw4R0eynJ8ommjBuCSaXQmlVDUbvVDVrIjM9Zf3Y8BtIvJhIAX0EKyM+kQkFq5UtgKnwv5DwDZgKHSV9QIXatojao9p1H5uAWMYxpKwmNkIljqzwft3rOO7xy/gCDgCJc9nOFPijn+yrmVjGqufZldCEyJyXfRCRK4H8rMdoKqfVtWtqno5gbDgaVX9X4G/Bz4adrsT+Hr4fF/4mvD9p8NYzT7g46Gy7QpgJ/Bd4DlgZ6iES4Rj7AuPme8YhtFyFjOw3w6RwLNHLzDQnSDhOvhhyYWB7gTPHrX7OGPhNLsS+jXgr0UkWlFsBn5ugWPeAzwqIr8HvAh8OWz/MvBfROQIwerk4wCq+oqIfBX4IVABPqmqHoCI/DKBSMIliFm9spAxDGMpqM1GANCZiJErVRZUvG0xz9UsJ0ZzbOhOMpBOVdtU1WJCxiXRlBFS1edEZBdwFUFc5ZCqlpsdRFX3A/vD50eB9zboUwA+NsPxv0+gsJva/jjweIP2eY9hrH7akZizlsUM7LdDJLCtv3OaOs42shqXyqxGSEQ+oKpPi8j/POWtnSKCqv5NC6/NMBaNyH0Vd6XOfXUfLMgQRQbt9bPjlD0lEXPYOZie1bAt1iS+/9Aw4/kyp8fypGIuA+kk6VS85Qbh7pt28Jl9r5ArVeiIu+TLntX9MS6ZuWJCPx4+/kyDn3/WwusyjEVlMRNzRgbt2Lks44UK+bLHWK7M8fPZWeMyc2UjmM/YnQkXR4SS53NyNM+5bOGSDcJc2RBm28hqGAtlzs2q4UbVj6rqV5fmkpYXtll1dXDjA0/T1xFHJFDpZwplhscLFD3lvZev4/071vHs0QtNueqiTZtnxgpUPMUJy1PHHGFTb2rWzZu1xdu2LsAlWLthdDxf5ly2SLHi05lwefDjP3pJSrtopVi7yjEjYyyURdusqqp+KABYk0bIWB3UusIyhTKnLhZQlFTM4di5LN89foHBdIL1Xck5XXVRPKbk+bihUZNQsjxXXOZSi7fVxoJ6OuL0dMRRVcby5Us6bzuEDoYBzUu0nxKR3xSRbSKyLvpp6ZUZxiJS6wobHg8MEMCG7iSZQgVHYDxfacpVF2WaTrgOkSNBQ8lyq+MyrcpybSUcjHbRrBH6V8AvAf8AHKz5MYwVQW08o+gpCdfhst4OesIVTbT5MmK2CTgyaOlUDB+l4vv4vtLTEWt5oH4x4kqNsBIORrtodp/QNQRG6EaCNDffBv64VRdlGK0gcoVNTcSZcB1Knk/Cnbwnm20CvnnXIPcRuLAq3jilUB13+frulsu+a8deaFxpKvsPDTM6UeT4+QnijsPGniQx1zHlm7EkNJtF+6sEBe3+Imy6A+hT1Z9t4bUtC0yYsDyZz56fqX3fv2Mdj71wshqEP5ctMpItVWNCtUF5oK17i1pNrSCh4vmczRQpe8qVg93cc8uuVfVZjaWlWWFCs0boJVV991xtqxEzQsuPqUquc9kio7ky6VRs2l6dmVRfH71uC88evVBdTUTquNrVBbDqFWNWnsFoFYtdyuFFEblBVQ+EJ38f8N8v5QKNtc2lZC+oVXKN58ucnwiK/uaKlWnKtplUX88evTBtkv3UlHHu2Hug5Yqx1ZTFYSrt/mztZq1//mZpVpjwPuB/iMhxETkOPAv8uIi8LCLfb9nVGauSS02+WavkOpct4iC4jlD2dZqy7VJUX61SjEWbQq//3N9x958/z/Hz2bZVKm2VIGGtV2Fd659/PjRrhG4BriDIoPDj4fMPE2RN+JnWXJqxWrnU7AW1E2fJ8xGZlEhDvaG4lEm2FRN07eRUKPv4qpzPlskWK5eUxWGhtEptt5gZKlYia/3zz4dmE5i+2eoLMdYO83EBNXJp1OYwi5RtgjCQTgL1hmKufGezuUxakSutdnKKNrsqMJIpkk7F5/we5pOrbq7vMVILLrbaDqwK61r//POh2ZiQYczIfH3fzSbynDHp6G3Xct9t1/LQM0cZy5Wo+Mq6rjjdydi0O/nZJtm5kpq2YoKunZwSrkPFU8SZ3KM02/dQqniMF4Kq9fmSV81VN1cS1mY+52LHKtZ6xu21/vnngxkh45JYSHbqZlcYs6WS+cqeG+oUcLMZipkm2WZS1cx07EKDzrWT04buJKfG8vgVxVf44ekxYo7D7e++rOF1ns9WcJBqrrrxfIVNvbFpQomp1zY6Uaz7nBVPGc4UuPvPn+e67f0tCZiv9Yzba/3zzwczQsYl0cxE3mjCjlYytRLph545ym9//QfVPs26NBZ6Jx+dP1MoM5IpUvJ84o4wlp+9VNallIWonZzSqRhdeZeL+QquQCrmkk7FeOyFk7xra1/1XPPJVdfo2o6fn2BrXwcA4/kyp8byCOCrXnJJi5lolZtvpbDWP/98aJkREpEU8AyQDMd5TFU/G5bofhRYB7wA/G+qWhKRJPAIcD1wHvg5VT0enuvTwF2AB3xKVZ8M228BvkBQWfVPVPX+sH3eYxgLYy5DMZtLLZJIz9QnnYyRL3stc2ls6+/k+Pks57NlRKgq7DKFCvsPDc84YVxKss+pk5OvsLk3yYbuyWqlU88VrZ6q7juZOVddo2uLOw5nM0V6OhJVNSECSddpaaLSVrj5VhJr/fM3S7PquIVQBD4Qbmh9D3CLiNwAPAB8XlV3AqMExoXwcVRV3w58PuyHiFxDUIb7WgKV3n8SEVdEXOCLwK0EaYXuCPsy3zGMhTOXgqwZldBMfVS1JcotCAzf0GiO02PBCqhY8al4wcbt/s74jCqmB7/5OgeOneeNkQleOTXG2bE80HiFNlN9npt3DfKVPTfw7Xs+QE9HnPVdybrjpp5rrlx179+xrjrOC2+NUqnJgQewsSdZ/R5Lno+iqFIVcrQjYD5X7SJj7dCylZAGqRiy4ct4+KPAB4CfD9sfBn4X+BJwe/gc4DHgP0pQ/OV24FFVLQLHROQIk6W7j4SlvBGRR4HbReTV+Y6hzaSNMBoyl++7GZfaTH3G8mU+d/s75nRpzBSfma39tx57idFcvdut4iuD3Qk2dCcbTsoPfvN1vvD0kWrmbF9hOBtslE13xOtWJDOt7j46dLGublF3wp222js/UWSi6HHjA09Pc19GueoAJooevpb54v43WNcVGLNzmSInLxYAoSf8TmOuw5WD3fR1JhgaDVxxm3pTpFPB+0sdMF/sKrfGyqalMaFwtfI88HaCVcsbwEVVrYRdhoAt4fMtwAkAVa2IyBiwPmw/UHPa2mNOTGl/X3jMfMc4d8kfdo0yl++7GZXQbH3mcmlEE1rZC6qbnh7L88Jbo3z4HRt5/q2xhhPdQ88cJVOo4IYBftXg7kgEJkretOuLjNmBY+dRhZgjVPzJ+5bhbAkf+J1/ek21rZFb7Fy2wBf3v8HW/o7qNY3ny0Rn6oi7nJ8oMpwpMdCdqDde1wV/wqlEjA0Jl/MTJXo64pwZK+Crz/lsmWTMZVNviqHRPGczBdKpwJ05li8z0J3kxGiOHRu6GMkWcZ1gpdmOgLnVLjJqaakRUlUPeI+I9AFfA65u1C18lBnem6m9kStxtv6zjVGHiOwB9gBs3769wSFGLbMZimZUQpeiJHromaOUPa8a14m7Dp6v/O1Lp9nUk6S3I4i11E50J0ZzVHyfmOsQcxzKoftKFQqV6fuIorv2aAVUa4Aiav+w9h8a5oW3RvF8n2TMZSCdJJ2KM5Yr44VZHSIxRKHikXRdBrqTjOXLTBQ9BroTDKQnr3uq8ToykqXiKV3J+r1GZ8YKVcNaLCtnxgts6EogBCKGvo44+bKHQFWA0eqAeaPVqO2hMWpZEnWcql4Ukf3ADUCfiMTClcpW4FTYbQjYBgyJSAzoBS7UtEfUHtOo/dwCxph6vXuBvRAkML2Ej77maUYl1KySaKYJbSwXGCAnVI65AmVVxnLluoB/NNFt6+/kXKaIaiBGgElD1JWI1SUorb1rdyRwwUWIABo89nRMxpE+s++V6vVUfOXUxQKX9UHR80nF3GplV5FgVVXyfCZKHp+7/R389td/MG2CrjVeAJ6vOBJscI3ECopS9JRkzCHmBDLuKMbW0xGvW3UA9Hcl+cavtzZB6Uxut0YuSNtDs3ZppTpuACiHBqgD+CkCIcDfAx8lUK/dCXw9PGRf+PrZ8P2nVVVFZB/wlyLyH4DLgJ3AdwluPneGSriTBOKFnw+PmdcYrfoOjIBmVELNut0aKehOj+WJ19QCUgVHgkm/lmiiu/umHdWYkErw63cdoTPucFlvKpCJPzP9rn1DV6IaAwoGCpbRg93JqoGLjNbGdCqQQmvQ8cxYgZjjkE7FGMkUq0bK9yEZk6pYo5FrMjJeEQk3MJolz+ey3g5OjeUpe1p3TRvTKWKucPTcBDsHu+u+h6VadczkdhMRyp5ve2gMoLXquM3A34cJTp8DnlLV/wbcA/xGKDBYD3w57P9lYH3Y/hvAvQCq+grwVeCHwDeAT6qqF65yfhl4EngV+GrYl/mOYSx/HnrmKKWKx5mxAq+dzXBmrECp4qGqxJzABaeq+L7io/SkYsQcp6Gy7uZdg/zBR9/NzsFuRAQRYVNPkkTcpexrw7t2gI29HQx2J6rXJAIb00kGe1JVAxclPe3piHNZbwcxN3CVKfDJm99GIuZSKHuUKj75skfR88mVgs91+Ox4w1xukfGK2NCdxFdwRUinYqzvCq7JdYSYK9WKsVHy1XZVTJ0pAWy2WKlWuR3LlxlMp1ZVeQxjfjRVT2gtY/WElgfXf+7vGC8EGQOifTI+Sm8qxifefzlf3P8Gnh+4o9KpGImYO61m0Gyxj5nq6iRch4mSV1dTaCxfRqA60dfWMypVfDoTbjWmE50nqs/z4Ddf5/PfOszUfzvXgZjj8NC/uB6od01OLcJXKzbIFits7e/kYq5EyfOnXX/cEXJlf0E1kS61FIHVKlq7qCqO4yxqPSHDaCuRu8lxJjMG+L5S8pRP/dSVvGtrX3Xi7kq4iAiPHHiTUiWYgOdiNpn4x67fyp/84zGyxcCV1Bl32NLXgYhwZixPpuhVJdKRug2CFUutq2n/oWH+5B+PEYaSgs8RPvo+rEvHp6Ukiqj9fFv7O/mdf3rNtFQ9jcQdkWJvvjv3F0NGvZZS16z12kGlik+x4lGsBHvu/AbinZkwI2SsCBIxh3zJw9fJjAFo0A6TMaWpkm0E8mU4dq5xss9o8hjJFDmXKU7bP9OVcHnshZN0JV3yJQ8EChWfi/kyFV/JlX0qvs94vkIy5laFEBNFr059BoFgYaJUIe4KxUrwT6oE8StHhPVdyWkpeJqd2OYSd8x3QlwMGfVaSV2z1vY9eb4GBqfsh0bHw/OV0VyJ185kOHQmw+tnM02fz4yQ0RIW+85w52Ca4+ezjOeDXf8J16GnK87l6+uD7nXJPh0Jg/9BKp4o2WfU7/BwhkyhQn9nnE09SU5eLDA0mmdLnxJznaBkgus0PN9oqFir+EoyVq+CW9+VJOYAxirhAAAgAElEQVSU+fY9H6heV1SlNRVzqfiKI0HSUieUlcccqYvVLGRiW8w0MYslo14LqWtW874nVa2ubiLDU/Z8soUKr53N8NqZTPVxOFNc0BhmhIxF51LuDGcyXpFrp6cjkCwXKh6VCeWOf7Ku7vi6ZJ81rrso2efhs+PVa8sVK0FRuYkSl/V2sKWvg7OZAmfGi1yxvpOE6/D6cJakKxQ9rbr1RKAYuvkEQMPM1igjmWAj6NTA/+HhDLlihZKnVYk1BJJv31d6uuJ1KXheeGu0mtkgSmW0lBOblSJontW076kcprAqlj0KFT8Q0JQqHBnOcujMpNEZGs03PN51hB0burhmcw8PNjmmGSFj0VnonWEj4/Vbj73E+q4E2ZIHvs+5bAnVION03BW+uP8NHjnwZrXAW12yT396ss+Sp/S6gucr+bJfjc28eSGIJW1MJ5koVqrB/FgY2IfAWMRdp25jasINjA8+IEqh4k+Le+w/NEymUKHi+9XsDJGi2nWE3s5gRVcrQPBVEaiurmYreDf1O1zICnTqcdG1rIV4zqWyUg2272t1hVMoR48ex85NBMbmTIZDZzMcPzdBoxCPANvXdXLVpjRXbUqza1Oatw10k4g5JGKOGSFjcZnP5LbQEglTjZcXur0yxQpvH+jmyEgWVdja34EqnAqTh+aKFY6dy3LXw89VJ3cI/kli4eolnYozni8HxeDCf6qp/1fFis/JiwXirtDbmaDiaXUTKwT9gxUWxJxgFRNdy7lskWJFq5tdIXDBnRjNMZ4vE3egWGEav/qBt/Opn7qy2j/6/NEmVGSy6upcE9tCV6CNjnvshZPzUhfOdu7VHrBfCQIMVaXk+VVjUyz7FMoeb13I1bnU3hjJTu45m8Lm3hS7QoNz1cY0Ozd205kItkIkYg7JmEMy7pBwHWJu87t/zAgZczLfyW2hJRKmujVGMkUcCYxRsMExcGW9eT5X3ewZc4VCxaeQKU67Wwv3iaIELjdVrW4QbYTnK26YF64j7nJsbIK44+Cr4ulkDEeAtw10c36ihOsE0ueYK1XpM1D3fZ0ZKzRM9QPwpX94g68+P8S2/k4OD2foSrgcHcmSL3vVz1MEDp/NkE7F6vLTTaXRCnQkU+BTj75IT0d8RiMw08r12aMXLklKvVYC9stRgFHxfAqhW61YCQzOqYv5qnDgtbMZDp/NTttDFrG+K1G3wrlyY5rejjgxZ9LQLMTgNMKMkDEnD3zjEMPjBTwNAvUD6WR1h3+jf7S7b9rB3X/+PIriIGg46UclEmb655zq1ih5frABVeGHp8fxaiZyX4OibAoNXQURFVV2bOhi6EKOog8NUgXWsa4zzki2xKEzmarhiCTVTmhQHRHuvTVIg9ho4qld0cD0hKe15Ms+Z8cLnMsWKVV8RieCrNfulBRByFxXPt2Ij+fLnJ8o4auyfV3njEagVTGN1Rywn0ptmqcTYeaM2vZWUutWC+I5PqfH8rx+NlON47x+NlMtDT+VnlSMKzdOGpyrNqXZ0J0k7k6ucIJHtxpnXUzMCBmzsv/QMIfOBHJLBSq+x4kLObb2d8w4Sd28a5DupEuh7FeVbBu6U6RTsVkntqluDQHKPsTC1VBEZBB8rW9vhCocP5+bs1/cFVwRLuTKxMOYUe14EIznaLAqm1qYr5apk/pcG8JdCQx1NGRMqXMrpmIOOwfTc1asnZqT7Vy2GB7vzipuaFVMYzUF7OdiKVd9tcamUPY4ny3yWo3Bee1MhvMTpYbHpuJOYHA2Thqczb0pEjG3ztgkYk5LDE4jzAgZs3L/E6/W3YGrBuVtT13Ms/vy9TMed+XGnoa75Web2Ka6NWKuQ8XzcF2HSsWvrkhkyv/G1MSiU2lm41zZU8QN+roxB6+B4RDA82FrfwrXmVwJzmUMZvD+VSlW6nuUfR8/CAcFBjG8lrkq1k4tC1EMv7OoeN3Uc0S0KqaxUgP2C6FVq75KpFYLXWoXc6VJaXToWjs9Vmh4bNwVdgx0sytc5Vy1Kc32dZ2k4oHBiYxNMuZUN4G3AzNCxqwcO58j5gSTb4QSrFAaTVLRhPzKqTEyxQqi0JFwq6l05prYajed3v3nzyOOTIoDBOISBGV2berhlZNjjYtzNKA2S0EjAkOmiMO0yqS1uI6QTsVRVQ6fHeeWz/8Dh0eyxB2HnlSM59+8UC06l3CFzb2pasXWmZj6btxxqhJzESEeThAzVayFyezYCdcJi9fl6Ey4dCXd6ubbqeeIaFVMYyUE7BeLxVj1BRlAwhVOxWM8X+b1mr04h85keOt8ruHfsSNw+YYudm1Mc2XoVrtiQxediRjJuEPSdasxnGYMzlIKSswIGXPiiOC4UpUYQ1CPplGphShbQb7kVVcouZJHxVc+efP2puXCn9n3CkJQlkEkWJmoKuVwVfPyyTEAuuIOOwbSvH5mnGKDyb7Z+7vLelOcHi/O6bZLhhkazmWLZIoe2VIOVwLhRV2GbaDkKW9eaLyfovb6phe0CiaVKI60qSc5rbT5bGmGnvi1m4CZU/k0MgKt2FQ6H+O20lV0C1n1lWrk0ROlMofPZutWOMfOTcwYS9za38FVNXGctw9209MRr7rTkuEKR6a6DZpgqQUlZoSMWdmxoYvDw1lcJ0iRoxqsFOIxp1p++v071vHs0QvVDZYQ5HiLiYMvSswVNvWmePzl03WlraPJcOrkE93lb+pNcepiIQjK+8pUGyPARMnnlZNjs65yIuXabAxdLDDXDaICG7oT5EoVzk+UEKim31koU48Wgg2D0dyzuTeFrzCYTs27Yu1yUG01Y9xWg4purlWf5yuFUKmWL1c4OjzBq2fGgxQ3ZzIcGc5SqDRegQ+mk1VZdBTH6e9K1LnTFmpwGrHUghIzQsas3HPLLn7rsZeCzZaejxNW8exJxejriHP8fJbvHr/AQHeiusGy5CmJYAlTzVZQ8XyOn89zeU2phN987CVKZY+SH0ivz2WL/OZjL6GqbO4NEoT2d3qcnSEdSFUwMMv1K8GqLbqu2XAdwZ+lT9IVfA3O5/kaFI+bIx7ViLoEplEevJrrTbhOmIU4cMdFsu+Hnjka1Dqq2Uw6kimQKVQoVgL33e3vvqxurJWQNmc1qOhqDf6JCxNc1tfJne//Ea7enObg8Qv84NRYdZXz2tkME8XG0ui+jnjV4Fy1Kc2uzT1s6k3VSaIX0+A0YqkFJVbKYQ6slMOkq2RoNMdYvlxXquDoSLaqgAOoeIFfGwlUWb4frIS8MFXAzo3p6nkPnR6j7EPSDfbilGtm87gD/Z0JLuYrlGvcgAvFdQT1dU6RwGwMdCf4g4++m4eeOcqLJ0ZDRVv9dS/o2qReDZd0HXy0Wo+oUTmJsqdcv72Xx39wlorvk3QdejuD1D8D3UlGQsl33BWu3NizrN1gNz7wNH0d8bqJVVUZy9fn31uu1GaQPjNW4PtDY7x2ZrwqIBjNNd6k3ZVwubJuhdPDtnUdgTstHkqj3dYanEYsVgkOEbFSDsbiUHs3HU0YESXPxwlXO1GVT9eBig+VcFdoOhXsvdnal6o7b+R9CIQO9RN52YfhbAl3ykphoXi+Nh0faoQjkClUeOAbh8gUK2xMJzk1VghLil/aBXoaKJkil2HMFTZ0p+jpCAUQw1m29ndMWyl869BIXft4vsxIJh+o5JSmMogvBzfYSlLR1WaQPpct8vLJMV49PV6N48yUxDMRc3j7QHfVnbZrcw87BrroiE9Ko9thcBqx1IISM0LGvJg6YSRcZzKrdUecfKnCuXCPgq/QGXe4YkM3/Z1B0bVaoqm7MlMKg/Acc7m8on/bmbrEw4wNCzUVQjCJ+L7y+nCWKwe7q0b3XLZIRWVWQcPUlU4jEq6DK4Eue8fAZGbwaEd7owqlEyWP7TXt57JBhomypyRiTsMM4rXGZbm4wZariq42g/RYrsQPTo3zw1Nj1f04J2ZJ4nnFhq5qpoGrNwerna5krG4fznJlqWOJLTNCIrINeATYROC236uqXxCRdcBfAZcDx4GfVdVRCW4BvgB8GMgB/1JVXwjPdSfw2+Gpf09VHw7brwf+DOgAHgd+VVV1IWMYzTF1wujpiDGcKZFOxRjPl7iQK+OIsK0/VS2HUFtPp3aiibtCxdNZDYwjsLW/k5MX8zMqhcKb/oYk3GDTnadeUwYtonZ9EyQtDZKKBiuTDK7jsK4rzhUbusiXPc5li0wUK4BUq7tWQgNQrPjTYj9TyZU8NnQnUJg2GV+xvrNu3xFM1jqqbS95fvV7iG6oazOIT/XpL5fNpMtBQAGBIKRQ9pgoVvjh6QwvnxzjtdOBW+1Yk0k8r97cw9Wb0nVKteVscGZiKWOJrVwJVYD/U1VfEJE08LyIPAX8S+Bbqnq/iNwL3AvcA9wK7Ax/3gd8CXhfaFA+C+wmmBeeF5F9qjoa9tkDHCAwQrcAT4TnbHqMFn4Hy5aFxgKmThiXr+/mjn8yqY6LucLGdOBKAhjJFPilv3wBz1c83yfhunQmXXYOphnsTvDssdFZx/MJ3FNb+lKcHitQ8pS4I7xtoIt7b72aX/rLF8iVgtVCVBwuMlaORNkKwjpAqugU11cttYYnenRDuXTZ86n4SlSktTPhcHqsyOmxwP2ScISfeddmzoyX6ibS+594lSMjE0CQQmgmAuVdkl2buvnWoREmSoGR+dc3XsG7tvY1XCn85K4BHv/B2WpZc1TxQyOoyrQM4lPdW8vJDbbUAorIrZYrehwezvL9oYuBW+1soFSbSU25qSc1aXA2pXnn1l76OxMk4y6JMM2NMT9aZoRU9TRwOnyeEZFXgS3A7cDNYbeHgf0EBuJ24BENZo0DItInIpvDvk+p6gWA0JDdIiL7gR5VfTZsfwT4CIERmtcY4bWuGWpjAa7Ai2+Nctcjz7FzoJt7b716zozLjYzXpwjiRa4EbqFTY3kcEUoVP1R8BYag5Pl04vL+Hev4o78/Mue1Jt1Annz47Dgx18HzPSqqHBnJ8sA3DuFIUOfHCVcqtbGlyfxyHqqRUQJvhgmmUaun4IXBK4cgr5sAY7n6PFwlX/na906zrb+j7jzZkkdnXBgvzi2JODte4PXhLAPdCbavC1Y/j71wkndt7eO+266tWylE6rh1XfFqfSVHhESYpy5SAroO9KeSDd1bS+0Ga5cIInKrFcoex85P8P0TY7wSqtVenyWJZzoV451berlqY5qrL0vzzi29bOzpqMqi45eYuNMIWJKYkIhcDvwo8B1gYzTpq+ppEYn+CrcAJ2oOGwrbZmsfatDOAsZYU0YoigV4vnJ6rBhkuhbh+IXcrIHpyHiVKh6ZQoUzYwVeeGuUD79jI4fOZDk5mp90YTmBwamuKJzgH1Z8JVus8KV/eGPOvTsAjgRZFn7pL56v1vUBqCi8diaD6wpdCZeyp0yUGk/0tdsvNqaTZIsVKp5SnCUzQiN8qEqzZzryxGieZMzh7Fie7xw7Py/59miuXBVADKRTdTGar+y5oe53EiVJ7e1IVUuKn8sWOJct1bkmPR8uTATu0qlJNZfSDXapIoj5GLCoMNvJ0RzfO3GRH5wc59CZQDwwUxLPVNzB12AzcncyFu4ZEz5xw4/wk9dsNIPTQlpuhESkG/ivwK+p6vgs6o9Gb8zk7p+tfdbLaeYYEdlD4OZj+/btc5xy5RHFAo6dm6iWRFCCCbZRduxoAnjhrVF838dHiElQ8K3s+Xzte9NteO2KJPrSPT+oz6PeZPtcv7BCxeNXvvJCnQGK8AH1AqP2I+u7eCN0e81GtOcoFXOaGn8qlbCk92xMzQXXLEqw8sqVgpQtPR0zF7JrFM8ZC0uOu47Uyd5LXlDJ9cUTo9z1yEGuHOzmnlt2VV1gS7EauRQRxGwG7KYrByhWfIYzBb534mKdWu1cduYknjsH01y1qZtrNvfyzi293P/Eq1zIlehKxKoKtVypwsPPvskt79y8qN+FUU9LjZCIxAkM0F+o6t+EzWcjF1jobhsO24eAbTWHbwVOhe03T2nfH7ZvbdB/IWPUoap7gb0Q7BNq+gOvEKJYQG0J7Ch2MHXSq50APN/H84P6PK4bxVnmHi8ycOWalVHU3gyZGTb2ReeINpDOh5l2py8HFBi6mGcrQTysUYwmnYxxZDhbV14jWtk5BOXGyzVJX89ly6ECD46dm1hyKfZsIoi5VjlRKZGKH6gw13UlKPs+//ffvsyVG9O8djYTZNZoQG0Sz6s39/Curb1ctSlNR8KtK01wNlOctldptWb8Xm60Uh0nwJeBV1X1P9S8tQ+4E7g/fPx6Tfsvi8ijBGKBsdCIPAn8WxHpD/t9EPi0ql4QkYyI3EDg5vsE8EcLGWOxP/tyJ4oFuKGEVwAfpTMRq1YvvWPvgboUOp2JGMlYIAuGQFbtOm7ThmSqPLtZmjFyMUfo70oC2QWNsRzxfOXkxTyX9XVMi9HsPzTMSLZYTe9T9jwmzudwHUAnlXFTMzE4TuMV71LEamYSQajvc/efP18VV1Q8v2ogb9y5gcdfPs2hM5nqZ8qVfXI1BudkzXNH4EfWd3HVxjTXXBYYnGs299Cdis1ZC2c5iTTWGi3LmCAiNwLfBl5mMrPKvyEwGF8FtgNvAR8LDYoA/5FA4ZYDfkFVD4bn+lfhsQC/r6r/OWzfzaRE+wngV0KJ9vr5jjETqzVjwv5DwzzwjUO8Ppwl7grppMtoGGzf0jcpr86VKmzqSSEiZApl3gyz+EZ7ZxbqelpsLn3L6PLkN35qZ7X8d8Qdew9w7FyW4UyRMBEFEv64oXzPdYRiZTLThCOQjLn4GsS0rtjQxVi+zOduf0d1pVsrTrjvtmsX1RDVrqijccby5WrJd4cgi7RPkHU9FXNZ153g8NnsjL/XRMzhxrdv4JpwhfPOLb30dSYWVAun0fW14ntYSzSbMcHS9szBajVCEbXxHgE29aaqqf9zpQojmSKdCZdMoULJ8/H82ff1tIKYBEKE1cR8cs51Jx3euaW/ukK58YGnGZ0oUfG0mpZfCdydm9NJRibKVHyfWChX9zSQmsfcQLp9WV9QD2kwTL20GClamqE2/dPm3hTD40XevJCrfhfNBHQj2+Ir/PG/uI6fvmbTotXCqb2+5VCie6VjRmiRWO1GKGKm/F1HhseZQVBkLICFrtj6OmLEXIeB7iRHz01QqvjEHIi5QcaEshdsxhURdg50ISJkixW6kzEy+RKnM0XijsPGnmR1lXvfbdfy21//QcPf+5nxAjsH0/Ny0c3k1itXPN68kOPFty7y/ZNj/PBUIBzIFmf+w0qnYvwv123l24dHqsX5zmeLlEPhxeXrOvnGr//4Ar5JY6mw3HHGvOhOuBwZyQbqKhFUA1XVcrhFEYI4x1KvwFrBQj/CxXwlnIhLwX4ngvx64IVl14M9QUk3SFFU9nw+d/s7ppUCHxrN1ZWF2PbM9FjI+YkimUKF4UyhaTl15M6KOcFG3jdGsvzKoy+yta+DM+OFGZN41mZ4iPZxOQIP/tx7+ImrN9a5yXYMdFfdZPfeevUCv0ljuWFGaI0RxYKOngvkzFes7+TD79zM+dC9A0qhif07S4kSTE6rNe7TLLXZHKLvouwHk7brgCsOgz2T+4se+MahOQUHjTasXpgo098Zn1VOHRm14+ez9KTinM8WyZV9ShW/TsL+6plM9XnMEa7cmOaay9K8c0sfnufzp//9GBXfZzxfoej5xFyHT978NkSEO/Ye4MRojnQyVs2qbW6y1YcZoTXE/kPD/NZjL1U3RQK8fjbLa2cP40iQI22umjvtZPle2dISzfGhGA4RIekKgz2T8bygflOOy9d3hnvCstz958+TTsXYOZiuKyg4USxXk57uHExzMVdiQ3eybsyOuMtb57P8jyPn+K8vDPH4y6cpe8GeqSh10Vz0plx+64NX8hNXb6y27RjonhaHAer2BUWrn9qVnbF6sJjQHKz0mFCtn348X6ZQ9hAJ9vhUPP+Sa+EY7SNSKF63vZ/hTIGKFxQGjAQkrsCuzb2M58ucGgsyPscdYXNfB2P5MkKwD2ksVw5WIU6wCnn85dMcPZfF8wOVXRBD8ufMcOEI9KTiXMw3dr25Drz38vVzCh4Wq56N0V4sJmRM22l+ZqxAxVcSLiBhoTljxaIEhfYu5koNszyrBjWGzmWLOAjiBJksOhMxTl7M4/uTjr0oSevnv3m4Lv7meUrJq98s7Ap0JGJ0Jlw64i4dCZd8qcKJ0fysYgPPp6nNn8slu7exNJgRWsVMTZWSjDlUSl7oRvGmTVprPeayEhnPl0nG3WCfzZT3FDgzlseHUGwSrISKZY9SJdjoKkwXn0y9N3GAvq44v/eRd3Dd9n5+/a9emrZSKYdlpwORxMx/Rc1s/rSNo2sLy8q3ijkxmqsrhrahO1lVVjWaJ8wArTzGix5nxgrVonnRXpqo3HrRCzJiFCuBaKBQ8Xl9OFv9/Tf6nQuwtb+DKzd2847Lerjmsh66EjE+/M7L2NQbZHCINjKrBo9lT+lKxnj7QDczbdsRaCpD90znb3eRO6M1mBFaxWzr76xLU9/TEceVyd31xuqgUlM1NrrBqK1WG0ntZ7r5qCVaDZ8dL3AydK9NXYXcvGuQ+267lsF0irF8mcF0ivtuu5adg2nyZY+BKaKGiI+8Z3PzNasanN9ECasTc8etYhrJb31g+7oOejoSHB3Jkit5tgJa4ZQb5OWrNTauBHceGhb1i95q5H6NXvu+UsZnaDRPf2ec3/mn19T1myn79mf2vUJ3KpBUj2RLVTl5OhXjzHiJ/YeGmzZEZnTWBrYSWsU0uqPcnE5yNlPk5ZNjTJgBWhXMtLpxBG7auZ7ORCDm7oi7JMOCbNVNolOOEYL9PNHKKeYK67sSM9aXumPvAW584Gnu2HsAoPr3loy77NqUZkN3gh0DXWxf11nd9Lr/0PC0cxlrF1sJrUJmSp8S7RMqNKjNYywPYuHmn0vNlZd0ha5kjH88MllYL1f2wpIdUrcaih6jtpgjeAq7NvVUN4lOZcYaP7ddW5VR37H3QFBJdwE1hIy1gxmhFcpshqZR9dNP3vw2nj16gZhr0aDlzGIlJfcUMsVK/SopfF4OxQpMfyt4rpPChplUac0UqTOptdEMZoRWILV3ocVyhQNHz/Ps0fOhKwW6UzGyRQ8HCe9qlS/uf4PupEtmho2ExuqikUxapzw2IpBY+2zq7ZhVldaMgTGptdEMZoRWINFd6Hi+zEh20qhEucRGcxVcCVJbaljkzEfJl4SSeeKWPfMp89Ds+VyRuuwYSVco1mRAcCRI/+NKkCXBV+oSnU6lGQPTSBiz3KTWS1HQz5gdM0IrkOgu9FiYhLQRtRlWtCYmYCx/Fj2TkoIbEyqhOi4Vc9i5Mc2hM+MIQc7AHQPdQdcwBvTtez4w6ymbMTA37xrkPli2NXpmjGuxdGXPjRaq40TkT0VkWER+UNO2TkSeEpHD4WN/2C4i8qCIHBGR74vIdTXH3Bn2Pywid9a0Xy8iL4fHPBhWTV3QGCuNaP+PpX0zZiPaD+YDpXJQZdUV6O2Ic3QkS8ULynUUagJRzbrLmt3Lc/OuQb6y5wa+fc8H+MqeG5bV5F4b1xIJHqOy58bS0cqV0J8RlNJ+pKbtXuBbqnq/iNwbvr4HuBXYGf68D/gS8D4RWQd8FthN4G16XkT2qepo2GcPcAB4nKBk9xPzHaNln74FRK6Dw8MZMlZpzpiDhCv0dsQZL1RQgrRNMUcYzZURgbgrlDzF95XxfKla7K5Zd9lK38tjwonlQctWQqr6DHBhSvPtwMPh84eBj9S0P6IBB4A+EdkMfAh4SlUvhIbnKeCW8L0eVX1WgzTgj0w513zGWBFEroPhTIFNPSnWdcXnPshYM0z9Rw5eC6P5Mht7kgymkzz48R8N94ZpuEqSIEu2A2fGi2suM8HUjCJgwol2sNQxoY2qehpAVU+LSPTXvgU4UdNvKGybrX2oQftCxjh9qR9qKYhcBxVPOXwhs6zr/hhLTyruUvZ8PNWwNlSYM9CHUxfzdCRi/OZjL1EMXW8VgvpBW3s7SKdijOXLM5ZJ2H9omPufeJVj54MVwo4NXdxzy64Vb6xWgnBiLbBchAmNNq/oAtoXMsb0jiJ7CFx9bN++fY7TLg0nRnO4AidG8xYLMqrEHGFDd4IrNnRX/0ZOjxXxVREJ5NaeQtoJ6gZFuBLIuM+M5Tl5MXDN3fqHz5ApVqbtO/vNx17iYk0hxMPDWX7rsZf4g4++e0UbouUunFgrLLUROisim8MVymYgyt8xBGyr6bcVOBW23zylfX/YvrVB/4WMMQ1V3QvshaCo3Xw+4GLQSDaaTsZ47WzGDJBRRYB1nXHG82W+e/wCXQmXrqTLZX0pRjJBcTsliA0VKz6OIyREgqJ3Gijh/PA8JVUOD2fZ0peqU4k99MxRssUKrghOaIVElUxhdWQ+WOlxrdXAUueO2wdECrc7ga/XtH8iVLDdAIyFLrUngQ+KSH+ocvsg8GT4XkZEbghVcZ+Ycq75jLFs2H9omFv/8BnueuQgB4+fZyRT5LnjF7jr4ed49YwZoLVObYqdmCMkYw4X8xXKvpKKOXQmXIYzJcbzZVQVz1d8JSyL4KGquI4Qd6TOBZCIOcQcB9cRzmVLdSqxE6M5PD9YVVWvI1xhWQDfWAxathISka8QrGI2iMgQgcrtfuCrInIX8BbwsbD748CHgSNADvgFAFW9ICKfA54L+92nqpHY4RcJFHgdBKq4J8L2eY3RTmpXPOlkjJFskWyhAqqUFfBtZ6kRIMDGniTJmMPJiwUgzIodvr+hO0lPR5xC2WM0LN0d7Q+L/NclT0mgOCIIStwV4q5DyfOrmbZLYUbuSCW2rb+Tc9ki6lM1RKoQcxwL4BuLgqiVeJ6V3bt368GDBxf9vLUb5TriLkeGs1R8xQ9/H7bqMSI64w4/sr6Lvs4EQ6M5uhIuIsLrw1lSMadqgACOjmQpVDxSMZeKF0wTtxoAAA3MSURBVPw9laf8McVdAQ00clv6OjiXLVLxFCRYYe0Y6CZXqlQzJkyNCfkK/Z3xFR8TMlqLiDyvqrvn6rdchAlrjqkJICNVk69mgIxJkq5D2VdGMgWe+LWb6t67Y++BaalzihWfZM3qJuY4gEfFB8cRfFV6UzEG0ilGskVibiBsOHmxAAqbepJ1OeNu3jXIv//ou+vUcTsHVoc6zlgemBFqE1M3yiVch7LnIyJIjZvFWNs4juD72lCS30hi7DpCb2ec8XyFihfEchzHoTMmbOpNMZhOVaXYkTt4aDTH2we6EBGyxcq0nHEWvDdaiRW1axNTN8oNpJP4YVoVM0Crl5mKyc2EH5ZCTcSm/6s2Sp3zyZvfRtx1Sadi+CgV38f3lZ6OWMPcbl/ZcwOfu/0d9HclyRQrJlM2lhxbCbWJqXexriMkYg65kiUZXc3EHECkKhSYi1LFJ+4KA93JattcmZ/ftbWPh545SsUbD8QIMYfL13c3NC6WxNNoN2aE2sTUjXKAGaA1gONECXUg7gby6ZmoLaswki1Wy2LPZTTm4z5rpjidYbQSM0JLzNS72E09Cb53YpS8ldxeE1yxvpN7b72az+x7hZMXc8SdoAbUVOLhiinuBuq3kufxqUdfpFjxEWBTb6qa+flSjIYl8TTajRmhJWSq6+PloVGetSpzawoRqa6CP/Xoi+RKHl2JSZl1rlRhaDTPzsFuwuokZAplzmVKwX6fYDsPpy4WuKwP0qn4JRkNq35qtBsTJiwhta6P4fECWTNAa46RbBEIXGYPfvxHuayvg029KdKpWFUavWNDV51oZSRTBAlKMSRcB0EQCdu5NKNx9007wowKFVR11pLehtEKzAgtISdGc3TEXYbHCwxnS+2+HKMNlGoKyM1UGO6eW3bVGYZCJTBIG7qTbOhO4qOoKsWKd8lGo9nidIbRKswdt4Rs6+/k+PksZ8M7WGN1IjSW2btOkEy0lplEBLWila5EjM6EW82KAHA2U0BUpu3pWQi2D8hoJ2aEloD9h4Z54BuHODycoWIeuFVLf2eMi7lKQwOUjDn0d8a5YkN3U+eqNQxRLDGS88dcsRWLsWowI9Rifv3RF/jb7522DairgLgrDSXVAmzt70BESMZcMoUKhbKHp9DXEWNruDF5oW4zq3tjrGbMCLWQB7/5Ol/73rKqFmHMk7gDPrClr5P7bruW7w9d5I/+/kjVGCVcobcjzudufwdQbyjev2Mdzx69sCiGw1xmxmrFjFAL+eNnjrb7EoxLIBlz8HylK+FWXV837xqsZiRoZFymGopPtePCDWMFYUaoBUQbUi0DwvLFFZgta05v0mVDT4qyp9NiL7YqMYzFw4zQIlO7IdVYnjgC8ZhDpxOUrM4WK3g1gpENXXE6ErFFUZ4ZhjE7ZoQWkYlihT948jUmihUqVhRoyYk5UPEDoYAIpOIuHTGpbgoeTCfpSrhMlLyqGw0s4G8Y7WTNGSERuQX4AuACf6Kq9y/kPMWKx6unM7w8dJGXhsZ46cRF3hjJWkG6FtMZd/nQtYOcGS8xNJqjOxkjUygzEm7+fftAkJttPobEjI5htI81ZYRExAW+CPw0MAQ8JyL7VPWHsx1X8XyOjGT5/okxXhq6yEsnLnLobCYoidyAuCN0JoMNhqrKmXHbnFpLTyrGv77xilkD/IZhrA3WlBEC3gscUdWjACLyKHA7MKMRemMkyzt/9+/qcnnVsqknxbu39fKurX28e2sfY7kSDzz5GnFXaqpdFlnXGWckW27FZ1pWCNCRcOlMuOwcTM9pWMzoGMbaZq0ZoS3AiZrXQ8D7ZjsgV/KqBqi/Mx4am8DovGtrL4M9qWnHdCVjdXf4cUco+0qx7DNeXHmKuX/+ns18/uPXtfsyDMNYhaw1I9RIsjbNpyYie4A9AP1bruCLP38d79raW90VPxdTJbyRYm6gJwXjecaLS5u7J+kKybgLBJsrd27sMdeXYRj/f3v3HuRlVcdx/P3hsqvgZSHLQEjBmBoyrpsDpImXUbxFf1hQTJGN/9hUWoMNDv3jX47VNOlUGHkpw5Qkpxi8ECF/aBcUBETitoEJQoGjrJqjSH7745xlf63L4u5vl8Py+7xmfvM7z3nOPvucL2f3y/P8nj3nmFBrSWgXMLxiexiwu22jiFgALABobGyMK8cMqeqbVk67cuDguwxt6Msbbx9k9/63aElH9f36cFJdH5rfOpj+fiXanwSzxYC6vowd1uBkYma9Wq0loWeAUZJGAC8BM4EvHY1v7D9wNDN7r5pKQhFxUNI3gGWkR7TviYiNhU/LzKxm1VQSAoiIR4FHS5+HmZl5ZVUzMyvIScjMzIpxEjIzs2KchMzMrBgnITMzK0YRnva5I5L2Af9sZ9dpwMtH+XSORY5DK8eilWPRqlZjcWZEfPBIjZyEukjS6ohoLH0epTkOrRyLVo5FK8eiY74dZ2ZmxTgJmZlZMU5CXbeg9AkcIxyHVo5FK8eilWPRAX8mZGZmxfhKyMzMinES6iRJ0yRtkdQkaW7p8+kJkoZLWilpk6SNkm7I9YMlLZe0Lb8PyvWSdEeOyXOSJlQca3Zuv03S7FJ9qoakvpLWSlqat0dIWpX7tEhSXa6vz9tNef9ZFce4OddvkXRZmZ5UR1KDpMWSNuexMbmGx8S388/G85IekHRCrY6LqkWEX+/zRVr+4R/ASKAOWA+MLn1ePdDPIcCEXD4Z2AqMBr4PzM31c4HbcvkK4DHSyrWTgFW5fjCwPb8PyuVBpfvXhXh8B/gNsDRv/xaYmct3Atfn8teBO3N5JrAol0fnsVIPjMhjqG/pfnUhDr8CrsvlOqChFscEcAawAzixYjx8tVbHRbUvXwl1zrlAU0Rsj4gDwIPA9MLn1O0iYk9EPJvLrwObSD9400m/iMjvn8vl6cB9kfwNaJA0BLgMWB4Rr0TEq8ByYNpR7ErVJA0DrgTuytsCLgIW5yZt49ASn8XAxbn9dODBiHg7InYATaSx1GtIOgX4DHA3QEQciIj91OCYyPoBJ0rqBwwA9lCD46I7OAl1zhnAzortXbnuuJVvHYwHVgGnR8QeSIkKaFkq9nBxOR7i9WPgu3BoJfYPAPsj4mDeruzTof7m/c25/fEQh5HAPuDefGvyLkkDqcExEREvAT8EXiQln2ZgDbU5LqrmJNQ5aqfuuH28UNJJwO+AGyPitY6atlMXHdT3CpKuAvZGxJrK6naaxhH29eo4ZP2ACcD8iBgP/Id0++1wjttY5M+9ppNuoQ0FBgKXt9O0FsZF1ZyEOmcXMLxiexiwu9C59ChJ/UkJ6P6IeDhX/zvfUiG/7831h4tLb4/Xp4HPSnqBdOv1ItKVUUO+DQP/36dD/c37TwVeoffHAVIfdkXEqry9mJSUam1MAFwC7IiIfRHxDvAwMIXaHBdVcxLqnGeAUfkpmDrSh4xLCp9Tt8v3q+8GNkXEjyp2LQFanmaaDfyhov4r+YmoSUBzvjWzDLhU0qD8v8dLc12vEBE3R8SwiDiL9G/9RETMAlYC1+RmbePQEp9rcvvI9TPzU1IjgFHA00epG90iIv4F7JT0sVx1MfB3amxMZC8CkyQNyD8rLbGouXHRLUo/GdHbXqSnfraSnmSZV/p8eqiP55FuCzwHrMuvK0j3sVcA2/L74NxewE9zTDYAjRXH+hrpA9cm4NrSfasiJlNpfTpuJOmXRRPwEFCf60/I2015/8iKr5+X47MFuLx0f7oYg3HA6jwufk96uq0mxwRwC7AZeB74NekJt5ocF9W+PGOCmZkV49txZmZWjJOQmZkV4yRkZmbFOAmZmVkxTkJmZlaMk5CZmRXjJGRWUP5DxT9JWidphqQbJQ3o4rFekHRad5+jWU/qd+QmZtaDxgP9I2IcpEQCLATeLHlSZkeLr4TMupmkgZIekbQ+L3o2Q2kxxM2SnsqLvS2V9CFSwhmXr4RuIE2IuVLSyg6OP1/S6ryo2i1tdt8k6en8+mhuf6akFXlxuRWSPiLp1Hzl1Ce3GSBpp6T+ks6W9LikNZKelPTxHgqVmZOQWQ+YBuyOiLERcQ7wOPAL4GrgfODDABGxF7gOeDIixkXE7aQJLC+MiAs7OP68iGgExgAXSBpTse+1iDgX+AlpslVy+b6IGAPcD9wREc2kBdUuyG2uBpZFmpBzAfDNiJgIzAF+Vk0wzDriJGTW/TYAl0i6TdL5pCn/d0TEtkjzZC2s8vhfkPQssBb4BGmFzhYPVLxPzuXJpJVhIc1zdl4uLwJm5PJMYFFevmMK8JCkdcDPSSvtmvUIfyZk1s0iYqukiaRJX28F/kg3rROTZ1ueA3wqIl6V9EvSBJmHvv1hyrRTvwS4VdJgYCLwBGltnP0tn1GZ9TRfCZl1M0lDgTcjYiFpBc4pwAhJZ+cmX+zgy18HTu5g/ymkBeWaJZ3OexdTm1Hx/tdc/gvpSgdgFvAUQES8QZrV+XbSDOH/jbR44Q5Jn899kaSxHfXXrBq+EjLrfp8EfiDpXeAd4HrgNOARSS+TksA5h/naBcBjkva097lQRKyXtBbYCGwH/tymSb2kVaT/YLYku28B90i6ibRE97UV7ReRlhmYWlE3C5gv6XtAf9KCfuvfT8fNOstLOZgdZZKmAnMi4qrS52JWmm/HmZlZMb4SMjtG5dtq9W2qvxwRG0qcj1lPcBIyM7NifDvOzMyKcRIyM7NinITMzKwYJyEzMyvGScjMzIr5H7b0rMaWsdqHAAAAAElFTkSuQmCC\n", 
                        "text/plain": "<matplotlib.figure.Figure at 0x7f24f8c5ca58>"
                    }, 
                    "metadata": {}
                }
            ], 
            "source": "sns.regplot(x='sqft_above', y='price', data=df)"
        }, 
        {
            "source": "\nWe can use the Pandas method <code>corr()</code>  to find the feature other than price that is most correlated with price.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 16, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 16, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "zipcode         -0.053203\nlong             0.021626\ncondition        0.036362\nyr_built         0.054012\nsqft_lot15       0.082447\nsqft_lot         0.089661\nyr_renovated     0.126434\nfloors           0.256794\nwaterfront       0.266369\nlat              0.307003\nbedrooms         0.308797\nsqft_basement    0.323816\nview             0.397293\nbathrooms        0.525738\nsqft_living15    0.585379\nsqft_above       0.605567\ngrade            0.667434\nsqft_living      0.702035\nprice            1.000000\nName: price, dtype: float64"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "df.corr()['price'].sort_values()"
        }, 
        {
            "source": "# Module 4: Model Development", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "Import libraries ", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 17, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "import matplotlib.pyplot as plt\nfrom sklearn.linear_model import LinearRegression\n"
        }, 
        {
            "source": "\nWe can Fit a linear regression model using the  longitude feature <code> 'long'</code> and  caculate the R^2.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 18, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 18, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "0.00046769430149007363"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "X = df[['long']]\nY = df['price']\nlm = LinearRegression()\nlm\nlm.fit(X,Y)\nlm.score(X, Y)"
        }, 
        {
            "source": "### Question  6\nFit a linear regression model to predict the <code>'price'</code> using the feature 'sqft_living' then calculate the R^2. Take a screenshot of your code and the value of the R^2.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 20, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 20, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "0.49285321790379316"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "X = df[['sqft_living']]\nY = df['price']\nlm = LinearRegression()\nlm.fit(X, Y)\nlm.score(X, Y)"
        }, 
        {
            "source": "### Question 7\nFit a linear regression model to predict the 'price' using the list of features:", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 21, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "features =[\"floors\", \"waterfront\",\"lat\" ,\"bedrooms\" ,\"sqft_basement\" ,\"view\" ,\"bathrooms\",\"sqft_living15\",\"sqft_above\",\"grade\",\"sqft_living\"]     "
        }, 
        {
            "source": "the calculate the R^2. Take a screenshot of your code", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 22, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 22, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "0.65769516660374938"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "X = df[features]\nY= df['price']\nlm = LinearRegression()\nlm.fit(X, Y)\nlm.score(X, Y)"
        }, 
        {
            "source": "#### this will help with Question 8\n\nCreate a list of tuples, the first element in the tuple contains the name of the estimator:\n\n<code>'scale'</code>\n\n<code>'polynomial'</code>\n\n<code>'model'</code>\n\nThe second element in the tuple  contains the model constructor \n\n<code>StandardScaler()</code>\n\n<code>PolynomialFeatures(include_bias=False)</code>\n\n<code>LinearRegression()</code>\n", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 23, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "Input=[('scale',StandardScaler()),('polynomial', PolynomialFeatures(include_bias=False)),('model',LinearRegression())]"
        }, 
        {
            "source": "### Question 8\nUse the list to create a pipeline object,  predict the 'price', fit the object using the features in the list <code> features </code>, then fit the model and calculate the R^2", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 24, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 24, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "Pipeline(memory=None,\n     steps=[('scale', StandardScaler(copy=True, with_mean=True, with_std=True)), ('polynomial', PolynomialFeatures(degree=2, include_bias=False, interaction_only=False)), ('model', LinearRegression(copy_X=True, fit_intercept=True, n_jobs=1, normalize=False))])"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "pipe=Pipeline(Input)\npipe"
        }, 
        {
            "execution_count": 25, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 25, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "Pipeline(memory=None,\n     steps=[('scale', StandardScaler(copy=True, with_mean=True, with_std=True)), ('polynomial', PolynomialFeatures(degree=2, include_bias=False, interaction_only=False)), ('model', LinearRegression(copy_X=True, fit_intercept=True, n_jobs=1, normalize=False))])"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "pipe.fit(X,Y)"
        }, 
        {
            "execution_count": 26, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 26, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "0.75134126473712171"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "pipe.score(X,Y)"
        }, 
        {
            "source": "# Module 5: MODEL EVALUATION AND REFINEMENT", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "import the necessary modules  ", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 27, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "output_type": "stream", 
                    "name": "stdout", 
                    "text": "done\n"
                }
            ], 
            "source": "from sklearn.model_selection import cross_val_score\nfrom sklearn.model_selection import train_test_split\nprint(\"done\")"
        }, 
        {
            "source": "we will split the data into training and testing set", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 28, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "output_type": "stream", 
                    "name": "stdout", 
                    "text": "number of test samples : 3242\nnumber of training samples: 18371\n"
                }
            ], 
            "source": "features =[\"floors\", \"waterfront\",\"lat\" ,\"bedrooms\" ,\"sqft_basement\" ,\"view\" ,\"bathrooms\",\"sqft_living15\",\"sqft_above\",\"grade\",\"sqft_living\"]    \nX = df[features ]\nY = df['price']\n\nx_train, x_test, y_train, y_test = train_test_split(X, Y, test_size=0.15, random_state=1)\n\n\nprint(\"number of test samples :\", x_test.shape[0])\nprint(\"number of training samples:\",x_train.shape[0])"
        }, 
        {
            "source": "### Question 9\nCreate and fit a Ridge regression object using the training data, setting the regularization parameter to 0.1 and calculate the R^2 using the test data. \n", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 29, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": "from sklearn.linear_model import Ridge"
        }, 
        {
            "execution_count": 30, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 30, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "0.64787591639391107"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "RidgeModel = Ridge(alpha = 0.1)\nRidgeModel.fit(x_train, y_train)\nRidgeModel.score(x_test, y_test)"
        }, 
        {
            "source": "### Question 10\nPerform a second order polynomial transform on both the training data and testing data. Create and fit a Ridge regression object using the training data, setting the regularisation parameter to 0.1.  Calculate the R^2 utilising the test data provided. Take a screenshot of your code and the R^2.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": 34, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [
                {
                    "execution_count": 34, 
                    "metadata": {}, 
                    "data": {
                        "text/plain": "0.70027442436889054"
                    }, 
                    "output_type": "execute_result"
                }
            ], 
            "source": "from sklearn.preprocessing import PolynomialFeatures\nfrom sklearn.linear_model import Ridge\npr = PolynomialFeatures(degree=2)\nx_train_pr = pr.fit_transform(x_train)\nx_test_pr = pr.fit_transform(x_test)\npoly = Ridge(alpha=0.1)\npoly.fit(x_train_pr, y_train)\npoly.score(x_test_pr, y_test)\n"
        }, 
        {
            "source": "<p>Once you complete your notebook you will have to share it. Select the icon on the top right a marked in red in the image below, a dialogue box should open, select the option all&nbsp;content excluding sensitive code cells.</p>\n        <p><img width=\"600\" src=\"https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/coursera/project/save_notebook.png\" alt=\"share notebook\"  style=\"display: block; margin-left: auto; margin-right: auto;\"/></p>\n        <p></p>\n        <p>You can then share the notebook&nbsp; via a&nbsp; URL by scrolling down as shown in the following image:</p>\n        <p style=\"text-align: center;\"><img width=\"600\"  src=\"https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/coursera/project/url_notebook.png\" alt=\"HTML\" style=\"display: block; margin-left: auto; margin-right: auto;\" /></p>\n        <p>&nbsp;</p>", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "<h2>About the Authors:</h2> \n\n<a href=\"https://www.linkedin.com/in/joseph-s-50398b136/\">Joseph Santarcangelo</a> has a PhD in Electrical Engineering, his research focused on using machine learning, signal processing, and computer vision to determine how videos impact human cognition. Joseph has been working for IBM since he completed his PhD.", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "source": "Other contributors: <a href=\"https://www.linkedin.com/in/michelleccarey/\">Michelle Carey</a>, <a href=\"www.linkedin.com/in/jiahui-mavis-zhou-a4537814a\">Mavis Zhou</a> ", 
            "cell_type": "markdown", 
            "metadata": {}
        }, 
        {
            "execution_count": null, 
            "cell_type": "code", 
            "metadata": {}, 
            "outputs": [], 
            "source": ""
        }
    ], 
    "metadata": {
        "kernelspec": {
            "display_name": "Python 3.5", 
            "name": "python3", 
            "language": "python"
        }, 
        "widgets": {
            "state": {}, 
            "version": "1.1.2"
        }, 
        "language_info": {
            "mimetype": "text/x-python", 
            "nbconvert_exporter": "python", 
            "version": "3.5.5", 
            "name": "python", 
            "file_extension": ".py", 
            "pygments_lexer": "ipython3", 
            "codemirror_mode": {
                "version": 3, 
                "name": "ipython"
            }
        }
    }, 
    "nbformat": 4
