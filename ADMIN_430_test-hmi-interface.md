---
id: admin-test-hmi-interface
title: Test HMI Interface
sidebar_label: HMI Interface
---

**Note** please be sure to open also a browser with the job that you've created before opened.

## Checklist
* Access to the HMI web interface
* Sign in
* See operation details
* Working session and machine connection
* End shift

## Open HMI web interface
To access HMI web interface, power on the HMI attached to the machine and wait that the interface is loaded.

The first screen will show the current time (top right corner) and the name of the machine (top left corner).

Process with the next screen, clicking on _Start_ button.

## Sign in
Check that the list of the user contains the operator added during the [previous step](admin-test-manager-interface.html#create-operator) then choose the operator.

## Operation details
Check that the operation that is shown is the same that was added during the [previous step](admin-test-manager-interface.html#create-job).

Try to open an attachment and verify that is shown correctly.

Then start to work clicking on _Start material_ button.


## Work session
Wait at least a couple of seconds inside the **MATERIAL** view, because if the operator press the _Start setup_ button the Material step is not saved.

Then go to the production view, the one with three circles (timer, batch and shift productivity). 

**Note** Please verify on manager dashboard that the status of the operation is updated.

Please watch the shift productivity, if the machine is working you will see the increase of the value. If you cannot test using a real machine, you can also try the simulator, available on address `http://simulator.domain.tld`.

## End shift
Now you can finish the shift, clicking on _Delivery_ button. Check that the manager dashboard is updated correctly.