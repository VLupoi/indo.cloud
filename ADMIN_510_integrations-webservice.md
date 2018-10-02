---
id: admin-integrations-webservice
title: SOAP Webservice
sidebar_label: Webservice
---

There is a webservice that can be used to read and write informations inside Agile Factory.
The webservice can be mapped using the following url:
```
https://api.domain.tld/erp-integration/service?wsdl
```

## Available methods

### JobUpsert
Upsert a job, the primary key used for upsert is `code` field. If the job does not exists wil be created, otherwise it will be updated.

### JobDetails
Retrieve details from a job using `code` field.

### PhaseUpsert
Upsert a phase, the primary key used for upsert is `code` field. If the phase does not exists wil be created, otherwise it will be updated.

### PhaseDetails
Retrieve details from a phase using `code` field.

### SessionDetails
Retrieve details from a session using `code` field.

## Classes

### Job class
| Property	    | Type	                        | RO/RW	| Description               |
|-------------- |------------------------------ |------ |-------------------------- |
| _id  	        | string                        | RO	| Agile Factory unique Id   |
| code  	    | string                        | RW	| ERP unique Id  	       	|
| sort  	    | int                           | RW	| sort in queue (asc)       |
| name  	    | string                        | RW	| job name  	            |
| description  	| string                        | RW	| job description  	        |
| unit  	    | enum                          | RW	| see note below *          |
| qty  	    	| long                          | RW	| job quantity  	        |
| docs  	    | Doc[]                         | RW	| array of documents        |
| phases  	    | string[]                      | RW	| array of phase ids        |
| isDeleted  	| bool                          | RW	| job is deleted  	 	    |
| isError  	    | bool                          | RO	| job is in error  	        |
| status  	    | enum **                       | RO	| see note below **         |
| timers  	    | Timers                        | RO	| job timers     	  	    |

\* Job unit, ENUM: unit, lt, mt, sqm, kg

\** Job status, ENUM: NEW, TODO, WIP, DONE

### Phase class

| Property	        | Type	                        | RO/RW	| Description                               |
|------------------ |------------------------------ |------ |------------------------------------------ |
| _id               | string                        | RO	| Agile Factory unique Id                   |
| code              | string                        | RW	| ERP unique Id                             |
| sort  	        | int                           | RW	| sort in queue (asc)                       |
| name              | string                        | RW	| phase name                                |
| description  	    | string                        | RW	| phase description                         |
| unit  	        | enum                          | RW	| see note below *                          |
| qty  	    	    | long                          | RW	| phase quantity  	                        |
| producedByCycle   | int                           | RW	| how many pieces with a single cycle (cX)  |
| docs              | Doc[]                         | RW	| array of documents                        |
| sessions	        | string[]                      | RO	| array of sessions ids                     |
| notes             | string                        | RW	| notes from manager                        |
| isDeleted         | bool                          | RW	| job is deleted                            |
| isError           | bool                          | RO	| job is in error                           |
| status  	        | enum                          | RO	| see note below ** 	                    |
| timers            | Timers                        | RO	|                                           |
| totalPrice        | double                        | RW	| the price (see note below ***)            |
| eta               | long                          | RW	| time of arrival (see note below ***)      |
| etd               | long                          | RW	| time of departure (see note below ***)    |

\* Phase unit, ENUM: unit, lt, mt, sqm, kg

\** Status enum: NEW, APPROVAL, MATERIAL, SETUP, PRODUCTION, PAUSE, INTERRUPT, ERROR, CHECK, DONE, CRASH

\*** Used only with external working

### Session class

| Property	        | Type	                        | RO/RW	| Description                               |
|------------------ |------------------------------ |------ |------------------------------------------ |
| _id               | string                        | RO	| Agile Factory unique Id                   |
| user              | User                          | RO	|                                           |
| machine           | Machine                       | RO	|                                           |
| cycles            | long                          | RO	| the number of cycles made by machine      |
| scrap          | long                          | RO	| the number of scrap pieces, wrong units   |
| status  	        | enum                          | RO	| see note below * 	                        |
| timers            | Timers                        | RO	|                                           |
| error             | Error                         | RO	|                                           |

\* Status enum: NEW, APPROVAL, MATERIAL, SETUP, PRODUCTION, PAUSE, INTERRUPT, ERROR, CHECK, DONE, CRASH

### Error class

| Property	        | Type	                        | RO/RW	| Description                               |
|------------------ |------------------------------ |------ |------------------------------------------ |
| _id               | string                        | RO	| Agile Factory unique Id                   |
| message           | string                        | RO	| message from hmi                          |
| acknowledgeMessage| string                        | RO	| acknowledge message from manager          |
| isAcknowledged    | bool                          | RO	| error is fixed                            |

### User class

| Property	        | Type	                        | RO/RW	| Description                               |
|------------------ |------------------------------ |------ |------------------------------------------ |
| _id               | string                        | RO	| Agile Factory unique Id                   |
| code              | string                        | RO	| ERP unique Id                             |
| firstName         | string                        | RO	|                                           |
| lastName          | string                        | RO	|                                           |

### Machine class

| Property | Type   | Editable | Description             | 
|----------|--------|----------|-------------------------| 
| _id      | string | RO       | Agile Factory unique Id | 
| code     | string | RO       | ERP unique Id           | 
| name     | string | RO       | machine name            | 
| model    | string | RO       | machine model           | 
| position | string | RO       | machine position        | 


### Timers class

| Property     | Type | Editable | Description      | 
|--------------|------|----------|------------------| 
| production   | long | RO       |                  | 
| material     | long | RO       |                  | 
| setup        | long | RO       |                  | 
| check        | long | RO       |                  | 
| pause        | long | RO       |                  | 
| approval     | long | RO       |                  | 
| error        | long | RO       |                  | 
| total        | long | RO       | see note below * | 

\* total is sum of: production, material, setup, pause, approval and check

### Doc class

| Property | Type   | Editable | Description                                  | 
|----------|--------|----------|----------------------------------------------| 
| _id      | string | RO       | Agile Factory unique Id                      | 
| mimeType | string | RW       | mime type for document, e.g. application/pdf | 
| link     | int    | RW       | http link to the document                    | 
| type     | enum   | RW       | document type (see note below *)             | 

\* enum: BOM, CHECK, MATERIAL, SETUP, PRODUCTION, APPROVAL, BLUEPRINT, INVOICE

## Enums

### Phase status

| Status        | Description                                               |
|-------------- |---------------------------------------------------------- |
| NEW           | Just created                                              |
| APPROVAL      | Phase approval                                            |
| MATERIAL      | Worker is taking material from warehouse                  |
| SETUP         | Worker is preparing machine (e.g. changing mould, etc)    |
| PRODUCTION    | Production, phase started, counting cycles                |
| PAUSE         | Phase paused                                              |
| INTERRUPT     | Phase is stopped                                          |
| ERROR         | Phase has a blocking error                                |
| CHECK         | Operator checks the result                                |
| DONE          | Phase concluded, no other operation allowed               |
| CRASH         | HMI had a crash, stop timers!                             |

### Job unit

| Type          | Description                                               |
|-------------- |---------------------------------------------------------- |
| unit          | Unit                                                      |
| lt            | Liter                                                     |
| mt            | Meter                                                     |
| sqm           | Square meter                                              |
| kg            | Kilogram                                                  |

### Job status	

| Status        | Description                                               |
|-------------- |---------------------------------------------------------- |
| NEW           | Job is new and can be modified                            |
| TODO          | job is on queue and operators can work on it              |
| WIP           | Job is under working                                      |
| DONE          | Job is ocompleted                                         |

### Doc type	

| Type          | Description                                               |
|-------------- |---------------------------------------------------------- |
| BOM           | Bill of material                                          |
| CHECK         | Check after production                                    |
| MATERIAL      | Material                                                  |
| SETUP         | Setup machine                                             |
| PRODUCTION    | Production sheet                                          |
| APPROVAL      | Approval sheet                                            |
| BLUEPRINT     | Blueprint                                                 |
| INVOICE       | Invoice                                                   |
| TRANSPORT     | Transport document                                        |

## Examples

### Job details
```
<x:Envelope xmlns:x="https://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://api.domain.tld/erp-integration/service">
    <x:Header/>
    <x:Body>
        <ser:JobDetails>
            <ser:code>1891821631783727</ser:code>
        </ser:JobDetails>
    </x:Body>
</x:Envelope>
```

### Phase details
```
<x:Envelope xmlns:x="https://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://api.domain.tld/erp-integration/service">
    <x:Header/>
    <x:Body>
        <ser:PhaseDetails>
            <ser:code>1276514916423956</ser:code>
        </ser:PhaseDetails>
    </x:Body>
</x:Envelope>
```
### Session details
```
<x:Envelope xmlns:x="https://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://api.domain.tld/erp-integration/service">
    <x:Header/>
    <x:Body>
        <ser:SessionDetails>
            <ser:code>5a018a478a70992534974c6a</ser:code>
        </ser:SessionDetails>
    </x:Body>
</x:Envelope>
```
### Job upsert
```
<x:Envelope xmlns:x="https://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://api.domain.tld/erp-integration/service">
    <x:Header/>
    <x:Body>
        <ser:JobUpsert>
            <ser:code>6476514916424812</ser:code>
            <ser:job>
                <ser:code>6476514916424812</ser:code>
                <ser:name>lorem ipsum</ser:name>
                <ser:description>lorem ipsum lorem ipsum</ser:description>
                <ser:unit>pcs</ser:unit>
                <ser:qty>1000</ser:qty>
                <ser:docs>
                    <ser:Doc>
                        <ser:mimeType>application/pdf</ser:mimeType>
                        <ser:link>http://s3.domain.tld/file-storage/084_6F_Disegno-elemento-1509728058894.pdf</ser:link>
                        <ser:type>BLUEPRINT</ser:type>
                    </ser:Doc>
                    <ser:Doc>
                        <ser:mimeType>application/pdf</ser:mimeType>
                        <ser:link>http://s3.domain.tld/file-storage/084_6F_Disegno-elemento-1509728058894.pdf</ser:link>
                        <ser:type>APPROVAL</ser:type>
                    </ser:Doc>
                </ser:docs>
                <ser:isDeleted>false</ser:isDeleted>
            </ser:job>
        </ser:JobUpsert>
    </x:Body>
</x:Envelope>
```
### Phase upsert
```
<x:Envelope xmlns:x="https://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://api.domain.tld/erp-integration/service">
    <x:Header/>
    <x:Body>
        <ser:PhaseUpsert>
            <ser:jobCode>6476514916424812</ser:jobCode>
            <ser:code>1276514916423956</ser:code>
            <ser:phase>
                <ser:code>1276514916423956</ser:code>
                <ser:sort>0</ser:sort>
                <ser:name>lorem ipsum</ser:name>
                <ser:description>lorem ipsum lorem ipsum</ser:description>
                <ser:unit>pcs</ser:unit>
                <ser:qty>100000</ser:qty>
                <ser:producedByCycle>1</ser:producedByCycle>
              <ser:docs>
                  <ser:DocInput>
                      <ser:mimeType>application/pdf</ser:mimeType>
                      <ser:link>https://s3.ru-prms.agilefactory.it/file-storage/084_6F_Disegno-elemento-1509728058894.pdf</ser:link>
                      <ser:type>BLUEPRINT</ser:type>
                  </ser:DocInput>
                  <ser:DocInput>
                      <ser:mimeType>application/pdf</ser:mimeType>
                      <ser:link>https://s3.ru-prms.agilefactory.it/file-storage/084_6F_Disegno-elemento-1509728058894.pdf</ser:link>
                      <ser:type>APPROVAL</ser:type>
                  </ser:DocInput>
              </ser:docs>
                <ser:notes>lorem ipsum lorem ipsum lorem ipsum lorem ipsum</ser:notes>
                <ser:isDeleted>false</ser:isDeleted>
            </ser:phase>
        </ser:PhaseUpsert>
    </x:Body>
</x:Envelope>
```
