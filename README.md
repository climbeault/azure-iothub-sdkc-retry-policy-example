# azure-iothub-sdkc-retry-policy-example

**Introduction**

This is an example of code, based on the iothub_client_sample_amqp_shared.c sample, provided in the 2018/03/02 version of the IoT SDK-C.
The sample code has been modified in the following way:

- Remove all references to the 2nd device.
- Set the message count to 1.
- Add a connection status callback that simply print the result/reason and register it with the hub client.
- Add enum strings for connection status result/reason.
- Set the retry policy to none.

**Purpose**

The purpose is to show the behavior of the SDK, when sending a message while a communication error is forced and while the retry policy is set to none. To force the error, a valid IoT hub and a valid device are used, while the device key is wrong, causing an authentication error.

**Result**

As show in the output below, the following can be observed:

- Although the retry policy is set to none, five message transmission attemts are made.
- As expected, each attempt gives a connection status reason with bad credential.
- After the fifth attempt, a "retry expired" connection status is given.
- The send confirmation callback is never called, not even with an error result.

**Output**

(Device and hub information has been removed)

```
Starting the IoTHub client sample AMQP...
IoTHubClient_SetMessageCallback...successful.
IoTHubClient_SendEventAsync accepted data for transmission to IoT Hub.
-> Header (AMQP 0.1.0.0)
<- Header (AMQP 0.1.0.0)
-> [OPEN]* ...
<- [OPEN]* ...
-> [BEGIN]* {NULL,0,4294967295,100,4294967295}
<- [BEGIN]* {0,1,5000,4294967295,262143,NULL,NULL,NULL}
-> [ATTACH]* {$cbs-sender,0,false,0,0,* {$cbs},* {$cbs},NULL,NULL,0,0}
-> [ATTACH]* {$cbs-receiver,1,true,0,0,* {$cbs},* {$cbs},NULL,NULL,NULL,0}
<- [ATTACH]* {$cbs-sender,0,true,0,0,* {$cbs,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL},* {$cbs,NULL,NULL,NULL,NULL,NULL,NULL},NULL,NULL,NULL,1048576,NULL,NULL,NULL}
<- [FLOW]* {0,5000,1,4294967295,0,0,100,0,NULL,false,NULL}
-> [TRANSFER]* {0,0,<01 00 00 00>,0,false,false}
<- [ATTACH]* {$cbs-receiver,1,false,0,0,* {$cbs,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL},* {$cbs,NULL,NULL,NULL,NULL,NULL,NULL},NULL,NULL,0,1048576,NULL,NULL,NULL}
-> [FLOW]* {1,4294967295,1,99,1,0,10000}
<- [DISPOSITION]* {true,0,NULL,true,* {},NULL}
<- [TRANSFER]* {1,0,<01 00 00 00>,0,NULL,false,NULL,NULL,NULL,NULL,false}
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_cbs_auth.c Func:_on_cbs_put_token_complete_callback Line:168 CBS reported status code 401, error: 'The specified SAS token has an invalid signature. It does not match either the primary or secondary key.' for put-token operation for device '...'
-> [DISPOSITION]* {true,0,0,true,* {}}
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_BAD_CREDENTIAL
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_common.c Func:_IoTHubTransport_AMQP_Common_Device_DoWork Line:1132 Failed performing DoWork for device '...' (device reported state 4; number of previous failures: 0)
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_OK
-> [TRANSFER]* {0,1,<02 00 00 00>,0,false,false}
<- [DISPOSITION]* {true,1,NULL,true,* {},NULL}
<- [TRANSFER]* {1,1,<02 00 00 00>,0,NULL,false,NULL,NULL,NULL,NULL,false}
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_cbs_auth.c Func:_on_cbs_put_token_complete_callback Line:168 CBS reported status code 401, error: 'The specified SAS token has an invalid signature. It does not match either the primary or secondary key.' for put-token operation for device '...'
-> [DISPOSITION]* {true,1,1,true,* {}}
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_BAD_CREDENTIAL
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_common.c Func:_IoTHubTransport_AMQP_Common_Device_DoWork Line:1132 Failed performing DoWork for device '...' (device reported state 4; number of previous failures: 1)
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_OK
-> [TRANSFER]* {0,2,<03 00 00 00>,0,false,false}
<- [DISPOSITION]* {true,2,NULL,true,* {},NULL}
<- [TRANSFER]* {1,2,<03 00 00 00>,0,NULL,false,NULL,NULL,NULL,NULL,false}
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_cbs_auth.c Func:_on_cbs_put_token_complete_callback Line:168 CBS reported status code 401, error: 'The specified SAS token has an invalid signature. It does not match either the primary or secondary key.' for put-token operation for device '...'
-> [DISPOSITION]* {true,2,2,true,* {}}
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_BAD_CREDENTIAL
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_common.c Func:_IoTHubTransport_AMQP_Common_Device_DoWork Line:1132 Failed performing DoWork for device '...' (device reported state 4; number of previous failures: 2)
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_OK
-> [TRANSFER]* {0,3,<04 00 00 00>,0,false,false}
<- [DISPOSITION]* {true,3,NULL,true,* {},NULL}
<- [TRANSFER]* {1,3,<04 00 00 00>,0,NULL,false,NULL,NULL,NULL,NULL,false}
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_cbs_auth.c Func:_on_cbs_put_token_complete_callback Line:168 CBS reported status code 401, error: 'The specified SAS token has an invalid signature. It does not match either the primary or secondary key.' for put-token operation for device '...'
-> [DISPOSITION]* {true,3,3,true,* {}}
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_BAD_CREDENTIAL
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_common.c Func:_IoTHubTransport_AMQP_Common_Device_DoWork Line:1132 Failed performing DoWork for device '...' (device reported state 4; number of previous failures: 3)
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_OK
-> [TRANSFER]* {0,4,<05 00 00 00>,0,false,false}
<- [DISPOSITION]* {true,4,NULL,true,* {},NULL}
<- [TRANSFER]* {1,4,<05 00 00 00>,0,NULL,false,NULL,NULL,NULL,NULL,false}
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_cbs_auth.c Func:_on_cbs_put_token_complete_callback Line:168 CBS reported status code 401, error: 'The specified SAS token has an invalid signature. It does not match either the primary or secondary key.' for put-token operation for device '...'
-> [DISPOSITION]* {true,4,4,true,* {}}
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_BAD_CREDENTIAL
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_common.c Func:_IoTHubTransport_AMQP_Common_Device_DoWork Line:1132 Failed performing DoWork for device '...' (device reported state 4; number of previous failures: 4)
Error: Time:Tue Mar 13 22:48:42 2018 File:V:\azure-iot-sdk-c\iothub_client\src\iothubtransport_amqp_common.c Func:_IoTHubTransport_AMQP_Common_DoWork Line:1611 Device '...' reported a critical failure; connection retry will be triggered.
Connection status with result = IOTHUB_CLIENT_CONNECTION_UNAUTHENTICATED and reason = IOTHUB_CLIENT_CONNECTION_RETRY_EXPIRED
```
