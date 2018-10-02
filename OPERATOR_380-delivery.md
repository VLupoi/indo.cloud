---
id: operator-delivery
title: Delivery
sidebar_label: Delivery
---
## Delivery 
### Delivery during production

![3 circles](/docs/assets/operator_eng/150trecerchi.png)

To report to the System quantity of products manufactured click on button "Delivery".

 ### Check (auto-check)

![Check](/docs/assets/operator_eng/250autocontrollo.png)

After the click on "Delivery" operator will see screen with green frame. On this stage operator have to do quality check (auto-check or any kind of other check) or directly proceed to the next step depending on the instructions received on screen.

Operator can report on Partial or Last delivery by clicking on respective buttons.

## Partial delivery

Partial delivery is used to report to the System quantity of manufactured products even if the quantity is less then prescribed by the System.

Here are the cases for Partial delivery:  
* there is an Issue with material receipt in Pause stage
* there is an Issue during production
* there is partial delivery of manufactured products

 
## Last delivery
By click on "Last delivery" operator reports to the System that operation is completed and all required quantity of products are manufactured.

In both cases (Partial and Last delivery) operator can do the following:

After operator clicks on "Partial delivery" or "Last delivery" buttons the Unit's produced screen with grey frame will appear.
![Units produced](/docs/assets/operator_eng/200unitaprodotte.png)

On this screen operator can do the following:
* report to the System manually on quantity of manufactured products (e.g. insert respective amounts) via HMI
* confirm or correct products quantity if MES is connected to the work station and collects data automatically
* Click on "Back" button to go to the previous screen

By clicking on "Ok" button operator is guided to the Scrap screen with grey frame:
![Scrap](/docs/assets/operator_eng/210unitascarti.png)

On this screen operator can do the following:
* report to the System manually on quantity of scrap during production (e.g. insert respective amounts) via HMI
* confirm or correct scrap quantity if MES is connected to the work station and collects data automatically
* Click on "Back" button to go to the previous screen

By clicking on "Ok" button operator is guided to the "Add a note" screen with grey frame:
![Note](/docs/assets/operator_eng/220aggiunginota.png)

On this screen operator can leave note to the operation or click on "Back" button to go to the previous screen


## Finish work 
After click on OK button on "Add a note” screen operator can either:
* End shift
or
* Initiate New phase (operation)
 by clicking on respective buttons
 
![Finish work](/docs/assets/operator_eng/230finelavoro.png)

When operator clicks on "New phase" button he shall receive new operation to be processed. New operation will appear automatically.

## No operations assigned

Sometimes operator will see screen saying "Sorry, there is no phase...". This means that this work station does not have operation to be processed and the operator shall contact workshop steward for instructions.
![No operations assigned](/docs/assets/operator_eng/240nessunlavoro.png)
