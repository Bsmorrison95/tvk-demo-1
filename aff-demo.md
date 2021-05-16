# AFF Demo


### 1. Show cockroach app in OCP1 UI and no app in OCP2 UI
### 2 . Show Application Level Discovery
   - App Discovery - Quick visability of Apps by Helm
   - Applicatiob-level granularity allow quick and precise restore
### 3 . Start Helm backup
  - Aff Uses Helm Chats
  - Ease of knowing "it just works" 
### 4. Show filters, Backup Summary, Show multi cluster management, adding cluster
  - Quickly see failed/available backups, minimizes time spend monitoring 
### 5. Once Backup is complete: Start Recovery in same cluster different namespace
### 5. Show backup, talk about QCOW2, show QCOW2 in NFS
  - QCOW2 is space efficient and quick to recover
  - NOT a repackaged open-source project (Kasten uses Kopia, not yet GAed) 
  - Industry standard, can be used with other Linux tooling 
  - Stored in separate repository from Cluster, ensuring safety 
### 6. Once Same cluster recovery complete, delete Helm chart and show no error with original Helm Chart
### 7. Show Disaster Recovery Plan
  - Easy straightforward restore
  - Set up DR Plans beforehand for quick and easy revocery 
  - Changing cluster name away from UUID
  - Minimal clicks to recover
### 8. Execute multi-custer Recovery of Helm App
### 9. Show App resources: Hooks, Retention plans, etc
  -  Easy, Pre-made Hooksfor major databases
  -  Reduces time spent setting up successful backups, completed in UI
  -  ?? Did you work with Hooks with Kasten? Kasten uses Kanister CRDs, which are known to be much more complex
  -  Did you use Kasten CLI at all? Kasten CLI is not Kubernetes Native, need to learn new commands 
### 10. Show Successful restore of cockroach application in OCP2 UI
