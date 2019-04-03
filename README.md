# AWS_Cloud_Formation_Template_2-servers

/* Troubleshooting

The "CreationPolicy" section assigned to the two EC2::Instances (the servers) define the duration of timeout as 5 minutes until AWS CloudFormation rolls back the creation of the full stack if the EC2 Instances do not signal their creation as successful.

The log files to cfn-init (the AWS utility that verifies and runs the contents of the Metadata:config and Metadata:commands sections in the Template) are in the /var/log directory of the Instances:


[centos@mst1 ~]$ cd /var/log

[centos@mst1 log]$ ls -ltrh | grep -i cfn
-rw-r--r--. 1 root   root    48K Mar 26 20:13 cfn-init-cmd.log
-rw-r--r--. 1 root   root   1.8K Mar 26 20:13 cfn-init.log
-rw-r--r--. 1 root   root   3.0K Mar 26 20:58 cfn-wire.log
-rw-r--r--. 1 root   root    525 Mar 26 21:07 cfn-hup.log

[centos@mst1 log]$ cat cfn-init.log
2018-03-26 20:11:07,075 [INFO] -----------------------Starting build-----------------------
2018-03-26 20:11:07,076 [INFO] Running configSets: default
2018-03-26 20:11:07,077 [INFO] Running configSet default
2018-03-26 20:11:07,078 [INFO] Running config config
2018-03-26 20:11:07,107 [INFO] Command 10_parted_imsdb succeeded
2018-03-26 20:11:07,135 [INFO] Command 15_parted_imagebin succeeded
2018-03-26 20:11:07,153 [INFO] Command 20_mkpart_imsdb succeeded
2018-03-26 20:11:07,177 [INFO] Command 25_mkpart_imagebin succeeded
2018-03-26 20:11:07,663 [INFO] Command 30_format_imsdb succeeded
2018-03-26 20:11:08,518 [INFO] Command 35_format_imagebin succeeded
2018-03-26 20:11:08,530 [INFO] Command 40_createdir_imsdb succeeded
2018-03-26 20:11:08,534 [INFO] Command 45_createdir_imagebin succeeded
2018-03-26 20:11:08,576 [INFO] Command 50_put_imsdb_in_fstab succeeded
2018-03-26 20:11:08,598 [INFO] Command 55_put_imagebin_in_fstab succeeded
2018-03-26 20:11:08,612 [INFO] Command 60_create_swap succeeded
2018-03-26 20:11:08,626 [INFO] Command 65_attach_swap succeeded
2018-03-26 20:11:08,650 [INFO] Command 70_put_swap_in_fstab succeeded
2018-03-26 20:11:08,722 [INFO] Command 75_mount_all succeeded
2018-03-26 20:13:12,517 [INFO] Command 80_yum_update succeeded
2018-03-26 20:13:12,719 [INFO] Command 85_clear_yum_cache succeeded
2018-03-26 20:13:12,720 [INFO] ConfigSets completed
2018-03-26 20:13:12,721 [INFO] -----------------------Build complete-----------------------
2018-03-26 20:13:12,991 [DEBUG] CloudFormation client initialized with endpoint https://cloudformation.ca-central-1.amazonaws.com
2018-03-26 20:13:12,992 [DEBUG] Signaling resource instancei01dfb9d47f9d3face in stack PACS-stack-CF with unique ID i-059f12b383161bb21 and status SUCCESS

[centos@mst1 log]$ tail cfn-init-cmd.log
2018-03-26 20:13:12,518 P8946 [INFO] ============================================================
2018-03-26 20:13:12,518 P8946 [INFO] Command 85_clear_yum_cache
2018-03-26 20:13:12,718 P8946 [INFO] -----------------------Command Output-----------------------
2018-03-26 20:13:12,719 P8946 [INFO]    Loaded plugins: fastestmirror
2018-03-26 20:13:12,719 P8946 [INFO]    Cleaning repos: base epel extras updates
2018-03-26 20:13:12,719 P8946 [INFO]    Cleaning up everything
2018-03-26 20:13:12,719 P8946 [INFO]    Maybe you want: rm -rf /var/cache/yum, to also free up space taken by orphaned data from disabled or removed repos
2018-03-26 20:13:12,719 P8946 [INFO]    Cleaning up list of fastest mirrors
2018-03-26 20:13:12,719 P8946 [INFO] ------------------------------------------------------------
2018-03-26 20:13:12,719 P8946 [INFO] Completed successfully.

cfn-hup is the AWS utility that listens to any manual modifications to the stack during its lifetime.

cfn-signal is the AWS utility that sends a signal, e.g. SUCCESS, FAIL, etc. to AWS CloudFormation at the end of resource creation. In the present template it is used in the EC2 instance sections.*/
