# Snowflake Course Notes

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
## What is Snowpipe?
- Enables loading once a file appears in a bucket
- if data needs to be available immediately for analysis
- Snowpipe uses serverless features instead of warehouses
- Automatically detects that new files are added to a bucket via an event notification

## Setting up Snowpipe
- Create stage
    - establishes connection
- Test COPY COMMAND
    - ensure that the connection is working
- Create Pipe
    - Create pipe as object with COPY COMMAND
    - Contains copy command definition
- Set up S3 Notification
    - To trigger snowpipe

## Setting up Snowpipe in Azure
- when a file is added to an azure container, an event notification will fire and place the file to a queue storage and will be consumed by our notification integration in Snowflake. Snowflake can watch the queue storage. 
- Steps:
    - Storage Integration
        - Connection details to container
        - Grant permissions
    - Create stage
        - location to container
    - Queue + Notification
        - To trigger snowpipe
    - Notification integration
        - Notification can be received by snowflake
        - Grant permissions
    - Create pipe
        - create pipe as object with copy command

## Time Travel
- Enterprise or higher -> up to 90 days time travel
- Standard edition -> up to 1 day time travel
- Time travel on offset
- Time travel on timestamp
- Time travel on statement
    - To me, this is the most useful because you can get data from a historical query ID and you dont have to remember when you queried the right data last
- Retention time
    - Standard Edition:
        - Time travel up to 1 day
    - Enterprise Edition:
        - Time travel up to 90 days
    - Business Critical:
        - Time travel up to 90 days
    - Virtual Private:
        - Time travel up to 90 days
    - Set the retention period for what work for you
- Cost
    - SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.TABLE_STORAGE_METRICS

## Fail Safe
- Part of the data protection life cycle
- Protection of historical data in case of disaster
- Non configurable 7 day period for permanent tables
- Period starts immediately after time travel period ends
- No user interaction & recoverable only by Snowflake
- Contributes to storage cost
- How does it work?
    - Current Data Storage
        - Access and query data etc.
    - Time travel
        - 0 - 90 days retention time
        - SELECT ...AT | BEFORE Undrop
    - Fail safe
        - No user operations/queries
        - recovery beyond time travel
        - restoring only by snowflake support
        - transient table retention is 0 days
        - permanent table retention is 7 days

## Snowflake Table Types
- Permanent
    - ex. CREATE TABLE
    - Includes:
        - Time travel retention period
        - Fail safe
    - Can be more expensive
    - Travel retention period:
        - 0 - 90 days
    - Usages:
        - Permanent data
- Transient
    - ex. CREATE TRANSIENT TABLE
    - Includes:
        - Time travel retention period
    - Does not include:
        - Fail safe
    - Travel retention period:
        - only configurable to 1 day
    - Usages:
        - good for very large data and want to keep the storage costs low
        - if we dont need a lot of retention protection for our data
- Temporary
    - ex. CREATE TEMPORARY TABLE
    - Includes:
        - Time travel retention period
    - Does not include:
        - Fail safe
    - Travel retention period:
        - only configurable to 1 day
    - Usages:
        - non permanent data
- What is the difference between transient and temporary?
    - it exists only in the current session
    - other users will not see this table
    - once the session is closed, the data will not be recoverable
- Types are also available for other database objects (database, schema, etc)
- For temporary table, no naming conflicts with permanent/transient tables
    - Other tables will be hidden

## Zero Copy Cloning
- Create copies of a database, a schema or a table
- Cloned object is independent from original table
- Easy to copy all metadata & improved storage management
- Usage:
    - Creating backups for development purposes
- Works with time travel
- Command
    - CREATE TABLE <table_name> ... CLONE <source_table_name>
    - You can use this with the time travel feature where BEFORE is referenced
- Any structure of the object and metadata is inherited
    - clustering keys, comments, etc
- Data storage object (permanent & transient)
    - Databases
    - Schemas
    - Tables
    - Cannot use temporary
- Configuration objects
    - Stages
    - File formats
    - Tasks

## Swapping tables
- Use case:
    - development table into production table
        - swapping the metadata
    - ALTER TABLE <table_name> SWAP WITH <target_table_name>

## Data sharing
- Usually this can be a complicated process
- Sharing data without actual copy of the data and up to date
- Shared data can be consumed by the own compute resources
- Non-Snowflake users can also access through a reader account
- Account that is producing this data is called the 'Producer'
- Account that is receiving this data is called the 'Consumer'

## Data sharing with Non-snowflake users
- Independent account but we still have to pay for the consumption of the compute resources
- Steps:
    - New Reader Account
        - Independent instance with own url & own compute resources
    - Share data
        - Share database & table
    - Create database
        - In reader account, create database from share
    - Create Users
        - as administrator, create user and roles
- If you have a business critical account, then you are not allowed to share to a non business critical account. however, this can be overriden with share_restriction=FALSE

## Data Sampling
- Sometimes it is not necessary to test on a complete dataset, especially if its massive
- Instead of querying on the complete database, you would take a random sample to develop and test
    - Good use for data analysis
- Why sampling?
    - query development, data analysis
    - faster and more efficient
- Methods:
    - Row or Bernoulli
        - Every row is chosen with percentage p
        - More randomness
        - Recommended when dealing with smaller tables
    - Block or System
        - Every block is chosen with percentage p
        - More effective processing
        - Recommended when dealing with large tables

## Scheduling Tasks
- Tasks can be used to schedule SQL statements
- Standalone tasks and trees of tasks
- Not possible for one child task has multiple parent tasks
- One parent can have up to 100 child tasks
- One tree overall can have up to 1000 tasks
- Every task has one parent
- Tasks have all of the privileges of the owner
- A task can execute a single SQL statement, including a call to a stored procedure
- A virtual warehouse that is specific to the task is used to execute the SQL statement in a task

## Streams
- Scenario: if we are building our data warehouses, usually an ETL is involved to extract from a source table. Usually, we are also getting the delta related to this. We want to capture changes to these tables.
- Streams are objects that records (DML-) changes made to a table
    - The values are not actually stored in the stream object and are just retreived from the original table
    - No additional costs related to stream object tables
    - The only added cost is for the metadata that represents these tables
    - Once the data transition has occured in the deltas, the stream object will be terminated
- The process is called change data capture (CDC)
- Includes:
    - DELETE
    - INSERT
    - UPDATE
- Types of streams:
    - Standard
        - inserts
        - updates
        - deletes
    - Append-only
        - inserts

## Materialized Views
- Scenario: we have a view that is queried frequently and it takes a long time to process
    - We can create a materialized view to solve this problem
- use any SELECT-statement to create this MV
- results will be stored in a separate table and this will be updated automatically based on the base table
- view is automatically updated by an update service in snowflake
- When to use?
    - View would take a long time to be processed and is used frequently
    - Underlying data is change not frequently and on a rather irregular basis
- If the data is updated on a very regular basis...
    - Using tasks and streams could be a better alternative
- Dont use materialized view if data changes are very frequent
- Keep maintenance cost in mind
- Consider leveraging tasks (& streams) instead
- Limitations:
    - Only available in the enterprise edition
    - Joins (including self-joins) are not supported
    - Limited amount of aggregation functions
    - No user defined functions
    - No HAVING clauses
    - No ORDER BY clause
    - No LIMIT clause

## Dynamic Data Masking
- Create masking policies for data types that are specific to different roles in your organization
- Find out which policies are applied to certain roles
    - information_schema.policy_references(policy_name=>'policyName')
- If we want to drop or replace a policy, we need to unset the policy to ensure that its not applied on existing columns

## Access Control
- who can access and perform operations on objects in Snowflake
- two aspects of access control combined in snowflake
- Discretionary Access Control (DAC)
    - each object has an owner who can grant access to that object
- Role-based Access Control
    - acces privileges are assigned to roles, which are in turn assigned to users
- Securable objects
    - objects in snowflake to which priviliges can be granted
    - Account
        - User
            - People or systems
        - Role
            - Entity to which priviliges are granted (role hierarchy)
        - Database
            - Schema
                - Table
                - View
                - Stage
                - Integration
                - Other schema objects
        - Warehouse
        - Other account objects
- Every object owned by a single role (multiple users)
- Owner (role) has all priviliges per default
- Privilege
    - Level of access to an object (SELECT, DROP, CREATE, etc)

## Snowflake Roles
### ACCOUNTADMIN
- SYSADMIN and SECURITYADMIN
- top level role in the system
- should be granted only to a limited number of users
- manage and view all objects
- all configurations on account level
- account operations (create reader account)
- first user will have this role assigned
- initial setup and managing account level objects
- Best practices:
    - very controlled assignment strongly recommended
    - enable multi factor authentication
    - at leat two users should be assigned to this role
    - avoid creating objects with that role unless you have to
### SECURITYADMIN
- USERADMIN role is granted to SECURITYADMIN
- can manage users and roles
- can manage any object grant globally
### SYSADMIN
- create warehouses and databases (and more objects)
- recommended that all custom roles are assigned
### USERADMIN
- dedicated to user and role management only
- can create users and roles
### PUBLIC
- automatically granted to every user
- can create own objects like every other role (available to every other user/role)