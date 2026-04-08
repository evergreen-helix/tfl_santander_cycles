This file will stores various notes I make as I work, which I may refer back to at times.

I used this command to import the data
aws s3 sync s3://cycling.data.tfl.gov.uk/ data/raw/ --no-sign-request

at this point is a good time to add the gitignore


# first exploration nb
I will now create my first notebook just to get an understanding of what the data looks like

data just finished downloading, 16.6 GiB

using tree
data
└───raw
    ├───ActiveTravelCountsProgramme
    ├───CycleCounters
    │   ├───Blackfriars
    │   │   ├───July
    │   │   ├───June
    │   │   └───May
    │   └───Embankment
    │       ├───July
    │       ├───June
    │       └───May
    ├───CycleCountsDashboardDocumentation
    ├───CycleParking
    ├───CycleRoutes
    ├───CyclingInfrastructure
    │   ├───data
    │   │   ├───lines
    │   │   └───points
    │   └───documentation
    └───usage-stats

There are two pdfs in ActiveTravelCountsProgramme
- Active travel counts programme Release note
- Cycling use estimates: methodology note
Both very useful offer a lot of clarity on what this data is
As well as two CSVs that will be useful
- 1 Monitoring Locations
- 2 Availabiltiy matrix

The subfolder usage-stats contains entries in the format
Rental Id | Duration | Bike ID | End Date | Endstation Id | EndStation Name | Start Date | StartStation Id |StartStation Name

I picked out one, 321JourneyDataExtract08Jun2022-14Jun2022.csv and it containeds 293,625 rows, so a heavier solution than just pandas is going be needed for managing data.

#data plan:
under data/
the subdir /raw/ should remain untocuhed
a subdir /processed/ will be needed
a subdir /intermediate/ may be needed for handling the zips

#Ingestion Plan:
These are stored in csv files usually with a descriptive name
There are some zip files in usage-stats that must be handled, within those are just csvs
There is a single xlsx file in there too



Could ingest directly into a duckdb, and this seems quite fast, but I've hit some schema issues.
It seems that the schemas seem to vary a bit between different csvs


I have identified a number of different schemas the usage_stats data takes

I have built a system to normalise data in accordance with these difference schemas, and then write them to a single duckdb.

That duckdb is persistant.


#2 initial inspection

as we're using duckdb, a lot of the initial data inspection is done through .execute commands which have sql calls in them.

SQL calls via duckdb won't replace all of the data manipulation, but as it is so efficient for this work because of its columnar storage, it is a valuable tool

Already I am doing things that I find novel, like using an fstring to generate sql strings that i then call in executes to find the NULL count in each column

weather enrichment

using openmeteo, and they have a handy api call builder
uv pip install openmeteo-requests
uv pip install requests-cache retry-requests numpy pandas

uv pip install pyarrow

this one I'm just storing as a parquet as it's not huge, may become a data set later on, we will see

the need for improved file organisation is only going to grow

uv pip install holidays