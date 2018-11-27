## Creating a protocol

EnOS Edge supports various communication protocols. This article instructs how to develop your own protocol and upload the package that defines the protocol.

### Procedure

1. Click **Edge Gateway > Edge Management** in the navigation menu.
2. Click **Create Protocol** and provide the following settings:

  - **Name** (required): A complete protocol name takes the format of `Protocol Type - Role – Name`. For a standard protocol, please use the default Std as the name. For example, if the protocol type is Modbus TCP, the role is Client, and the name is Std, the user only needs to fill in Std, and the system will splice the complete protocol name "Modbus TCP-Client-Std". The developer names the protocol according to the naming convention.

  - **Type** (required): Click the drop-down menu for protocol types, which displays some common protocol types that are predefined, such as Modbus RTU, Modbus TCP, DL645/97, DL645/07, IEC104, etc. The developer selects the type as needed based on the new protocol to be created.

  - **Version** (required): Click the drop-down menu for protocol types, which displays some common protocol types that are predefined, such as Modbus RTU, Modbus TCP, DL645/97, DL645/07, IEC104, etc. The developer selects the type as needed based on the new protocol to be created.

  - **Jar package** (required): A jar package of the protocol program running on the platform, that is, the jar package running on EnOS™ Edge. The developer can click "Upload" to upload the protocol jar package.

  - **Description** (required): The developer may fill in the description dialog with a description of the applicable scenario, version,etc. of the protocol.
