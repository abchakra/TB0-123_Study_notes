# TB0-123_Study_notes

TIBCO BW
 
TIBCO BusinessWorks is a integration tool based on technology standards – it includes an Enterprise Service Bus (ESB) and Web services platform used to connect applications and data with little or no programming. It provides an Integrated Services Environment (ISE) for creating Web services and orchestrating process flows to improve the consistency and adaptability of both IT and business operations.
Benefits:
 
It speeds up the application development and deployment cycle by exposing discrete business functions as reusable services that can be easily invoked without requiring an understanding of their technical implementation.
It reduces costs, improves productivity by enabling one to configure application interfaces, build complex data transformations, and process orchestration without coding. At max, it requires minimal coding.
It reduces the cost of application development – a lot of  functions and data are available which can be used as reusable services that can be orchestrated to form complex business processes and assembled to form composite services and applications.
It simplifies the complexity of enterprise-wide SOA initiatives with its completely application–and platform-neutral approach to integration.
It improves the performance,  scalability and consistency of infrastructure with a standards-based, comprehensive and proven platform capable of integrating virtually any IT resource.
 
Transitions
 
The arrows are unidirectional, and you cannot draw a transition to a previously executed activity.
If you wish to perform looping, use groups to specify multiple executions of grouped activities.
Having multiple branches in a process definition does not imply that each branch is processed concurrently.
Transitions describe control flow of the process definition, not the concurrency of execution of activities.
Each activity in a process definition must have a transition to it, or the activity is not executed when the process executes.
Null activity in the General Activities palette useful for joining the multiple branches into a single execution path.
You cannot transition to a previously executed activity.
Rethrow and Generate Error can not have transitions to other activities.
 
Conditions
 
Conditions are specified on transitions to determine whether to take the transition to the next activity or not.
Success Take this transition unconditionally. That is, always 	transition to the activity the transition points to, if the activity completes successfully. This is the default condition for transitions
Success with Condition Specify a custom condition using XPath. If the activity completes successfully, and the condition you create evaluates to true, the transition is taken to the activity it points to. You can type in an XPath condition, and you can use the XPath formula builder to drag and drop XPath expressions and data into the condition.
Success if no matching condition  Take this transition when the activity completes successfully, but only if no other transitions are taken. This is useful when multiple transitions with conditions are drawn to other activities. This condition type can be used 	to handle any cases not handled by the conditions on the other transitions.
Error Take this transition if there is an error during processing of the activity.
 
Best Practices for Conditions:
 
We should not have a large number of conditions from  activity as it increases the overhead of the process, as on the starting of the process it evaluates all the conditions.
If we have a number of conditions then we should use a “Success with No Matching condition” to handled the not handled cases.
 
Groups
 
The main uses of groups are the following:
 
To create a set of activities that have a common error transition.similar to a try...catch block in Java. This allows you to have a set of activities with only one error-handling transition, instead of trying to individually catch errors on each activity.
To create sets of activities that are to be repeated. You can repeat the activities once for each item in a list, until a condition is true, or if an error occurs.
To create sets of activities that participate in a transaction. Activities in the group that can take part in a transaction are processed together, or rolled back, depending upon whether the transaction commits or rolls back.



Grouping Activities – Type of Group Actions in tibco bw.
None: Used for grouping without looping
Transaction Groups: Used to group activities that participate in a transaction. Eg. JDBC group activities
Iterate Loop: Used to iterate a group once for every item in a list
Repeat Until True Loop: Used to iterate a group until the specified condition is true
Repeat On Error Until True Loop: Used to iterate a group when an error occurs
If Groups: To conditionally execute business logic
While True Groups: Repeats the series of grouped activities as long as the given condition evaluates as true.
Critical Section: Used to synchronize process instances so that only one process instance executes the grouped activities at any given time.
Pick First: Allow process execution to wait for one or more events
No Action Groups
 
1.           If you do not wish for the activities in the group to repeat, specify the group action to be None.
2.           No action groups are primarily useful for specifying a single error transition out of the group so that if an unhandled error occurs in the group,
3.           you only need one error transition instead of an error transition for each activity.
4.           This behavior is similar to a try...catch block in Java.
If Groups
 
1.           You can use groups to conditionally execute business logic.
2.           The If group allows you to specify a set of conditions that are evaluated in order.
3.           The transitions from the start of the If group specify the conditions to be evaluated.
4.           Each transition is numbered and evaluated in order.
5.           The first condition to evaluate to true determines which execution path to take in the group.
6.            All other conditions are ignored once one evaluates to true.
7.           An "otherwise" condition can be specified to handle the case when no other conditions evaluate to true.
8.           If no conditions evaluate to true, and no otherwise condition is specified, the activities in the group are not executed and processing continues with the first transition after the group.
 
Critical Section Groups
 
1.           Critical section groups are used to synchronize process instances so that only one process instance executes the grouped activities at any given time.
2.            Any concurrently running process instances that contain a corresponding critical section group wait until the process instance that is currently executing the critical section group completes.
3.           Critical Section groups are particularly useful for controlling concurrent access to shared variables.
Synchronization Options
Critical section groups can be used to synchronize all process instances for a particular process definition in a single process engine, or you can synchronize process instances for multiple process definitions, or you can synchronize process instances across multiple process engines.
 
Single Group
If you wish to synchronize process instances for a single process definition in a single process engine
 
·             Multiple Groups
If you wish to synchronize process instances for multiple process definitions, or if you wish to synchronize process instances across multiple process engines
 
Usage Guidelines
Because Critical Section groups cause many process instances to wait for one process instance to execute the activities in the group, there may be performance implications when using these groups.
• Keep the duration of a Critical Section group as short as possible. That is, put only a very few activities in a Critical Section group, and only use activities that execute very quickly.
• Avoid nesting Critical Section groups. If you must use nesting, ensure that Lock shared configuration resources are used in the same order in all process definitions. Deadlocks can occur if you do not specify the Lock resources in the same order in nested Critical Section groups for all process definitions.
• Do not include any activities that wait for incoming events or have long durations, such as Request/Reply activities, Wait For activities, Sleep activities, or activities that require a long time to execute.
• When using Critical Section groups to retrieve or assign a value to a shared variable across multiple process engines, both the Lock and the Shared Variable shared configuration resources should have the Multi-Engine field checked. You can use a global variable to ensure that the Multi-Engine field is set to the same value for both resources.
Pick First Groups
 
1.           Pick first groups allow process execution to wait for one or more events.
2.           The first event that completes determines which transition to take to continue processing.
3.           For example, as part of an order-entry system, when an order is placed, a check is made to see if the order can be filled from stocked inventory or from returned merchandise. Whichever system returns the information first is used to fill the order. If neither system returns the information about available inventory, the order times out and cancels.
4.           To specify the events that you would like to wait for, draw transition lines from the start of the group to the desired activities.
5.           Only request/reply, Wait for...activities, and activities that have the pause symbol can have valid transitions from the start of the Pick First group. If the transition is valid, the activity is highlighted in green. If the transition from the start of the group is drawn to an invalid activity, the activity is highlighted in red.
Overview of Loops
 
1.           Loops allow you to execute a series of activities more than once.
2.           You can iterate based on the items in an array stored in the process data, you can iterate while or until a given condition is true, or you can iterate if an error is encountered while processing.
 
The following are the types of loops that are available:
 
·             Iterate Loop
·             Repeat Until True Loop
·             While True Loop
·             Repeat On Error Until True Loop
 
1.           Iterate and repeat until true loops allow you to accumulate the output of a single activity in the loop for each execution of the loop.
2.           This allows you to retrieve output from each execution of the activity in the loop.
 
Index variable
The index variable holds the current number of times a loop has executed. The iteration count starts at one the first time the loop is executed, and the count increases by one for each iteration of the loop.
 
You can access this variable like any other process data by referencing it with a dollar sign ($) in front of it. For example, if the index variable is i, and you want to specify a condition that the loop should execute three times (for a repeat until true loop), the condition would be $i=3.
 
For nested loops, the index of the contained loop resets at the beginning of each iteration of the parent loop.
 
Accumulate Output
 
For iteration and repeat until true loops, you can accumulate the output of one of the activities in a group by checking the Accumulate Output field. If you check this field, you can select one of the activities in the group, and each time the loop is executed, the selected activity’s output is placed into a list.
The list of accumulated output for that activity is stored in a variable whose name is specified in the Output Name field. After the loop exits, this variable can be accessed in the same way other process data can be accessed by other activities.
You should design your group so that only one activity in the group holds the data to accumulate for each iteration.
For example, you may want to accumulate a list of customer names from repeated executions of a JDBC Database Query task, or you may wish to accumulate the sum of the amounts for line items in an order.



Variables
 
Global variables : Which will be configurable while deploying and the scope is for project, and can not be editable at run time.
 
Shared Variables : Which will be configurable while deploying and the scope is for project, and can be editable at run time, using Set Shared Variable and can be retrived using Get Shared Variable.
 
Process Variables : Scope is for Process, Will be used only in the process. you can use assign activity to set the value to this variables.  
 
Global Variables
 
Global variables provide an easy way to set defaults for use throughout your project.
When you want to use the global variable in the fields of a resource, enter the variable name surrounded by %% on both sides.
You can provide a filter in the Filter string/pattern field to limit the display
Global Variables Editor
 
Use the global variables editor to create or modify global variables, mark variables as settable from TIBCO Administrator, and assign a type to a variable.
Global variable groups are used for organizing variables. Variable groups are especially useful if multiple developers share a project using a version control system.
You must add at least one variable to a group, or the group will not be saved.
You can provide a filter in the Filter string/pattern field to limit the display.
You can change the value of a global variable when you deploy your project in TIBCO Administrator.
·         You can also specify values for global variables when starting a process engine on the command line, using -tibco.clientVar.<variablePathAndName> <value>
Process Variables
 
Process variables are data structures available to the activities in the process.
Process variables are displayed in the Process Data panel of each activity’s Input tab.
This allows you to use process data to supply input values for an activity.
Each process variable name starts with a dollar sign ($).
There are four types of process variables:
1.    Activity Output
 
o	Activities have access to any data that is output from previously executed activities in the process definition.
o	An activity’s output is placed into a process variable with the same name as the activity(Eg $varName)
2.    Predefined Process Variables
 
o	two process variables that are available to all activities that accept input:$_globalVariables and $_processContext.
o	$_globalVariables contains the list of global variables defined on the Global Variables tab of the project.
o	$_processContext contains general information about the process, such as the process ID, the project name, whether the process was restarted from a checkpoint, and so on.
o	Only global variables that have the Deployment option checked (on the advanced editor dialog) are visible in the $_globalVariables process variable. Also, only global variables with well-formed XML names (for example, names containing a % are not well-formed) appear in the $_globalVariables process variable. However, global variables of the type ’Password’ are not listed under the $_globalVariables process variable.
3.    Error Process Variables
 
o	When an error occurs in a process, the data pertaining to the error is placed into process variables.
o	The $_error process variable contains general error information.
o	Activities can also have error process variables named $_error_<activityName>. In the event of an error, the activity’s error variable is populated with the appropriate error schema.
4.    User-Defined Process Variables
 
o	You can define your own process variables and assign values to them in your process definition.
o	Process variables are defined on the Process Variables tab of the Process Definition resource.
o	You create a process variable in the same way you create data schemas for activities.
Memory Usage of Process Variables
 
Memory saving mode allows memory used by a process variable to be released when the value of the variable is no longer needed.
As each activity is executed, the list of process variables is evaluated to determine if subsequent activities in the process refer to the variable. If no activities refer to the variable, the memory used by the variable is released.
By default, memory saving mode is disabled, but you can enable memory saving mode for specific process instances by setting the EnableMemorySavingMode.<processname> property to true.
You can enable memory saving mode for all process instances by setting the EnableMemorySavingMode property to true.
Shared Variables
A shared variable is a shared configuration resource in the General Activities palette.
Shared Variable
 
A Shared Variable resource allows you to share data across process instances.
All process instances can read and update the data stored in a shared variable.
This type of shared variable is useful if you wish to pass data across process instances or if you wish to make a common set of information available to all process instances.
For example, you may have a set of approval codes for incoming orders that change daily for security purposes. You can create a shared variable to hold the approval codes and create one process definition for setting the codes. You can then retrieve the shared variable in all processes that require the current approval codes.
 
Job Shared Variable
 
A Job Shared Variable resource is similar to a Shared Variable, but its scope is limited to the current job. A copy of the variable is created for each new process instance.
New process instances receive a copy of the Job Shared Variable, so data cannot be shared across process instances.
Therefore, if a called process has the Spawnconfiguration field checked, a new process instance is created and the process instance receives a new copy of the Job Shared Variable.
This type of shared variable is useful for passing data to and from sub-processes without creating an input or output schema for the called process.
You can use the Get Shared Variable and Set Shared Variable activities to access the data instead of mapping data to a called processes input or output schemas.



Inter-Process Communication
 
TIBCO ActiveMatrix BusinessWorks allows two executing process instances to communicate. You may need process instances to communicate if you wish to synchronize process execution or if your processes must execute in a specific order.
TIBCO ActiveMatrix BusinessWorks provides the Wait and Notify activities and the Receive Notification process starter to handle inter-process communication.
These activities are similar to semaphores in programming. A process containing a Wait activity waits for another process to execute a corresponding Notify activity. Alternatively, the Receive Notification process starter creates a new process instance when another process executes the corresponding Notify activity.
The data sent between the activities is defined by a Notify Configuration shared configuration resource.
The Notify activity only sends information to the Receive Notification process starter or Wait activity that specifies the same Notify Configuration resource.
The schema supplied to the Notify Configuration resource can be empty, if you do not wish data to be sent between processes.However, the Notify Configuration resources must be the same for the Notify, Receive Notification, or Wait activities if they are to be used to communicate with each other.
 
In general, using inter-process communication consists of these steps:
1. Define the data that must be passed between the processes by creating a Notify Configuration shared configuration resource.
2. Determine the key that correlates processes with Notify activities with the corresponding processes with Receive Notification process starters or Wait activities.
3. Create process definitions that use the Receive Notification, Wait, and Notify activities. These activities are located in the General Activities palette.
4. If your process engines are on different machines, a database must be used to store process instance information. Wait/Notify information is stored in a database so that process engines on different machines can share information.
 
Checkpoint concepts and issues
The following describes the procedure for duplicate detection by the process engine:
1. An incoming message is received and a process instance is created.
2. Activities in the process instance are executed until the first Checkpoint activity is
reached. The Checkpoint activity has a value specified for the duplicateKey input
element.
3. The process engine checks the current list of duplicateKey values for a matching value.
a. If no process instance has stored the given duplicateKey value, the process engine
stores the value and completes the Checkpoint activity.
b. If another process instance has already stored the given duplicateKey value, the
process engine terminates the process and throws a DuplicateException.
4. Once a process engine stores a duplicateKey value and performs the Checkpoint for a
process instance, no other Checkpoint activities in the process instance can be used to
store a different duplicateKey value.
 
Using the algorithm described above, process engines can guarantee that no newly created
or recovered process instances will execute if they have the same duplicateKey value.
Therefore, you should take care in choosing the value of duplicateKey and ensure that it will
be unique across all process instances.
 
Duplicate detection can only be done across multiple engines on different machines if a database is used to store process engine data.
If you are running fault tolerant process engines (that is, only one process engine is running at a particular time), or if all process engines run on the same machine, you can use a file system for process engine storage.
When a duplicate is detected, the Checkpoint activity fails with the DuplicateException. You can place an error transition from the Checkpoint activity to a series of activities to handle the duplicate message. If no error transition is specified, the process instance terminates and duplicate messages are effectively ignored.
 
The following process engine properties control duplicate key detection.
 
bw.engine.dupKey.enabled — specifies whether duplicate detection is performed. true (the default) indicates the process engine will check for identical duplicateKey values. false indicates duplicateKeys when specified are ignored.
bw.engine.dupKey.timeout.minutes — specifies how long (in minutes) to keep stored duplicateKeys. The default is 30 minutes.0 indicates the duplicateKey is removed when the job is removed. However, if bw.engine.enableJobRecovery=true, the job is not automatically removed after a failure so the duplicateKey will remain as long as the job remains. Such a job can be restarted or purged later.-1 indicates to store duplicateKey values indefinitely. Any positive integer greater than 0 indicates the number of minutes to keep stored duplicateKeys.
bw.engine.dupKey.pollPeriod.minutes — specifies the number of minutes to wait before polling for expired duplicateKey values.



Detecting Duplicate Process Instances
 
Duplicate messages should be detected and discarded to avoid processing the same event more than once.
Duplicate detection is performed when a process instance executes its first Checkpoint activity.
You must specify a value for the duplicateKey element in the Checkpoint activity input schema.
This value should be some unique key contained in the event data that starts the process.For example, the orderID value is unique for all new orders.
Duplicate detection can only be done across multiple engines on different machines if a database is used to store process engine data.
If you are running fault tolerant process engines (that is, only one process engine is running at a particular time), or if all process engines run on the same machine, you can use a file system for process engine storage.
When detecting duplicate messages, it is important to place the Checkpoint activity before any activities that you do not want to execute more than one
The checkpoint performed for the transaction may be the first checkpoint in the process definition. If this is the case, you can specify the duplicateKey as part of the transaction group configuration. The duplicateKey is specified in the Checkpoint Duplicate Detection Key field of the transaction group.



1. Use of check point.
Primary use of checkpoint is to save the state of the process. State of the process includes the values within process variables, shared variables and outputs of various activities executed before the checkpoint.
Checkpoint can also be used for duplicate message detection.
2. How to configure check point. If it is stored in file the location of the file where i can get this.
You simply need to place the checkpoint activity after all the activities you consider need to be checkpointed.
Location:
When file based repository is used, they are stored at the following location as .job files.
When running in Designer:
C:\Documents and Settings\\.TIBCO\working\\jdb_dupkeys
When Deployed:
C:\tibco\tra\domain\dvl\application\\working\- Process_Archive\jdb_dupkeys
3. What it writes in file or stores in db.Is it readable?
The content in which they are stored is XML. More analysis is needed to answer this.
4. Can we retrieve the informations from check point, i mean state of the process?
As per a post on TIBCOmmunity, these are for internal use by TIBCO. We cannot manually make use of these.
5. How to make a process incomplete in run time.......to store the state in check point. Custom/manually to make process incomplete in run time.
You cannot do this unless a checkpoint is already implemented in the process. Answer to question 4 again answers this question.
I tried to simulate a CheckPoint scenario implementation using TIBCO Designer. I've implemented a process that received an RV request, saves the request in a Process variable using Assign, Check Points the process state and then I suspend the process using the Engine Activity. By doing this, I'm able to see the .job file in the above said location. Later, I resume the suspended process using another process within the same project using the Engine Activity by specifying the job id.
Now, I have a these questions:
1. I have many suspended processes now. When I stop the test engine, to simulate a BW engine crash and start the Test Engine again, all the .job files get deleted when the test engine is stopped. Is there any way to simulate an Engine Crash in Designer?
2. When I deploy the above project in Administrator by creating an EAR, I can resume the suspended processes using Administrator GUI or Hawk MicroAgents (HMA). Now, when there are a few suspended processes, and when I restart the instance, the checkpointed processes are executed. How am I not able to do this in Designer when stopping a test engine is same as stoping the instance in Administrator? Are there any properties that are set? If yes, which? Could someone please put some light on this?
3. An ExceptionHandler component is used in our project, which retries the request to a service the specified number of times and sleeps (temporarily suspends the process) using the Sleep activity in between. When the no. of retries have reached the specified no. of limit, it depends upon the support personnel to retry, abandon or restart the request. My question is, in order to utilize the CheckPointed processes in Exception Handling mechanism, do we need to perform some kind of suspend operation like Suspend using the Engine Activity or Sleep Activty on the process?
 
Checkpoint and Confirm Activity
 
Sequencing Process Instances
 
BusinessWorks allows you to specify a sequencing key on process starters that determines which process instances to execute sequentially.
Process instances with sequencing keys that evaluate to the same value are executed in the order they were started.
Process instances can have sequencing keys that evaluate to different values, but only process instances with the same value for the sequencing key are executed sequentially. Process instances with different sequencing key values can be executed concurrently.
TIBCO Administrator allows you to control the maximum number of process instances in memory as well as the maximum number of concurrently executing process instances.
Using these settings, you can specify that all process instances should be executed sequentially in the order they were created. This method is not as flexible as using the Sequencing Key field on a process starter. Using the administrator settings is only recommended when you cannot change the process definitions in the project before deployment (for example, if you purchase a pre-built project from a third party).
 
Error Handling
 
$_error Process Variable
 
The $_error process variable contains general information about the process in which the error occurred.
The contents of each schema item are dependent upon the activity that throws the error. The Data schema item contains activity-specific error information.
You can use a coercion to change the Data element into the specific type for the activity.
When you create an error-handling procedure, you may find the data in the $_error process variable useful. You can map data from this process variable into Input items for activities in your error-handling procedure.
_error_ Process Variables
 
Each activity has one or more predefined error schemas.
The error schemas are displayed on the activity’s Error Output tab.
Typically, there are two or three types of exceptions an activity can encounter, and these are represented as a choice element in the error output schema.
 
 
 
 
Error Propagation
 
Groups and called processes define the scope of an exception. An exception that occurs within a group or a called process causes processing to halt.
Any unhandled exceptions are propagatged to the next highest exception scope.
Unhandled errors occur where there is no error transition or Catch activity that specifies the activities to execute in the case of an error.
Also, you can use the Generate Error activity to create an unhandled error. The Generate Error activity does not permit any transitions to another activity, so any error created by the Generate Error activity is propagated to the next highest exception scope.
 
Transitions cannot be drawn to a Catch activity. Also, you cannot draw transitions
from any activity on the path of the Catch activity to any activity on the regular
execution path. Activities on a Catch path can, however, have transitions to other
activities on other Catch paths.
 
The Rethrow activity allows you to throw the exception currently being handled
by a Catch path. This is useful if you wish to perform some error processing, but
then propagate the error up to the next level.






Service Pallete
Service resource separates the service definition from the transport and the implementation of operations, your implementations may require context information from the underlying request on the specific transport. p233




Schema Import Tool
The schema import tool allows you to import resources into a TIBCO ActiveMatrix BusinessWorks project.
 
The tool can also resolve references to other resources within the imported resources. The tool can be used to import the following types of resources:
 
• XSD — XML schema definition language for declaring schema components like elements, attributes, and notations.
• WSDL — Web Services Definition Language for declaring the interface definition of a web services. WSDL files typically include XSD definitions as well.
 
The tool helps to avoid such problems as broken references.
 
The Schema Import tool stores imported resources in a Location resource in the project.This resource is a temporary holding area for XSDs and WSDL files while importing and resolving references in the resources.
 
The Location resource corresponds to one XSD or WSDL resource. The Location resource holds the specified resource and any resources referenced by the XSD or WSDL file. You must create a new Location resource for each XSD or WSDL resource you wish to import.
 
The imported resources remain in the Location resource so that you can resolve any conflicts or errors in the imported resource. When all conflicts and errors have been resolved, the XSD or WSDL resources can be imported into the project. Once resources have been imported into the project, the Location object stores a pointer to where the imported resources were originally located. You can remove Location resources once the desired resources have been imported into the project.
 
Working With Secure Sockets Layer (SSL)
 
TIBCO ActiveMatrix BusinessWorks can use Secure Sockets Layer (SSL) to provide secure communication. The successor to SSL is Transport Layer Security (TLS), but the term SSL is used synonymously with TLS in this document.
 
TIBCO ActiveMatrix BusinessWorks can act as an initiator or a responder in an
SSL connection. Several types of connections can optionally use SSL, such as:
• FTP Connection
• HTTP Connection
• JMS Connection
• Rendezvous Transport
 
In addition, the following activities can also specify SSL connections:
• ActiveEnterprise Adapter activities using JMS or RV transports
• Send HTTP Request
• SOAP Request Reply
 
TRA- TIBCO Runtime Agent



TIBCO products use a three-digit release number which makes it possible to specify major, minor, and service pack releases. For example, release 2.1.3 uses major version 2, minor version1, and service pack 3.
 
If you are installing using a service pack release (for example, 5.6.1 over 5.6.0), the installer will silently overwrite the existing version of the software. You do not need to uninstall the existing installation. Simply stop all running TIBCO software before installing.
• If you are installing a major or minor release, the installer will create a new directory in the old installation directory that is named after the two-digit release number. For example, if your prior version was installed in c:\tibco\tra\5.6, and you install version 5.7, the product will be installed in
c:\tibco\tra\5.7. The old installation is not removed, and must be uninstalled separately if you wish to remove it.
 
If you install a product and that product is already installed on your machine, you cannot choose a different location from that specified above. If you wish to install the product in a different location, you must completely remove the product from the machine.
 
The TIBCO Runtime Agent has two main functions:
• Supplies an agent that is running in the background on each machine.
— The agent is responsible for starting and stopping processes that run on a machine according to deployment information.
— The agent monitors the machine. That information is then visible via TIBCO Administrator Enterprise Edition.
• Supplies the run-time environment, that is, all shared libraries including third-party libraries used by TIBCO products. The run-time environment includes the following:
— TIBCO Designer (see page 19)
— Java Runtime Environment (see page 21)
— TIBCO Hawk agent (see page 22)
— TIBCO Rendezvous (see page 23)
— Third-party libraries (see page 25)
— TIBCO runtime libraries















Cluster
 
A cluster is a group of machines that work together as a single system to ensure that applications and resources are available at all times. The machines are managed as a single system using cluster software, which provides a way to support fault-tolerance, high-availability, scalability, and so on.
 
The computers are physically connected by cables and programmatically connected by cluster software. These connections allow computers to use failover and load balancing, which is not possible with a stand-alone computer.
 
The next diagram shows a failover cluster solution. The nodes in a cluster are configured with a shared drive. The cluster software manages access to the shared drive so that only applications on a single machine use a particular resource at anytime.
 
The failover cluster solution primarily provides uninterrupted service in the event of a failure within the cluster. This solution is commonly used for database applications.
 
Applications that are deployed to a failover cluster can be managed under cluster software. Applications are under the control of cluster software, which ensures that only one application runs at once on one of the cluster machines. This kind of application is in active-passive mode.
 
A cluster environment supports two types of applications: cluster-aware applications and cluster-unaware applications.
 
TIBCO applications are cluster-unaware. The applications can be run seamlessly in a cluster environment, but do not use any cluster features.
 
Cluster-aware applications run in the cluster and are aware if the cluster itself is running, or whether a resource needed by the application is available. A cluster-aware application is supported by a resource type that is assigned by the cluster software.
Cluster-unaware applications know nothing about the cluster environment and consequently cannot exchange data with cluster objects or be aware that they are running under cluster software control. Cluster software manages cluster-unaware applications by using basic methods for failure detection and application shutdown. For example, the Microsoft Cluster Server solution manages cluster-unaware software as a generic application resource type.










HTTP Connection
 
There can be at most one process with an HTTP Receiver or Wait for HTTP Request that uses the same HTTP Connection resource. This restriction allows the HTTP server listening for incoming requests to dispatch the request to the correct process.
There can be more than one SOAP Event Source that uses the same HTTP Connection.
Also, SOAP Event Source and HTTP activities can use the same HTTP Connection resource and the HTTP server correctly dispatches the incoming request to the correct process
Two types of servers are available for the HTTP Connection resource: Tomcat and HTTPComponent.
The maxprocessor value should be almost equal to the number of concurrent requests the server can handle. Note that increasing the maxprocessor count also increases the memory footprint.
You can choose the server type at runtime by setting the global property bw.plugin.http.server.serverType in the file bwengine.tra. , which overrides the value that is set in design-time.
If you have multiple HTTP Connection resources specified by multiple HTTP Receiver process starters, the HTTP servers require that all of the connections must be valid to initialize all HTTP Receivers, before testing or deploying the project.
bw.plugin.http.server.minProcessors
bw.plugin.http.server.minProcessors
bw.plugin.http.server.acceptCount
The HTTP Receiver uses the minProcessors/maxProcessors properties to control the flow of incoming HTTP requests. If you set the Flow Limit deployment property for a process definition with the HTTP Receiver process starter, maxProcessors is set to  - 1 and minProcessors is set to /2. Therefore, the Flow Limit value will never be reached because the maxProcessors property will prevent new requests from being accepted before the Flow Limit value is reached.



HTTP Connections Funda:
 
For all the processes that having Http Receiver activity requires a separate Http Connection is required, If you use a same connection for 2 processes it will give an error on starting that for "Http Connection:host:port" there are 2 event sources.
 
Same for Wait for Http request, for that also you need to have a separate connection, If you use a single HTTP connection in a process having HTTP receiver and a process having Wait for HTTP Request, it will throw the same error message.





















TIBCO Designer projects store trusted certificates in PEM storage format.




AliasLibrary and LibraryBuilder
 
Alias are used in two resources, the AliasLibrary and the LibraryBuilder.
 
The AliasLibrary resource allows you to load files stored in the file system into your project.
The LibraryBuilder resource allows you to build a design-time library that includes resources defined in one project that can be shared with other projects.
When you build an enterprise archive file, the files referenced by the aliases defined in an AliasLibrary that you include in your project are included in your archive file.
The AliasLibrary resource allows you to specify aliases to file system resources (such as a jar file) that need to be included in your project.
To use a file system resource, a project needs to know where to find it. Since projects are exported or deployed to different machines and different environments, Designer uses aliases to specify file locations.
Before including a file, an alias is created that specifies the file’s location. An alias is part of your preferences and is common to all of your projects. Aliases are created and managed under the File Alias tab in the Preferences dialog.
 
The LibraryBuilder resource allows you to share resources you have defined in a project with other project developers. This allows you to create shareable resources once, then allow other project developers to use them in their projects.
LibraryBuilder resources are used as part of design-time libraries.




AppManage
 
The main advantage is not having to manually specify the different configuration options (Gloabal Variables, threads, etc) in Administrator for each domain.
 
With scripted deployments, you can store your configuration is source control and generate AppManage configuration files for each domain. No more "oops!It's not working because we mistyped the database password".
Other benefits are repeatability, consitency and speed. With a script that is parameterised on the domain, you know that the deployment to production will be exactly the same as how it was deployed in your test environements.
Also, scripted deployment can be much faster than manual ones, especially when you have a large number or ears to deploy (and use AppManage in batch mode), with a large number of settings that need to be changed in each environment.
 
The advantage is in the global variable configuration, and engine settings (heap size, max jobs/flow limit)
Think about an ear that have more and more variable to configure, and more and more env where run the ears.
With appmanage you can specify an application xml file where you configure all GV's and parameters like heap size, max jobs/flow limit and so on.
If you use tibco admin, every time you have to specify gv's env related, memory settings and all the stuffs that depend on env used (dev, test, prod).













TRA stands for TIBCO Runtime Agent.
The TRA has two main functions:
 
Supplies an agent that is running in the background on each machine. 	
1.   	The agent is responsible for starting and stopping processes that run on a machine according to the deployment information.
2.   	The agent monitors the machine. That information is then visible via TIBCO Administrator.
Supplies the run-time environment, that is, all shared libraries including third-party libraries.
 
Software that comes with TRA
 
TIBCO Designer
TIBCO Runtime Agent (TRA) - Includes Domain Utility, TIBCO Wrapper, etc
Third Party Libraries
JRE
TIBCO Rendezvous
TIBCO Hawk
 
TIBCO uses a 3-digit release number
   major.minor.servicePack
- When installing a Service Pack, it will overwrite the current installation
- installing a new major or minor version, it will create a new folder
- If there is a product already installed in the machine, installation path cannot be changed until it gets uninstalled.
 
Installation registry is named Vital Product Database
- Installation registry gets stored in vpd files (Vital Product Database)
- VPD files are updated after a new product is installed
- VPD files are checked by applications
- VPD file location
   Windows
       %SystemRoot%/vpd.properties
       %SystemRoot%/vpd.properties.tibco.systemName
   UNIX (non-root user)
       User_Home_Directory/vpd.properties
       User_Home_Directory/vpd.properties.tibco.systemName
   UNIX (root user)
 
- TIBCO Runtime Agent depends on TIBCO Rendezvous and TIBCO Hawk
- Version and location information is contained in many files, typically *.tra files
 
- Installation modes
 
GUI
1.   	TIB_tra-suite_version_win_x86_32.exe
Console
1.   	TIB_tra-suite_version_win_x86_32.exe -s -a -is:javaconsole -console
Silent (No prompting information)
1.   	silent argument uses default values
1.   	TIB_tra-suite_version_win_x86_32.exe -s -a -is:silent -silent    (install silent mode)
2.   	TIB_tra-suite_version_win_x86_32.exe -s -a -is:silent -silent -options <> (install in silent mode with response file)
1.   	options responseFile argument uses values specified when response file was generated
TIB_tra-suite_version_win_x86_32.exe -s -a -is:javaconsole -console -options-record <> (create responseFile)
TIB_tra-suite_version_win_x86_32.exe -s -a -is:javaconsole -console -options <>     	(install with responseFile)
 
If a database is used as administration domain repository, Domain Utility must be used in order to delete the domain before uninstalling TIBCO Runtime Agent in order to drop database tables
- Products that depend on TIBCO TRA should be uninstalled first
- Products uninstallation (after uninstalling all products that depend on TIBCO Runtime Agent)
   Using TIBCO Installation Manager
       TIBCO Runtime Agent
       TIBCO Designer
       Java Runtime Environment
       Third Party Core Libraries
   Using Add/Remove Programs utility
TIBCO Hawk. Before uninstalling, stop TIBCO Hawk HMA Service in Services dialog        TIBCO Rendezvous. Before uninstalling, kill rvd daemon from Task Manager
 
BW Palette
*******************************************************************************************************
 
The Assign activity allows you to assign a value to a user-defined process variable.
When the Assign activity is executed, the entire schema for the selected process variable is replaced with the specified values.
Elements that do not have a value specified in the Input tab are set to null. Therefore, be certain to set all necessary values when using the Assign activity to set a process variable.
*******************************************************************************************************
 
The Call Process activity calls and executes an existing process definition.
The input to the called process is defined in the Start activity of the called process. The output of the called process is defined in the End activity of the called process.
You should only call processes that have Start activities. Do not call processes that begin with a process starter such as HTTP Receiver or Receive Mail. Providing the name of a process with a process starter results in an exception at run time.
Process Name Dynamic Override An XPath formula specifying the name of the process to call. Use this field to dynamically determine which process to call when the process instance is running.
If you use the Process Name Dynamic Override field, make sure you include all potentially callable subprocesses when you create your Process Archive for deployment manually.
Spawn Specifies whether to spawn a new machine process for executing the called process. If this option is checked, the parent process cannot access the called process’ output. The called process is executed in a separate process instance.
*******************************************************************************************************
 
The Catch activity receives control of execution when an unhandled exception occurs.
You can select a specific exception type to catch or you can specify that this activity should catch all unhandled exceptions.
You can have more than one Catch activity in each exception scope, but each Catch activity must have a unique exception type.
The Catch activity allows you to transition to activities you wish to perform to handle the exception. Transitions are permitted between Catch tracks within an exception scope, but you can not transition back to the main execution track from the Catch track.
If you wish to propagate the caught exception to the next highest scope, use the Rethrow activity.
*******************************************************************************************************
 
The Checkpoint activity performs a checkpoint in a running process instance.
A checkpoint saves the current process data and state so that it can be recovered at a later time in the event of a failure.
If a process engine fails, all process instances can be recovered and resume execution at the location of their last checkpoint in the process definition.
If a process instance fails due to an unhandled exception or manual termination, it can optionally be recovered at a later time, if the process engine is configured to save checkpoint data for failed processes.
Only the most recent state is saved by a checkpoint. If you have multiple checkpoints in a process, only the state from the last checkpoint is available for recovering the process.
A Checkpoint activity cannot be placed in or in parallel to a transaction.
You can, however, specify that an implicit checkpoint should be taken as part of a transaction by checking the Include Checkpoint field on a transaction group. For explicit Checkpoints, place the checkpoint activity outside of any transaction group. Also, make sure that if you have multiple paths in your process definition, the Checkpoint activity does not occur in parallel with a path that has a transaction group.
Instead, any Checkpoint activities should be placed at points that are guaranteed to be reached before or after the transaction group is reached.
Checkpoints save the state of the entire process instance. By default when a process calls another process, the subprocess is executed in the same process instance as the calling process.
If the called process spawns a new machine process, however, the called process is a new process instance. When a checkpoint occurs in a called process, the checkpoint saves the state of the current process instance.
If no called processes spawn new process instances, then a checkpoint in any called process saves the state of the process instance, including state from the parent process(es) of the current process.
In the case of a called process that spawns a new process instance, only the spawned process instance is saved.
You must be careful with certain types of process starters or incoming events when placing your checkpoint in a process definition.
You can specify to shut down the process engine if any errors are encountered during startup so that checkpointed jobs are not lost in the event of an error. The custom engine property named Engine.ShutdownOnStartupError controls this behavior. Default value is false.
If the checkpoint is taken before the Confirm activity, then a crash occurs after a checkpoint but before a confirm, the original message is resent. In this case, the restarted process can no longer send the confirmation. However, a new process is started to handle the resent message, and you can implement your process to handle the restarted and new processes appropriately.
If the checkpoint is taken after a Confirm activity, there is potential for a crash to occur after the Confirm but before the checkpoint. In this case, the message is confirmed and therefore not redelivered. The process instance is not restarted, because the crash occurred before the checkpoint.
You must consider the type of processing your process definition performs to determine when a checkpoint is appropriate if your process definition receives confirmable messages.
 
*******************************************************************************************************
 
The Confirm activity confirms any confirmable messages received by the process instance. For example, if a process is started because of the receipt of an RVCM message, the Confirm activity can send a confirmation message to the publisher of the RVCM message.
It is not recommended to use the Confirm Activity with in the Service Resource process as this will unblock the TIBCO ActiveMatrix provider thread. The new message will still flow on that thread eventhough the TIBCO ActiveMatrix BusinessWorks process execution continues for the previous message.
In case of the Service Resource operation process, perform Auto-confirm in case of clientAck after the end Activity which would still work even if you are not using Confirm Activity in the Operation Process.




*******************************************************************************************************
The Engine Command activity allows you to retrieve statistics and information about process definitions, process instances, and activities in the currently running process engine.
This activity also lets you perform engine maintenance, such as suspending and resuming process instances and shutting down the engine.
 
*******************************************************************************************************
 
Generate Error activity generates an error and causes an immediate transition to any error transitions.
If there are no error transitions, the process instance halts execution.
This activity is useful in a group or in a called process. If you would like to catch and raise your own error conditions, you can use this activity to do so.
 
*******************************************************************************************************
 
The OnError activity provides error handling mechanism for all the errors that happen outside the job boundaries of all processes associated with the service resource.
You are recommended to create only one process definition with this event source for the whole project. If you create multiple processes, then all of them will be triggered and may not be in sequence.
Whenever an error occurs for messages triggered through Service Resource outside of Job boundaries, the Error handler process gets triggered with the appropriate data.
This error handler process can be used only for Logging purposes. The actual Service MEP will not be affected.
The Error handler handles the failure in Pre-process, Post-process (failure while sending the response), and Fault Sending scenarios. It does not catch the error during the execution process.
 
*******************************************************************************************************
 
The Rethrow activity throws the exception caught by the Catch activity again.
Use this activity when you wish to propagate the exception to the next level.
 
*******************************************************************************************************
 
A Shared Variable resource allows you to share data across process instances. All process instances can read and update the data stored in a shared variable.
This resource is useful if you wish to marshal data across process instances or if you wish to make a common set of information available to all process instances.
Multi-Engine Checking this field specifies that you wish to make the value of a Shared Variable resource available to process instances across multiple process engines.
If you choose this option, a database must be used to store process engine data. Also, only engines that are in the same deployment and use the same database to store process information can share variables.
 
The Get Shared Variable activity retrieves the current value of a Shared Variable or Job Shared Variable resource.
If you are using this activity to retrieve the value of a Shared Variable resource, you may want to use a critical section group to ensure that no other process instances are altering the value of the shared variable at the same time.
 
The Set Shared Variable activity allows you to change the value of a shared variable. If you are using this activity to set the value of a Shared Variable resource, you may want to use a critical section group to ensure that no other process instances are altering the value of the shared variable at the same time.
 
*******************************************************************************************************
 
The JNDI Configuration shared configuration resource provides a way to specify JNDI connection information that can be shared by other resources. This resource can be specified in any resource that permits JNDI connections.
For example, JDBC Connection and JMS Connection can use JNDI connections.
 
*******************************************************************************************************
 
The Notify Configuration resource specifies a schema to use for passing data between executing process instances. Corresponding Receive Notification, Notify, and Wait activities use the same Notify Configuration resource to define the data for inter-process communication.
 
The Notify activity allows a process instance to send data to a corresponding process instance containing a Wait activity or Receive Notification process starter.
The Notify Configuration resource specified on the Configuration tab and the key specified on the Input tab create the relationship between the Notify activity and the corresponding Wait or Receive Notification.
The data specified in the Notify’s input is sent to the Wait or Receive Notification that specifies the same Notify Configuration resource and has the same value for the key.
The Wait, Receive Notification, and Notify activities allow running process instances to communicate. inter-process communication.
 
The Wait activity suspends execution of the process instance and waits for a Notify activity with a matching key to be executed in another process instance.
The key specified in the Notify Configuration resource specified on the Configuration tab and the Key field of the Input tab creates a relationship between the Wait activity and the corresponding Notify activity.
The same Notify Configuration shared configuration resource must be specified by corresponding Wait and Notify activities so that data can be passed from the process instance containing the Notify activity to this process instance. The schema in the Notify Configuration resource can be empty, if you do not wish to pass data between processes.
 
The Receive Notification process starter starts a process when another process executes a Notify activity with a matching key and Notify Configuration resource.
The key specified in the Key field of the Configuration tab creates a relationship between the Receive Notification process starter and the corresponding Notify activity.



*******************************************************************************************************
 
The On Event Timeout process starter specifies a process to execute when a Wait For ... activity discards an incoming event due to a timeout. A Wait For ... activity’s event timeout is specified by the Event Timeout field on the Event tab of the activity.
The default behavior for an event timeout is to confirm and then discard the event. You can override the default behavior by creating a process definition using the On Event Timeout process starter.
You can specify an On Event Timeout process definition for a specific event source (that is, a specific Wait For... activity). You can specify one or more On Event Timeout process definitions for specific event sources, but you should not create more than one process definition for the same event source. Once a Wait For...activity experiences a timeout, that timeout can only apply to one On Event Timeout process definition.
You can also specify that an On Event Timeout process definition applies to any event source that experiences a timeout. If there is no On Event Timeout specified for a particular event source, the process engine calls the On Event Timeout process definition whose Any Event Source field is checked.
 
The On Notification Timeout process starter specifies a process to execute when a timeout is reached for storing notification data for a Notify activity.
 
The On Shutdown process starter specifies a process to execute when the process engine shuts down, after all process instances are executed.
 
The On Startup process starter specifies a process to execute when the process engine starts, before any incoming events are processed. In the event that the process engine is restarting and attempting to recover checkpointed process instances, the On Startup process definition must complete its execution before any recovered process instances execute.



*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
 
The HTTP Connection resource describes the characteristics of the connection used to receive incoming HTTP requests. This resource is used when TIBCO ActiveMatrix BusinessWorks expects to receive an HTTP request on a specific port where a process engine is running.
 
There can be at most one process with an HTTP Receiver or Wait for HTTP Request that uses the same HTTP Connection resource.
This restriction allows the HTTP server listening for incoming requests to dispatch the request to the correct process.
There can be more than one SOAP Event Source that uses the same HTTP Connection.
Also, SOAP Event Source and HTTP activities can use the same HTTP Connection resource and the HTTP server correctly dispatches the incoming request to the correct process.
 
Two types of servers are available for the HTTP Connection resource:
Tomcat and HTTPComponent.
 
Tomcat has a synchronous request response paradigm and can be used in scenarios where high throughput is important. To achieve a good throughput with Tomcat, the maxprocessor value should be almost equal to the number of concurrent requests the server can handle. Note that increasing the maxprocessor count also increases the memory footprint.
 
HTTPComponent is a light-weight and scalable server based on NIO which can be useful in scenarios where handling thousands of requests in a resource efficient manner is more important than the throughput. HTTPComponent server gives a consistent throughput for any number of concurrent requests with little or no increase in its worker thread (maxprocessor thread).
 
*** You can choose the server type at runtime by setting the global property bw.plugin.http.server.serverType in the file bwengine.tra
 
The HTTP Receiver uses the minProcessors/maxProcessors properties to control the flow of incoming HTTP requests. If you set the Flow Limit deployment property for a process definition with the HTTP Receiver process starter, maxProcessors is set to  - 1 and minProcessors is set to /2. Therefore, the Flow Limit value will never be reached because the maxProcessors property will prevent new requests from being accepted before the Flow Limit value is reached.



HTTP Receiver Starts a process based on the receipt of a HTTP request.





Accessing TIBCO ActiveMatrix BusinessWorks Global Variables and Java System Properties
 
You can use the com.tibco.pe.plugin.PluginProperties.getProperty() method to retrieve any Java system property or global variables defined in your project. The com.tibco.pe.plugin.PluginProperties  class is contained in the lib/engine.jar file
 
String var1 = com.tibco.pe.plugin.PluginProperties.getProperty( "tibco.clientVar.myVar");
 
String var1= com.tibco.pe.plugin.PluginProperties.getProperty("tibco.clientVar.myGroup/item1");



Code in the Java Code or Java Method activities can send messages to the log4j file by using the bw.logger class. For example, the following code obtains the bw.logger class and uses the class to send a message to the log file.
 
org.apache.log4j.Logger logger = org.apache.log4j.Logger.getLogger("bw.logger");
logger.warn("This is a warning message from the either a Java Code or Java Method activity");



Java Code
You can add custom code to your process definition with the Java Code activity. This activity allows you to write standard Java code that can manipulate any of the process data or perform any action you choose.
The Java Code activity automatically creates an invoke() method in which you place the code you wish to execute. This method is called when the engine processes the Java Code activity.
When you specify input and output parameters for the Java Code activity, get/set method code is automatically generated for the activity. You can use the get/set methods in your Java code, and you can display the code for the get/set methods when you select the Full Class radio button on the Code tab.
 
You may create instances of Java objects in your Java code or by using the Java Method or XML To Java activities. You can pass these Java objects using an output parameter to another activity later in the process definition. The Java Code activity receiving a Java object accepts the object into an input parameter and you must map the output Java object to the input object of the receiving Java Code object.
Any Java objects passed by input and output parameters between activities must be serializable.









The Java Custom Function resource allows you to create custom functions to use when mapping data in an activity’s input tab. These functions are also displayed when using the XPath Editor to build an XPath expression.
 
Only methods declared as public and static are loaded.
The input parameters and return values must be of one of the types described in Table 3.
The return value of the function cannot be void.
The method cannot be a constructor.
The method cannot explicitly throw an exception. Runtime exceptions are allowed, however.
Method names cannot be overloaded in a class or any imported classes in a single Java Custom Function resource. You can load methods of the same
name into separate classes in separate Java Custom Function resources and use the Prefix field to differentiate between the methods.
If you make references to any imported class files, these classes must be available in the classpath configured for TIBCO ActiveMatrix BusinessWorks.
The easiest way to make the imported classes available is to place them in the TIBCO/bw/2.0/lib directory.
Inner classes are not supported.



The Java Event Source allows you to create a custom process starter written in Java.
For example, you may wish to start a TIBCO ActiveMatrix BusinessWorks process when an application inserts a row into a database table.
Your custom process starter would observe the database for insert events, then call the onEvent() method with the desired data as input when an insert occurs.
The Java Event Source process starter creates a process when the onEvent() method is invoked and the object passed to the method is passed to the process definition.
This process starter uses an abstract class to define the interface. You can either write and compile your custom Java code in your own code editor and upload the class to TIBCO ActiveMatrix BusinessWorks or Edit in Code Dialog
 
The JavaProcessStarter abstract class defines the interface of your Java Process
Starter with the TIBCO ActiveMatrix BusinessWorks process engine. You must
define an implementation for the following methods:
• init() — this method is called when the process engine starts up. This method should initialize any resource connections. You could also specify a Java Global Instance on the Advanced tab that initializes resource connections. Java Global Instances are also loaded and initalized during
process engine start up. You can call this.getJavaGlobalInstance() to obtain the Java Global Instance resource in your process starter code.
• onStart() — this method is called by the process engine to activate the process starter. This method should activate any event notifier or resource observer code. The notifier or observer code can then call the onEvent() method to start a process instance.
• onStop() — this method is called by the process engine to deactivate the process starter. This method should deactivate any event notifier or resource observer code.
• onShutdown() — this method is called by the process engine when the engine shuts down. This method should release any resources and resource connections and perform any required clean up operations.
 
The following methods are already implemented and can be used in your code:
 
onEvent(Object object) — this method is called when a listener or resource observer catches a new event. The input to this method is a Java object containing the event data.
getGlobalInstance() — this method returns an object reference to the Java Global Resource specified on the Advanced tab of the process starter. This is useful if you wish to place initialization code or other shared information in a Java Global Resource instead of in the init() method of this class.
onError() — this method throws the exception specified in the input parameter. Use this method to propagate an error to the TIBCO ActiveMatrix BusinessWorks process instance when a listener or resource observer fails to generate an event.
 
The Java Global Instance shared configuration resource allows you to specify a Java object that can be shared across all process instances in a Java Virtual Machine (JVM).
When the process engine is started, an instance of the specified Java class is constructed. When the process engine is shut down, if specified, a cleanup method is invoked on the object and the object is released before the engine shuts down.
Any Java Method activity can be configured to access the shared Java Global Instance when the process engine runs. Any Java Code activity can access the shared Java Global Instance by invoking the static methods of the configured Java class.
If multiple process instances access the shared Java Global Instance, you may wish to ensure that only one process instance can access the object at a time. You can accomplish this by either declaring the methods of the configured class as synchronous or by using a critical section group.






The Java Method activity allows you to invoke a method contained in a Java class. You can construct an instance of the specified Java class, if you choose to invoke the constructor for the class.
The Java class file must be located in the classpath for TIBCO Designer and the TIBCO ActiveMatrix BusinessWorks process engine. Update the designer.tra and bwengine.tra file to contain the directory where your Java class files are located.
If you choose to cache the constructed instance of the class, the same Java object is shared by Java Method activities that invoke methods on the class. Therefore, be aware of potential concurrency issues, if your class or its methods are not threadsafe.
 
The Java Schema shared configuration resource allows you to specify a Java class that is used to configure a Java To XML or XML To Java activity.





The Java To XML activity allows you to convert a Java object’s data members into an XML document.
If the class does not have a public data member and only has a Java bean modifier that sets the data, the input schema contains an element for the modifier, but the resulting XML document has no value set for the corresponding element.
 
The XML to Java activity allows you to create an instance of a Java object based on data from an XML document. The XML schema for providing input to the Java object is created from the Java object or Java Schema specified on the Configuration tab of this activity. The
 
The specified Java class must meet the following requirements:
• The Java class must have a public default constructor (that is, a constructor with no arguments).
• The Java class must be serializable (that is, the class must implement or be a subclass of a class that implements java.io.Serializable).





The JDBC Call Procedure activity calls a database procedure using the specified JDBC connection
If this activity is not part of a transaction group, the SQL statement is committed after the activity completes.
If this activity is part of a transaction group, the SQL statement is committed or rolled back with the other JDBC activities in the group at the end of the transaction.
You can override the default behaviour of transactions groups for a JDBC Activities by checking the Override Transaction Behavior field on the Advanced tab so that statement will commit at the completion of activity.
The Refresh button on this activity allows you to synchronize the activity with the contents of the database.
 
The JDBC Connection resource describes a JDBC connection. JDBC connections are used when specifying activities from the JDBC palette.
TIBCO ActiveMatrix BusinessWorks creates a pool of JDBC connections for every JDBC Connection shared resource that uses the JDBC connection type. The maximum size of this pool is specified by the Maximum Connections configuration field.
Activities that use this JDBC Connection resource are given a connection from the pool.
Once a connection is freed by an activity, the connection is returned to the pool. Connections that are left open will eventually time out and be closed. These connections can be reopened at a later time, until the maximum number of connections specified in this field is reached.
If you wish to configure a timeout value for these connections, you can set the Engine.DBConnection.idleTimeout property.
All activities that are part of the same transaction will use the same connection in the connection pool. The first activity in a transaction attempts to reestablish an invalid connection. If a connection becomes invalid during a transaction, the transaction is rolled back and must be retried, if necessary.



The JDBC Get Connection activity retrieves an object reference to a JavaConnectionAccessor object for the specified JDBC Connection from the connection pool. This allows you to use the optimized TIBCO ActiveMatrix BusinessWorks database connection pool instead of specifying database configuration information in your Java code. This database connection can then be used within Java activities to access the specified database.
 
The database connection obtained with the JDBC Get Connection activity has the following restrictions:
• Checkpoints should not be taken after a JDBC Get Connection activity completes.
• Do not pass the JavaConnectionAccessor object reference to a subprocess that spawns a new process.





TIBCO ActiveMatrix BusinessWorks constructs any date and time data returned from the database with the time zone specified in the ServerTimeZone input element. Constructed DateTime data is in a UTC-normalized form, and daylight savings adjustments are also automatically applied










JMS  Activity Palette
 
The Get JMS Queue Message activity retrieves a message from the specified queue.
This activity allows you to perform a receive operation on the queue as opposed to waiting for a queue message to be delivered to the Wait for JMS Queue Message activity or the JMS Queue Receiver process starter.
You can use the Message Selector field on the Advanced tab to retrieve a specific queue message from the queue.
The Get JMS Queue Message activity is different from the Wait for JMS Queue Message activity in the following ways:
1.   	Unlike the Wait for activity, which starts listening for messages from the time the BusinessWorks engine starts, this activity starts listening for incoming messages on the specified queue from the time the activity is triggered.
2.   	This activity can receive only one message from the specified queue at a time, when the Message Selector is not used.
3.   	Once triggered, this activity can either gets a message from the specified destination queue name before timeout and proceeds or it throws a timeout error and exits.
