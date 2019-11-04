# workflow-popular-path

Every Workflow definition (“WorkflowDefinition_RecID”) is composed of a sequence of blocks (“BlockId”) . When a business object (a ticket request, service request) is created, it follows a Workflow definition, and eventually a workflow instance is created (“WorkflowInstanceLink_RecID”). So, Overall a workflow definition can have multiple workflow instances where each workflow instance will follow a specific sequence of block ids defined in the corresponding Workflow definition. 
“CreatedDateTime” captures the start of a block execution, while “CompletedDateTime” captures the completion of the block execution.

Each row in the “data.tsv” (tab separated file) represents the block execution for a Workflow Instance following a Workflow Definition. So, for a Workflow definition and a Workflow Instance (following it), if the Workflow Instance contains 3 blocks, there will be 3 rows in “data.tsv”.


Questions:-

1.    Plot the number of workflow instances for every workflow definitions. It should be a bar graph.
2.    For each Workflow definition, find the mean execution time.
3.    For each Workflow definition, and for each BlockId find the mean execution time across the workflow instances. 

Things to keep in mind while doing the analysis:-

1.    Make sure you only consider workflow instances for which “IsExecuting” field is 0.
2.    Make sure you only consider workflow instances for which BlockName = "stop" and ExitPort = "completed"
3.    Make sure you only consider workflow instances for which “BlockException” is NULL.
4.    Make sure you only consider workflow instances for which “Status” is “completed”.



There are multiple workflow definitions. Each of those workflow definitions can serve multiple business objects. When a business object follows a workflow definitions then a workflow instance is created. Hence, each workflow definition can result into multiple workflow instances and each of which may follow different paths in the respective workflow definition. So, It is possible to find the most popular path in individual workflow definitions which most number of workflow instances contribute to.

Input:- Workflow instance data, workflow definition id
Output:- 

{
  "workflow_id": "f632043caef54335bc298c5f63662a6d",
  "popular_paths": [
    {
      "block_exec_time": {
        "f632043caef54335bc298c5f63662a6d;AA18B5417630439189900D3B3D5701F8": 637.2635,
        "f632043caef54335bc298c5f63662a6d;03ABD5C1CAD5471BA5D1F4FBC0BDDA54": 637.1300000000001,
        "f632043caef54335bc298c5f63662a6d;37C6643DF8974905B4472981D3976D47": 636.6220000000002,
        "f632043caef54335bc298c5f63662a6d;76A23F1256844B7DB4528150DF57EC29": 4.6745,
        "f632043caef54335bc298c5f63662a6d;1A05913BC25145C8A34C11B288F041A5": 0
      },
      "median_execution_time": 1915.69,
      "path": [
        "f632043caef54335bc298c5f63662a6d;AA18B5417630439189900D3B3D5701F8;start;ok",
        "f632043caef54335bc298c5f63662a6d;76A23F1256844B7DB4528150DF57EC29;invokeworkflow;completed",
        "f632043caef54335bc298c5f63662a6d;37C6643DF8974905B4472981D3976D47;task;completed",
        "f632043caef54335bc298c5f63662a6d;03ABD5C1CAD5471BA5D1F4FBC0BDDA54;update;ok",
        "f632043caef54335bc298c5f63662a6d;1A05913BC25145C8A34C11B288F041A5;stop;completed"
      ],
      "min_execution_time": 1534.9370000000001,
      "mean_execution_time": 1915.69,
      "workflow_instance_count": 2,
      "max_execution_time": 2296.443
    }
  ]
}

PreProcess:-
1.    Retain only those Workflow Instances which are finished executing i.e. “IsExecuting = 0”
2.    Remove the Workflow Instances which do not have “BlockName” = "stop" and “ExitPort” = "completed"
3.    Remove the instanes having “CompletedDateTime” as Null
4.    Remove the instances having “BlockException” as non Null

Data Statistics:
Total number of Workflow definitions :  42



Process:-
1.    Calculate the execution time for each block of the workflow instances (resolve_time - creation_time)
2.    Group by workflow definition and Block id to find mean execution time for each blocks corresponding to a workflow definition.
3.    Group by workflow instance to find the path taken for individual workflow instances.
4.    Group by workflow definition to find unique paths taken for individual workflow definitions.
5.    For each workflow definition, sort the unique paths based on the workflow instance count and select top_k popular paths.





Distribution of unique paths for workflow definitions:-




