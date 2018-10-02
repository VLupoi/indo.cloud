---
id: admin-architecture
title: Architecture
sidebar_label: Architecture
---

Agile Factory has three main components:

* Engine
* Manager interface
* HMI interface

## Agile Factory Engine
The engine is the core of the application, it is connected to database and queue manager and provides an API used by other components.

## Manager interface
It is a website where managers can create batches and phases, 

## HMI interface
It is a website that will be shown near to the machines, operators will use this interface.

## Software architecture
See the picture

```                     
                                  ╔ AF Server ════════════════════════════════════════════════════════╗
                                  ║                                                                   ║
                                  ║  ┌ Nginx ────────┐          ┌ Engine ───┐          ┌ Database ─┐  ║
                                  ║  │               │          │           │          │           │  ║
             ┌ Manager ─┐         ║  │ API           ├──────────┤           ├──────────┤           │  ║  
             │          ├──────┐  ║  │ Reverse proxy │          │           │          │           │  ║
             └──────────┘      │  ║  │               │          └─────┬─────┘          └───────────┘  ║ 
                               │  ║  ├───────────────┤                │                ┌ Storage ──┐  ║       
                               │  ║  │ Manager       │                │                │           │  ║ 
                               └─────┤ Interface     │                │          ┌─────┤           │  ║              
             ┌ HMI ─────┐         ║  │               │                │          │     │           │  ║            
   ┌ Press ──│          ├──────┐  ║  ├───────────────┤                │          │     └───────────┘  ║            
┌──┤         └──────────┘      └─────┤ HMI           │                │          │                    ║
│  └──────────┘                   ║  │ Interface     │                │          │                    ║              
│            ┌ HMI ─────┐      ┌─────┤               │                │          │                    ║  
│  ┌ Press ──│          ├──────┘  ║  ├───────────────┤                │          │                    ║
│ ┌┤         └──────────┘         ║  │ Storage       │                │          │                    ║
│ │└──────────┘                   ║  │ Reverse proxy ├────────────────│──────────┘                    ║
│ │                               ║  │               │                │                               ║
│ │                               ║  └───────────────┘                │                               ║
│ │                               ║                                   │                               ║
│ │                               ║  ┌ Queue manager ┐                │                               ║
│ └──────────────────────────────────┤               │                │                               ║
│                                 ║  │               ├────────────────┘                               ║
└────────────────────────────────────┤               │                                                ║
                                  ║  └───────────────┘                                                ║
                                  ║                                                                   ║
                                  ╚═══════════════════════════════════════════════════════════════════╝
```
