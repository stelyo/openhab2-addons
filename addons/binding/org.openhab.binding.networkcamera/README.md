# Network Camera Binding

This binding integrates large number of network cameras which can send images to a FTP server when motion or sound is detected.
Binding acts as a FTP server.
Images stored on the FTP server are not saved to the file system, therefore the binding shouldn't cause any problems on flash based openHAB installations.


## Supported Things

This binding supports ```motiondetection``` Thing. Every camera is identified by FTP user name. Therefore, every camera should use unique user name to login FTP server.

## Discovery

Automatic discovery is not supported.

## Binding Configuration

Bindings FTP server listening 2121 TCP port by default, but port can be configured. Also idle timeout can be configured.

## Channels

This binding currently supports following channels:

| Channel Type ID | Item Type    | Description                                                                            |
|-----------------|--------------|----------------------------------------------------------------------------------------|
| image           | Image        | Image received from network camera.                                                    |


### Trigger Channels

| Channel Type ID | Options                | Description                                        |
|-----------------|------------------------|----------------------------------------------------|
| motion-trigger  | MOTION_DETECTED        | Triggered when image received from network camera. |


## Full Example

Things:

```
Thing networkcamera:motiondetection:garage [ userName="garage", password="12345" ]
```

Items:

```
Switch Garage_NetworkCamera_Motion       { channel="networkcamera:motiondetection:garage:motion" } 
Image  Garage_NetworkCamera_Motion_Image { channel="networkcamera:motiondetection:garage:image" } 
```

Rules:

```
rule "example trigger rule"
when
    Channel 'networkcamera:motiondetection:garage:motion-trigger' triggered MOTION_DETECTED 
then
    logInfo("Test","MOTION DETECTED trigger example")
end
```

Sitemap:

```
Frame label="Garage network camera" icon="camera" {
    Image item=Garage_Motion_Image
}
```
        
## Logging and problem solving

For problem solving, if binding logging is not enough, Apache FTP server logging can also be enabled by the following command in the karaf console:

```
log:set DEBUG org.apache.ftpserver
```

and set back to default level:

```
log:set DEFAULT org.apache.ftpserver
```

If you meet any problems to receive images from the network cameras, you could test connection to binding with any FTP client. You can send image files via FTP client and thing channels should be updated accordingly.
 