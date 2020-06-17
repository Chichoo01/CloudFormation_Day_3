
## Stack States
**Presenter:**  *Deepak Kovvuri*

---
### Create Operations
---

* Customer Initiates a stack creation. This is possible via two APIs:
  * CreateStack API
  * CreateChangeSet API

Let's try this out:

```bash
aws cloudformation create-stack \
    --template-url https://deepak-content.s3.amazonaws.com/bucket.yml \
    --stack-name training-1
```
It's also possible to create a stack as change-set. Note: When a new stack is created as a change-set, the stack will not have any resources until the change-set is executed. 

**Why would one use Change-Sets ?**

*When making enterpise deployments, it's always essential to make sure the right resources are being created before the stack is deployed. Change-sets help preview your changes and then execute them. Essentially breaking down the stack management process into two steps:*

* Review - CreateChangeSet API
* Accept Changes - ExecuteChangeSet API

```bash
CHANGE_SET_ID=$(aws cloudformation create-change-set \
                  --template-url https://deepak-content.s3.amazonaws.com/bucket.yml \
                  --change-set-type CREATE --stack-name training-2 \
                  --change-set-name initial --query Id --output text)
```

```bash
aws cloudformation execute-change-set --change-set-name $CHANGE_SET_ID
```

---

### Flowchart

---

<img src="https://deepak-content.s3.amazonaws.com/CreateStack.png"
     alt="CreateStack Process"
     style="float: center; margin-right: 10px;" />

---

### Update Operations

---

A customer can now update a stack in the CREATE_COMPLETE state using the following operations:

  * UpdateStack
  * CreateChangeSet

```bash
aws cloudformation update-stack --stack-name training-1 --use-previous-template 
```

This will fail with `An error occurred (ValidationError) when calling the UpdateStack operation: No updates are to be performed.` as we are using the previous template.



```bash
aws cloudformation update-stack \
      --template-url https://deepak-content.s3.amazonaws.com/bucket_new.yml \
      --stack-name training-1
```
(or)

```bash
CHANGE_SET_ID=$(aws cloudformation create-change-set \
      --template-url https://deepak-content.s3.amazonaws.com/bucket_new.yml \
      --change-set-type UPDATE --stack-name training-1 \
      --change-set-name initial --query Id --output text)
```

```bash
aws cloudformation execute-change-set --change-set-id $CHANGE_SET_ID
```

---

### Important things to keep in mind during Updates

---

*  During an update a stack always updates existing resources or creates the new resources specified in the template before deleting the unwanted resources.

* A resource update can either be immutable or mutable.

  * Immutable Resource Update - A resource update that results in the creation of a new resource.
  * Mutable Resource Update - A resource update that simply updates an existing resource and doesn't replace it.

* A ChangeSet helps understand if a resource is being mutated during an update. Any if the mutation results in a replacement (immutability).

---

### Flowchart

---

<img src="https://deepak-content.s3.amazonaws.com/UpdateStack.png"
     alt="CreateStack Process"
     style="float: center; margin-right: 10px;" />

---
### Create Operations
---

A stack in the following states can be deleted:

 * CREATE_COMPLETE
 * ROLLBACK_COMPLETE
 * ROLLBACK_FAILED
 * UPDATE_COMPLETE
 * UPDATE_ROLLBACK_COMPLETE
 * UPDATE_IN_PROGRESS
 * UPDATE_ROLLBACK_IN_PROGRESS

---

### Flowchart

---

<img src="https://deepak-content.s3.amazonaws.com/DeleteStack.png"
     alt="CreateStack Process"
     style="float: center; margin-right: 10px;" />

**Note:** *A stack can go to DELETE_FAILED if a resource fails to delete. For example, `TerminationProtection` can be enabled on a `AWS::EC2::Instance` resource which would cause the instance termination to fail. In that case, you can disable termination protection and re-attempt deletion.*

### Test this out:

1. Upload a file to the bucket that was created in the stack earlier `training-1`.
2. Later, attempt deleting that stack. The stack should fail to delete as the S3 Bucket has files with `NotEmptyBucket` Exception.

* Now, you can either empty the bucket and re-attempt deleting the bucket; or, 
* Retain the bucket and continue with stack-deletion using the following command:

```bash
aws cloudformation delete-stack --stack-name training-1 \
                                --retain-resources Bucket
```
