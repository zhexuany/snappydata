#Overview of iSight#
iSight from SnappyData is a cloud based service that allows for instant visualization of analytic query results on large datasets. Powered by the SnappyData Synopsis Data Engine (SDE), users interact with iSight to populate the synopsis engine with the right data sets and accelerate SQL queries by using the engine to provide latency bounded responses to large complex aggregate queries. 
iSight uses Apache Zeppelin as the frontend notebook to display results and allows users to build powerful notebooks representing key elements of their business in a matter of minutes. 
The service provides a web URL that spins up a cluster instance on AWS or users can download the iSight EC2 script to configure a custom sized cluster, to create and render powerful visualizations of their big data sets with the click of a button. 
With iSight, you can speed up the process of understanding what your data is telling you, and move on to the task of organizing your business around those insights rapidly.
In this document, we describe the features provided by SnappyData for analysing your data. It also provides details for deploying a SnappyData cluster on AWS CloudFormation or on AWS using the EC2 script. 
Refer to the the examples and guidelines provided in this document to help you create notebooks using which, you can execute SQL queries or data frame API to analyse your data.
##Key Components##
This section provides a brief description of the key terms used in this document. 

* **Amazon Web Services (AWS**):  Amazon Web Services (AWS) is a comprehensive, evolving cloud computing platform that offers a suite of cloud-computing services. The services provided by this platform that are important for SnappyData are, Amazon Elastic Compute Cloud (EC2) and Amazon Simple Storage Service (S3).
* **SnappyData Cluster**:  A database cluster which has three main components - Locator, Server and Lead
* **Apache Zeppelin**: Apache Zeppelin is a web-based notebook that enables interactive data analytics. It allows you to make data-driven, interactive and collaborative documents with SQL queries or directly use the Spark API to process data
* **Interpreters**: A software module which is loaded into Apache Zeppelin upon startup. Interpreters allow various third party products including SnappyData to interact with Apache Zeppelin. The SnappyData interpreter gives users the ability to execute SQL queries or use the data frame API to visualize data.

#Quick Start Steps#

Do want to understand the product and start using it in a few minutes? Follow these easy steps that can get your started quickly:

1. [Setting up SnappyData Cluster](#SettingUp)<br>
		- [Deploying the Cluster with AWS CloudFormation](#DeployingClusterCloudFormation)<br>
		- [Deploying  the Cluster with AWS using Script](#DeployingClusterScript)
4. [Loading Data from AWS S3](#dataAWSS3)
5. [Logging into Apache Zeppelin](#LoggingZeppelin)
		- [Using Predefined Notebook](#predefinednotebook)<br>
		- [Creating your own Notebook](#Creatingnotebook)

<a id="SettingUp"></a>
#Setting Up SnappyData Cluster#
##Prerequisites##
* Existing account with AWS with required permissions to launch EC2 resources.
* Using the Amazon Web Services (AWS) Secret Access Key and the Access Key ID, set the two environment variables, AWS_SECRET_ACCESS_KEY and AWS_ACCESS_KEY_ID .
If you already have set up the AWS Command Line Interface on your local machine, the script automatically detects and uses the credentials from the AWS credentials file.
You can also find this  information on the  AWS homepage, from the **Account** > **Security Credentials** > **Access Credentials** option.
	For example,
	
```
export	 AWS_SECRET_ACCESS_KEY=abcD12efGH34ijkL56mnoP78qrsT910uvwXYZ1112
export AWS_ACCESS_KEY_ID=A1B2C3D4E5F6G7H8I9J10
```
<a id="DeployingClusterCloudFormation"></a>
##Deploying the Cluster with AWS CloudFormation##
SnappyData uses the Amazon EC2 with AWS Cloud Formation method to automatically install, configure and start a SnappyData cluster. In this release, the configuration supports launching the cluster on a single EC2 instance.
It is recommended that you select a instance type with higher processing power and more memory for this cluster, as it would be running five processes (locator, lead, two data servers and an Apache Zeppelin server) on it.
This method is recommended as the quickest way to deploy SnappyData. All you need is an existing AWS account and login credentials to get started! 

###Configuring and Launching the Cluster###

The configure and launch the cluster:

Go to [http://www.snappydata.io/cloudbuilder](http://www.snappydata.io/cloudbuilder). On this page, you need to complete the steps to configure and start the cluster.

1. Enter the name for your cluster. Each cluster is identified by it’s unique name. The names and details of the members are automatically derived from the provided cluster name.
![STEP](./Images/AWS_clustername.png)

2. Enter a name of an existing EC2 KeyPair. This enables SSH access to the cluster. Refer to the Amazon documentation for more information on generating your own key pair.	
 ![STEP](./Images/aws_ec2keypair.png)
 
 3. Select an instance and storage based on the capacity that you need..
 ![STEP](./Images/aws_instancetype.png)
 
4. Enter a value (between 256GB - 1024 GB) in the **EBS Volume Size(gigabytes)** field.  ![STEP4](./Images/aws_ebsvolumesize.png)
 
	>### Note: Currently only Amazon Elastic Block Storage (EBS) is supported.


 5. Enter your email address 
 ![STEP4](./Images/aws_email.png)
 
6. Click **Generate**. 
7. On the next page, select the AWS region, and then click **Launch Cluster** to launch your single-node cluster.
  ![STEP4](./Images/aws_selectedregion.png)
  
	> ### Note: Ensure that the key pair is created in the region selected.
	> If are not already logged into AWS, you are redirected to the AWS log in page. Enter your credentials to continue.
	> It may take a few minutes for the cluster to be created. When you launch a cluster, an instance of Apache Zeppelin is also launched, and can be accessed from http://`<zeppelin_host>`:`<port_number>`. 



8. The **Select Template page**, allows you to select a template that describes the stack you want to create. By default, the URL for the Amazon S3 template is provided. Click **Next**.  ![STEP4](./Images/aws_selecttemplate.png)

9. Enter a stack name or continue to use the default value. Click **Next**.

	>### Note: The stack name must contain only letters, numbers, dashes and should start with an alpha character.

10. Specify the tags (key-value pairs) for resources in your stack or leave the field empty to use the default values. Click Next.
11. On the **Review** page, verify the details and click **Create** to create a stack. 

	> ###Note: This operation may take a few minutes to complete. 

12. The next page lists the existing stacks. Click **Refresh** to update the list and to view the current status of the stack. Ensure that the status of the stack is “Create_Complete”. 

13. Open a web browser and enter the http://`<Public DNS>`:`<Port Number>` to launch Apache Zeppelin. You can find the public DNS by loading the EC2 home page of the stack.

	> ###Note: To stop incurring charges for the instance, you can either terminate the instance or delete the stack. You can however, not connect to or restart an instance after you've terminated it.

<a id="DeployingClusterScript"></a>
##Deploying the Cluster on AWS using Scripts##
SnappyData provides a script that allows you to launch and manage SnappyData clusters on Amazon Elastic Compute Cloud (EC2). 
Download the script from the [SnappyData Release page](https://github.com/SnappyDataInc/snappydata/releases/). The package is available in compressed files (snappy-ec2.tar). Extract the contents to a location on your computer.

Using the **snappy-ec2 **script, you can identify each of the clusters by it's unique cluster name. The script internally ties members (locators, leads and stores) of the cluster with EC2 security groups. 

###Launching SnappyData Cluster###
To execute the script,  type the following at a command prompt:


	 ./ec2/snappy-ec2 -k <your-key-name> -i <your-keyfile-path> --snappydata-version=CUSTOM --with-zeppelin=embedded launch <your-cluster-name>
 
The  names and details of the members are automatically derived from the provided cluster name, and one instance of locator, lead and server is started.
For example, if you launch a cluster named **my-cluster**, the locator is available in security group named **my-cluster-locator** and the stores are available in **my-cluster-stores**.

When running the script you can also specify properties like number of stores and region.
For example, using the following command, you can start a SnappyData cluster named **snappydata-cluster** with 4 stores (or servers) in the North Virginia region on AWS.

```
./snappy-ec2 -k ec2-keypair-name -i /path/to/keypair/private/key/file --stores=4 --region=us-east-1 launch snappydata-cluster
```
<a id="dataAWSS3"></a>
#Loading Data from AWS S3#
SnappyData provides you with predefined buckets which contain datasets. When data is loaded, the table reads from the files available at the specified external location (AWS S3). 

> ###Note:
> The Amazon S3 buckets and files are private by default. Ensure that you set the permissions required to make the data publicly accessible. Please refer to the documentation provided by Amazon S3 for detailed information on creating a bucket, adding files and setting required permissions.
> You can also find AWS related information on the AWS homepage, from the Account > Security Credentials > Access Credentials option.
> Information related to the Bucket Name and Folder Location can be found on the AWS S3 site.

To define a table that references the data in AWS S3, create a paragraph in the following format:

```
%sql
DROP TABLE IF EXISTS <table_name> ;
CREATE EXTERNAL TABLE <table_name> USING parquet OPTIONS(path '<AWS_SECRET_ACCESS_KEY>:<AWS_ACCESS_KEY_ID>@<bucket_Name>/<folder_name>');
```

The values are:

**Property** | **Description/Value**
---------------|-----------------------------
```<table_name>``` |The name of the table
```<AWS_SECRET_ACCESS_KEY>:<AWS_ACCESS_KEY_ID> ```| Security credentials are used to authenticate and authorize calls that you make to AWS. 
```<bucket_Name> ```| The name of the bucket where the folder is located. Default value: zeppelindemo 
```<folder_name>``` | The folder name where the data is stored. Default value: nytaxifaredata 

<a id="LoggingZeppelin"></a>
#Logging into Zeppelin#

Apache Zeppelin provides web-based notebooks for data exploration. A notebook consists of one or more paragraphs, and each paragraph consists of a section each for code and results.
Launch Apache Zeppelin from the web browser by accessing the host and port associated with your Apache Zeppelin server. For example,http://`<zeppelin_host>`:`<port_number>`. The welcome page which lists existing notebooks is displayed.  
SnappyData provides predefined notebooks which are displayed on the home page after you have logged into Apache Zeppelin. For more information, see [Using Predefined Notebooks](###Using Predefined Notebooks).
##Using the Interpreter##
Snappydata Interpreter group consists of the interpreters `%snappydata.snappydata` and `%snappydata.sql`.
To use an interpreter, add the associated interpreter directive with the format %<Interpreter_name> at the beginning of a paragraph in your note. In a paragraph, use one of the interpreters, and then enter required commands.
>### Note:
* The SnappyData Interpreter provides a basic auto-completion functionality. Press (Ctrl+.) on the keyboard to view a list of suggestions.
* It is recommend that you use the SQL interpreter to run queries on the SnappyData cluster, as an out of memory error may be reported with running the Scala interpreter.

###SQL Interpreter###
The `%snappydata.sql` code specifies the default SQL interpreter. This interpreter is used to execute SQL queries on SnappyData cluster.
####Multi-Line Statements####
Multi-line statements as well as multiple statements on the same line are also supported as long as they are separated by a semicolon. However, only the result of the last query is displayed.
SnappyData provides a list of connection-specific SQL properties that can be applied to the paragraph that is executed. 
In the following example,  spark.sql.shuffle.partitions allows you to specify the number of partitions to be used for this query:

```
%sql
set spark.sql.shuffle.partitions=6; 
select medallion,avg(trip_distance) as avgTripDist from nyctaxi group by medallion order by medallion desc limit 100 with er
```
####SnappyData Directives in Apache Zeppelin####
You can execute approximate queries on SnappyData cluster by using the %sql show-instant-results-first directive. 
In this case, the query is first executed on the sample table and the approximate result is displayed, after which the query is run on the base table. Once the query is complete, the approximate result is replaced with the actual result.
In the following example, you can see that the query is first executed on the sample table, and the time required to execute the query is displayed. 
At the same time, the query is executed on the base table, and the total time required to execute the query on the base table is displayed.
```
%sql show-instant-results-first
select avg(trip_time_in_secs/60) tripTime, hour(pickup_datetime), count(*) howManyTrips, absolute_error(tripTime) from nyctaxi where pickup_latitude < 40.767588 and pickup_latitude > 40.749775 and pickup_longitude > -74.001632 and  pickup_longitude < -73.974595 and dropoff_latitude > 40.716800 and  dropoff_latitude <  40.717776 and dropoff_longitude >  -74.017682 and dropoff_longitude < -74.000945 group by hour(pickup_datetim
```
![Example](./Images/DirectivesinApacheZeppelin.png)

>###  NOTE:
> This directive will only work for the SQL interpreter and an error may be displayed for the Scala interpreter

###Scala Interpreter###
The `%snappydata.snappydata code` specifies the default Scala interpreter. This interpreter is used to write Scala code in the paragraph.
SnappyContext is injected in this interpreter and can be accessed using variable **snc**.
###Using Predefined Notebooks###  
<a id="predefinednotebook"></a>
SnappyData provides you a predefined notebook **NYCTAXI Analytics** which contains definitions that are stored in a single file. 
When you launch Apache Zeppelin in the browser, the Welcome page displays the existing notebooks. Open a notebook and run any of the paragraphs to analyse data and view the result. 
<a id="Creatingnotebook"></a>
##Creating Notebooks - Try it Yourself!##

1. Log on to Apache Zeppelin, create a notebook and insert a new paragraph.
2. Use `%snappydata.snappydata` for SnappyData interpreter or use `%snappydata.sql` for SQL interpreter.
3. Download a dataset you want to use and create tables as mentioned belo

###Examples of Queries and Results###
This section provides you with examples you can use in a paragraph.

* In this example, you can create tables using external dataset from AWS S3

![Example](./Images/sde_exampleusingexternaldatabase.png)

* In this example, you can execute a query on a base table using the SQL interpreter. It returns the number of rides per week. 

![Example](./Images/sde_exampleSQLnoofridesbase.png)

* In this example, you can execute a query on a sample table using the SQL interpreter. It returns the number of rides per week

![Example](./Images/sde_exampleSQLnoofridessample.png)

* In this example, you are processing data using the SnappyData Scala interpreter

![Example](./Images/sde_exampledatausingSnappyDataScala.png)
