# Inspiration
The inspiration for this project stemmed from the need to provide a convenient way for students to discover local events happening in our community. We wanted to aggregate event data from various sources, visualize it on a map, and keep it up-to-date to enhance the user experience.

# What it does
This visualization pulls data from Mason360-Events about current and future events and maps it onto an 8-bit themed map created using Tableau. It is an interactive map which displays information of today's and tomorrow's events, when a building is clicked on.

# How we built it
## Step-1: 
Collecting Data and Text Processing We pulled the data from Mason360-Events using python, processed the strings to extract required information using regular expressions. The data file consists of the event name, location, time and date based on which current and future events were divided. It is in csv format.

## Step-2: Building Custom Maps on Tableau Collecting Data:**

Custom maps were built using Power Tools for Tableau (CBI Studio).
After selecting the location to work on, the tool generates a CSV file with latitude and longitude values, shapeID, pointID and shapeLabel for each building based on their end-points.
Building Custom Map:

Upload the file to Tableau and set column to longitude values and row to latitude values to display the polygon-shaped map.
Then, Under 'Marks' tab, select 'Polygon' and set color value to shapeID attribute to display building with unique colors.
Set path value to pointID to form the shapes on the map.
The, to assign names to the buildings, create a fuser-defined field named Location created from 'Create Calculated Field' option on Tableau. Using the following command, label names were created: CASE WHEN shapeID THEN Building Name
Uncheck 'Include in Tooltip' option for shapeID and pointID to stop displaying them when hovered over buildings but retain it for Location to display the building name.
Finally, add the required terrain features from 'Map' tab.

## Step-3: Creating Events Sheet

Upload the web-scraped data to a new sheet called Events on Tableau (events.csv).
Then, set column value to datetime and event_status attributes from events.csv and row value to shapeID, location and event_name attributes.
Then, sort based on event_status to display from current to future events.
Finally, show only required attributes on the sheet using 'Show header' option.
Under 'Marks', select 'Square' to display a square for each event and its details when hovered on the square.

## Step-4: Mapping Events to Build Interactive Map

Under 'Data source' tab, drop both the source files (events.csv and shapes.csv).
Link the files based on shapeID attribute to display the events in their correct locations.

## Step-4: Creating Dashboard

Create a new Dashboard and drop two horizontal containers in it.
In the left container, drag and drop the sheet with map.
In the right container, drag and drop the sheet with event details.

## Step-5: Adding Filter Feature

In the 'Actions' field of the dashboard, add action 'Filter'.
Under 'Source Sheet', select the sheet with GMU map and run action upon 'select'.
Under 'Target Sheet', select the sheet with events and clear selection when show all i.e., if no filter is applied, all the values are displayed.

## Step-6: Stretch-Goal Trying to Achieve Continuous Data Ingestion**

To ensure that the data in the Tableau dashboard remains up-to-date, we implemented an automated solution using AWS services.
Specifically, we created a Lambda function responsible for web scraping event details and converting them into a CSV file.
This CSV file is then saved to an S3 bucket. To ensure regular updates, we scheduled the Lambda function to run at 30-minute intervals.
However, linking this to Tableau was not successful, so we used events.csv file.

# Challenges we ran into

Challenges were mostly with figuring out how to build custom maps in Tableau and how to link the two different data sources such that the locations are mapped accurately. Another big challenge is pulling data at timed intervals everyday. We did it using AWS but could not link S3 bucket to Tableau due to access issues. Also, as we're scraping for the first time, we had to learn along the way that beautiful-soup doesn't work for dynamic pages and change our approach.

# What we learned

A lot about using Tableau in general and creating custom maps, using AWS Lambda to ingest data at continuous intervals!!

# What's next for Patriots Event Finder

So far, we were able to scrape data currently available on Mason360-Events site and link it on Tableau. But moving forward, we want to automate the process of data collection using some service that looks for data updates at regular intervals like AWS Lambda, so that the Event Finder remains up-to-date.

# Built With

amazon-web-services
mapbox
python
tableau
