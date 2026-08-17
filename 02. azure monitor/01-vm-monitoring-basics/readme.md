
## Overview
Building an Azure VM monitoring environment and learn how Azure Monitor uses metrics, Activity Log, and Log Analytics to monitor Azure resources.

By the end of this lab, I will  understand the difference between:

* **Metrics**
* **Activity Log**
* **Resource logs**
* **Log Analytics**
* **Azure Monitor**


## Scenario
You are an Azure administrator responsible for a virtual machine.

The company wants you to:

* **Monitor VM performance.**
* Investigate administrative activities.

Centralize monitoring data.
Determine whether the VM is reporting monitoring data.
Understand the different monitoring signals available in Azure.
  ## Architecture
                    Azure Monitor
                       │
          ┌────────────┼────────────┐
          │            │            │
       Metrics    Activity Log   Logs
          │            │            │
          └────────────┼────────────┘
                       │
                Log Analytics
                  Workspace
                       │
                     VM
