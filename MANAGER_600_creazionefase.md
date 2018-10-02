---
id: manager-creazionefase
title: Add operation
sidebar_label: Add operation 
---

## Add operation

#### Intro
Operation is added every time there is a need to add stage or phase of production to the Job.

It is possible to add operation in following cases: 
* when "New Job" is being created
* when Job in section "New jobs" is being edited 
* when Job in subsection "Queue" in section "Assigned" is being edited 

Before adding operation to New Job, the later has to be saved first. As soon as the Job is saved an operation can be added by clicking on button "Add operation".

To add/edit operation to the Job which has been previously assigned a manager shall follow the Edit Job guide (see "Edit job")

![Create job](/docs/assets/manager_en/50creazionecommessa.png)

When a manager clicks on the button "Add operation" he can describe production process in all details. Description is performed by selecting from the provided list of different macroprocesses, processes and stations (pieces of equipment).

Each time when manager adds an operation he needs to select: 
* production macroprocess:
    ![Select process 1](/docs/assets/manager_en/60selezionaprocesso1.png)

*  production process: 
    ![Select process 2](/docs/assets/manager_en/70selezionaprocesso2.png)

* production station (choose station)
    ![Select station](/docs/assets/manager_en/80selezionastazione.png)

If macroprocess, process or production station is not presented in the list please contact Administrator of the System.

After selecting processes and station you can add details of the operation. 

  
### Operation details

Each stage of Job processing requires a detailed and precise description so operator who will be assigned to this Job will be able to perform operations correctly.

 ![Operation details](/docs/assets/manager_en/90dettagliofase.png)
 
 In order to describe operation please fill in the following fields:
* Quantity*, required to be produced at this stage
* Units*
* Amount of units per cycle (Cx)
* Theoretical productivity
* Documents
* Note
 
 Field marked with * are mandatory.
 
 #### Definitions
 Amount of units per cycle (Cx) means quantity of products, which has been processed/produced during one cycle of the equipment run. This coefficient is to be adjusted if for each cycle of equipment run more than 1 unit is produced.
 
Theoretical productivity reflects optimal quantity of units produced in one hour. Initially this parameter is inserted manually and over the time the System will update it based on actual data collected.
 
In this section manager can add documents and notes to help operator during production. These documents and notes are visible to the assigned operator only.


#### Important

When operation is created it has to be saved. To do so please click on "Save" button in left upper corner. In order to check whether operation is saved please have a look on the name of the operation (located in the centre of the screen) and check that there are following data there: 
* quantity
* units

Operation is NOT saved
![Operation is not saved](/docs/assets/manager_en/100fasenonsalvata.png)

Operation is saved
![Operation is saved](/docs/assets/manager_en/110fasesalvata.png)


If there are no quantity and units information you shall open operation and fill in operation details again. Please do not forget to save! 


When operation is saved manager can do the following: 
* add more operations to Job
* adding job in production (see "Put into the queue")
