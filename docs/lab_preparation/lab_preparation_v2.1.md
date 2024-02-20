## Prepare PC & PE
1. Login to PC or PE, find the IP Address of a WinToolsVM and login through remote desktop.
2. Open Chrome, login to portal.nutanix.com using your account, go to Download > Prism Central.
3. Find Prism Central Upgrade (version: pc.2023.x), download the image (make sure it is .tar.gz) and matadata.
4. Login to PC in the WinToolsVM
5. Click gear icon on the top right corner > Upgrade Prism Central > Upload the Prism Central binary, upload the image and metadata and then choose Upload Now
6. After uploading the image, choose the uploaded version of pc.2023.x, click Upgrade > Upgrade Now > Continue (It will take more than 30 minutes, so can prepare the file server in parallel)
7. After upgrade complete, login to PC again, go to Admin Center > LCM > Inventory > Perform Inventory
8. After inventory complete, go to update, update Files Manager twice, and Objects Manager & Objects Service
9. Target versions
   1.  PC : 2023.4
   2.  AOS : 6.5.2.5
   3.  Files : 4.4.0.1
   4.  Files Manager : 4.4.0.1
   5.  Objects : 4.2
10. Delete unuse VMs : SSH to CVM > acli > vm.delete Peer* > yes

## Prepare File Server and FA:
1. **[for Data Lens Lab only] ** Create a new File Server in PE
   1. Create a file server named FS*XYZ* for Data Lens lab (e.g. POC012)
      1. 4x FSVM, minimum vCPU & RAM
      2. Both storage network and client network are Primary Network
      3. Use SMB
         1. AD = ntnxlab.local
         2. Username = administrator@ntnxlab.local
         3. Password = nutanix/4u
         4. Make administrator@ntnxlab.local as the file server admin
      4. Use NFS
         1. Authenication = unmanaged
      5. Do not create protection domain
2. Create file servers for general lab per user in PC
   1. Login to PC > admin center > inventory > perform inventory
   2. Go to PC > Files > Create file server 
      1. Choose FS version = 4.4.0.1
      2. Named FS*XYZ-#-prod* (e.g. FS012-1-prod)
      3. 1x FSVM, minimum vCPU & RAM
      4. Both storage network and client network are Primary Network
      5. Follow the setting of FS*XYZ*
      6. Once created, go to the XXX to enable SMB and NFS
      7. Make adminuser##@ntnxlab.local as the file server admin
3. Remove File Server BootcampFS
   1. leave domain
   2. Delete the File Server
   3. Delete specific related entities

## Prepare Active Directory **[for Data Lens Lab only]**
1. Create user groups
   1. Remote desktop to AutoAD (10.xx.xx.41)
   2. Go to PowerShell ISE, login as administrator
   3. paste the following and execute
    ```bash
        for ($num = 1; $num -le 9; $num++) {
            New-ADGroup -Name Group0$num -GroupCategory Security -GroupScope Global
            Add-ADGroupMember -Identity "Group0$num" -Members adminuser0$num, user0$num
        }

## Enable Data Lens **[for Data Lens Lab only] **
1. VM > FSVM > Launch Console > login : nutanix | nutanix/4u
2. enable FSVM ssh : 
        ```bash
            afs misc.ssh_on_client_network enable
        ```
3. find CVM Cluster UUID from PE
4. find FSVM UUID in FSVM CLI: 
        ```bash
            zeus_config_printer | grep cluster_uuid
        ```
5. go to website : [register UUID](https://awsinfra.nusightsinfra.com/help)

    - put "faqa" in Data Pipeline
    - add both UUID in this site to register 

6. Modify cfs.config
        ```bash
            allssh "vi /home/nutanix/minerva/nusights/config/cfs/cfs.config"
        ```
      - Add the following code to the file
        ```bash
            modify endpoint_info_list as follow
                endpoint_info_list {
                endpoint: "faqa.nusightsinfra.com:443"
                uuid: "<CLUSTER_UUID>"
                is_governing: true
            }
        ```

7. Modify minerva_nvm.gflags
        ```bash
            allssh "vi /home/nutanix/config/minerva_nvm.gflags"
        ```
      - Add the following code to the file
        ```bash
            --minerva_cfs_dump_transported_data_to_file=true
            --minerva_cfs_debug_level=3
            --minerva_cfs_control_center_poll_freq_secs=120
        ```

8. Stop FS
        ```bash
            allssh genesis stop minerva_nvm
        ```

9. Run this
        ```bash
            zkrm /appliance/physical/cfs_configuration
            allssh USE_SAFE_RM=no rm -rf /home/nutanix/data/nusights/*
            cluster start
        ```

10. Check if fa_cloud_collector is running
        ```bash
            ps -aef | grep fa_cloud_collector
        ```

11. Verify pulse
        ```bash
            allssh "curl http://127.0.0.1:2042/h/connectivity_status | python -m json.tool"
        ```

12. Wait for 10 mins, login to [https://datalens-qa.nutanix.com/home](https://datalens-qa.nutanix.com/home) to check if the FS is there.

13. Enable Data Lens for FS

14. Go to Ransomware Protection, enable

