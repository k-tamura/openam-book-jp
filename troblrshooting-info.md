## Recording Troubleshooting Information

The OpenAM recording facility lets you initiate events to monitor OpenAM while saving output that is useful when performing troubleshooting.

OpenAM recording events save four types of information:

    OpenAM debug logs

    Thread dumps, which show you the status of every active thread, with output similar to a JStack stack trace

    Important run-time properties

    The OpenAM configuration

You initiate a recording event by invoking the ssoadm start-recording command or by using the start action of the /json/records REST API endpoint. Both methods use JSON to control the recording event.

This section describes starting and stopping recording using the ssoadm command, using a JSON file to configure the recording event, and locating the output recorded information. For information about using the /json/records REST API endpoint to activate and deactivate recording, see RESTful Troubleshooting Information Recording.

### Starting and Stopping Recording

Start OpenAM recording with the ssoadm start-recording command. For example:

```
$ ssoadm \
 start-recording \
 --servername http://openam.example.com:8080/openam \
 --adminid amadmin \
 --password-file /tmp/pwd.txt \
 --jsonfile recording.json

{
 "recording":true,
 "record": {
  "issueID":103572,
  "referenceID":"policyEvalFails",
  "description":"Record everything",
  "zipEnable":false,
  "threadDump": {
   "enable":true,
   "delay": {
     "timeUnit":"SECONDS",
     "value":5
   }
  },
  "configExport": {
   "enable":true,
   "password":"admin password",
   "sharePassword":true
  },
  "debugLogs": {
   "debugLevel":"message",
   "autoStop": {
    "time": {
     "timeUnit":"MILLISECONDS",
     "value":15000
    },
    "fileSize": {
     "sizeUnit":"KB",
     "value":1048576
    }
   }
  },
  "status":"RUNNING",
  "folder":"/home/openam/debug/record/103572/policyEvalFails/"
 }
}
```

Note

The ssoadm command output in the preceding example is shown in indented format for ease of reading. The actual output is not indented.

In the preceding ssoadm start-recording command example, the recording.json file specifies the information to be recorded and under what conditions recording automatically terminates. This file is known as the recording control file. The Recording Control File describes the format of recording control files and provides an annotated example.

An active recording event stops when:

    You explicitly tell OpenAM to stop recording by executing the ssoadm stop-recording command. See the OpenAM Reference for details about this command.

    Another ssoadm start-recording command is sent to OpenAM that specifies an issue ID other that differs from the active recording event's issue ID. In this case, the initial recording session terminates and the new recording event starts. Note that you can determine whether an OpenAM recording event is active by using the ssoadm get-recording-status command.

    A timer configured in the recording control file determines that the maximum amount of time for the recording event has been reached.

    A file size monitor configured in the recording control file determines that the maximum amount of information in debug logs has been reached.

#### The Recording Control File

A JSON file that is input to the ssoadm start-recording command controls the amount of information OpenAM records, the recording duration, and the location of recording output files.
24.5.2.1. File Format


24.5.2.2. Recording Control File Example

The following is an example of a recording control file:

```
{
  "issueID": 103572,
  "referenceID": "policyEvalFails",
  "description": "Troubleshooting artifacts in support of case 103572",
  "zipEnable": true,
  "configExport": {
    "enable": true,
    "password": "5x2RR70",
    "sharePassword": false
  },
  "debugLogs": {
    "debugLevel": "MESSAGE",
    "autoStop": {
      "time":  {
        "timeUnit": "SECONDS",
        "value": 15
      },
      "fileSize": {
        "sizeUnit": "GB",
        "value": 1
      }
    }
  },
  "threadDump" : {
    "enable": true,
    "delay" :  {
      "timeUnit": "SECONDS",
      "value": 5
     }
  }
}    
```

The recording control file properties in the preceding example affect the recording output as follows:

表. Recording Control File Example Properties and Their Effect on Recording Behavior

Recording Control File Property	Value	Effect
issueID, referenceID 	103572, policyEvalFails 	Recording output is stored at the path debugFileLocation/record/103572/policyEvalFails_timestamp.zip. For more information about the location of recording output, see Retrieving Recording Information .
Description 	Troubleshooting artifacts in support of case 103572 	No effect.
zipEnable 	true 	Recording output is compressed into a zip file.
configExport / enable 	true 	The OpenAM configuration is exported at the start of the recording event.
configExport / password 	5x2RR70 	Knowledge of this password will be required to access the OpenAM configuration that was saved during recording.
configExport / sharePassword 	false 	The password is not displayed in output messages displayed during the recording event or in the info.json file.
debugLogs / debugLevel 	MESSAGE 	Recording enables message-level debug logs during the recording event.
debugLogs / autoStop / time 	SECONDS, 15 	Because both the time and fileSize properties are set, recording stops after 15 seconds, or after the size of the debug logs exceeds 1 GB, whichever occurs first.
debugLogs / autoStop / fileSize 	GB, 1 	Because both the time and fileSize properties are set, recording stops after 15 seconds, or after the size of the debug logs exceeds 1 GB, whichever occurs first.
threadDump / enable 	true 	Thread dumps are taken throughout the recording event.
threadDump / delay 	SECONDS, 5 	The first thread dump is taken when the recording event starts. Additional thread dumps are taken every five seconds hence.


### Retrieving Recording Information

Information recorded by OpenAM is stored at the path debugFileLocation/record/issueID/referenceID . For example, if the debug file location is /home/openam/debug, the issue ID 103572, and the reference ID policyEvalFails, the path containing recorded information is /home/openam/debug/record/103572/policyEvalFails.

When there are multiple recording events with the same issueID and referenceID, OpenAM appends a timestamp to the referenceID of the earliest paths. For example, multiple recording events for issue ID 103572 and reference ID policyEvalFails might be stored at the following paths:

    Most recent recording: debugFileLocation/record/103572/policyEvalFails

    Next most recent recording: debugFileLocation/record/103572/policyEvalFails_2015-10-24-11-48-51-902-PDT

    Earliest recording: debugFileLocation/record/103572/policyEvalFails_2015-08-10-15-15-10-140-PDT

OpenAM compresses the output from recording events when you set the zipEnable property to true. The output file can be found at the path debugFileLocation/record/issueID/referenceID_timestamp.zip. For example, compressed output for a recording event for issue ID 103572 and reference ID policyEvalFails might be stored at the following path: debugFileLocation/record/103572/policyEvalFails_2015-08-12-12-19-02-683-PDT.zip.

Use the referenceID property value to segregate output from multiple problem recreations associated with the same case. For example, while troubleshooting case 103572, you notice that you only have a problem when evaluating policy for members of the Finance realm. You could trigger two recording events as follows:
Table 24.4. Segregating Recording Output Using the referenceID Value
OpenAM Behavior	referenceIDValue	Recording Output Path
Policy evaluation behaves as expected for members of the Engineering realm. 	policyEvalSucceeds 	debugFileLocation/record/103572/policyEvalSucceeds
Policy evaluation unexpectedly fails for members of the Finance realm. 	policyEvalFails 	debugFileLocation/record/103572/policyEvalFails


