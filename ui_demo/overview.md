Show OperatorHub
- Show the certified operator
- Show CRDs in OCP UI

Show TVK UI
- Show log in with kube/config file, discuss ease of use with RBAC
- Show discovery of all namespaces, clusters, and objects

Show TVK UI backup of a namespace
- Click on a namespace, make sure namespace toggle is on, click backup
- Select the OCP Wordpress backup plan
- While the backup is happening I will go over the different peices of the TVK UI
- Left hand side - Navigation panel, this shows the different namespaces and clusters
- Middle of the screen, this is where we show the different HELM, Label, and Operator apps.
- Objects tab,  With the Objects tab you can search through the different objects, show filters and search bar
- Backup Plans tab, With the Backup Plans tab you can see all the back up plans you have created, show backup/recovery summary on right panel

Show Backup in progress
- Status log: Show MetaSnapshot, DataSnapshot, DataUpload, etc. being completed 
- Backup Summary tab, Show details - Meta mover pod, Data mover pod, Snapshot, etc..
- Meta Data Summary, Show a list of objects that were backed up. speak about transforms.

Show Resources tab on the far left
- Look at Backup Plans, Hooks, Retention Policies, etc..

Log into a different cluster
- Log in with kube/config file
- Make sure the namespace exists for the restore
- Browse the target and find your backup.
- View the the meta data in the backup
- Click on restore and give it a name, choose namespace, give it a flag - skip if already exists.
- Show the Transform, Exclude, and Hooks tabs
- Kick off restore, then show steps and progress
- While the restore is happening you can now show the help tab and walk them through that
- Show restore summary tab and metadata summary tab.

On the cli/Lens
- find the service that was created and get the IP address of the newly restored WP app.
- Log onto the restored WP app

Wrap up
- talk about what we did

