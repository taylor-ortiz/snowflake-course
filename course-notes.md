# Snowflake Class Notes

- The three layers of snowflake are storage, query processing, and cloud services
- Query processing is the muscle of the system
- Cloud services is referred to as the brain of the system
- Virtual warehouses are used as compute resources to process queries
## Scaling policy
    - Multi clustering
        - At certain time we have more queries than a certain warehouse can process. The solution is that we can automatically shut down or start additional clusters to handle the load. Redistributing queries
        - Auto scaling
            - If we have more queries that can be processed by a single warehouse, there will be a queue
            - When do we start an additional cluster?
            - Scaling policies
                - Standard: favors starting additional warehouses
                - Economy: favors conserving credits rather than starting additional warehouses
            - What is a check?
                - Determines whether the load on the least loaded cluster could be redistributed to the other clusters

## What is the purpose of a data warehouse?
    - Database that is used for reporting and data analysis
    - Database that combines different data from different data sources
    - Integrate various sources of data
    - ETL process is process of creating a data warehouse (Extract, Transform, & Load)
## What are the layers of a data warehouse?
    - Raw data: just extracting raw data as is from the data sources into data warehouse
        - Staging area
        - Doesn’t have to be within the data warehouse 
    - Data integration: transforming the data into how you’d like to utilize it
        - Data transformation
    - Access layer: make data accessible to different solutions such as reporting, data science and other apps
## Why cloud computing?
    - Historically, it was necessary to have a data center
        - Issue is that this can cause a ton of overhead. the infrastructure, security and electricity play a part in the maintenance. Also the need for software/hardware upgrades. 
    - Application: we are only responsible for this part. Beauty of SaaS
    - Software: Snowflake
    - Data: Snowflake
    - Operating system: Snowflake
    - Physical servers: AWS, Azure or GCP
    - Virtual machines: AWS, Azure or GCP
    - Physical storage: AWS, Azure or GCP
## What editions are available in Snowflake?
    - Enterprise: additional features for the needs of large scale enterprises
        - All standard features
        - Multi cluster warehouses
        - Time travel up to 90 days
        - Materialized views
        - Search optimization
        - Column level security
    - Standard: introductory level
        - Complete DWH
        - Automatic data encryption
        - Time travel up to 1 day
        - Disaster recovery for 7 days beyond time travel
        - Secure data share
        - Premier support 24/7
    - Virtual Private: highest level of security
        - All business critical features
        - Dedicated virtual servers and completely separate snowflake environment
    - Business Critical: even higher levels of data protection for organizations with extremely sensitive data
        - All enterprise features
        - Additional security such as data encryption everywhere
        - Extended support
        - Database failover and disaster recovery
## Snowflake pricing
    - Pay only what you need
    - Scalable amount of storage at affordable cloud price
    - Pricing dependent on the region
    - Compute and storage costs decoupled
    - Storage:
        - Monthly storage fees
        - Based on average storage user per month
        - Cost calculated after compression
        - Cloud providers
        - On demand storage
            - Pay for what you are using
            - $40/TB
        - Capacity storage
            - Pay up front for a predefined capacity
            - $23/TB
    - Compute
        - Charged for active warehouses per hour
        - Depending on the size of the warehouse
        - Billed by second (minimum of 1 min)
        - Charged in snowflake credits
    - Dollars get converted into credits that are consumed by usage across both pillars
    - Pricing might differ per region
    - Not being charged for the data that you want to push data in from such as AWS or Google Cloud Platform
￼
## Snowflake Roles
    - Account admin
        - Sysadmin and securityadmin
        - Top level role in the system
        - Should be granted only for a few years
    - Security admin
        - Useradmin role is granted to security admin
        - Can manage users and roles
        - Can manage any object grant globally
    - Sysadmin
        - Create warehouses and databases (and more objects)
        - Recommended that all custom roles are assigned 
    - Useradmin
        - Dedicated to user and role management only
        - Can create users and roles
    - Public
        - Automatically granted to every users
        - Can create own objects like every other role
## Loading Data
    - Bulk loading
        - Most frequent method
        - Uses warehouses
        - Loading from stages
        - Copy command
        - Transformations possible
    - Continuous loading
        - Designed to load small volumes of data
        - Automatically once they are added to stages
        - Lates results for analysis
        - Snow pipe (serverless feature)
    - Understanding stages
        - Not to be confused with data warehouse stages
        - Location of data files where data can be loaded from
        - External stage
            - External cloud provider
            - S3
            - Google cloud platform
            - Azure
            - These external stages are database objects created in a schema
                - CREATE STAGE (URL, access settings)
                - (There can be a different costs if the region/platform differs)
        - Internal stage
            - Local storage maintained by snowflake
        - Storage integration objects
            - Best practice on where to hold credentials for stage objects
## Copy Options
    - Specify maximum size (in bytes) of data loaded in that command (at least one file)
    - When the threshold is exceeded, the COPY operation stops loading
￼
    - RETURN_FAILED_ONLY: specifies whether to return only files that have failed to load in the statement result
        - Usually only makes sense to use in combination with ON_ERROR=CONTINUE
    - TRUNCATE_COLUMNS: specifies whether to truncate strings or not based on character size allotment 
        - TRUE = string would be automatically truncated to the target column length
        - FALSE = copy produces an error
    - FORCE
        - TRUE: a file that has been loaded previously and has not been changed can be loaded anyways
        - This has the potential to duplicate data in a table
## Load History
    - Enables you to retrieve the history of data loaded into tables using the copy into <table> command
## Load Unstructured Data
    - Create Stage
    - Load raw data
        - into separate table
        - Variant data type
            - this data type can handle complex data types
    - Analyze & parse
        - Analyze with snowflake functions
    - Flatten & load
## Performance Optimization
    - Automatically managed micro partitions
    - Sizing virtual warehouses
    - Cluster keys
    - Dedicated virtual warehouses
        - separated according to different workloads
    - Scaling up
        - for known patterns of high work load
    - Scaling out
        - Dynamically scale our multi cluster warehouse out and automatically create new warehouse clusters
    - Maximize cache usage
        - Automatic caching can be maximized
    - Cluster keys
        - for large tables
## Dedicated virtual warehouses
    - Identify & Classify
        - Identify and classify groups of workload/users
        - BI team, data science team, marketing department
    - Create dedicated virtual warehouses
        - For every class of workload and assign users
    - Considerations
        - Not too many virtual warehouses
            - Avoid underutilization 
        - Refine classifications
            - Work patterns can change
## Scaling up/down
    - Changing the size of the virtual warehouse depending on different workloads in different periods
    - Use cases:
        - ETL at certain times (for example between 4pm and 8pm)
        - Special business event with more work load
    - NOTE: common scenario is increased query complexity, NOT more users (then scaling out would be better)
## Scaling Out
    - Using additional warehouses / multi cluster warehouses
    - More concurrent users/ queries
    - Handling performance related to large numbers of concurrent users
    - Automation of the process if you have fluctuating number of users
## Multi clustering
    - If we have more queries that could be processed by a single cluster, new instances of clusters can be spawned 
    - Considerations:
        - If you use at least enterprise edition, all warehouses should be multi cluster
        - Minimum: default should be 1
        - Maximum: can be very high
## Caching
    - Automatic process to speed up the queries
    - if query is executed twice, results are cached and can be re-used
    - results are cached for 24 hours or until underlaying data has changed
    - What can we do?
        - Ensure that similar queries go on the same warehouse
        - Example: team of data scientists run similar queries, so they should all use the same warehouse

## Cluster Keys
    - snowflake automatically maintains these cluster keys
    - in general, snowflake produces well-clustered tables
    - cluster keys are not always ideal and can change over time
    - When to cluster?
        - clustering is not for all tables
        - mainly very large tables of multiple terabytes can benefit
    - How to cluster?
        - columns that are used most frequently in WHERE-clauses (often date columns for event tables)
        - if you typically use filters on two columns then the table can also benefit from two cluster keys
        - column that is frequently used in joins
        - large enough number of distinct values to enable effective grouping 
        - small enough number of distinct values to allow effective grouping
        - Example:
            - CREATE TABLE <name> ... CLUSTER BY ( <column1> [, <column2> ...])
            - CREATE TABLE <name> ... CLUSTER BY ( <expression>)
            - ALTER TABLE <name> CLUSTER BY (<expr1> [, <expr2> ... ])
            - ALTER TABLE <name> DROP CLUSTERING KEY
