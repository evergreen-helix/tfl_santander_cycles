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
