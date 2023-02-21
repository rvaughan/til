# Device Shadow

A [device shadow](https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html) is a JSON document that stores the current or desired state information for an AWS IoT thing, such as a client device. Custom components can access client devices' shadows to manage their state, even when the client device isn't connected to AWS IoT. Each AWS IoT thing has an unnamed shadow, and you can also create multiple named shadows for each thing.

Device shadows are used when you want to be able to update the configuration for an IoT device which is not currently connected.
