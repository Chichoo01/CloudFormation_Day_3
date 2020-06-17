## Exporting Stack Output Values

- To share information between stacks, export a stack's output values. Other stacks that are in the same AWS account and region can import the exported values. 

- For example, you might have a single networking stack that exports the IDs of a subnet and security group for public web servers. Stacks with a public web server can easily import those networking resources. You don't need to hard code resource IDs in the stack's template or pass IDs as input parameters.

- To export a stack's output value, use the ```Export``` field in the ```Output``` section of the stack's template. To import those values, use the ```Fn::ImportValue``` function in the template for the other stacks.

### Note:
After another stack imports an output value, you can't delete the stack that is exporting the output value or modify the exported output value. All of the imports must be removed before you can delete the exporting stack or modify the output value.

## Exporting Stack Output Values vs. Using Nested Stacks

- A nested stack is a stack that you create within another stack by using the ```AWS::CloudFormation::Stack``` resource. With nested stacks, you deploy and manage all resources from a single stack. You can use outputs from one stack in the nested stack group as inputs to another stack in the group. This differs from exporting values.

- If you want to isolate information sharing to within a nested stack group, we suggest that you use nested stacks. To share information with other stacks (not just within the group of nested stacks), export values. For example, you can create a single stack with a subnet and then export its ID. Other stacks can use that subnet by importing its ID; each stack doesn't need to create its own subnet. Note that as long as stacks are importing the subnet ID, you can't change or delete it.

## Listing Exported Output Values

- Console: From the **CloudFormation** navigation pane, choose **Exports**.

![Exports-Console](https://github.com/jabadesj/CloudFormation_Day_3/blob/master/exports/images/console-exports.png)

- AWS CLI: Run the ```aws cloudformation list-exports``` command.
- API: Run the ```ListExports``` API.
