# firmware-tooling

Auxiliary tools for BACON firmware development.

## GAS

The Gateway Appliance Simulator (or just GAS for short) allows for the Simulation of either an Appliance or a Gateway within the Context of Device Communication. This allows Users to test the way their Appliances or Gateways receive data or respond to requests. 

The GAS is based on three central concepts:

Resource (IResource): The smallest unit of data. It represents a single, addressable data point, such as a sensor value, a configuration setting, or a status.

Parameter Database (ParameterDatabase): Manages a collection of resources. It is responsible for serializing (Pull) and deserializing (Push) data blocks and implements the logic for handling transmissions that are larger than the maximum payload.

Resource Group (ResourceGroup): A logical container that bundles a database and serves as the primary interface for the application. It controls access to resources, checks permissions, and manages pending operations.

### IResource Interface

The IResource interface is the fundamental abstraction for all data points in the system. Every resource, regardless of its data type, must implement this interface.

Implementations:

- Uint8Resource: For a single 8-bit value.

- Uint16Resource: For a 16-bit integer value (little-endian).

- StringResource: For an ASCII string with a defined maximum length.

### ParameterDatabase Interface

This interface abstracts the management, storage, and serialization of a collection of IResource instances.

Implementations:

StaticDatabase: A simple implementation for a fixed number of resources that are always read or written as a whole.

DynamicDatabaseV1 / DynamicDatabaseV2: Advanced implementations that can transfer large amounts of data through pagination (splitting into multiple packets). They are suitable for use cases where the total size of the resources exceeds the maximum packet size (MaxPayloadLength).

### ResourceGroup Struct

The ResourceGroup is the primary control unit for the application. It encapsulates a ParameterDatabase and the logic for managing communication cycles.

### Important Data Structures

- OutgoingMessage: Represents an outgoing message with an action (e.g., OpReturn), parameters, and status.

- BLWPError: Defines a protocol-specific error that contains a standardized status code.

- PendingType: Describes the state of a ResourceGroup, e.g., whether it is waiting for a read or write response.

### Requirements

If you just want to run the Simulator on your device you can grab one of the executables from the releases. (https://github.com/bosch-bacon/firmware-tooling/releases)

If you want to develop GAS you will need Golang Version 1.25.0 or newer, Node Version 24 or newer and Wails.

Go can be installed using their website (https://go.dev/dl) or using the package manager of your choice.
Node can be installed using nvm or fnm, instructions on the installation can be found here: https://nodejs.org/en/download 
Wails is used to generate the frontend for the Simulator, instructions on how to install Wails can be found here: https://wails.io/

### Running it

`wails dev` is used for spinning up a development server for wails, just navigate into the root directory of the project and run the command.
If you have all dependancies installed it should open the GUI.

<!-- ### <TODO> Datamodel -->

<!-- ### <TODO> Errortypes/Errorhandling -->

## Running in Devcontainer

In order for `wails dev` to work inside the devcontainer, you need to allow connections to the `xserver`
of the host machine.

Run

```
xhost +SI:localuser:$(id -un)
```

to allow access of your user to it. If the container is executed as `root`, run

```
xhost +SI:localuser:root
```

instead.

