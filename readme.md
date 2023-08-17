# QuandaGo Interactions example lambda

QuandaGo interactions support direct integrations by calling lambda functions. These functions can be used as an RPC style
framework for sending and retrieving arbitrary data. To use this you need an active subscription of QuandaGo interactions. 
each subscription comes with a maximum for a few dozen of active functions. So make sure to deactivate 
any test functions that you configure with this example

## creating the lambda function

Before you can use the lambda function you should set it up with some very specific requirements. This includes creating
the lambda function, the lambda function role and the role in your account that allows us to invoke the lambda function.
We've included in this demo a set of cloud formation templates that will configure all those resources for you.
to use them goto cloudformation in your AWS account

### cloud formation

first upload the relevant yaml stack template of one of the example folders.

![](./cloudformationcreate.png)

we need to fill in the parameters, most of these will be autofilled except for your tenant guid. This is something you can 
get from the interactions manager application under Management > tenants

![](./managertenant.png)

add this as tenant guid on the screen to create a cloud formation stack

![](./cloudformationparameters.png)

once this is created we will need the resource ids in the next step
### interactions configuration

In order to use the created function in a dialplan you have to configure the created lambda, both it's id (ARN) and 
the id (ARN) of the role you made to give access to the function in your account. you can do this in the manager 
application under management > lambda functions. optionally you can also configure input and output parameters.

*make sure to use the AssumeRole and not the ExecutionRole*


![](./cloudformationoverview.png)


![](./lambdaoverview.png)


![](./iamoverview.png)

use the ARN of the assume role and of the lambda to configure a new lambda function in management > lambda functions
within interactions.

![](./lambdaedit.png)


![](./lambdaeditinput.png)
optionally you can also configure input and output on this particular lambda function. Because these calls can often
fail both as an error of communication and as an application level error, such as not found etc. We recommend to always return and check some kind of error code as an output

![](./lambdaeditoutput.png)

### configuration in dialplan

under Telephony > routing "+ add visual dialplan" you can drag a "script access" and "variables" block into the dialplan 
and you will notice that you can now use the previously configured lambda function in the "script access" block, with
the defined input and output. You should define a variable with an initial value. There should also be explicit 
assignments when using runtime variables like caller id as part of your script input.

![](./incompletedialplan.png)

if you've added some inputs and conditional outputs to your function the dialplan should look something like this.

![](./completedialplan.png)

### assign dialplan to phone number

after creating saving and publishing the dialplan you should pick a phonenumber and assign it 

![](./destinationnumberedit.png)