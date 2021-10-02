# Data 512 A1

By: Andrew Zhou

# Goal

The goal of this project was to replicate a plot of wikipedia number of views. The data can be found using Wikipedia's API. Two API's were involved in this project, the legacy pagecounts API and the new pageview API. My goal of this project is also to make instructions as clear as possible on how to replicate my work to produce the same visualized graph. The graph can be seen below:

![alt text](https://github.com/azhou5211/data-512-a1/blob/main/Page%20Views%20on%20English%20Wikipedia.png)  

# API

### License and Terms of Use

The license and terms of use for the API can be found [here](https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions).  
Documentation for the legacy pagecounts API can be found [here](https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts)  
Documentation for the pageview API can be found [here](https://wikitech.wikimedia.org/wiki/Analytics/AQS/Pageviews)  

### API Notes
These are important notes to take care of when attempting to replicate my work:
- Pageview API excludes spiders/crawlers
- Pagecounts API includes spiders/crawlers
- The Pageview API (but not the Pagecount API) allows you to filter by agent=user. I used the agent=user filter for the Pageview API.

# Processed Data Notes:
My processed data can be found [here](https://github.com/azhou5211/data-512-a1/blob/main/en-wikipedia_traffic_200712-202108.csv).  
The data to be analyzed expands from 2008 to 2021.  

The following process was used to obtain the final data file:
- For data collected from the Pageviews API, combine the monthly values for mobile-app and mobile-web to create a total mobile traffic count for each month.
- For all data, separate the value of timestamp into four-digit year (YYYY) and two-digit month (MM) and discard values for day and hour (DDHH).


My final data file has the following schema:  
Column        | Value
------------- | -------------
year          | YYYY
month         | MM
pagecount_all_views         | num_views
pagecount_desktop_views         | num_views
pagecount_mobile_views         | num_views
pageview_all_views         | num_views
pageview_desktop_views         | num_views
pageview_mobile_views         | num_views

Notes to be wary of:
- pagecount_all_views was obtained by summing pagecount_desktop_views and pagecount_mobile_views
- pagecount_desktop_views was obtained directly from the API
- pagecount_mobile_views was obtained directly from the API
- pageview_all_views was obtained by summing pageview_desktop_views and pageview_mobile_views
- pageview_desktop_views was obtained directly from the API
- pageview_mobile_views was obtained by summing mobile_web and mobile_app values from the API.
  - You must call the API twice. "access" : "mobile-web" and "access" : "mobile-app" and sum the values together to obtain a total mobile views
- The legacy pagecount data has a dip before it is no longer in use. Delete the last month num_views of the legacy pagecount data.

# Replication Process:
- Download the data using the API:
  - The dataset can be downloaded using 5 API requests (Make sure to get monthly data and agent=user for Pageview API):
    - pagecount_desktop
    - pagecount_mobile
    - pageview_desktop
    - pageview_mobile-web
    - pageview_mobile-app
- After obtaining data, start processing the data:
  - Extract the year and month from the timestamp. Day and hour can be discarded.
  - Combine pageview_mobile-web and pageview-mobile-app data to get total pageview-mobile
  - Combine pagecount_desktop and pagecount_mobile to get pagecount_all
  - Combine pageview_desktop and pageview_mobile to get pageview_all
  - Combine all data into one table with the schema provided above
- Analysis:
  - Plot a visualization of the timeseries
  - Visualization will track three traffic metrics: mobile traffic, desktop traffic, and all traffic (mobile + desktop). (In my plot, I used colors: red, green, blue)
  - Seperate pagecount and pageview plots by category. (In my plot, I used dashed line and solid line)
