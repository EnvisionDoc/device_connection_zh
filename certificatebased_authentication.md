# 基于证书的双向认证

Security is a critical in an IoT system. When certificate authentication is enabled in the product configuration，EnOS enforces the following security schemes to secure the connection between the EnOS Edge and EnOS IoT Hub:

- The communications between EnOS Edge and EnOS IoT Hub are enfored to use certificate-based bi-directional authentication.
- Support for RSA algorithm to verify signature, with enforcement for 2048 bits.

## Setup phase

The following diagram illustrates the process of secure communication between the edge and IoT Hub based on X.509 certificates:


### 1. IoT Hub acquires X.509 certificate
![image](media/certificate_service_secure_communication_01.png)

1a. The IoT Hub creates key pairs and CSR locally, acquires the X.509 certificate with the CSR by using the X.509 Certificate Service API.

1b. The EnOS CA issues the X.509 certificate and sends the certificate to the IoT Hub.

1c. The IoT Hub receives and stores the X.509 certificate.


### 2. Edge acquires X.509 certificate
![image](media/certificate_service_secure_communication_02.png)

2a. Edge设备出厂预烧录了产品证书（`ProductKey`，`ProductSecret`），edge设备序列号（SN）。设备上电联网以后，上报产品证书以及序列号至云端去动态激活。如果云端鉴权认证通过，会返回`DeviceSecret`给edge。


2b. 在IoT Hub上使用edge设备序列号作为`DeviceKey`预注册edge设备，可以在EnOS控制台注册设备，也可以通过调用REST API接口注册设备。


2c. The edge receives the responses from the IoT Hub, creates key pairs and CSR, calls the API to get its X.509 certificate, 同时使用设备三元组登录至云端，设备的第一次登录会激活该设备.

2d. The IoT Hub receives the CSR from the edge, after verifing its identity, forward the CSR to the EnOS CA.

2e. The EnOS CA receives the CSR, issues the edge certificate and responds to the IoT Hub.

2f. The IoT Hub receives the issued X.509 certificate, binds it with the device id, and then sends the edge certificate to the edge.

2g.The edge receives the edge certificate, saves them securely in the local repository, for example, the Trusted Platform Module (TPM).

## Communication phase

The diagram below illustrates the certificate-basd authentication process:


### 3. Edge commmunicates with the IoT Hub through certificate-based bi-directional authentication
![image](media/certificate_service_secure_communication_03.png)

3a. The edge validates the certificate of the IoT Hub.

3b. The IoT Hub validates the certificate of the edge.

When the TLS handshake in step 3a and 3b succeeds, the TLS connection is established between the edge and the IoT Hub.

3c. The edge transmits device telemetry through MQTT over the TLS connection.

3d. The IoT Hub transmits configurations and control signals through MQTT over the TLS connection.

## Revocation phase

Under some circumstances, user needs to revoke the X.509 certificate of the edge. The following diagram illustrates the revocation process.

### 4. The IoT Hub revokes the X.509 certificate of the edge
![image](media/certificate_service_secure_communication_04.png)

4a. The IoT Hub calls the revocation API to revoke the X.509 certificate with the serial number of the certificate.

4b. The EnOS CA receives the request from the IoT Hub, verifies the identity, revokes the certificate, and updates the CRL.

## Edge security best practices

In the certificate-based security connection, consider the following best practices to secure the edge:

- Create the private key for the edge and keep it secret in a storage such as TPM.
- Use TLS 1.2 when communicating with the IoT Hub, and verify that the server certificate is valid.
- Each edge must have a unique public/private key pair.
- The key pair used to be authenticated by IoT Hub should not be used for other purposes or communications through other protocols.
- The key must be revoked when the edge is reset.
- When your edge runs on an operating system, make sure your operating system is secured through certain machanisms, for example, firewall.
- Ensure that you have a way to update root CA certificates and CRL.
- Ensure that the clock on the edge is not tampered with.
