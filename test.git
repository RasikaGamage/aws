#s script is used for taking snapshots for SVT EOMSYS db vms. The procedure is as below:
 #1. stop the db instances in sequence: standby db servers; master db servers
 #2. take snapshots
 #3. start the instances in sequence: master db servers; standby db servers.


export AWS_DEFAULT_REGION="ap-southeast-2"
export AWS_PROFILE="home"
ec2UsmDbFilter="EOMSYS-EC2Instances-SVT_ec2UsmDb"
ec2BpmDbFilter="EOMSYS-EC2Instances-SVT_ec2BpmDb"
logFile="/tmp/snapshotlog"
#export aws_access_key_id="AKIAJWJ5AVDPRI75RAMA"
#export aws_secret_access_key="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"

 vmRunning="running"
 vmStopped="stopped"
 currStatus=""


 function checkVmStatus() {
         statusVMExp=$1
         vmId=$2
         if [[ -n "$statusVMExp" && -n "$vmId" ]]; then
                 echo "checking the vm status, the vm is $vmId"


                 waitLoop=0
                 currStatus=$(aws ec2 describe-instances --instance-ids $vmId --query 'Reservations[].Instances[].State.Name' --output text)
                 while [ $currStatus != $statusVMExp ]
                 do
                         let "waitLoop++"
                         if [ $waitLoop -ge 5 ]; then echo "the vm reponse time out!" && break; fi
                         sleep 15
 #                        sleep 1
                         currStatus=$(aws ec2 describe-instances --instance-ids $vmId --query 'Reservations[].Instances[].State.Name' --output text)
                         echo "$currStatus"
                 done
                 echo "after waiting,, the vm stauts is: $currStatus"
         else
                 currStatus=""
                 echo "something wrong with the status or instanceId!"
         fi
 }


 if [ -f $logFile ]
 then
         cat /dev/null > $logFile
 else
         touch $logFile
 fi

