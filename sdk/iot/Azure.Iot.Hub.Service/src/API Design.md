# Azure Iot Hub Service API Design Doc
This document outlines the APIs for the Azure Iot Hub Service SDK

<details><summary><b>Type definition names</b></summary>
    
```
Configuration - TwinConfiguration
Module - ModuleIdentity
Device - DeviceIdentity
Twin - TwinData
Interface - PnpInterface
Property - PnpProperty
Reported - PnpReported
Desired - PnpDesired
```
</details>

<details><summary><b>Constructors</b></summary>
    
```csharp

```
</details>

<details><summary><b>Configurations</b></summary>
APIs for managing configurations for devices and modules

```csharp

```
</details>

<details><summary><b>Statistics</b></summary>
APIs for getting statistics about devices and modules, as well as service statistics

```csharp

```
</details>

<details><summary><b>Devices</b></summary>
APIs for managing device identities, device twins, and querying devices

```csharp
public class Devices
{
    /// <summary>
    /// Create a device.
    /// </summary>
    /// <param name="device">The device to create.</param>
    /// <param name="cancellationToken">The cancellation token.</param>
    /// <returns>The created device.</returns>
    public virtual Response<DeviceIdentity> Create(DeviceIdentity device, CancellationToken cancellationToken = default)
		
    /// <summary>
    /// Update a device.
    /// </summary>
    /// <param name="device">The device to update.</param>
    /// <param name="ifMatch">A string representing a weak ETag for this device, as per RFC7232. The update operation is performed
    /// only if this ETag matches the value maintained by the server, indicating that the device has not been modified since it was last retrieved.
    /// The current ETag can be retrieved from the device identity last retrieved from the service. To force an unconditional update, set If-Match to the wildcard character (*).</param>
    /// <param name="cancellationToken">The cancellation token.</param>
    /// <returns>The created or updated device.</returns>
    public virtual Response<DeviceIdentity> Update(DeviceIdentity device, string ifMatch = null, CancellationToken cancellationToken = default)

    /// <summary>
    /// Get a single device.
    /// </summary>
    /// <param name="deviceId">The unique identifier of the device to get.</param>
    /// <param name="cancellationToken">The cancellation token.</param>
    /// <returns>The retrieved device.</returns>
    public virtual Response<DeviceIdentity> Get(string deviceId, CancellationToken cancellationToken = default)

    /// <summary>
    /// Delete a single device.
    /// </summary>
    /// <param name="deviceId">The unique identifier of the device to delete.</param>
    /// <param name="ifMatch">A string representing a weak ETag for this device, as per RFC7232. The delete operation is performed
    /// only if this ETag matches the value maintained by the server, indicating that the device has not been modified since it was last retrieved.
    /// The current ETag can be retrieved from the device identity last retrieved from the service. To force an unconditional delete, set If-Match to the wildcard character (*).</param>
    /// <param name="cancellationToken">The cancellation token.</param>
    /// <returns>The http response.</returns>
    public virtual Response Delete(string deviceId, string ifMatch = null, CancellationToken cancellationToken = default)

	/// <summary>
	/// Create multiple devices.
	/// </summary>
	/// <param name="devices">The devices to create.</param>
	/// <param name="cancellationToken">The cancellation token.</param>
	/// <returns>The result of the bulk operation.</returns>
	public virtual Response<BulkRegistryOperationResult> Create(IEnumerable<DeviceIdentity> devices, CancellationToken cancellationToken = default)

    /// <summary>
    /// Create multiple devices with an initial twin.
    /// </summary>
    /// <param name="devices">The pairs of devices their twins that will be created. For fields such as deviceId
    /// where device and twin have a definition, the device value will override the twin value.</param>
    /// <param name="cancellationToken">The cancellation token.</param>
    /// <returns>The result of the bulk operation.</returns>
	public BulkRegistryOperationResult CreateWithTwin(Dictionary<DeviceIdentity, TwinData> devices, CancellationToken cancellationToken = default)

	/// <summary>
	/// Update multiple devices.
	/// </summary>
	/// <param name="deviceUpdates">The devices to update.</param>
	/// <param name="force">If true, the devices will be updated even if their ETag is out of date.
	/// If false, each device will only be updated if its ETag is up to date.</param>
	/// <param name="cancellationToken">The cancellation token.</param>
	/// <returns>The result of the bulk operation.</returns>
	public virtual Response<BulkRegistryOperationResult> Update(IEnumerable<DeviceIdentity> deviceUpdates, bool force, CancellationToken cancellationToken = default)

	/// <summary>
	/// Delete multiple devices.
	/// </summary>
	/// <param name="devices">The devices to delete.</param>
	/// <param name="force">If true, the devices will be deleted even if their ETag is out of date.
	/// If false, each device will only be deleted if its ETag is up to date.</param>
	/// <param name="cancellationToken">The cancellation token.</param>
	/// <returns>The result of the bulk deletion.</returns>
	public virtual Response<BulkRegistryOperationResult> Delete(IEnumerable<DeviceIdentity> devices, bool force, CancellationToken cancellationToken = default)

	/// <summary>
	/// List a set of device twins.
	/// </summary>
	/// <param name="pageSize">The size of each page while paging over the device twins.</param>
	/// <param name="cancellationToken">The cancellation token.</param>
	/// <returns>A pageable set of device twins.</returns>
	public virtual Pageable<TwinData> ListTwins(string pageSize = null, CancellationToken cancellationToken = default)

	/// <summary>
	/// Get a device's twin.
	/// </summary>
	/// <param name="deviceId">The unique identifier of the device to get the twin of.</param>
	/// <param name="cancellationToken">The cancellation token</param>
	/// <returns>The device's twin, including reported properties and desired properties.</returns>
	public virtual Response<TwinData> GetTwin(string deviceId, CancellationToken cancellationToken = default)

	/// <summary>
	/// Replace a device's twin.
	/// </summary>
	/// <param name="deviceId">The unique identifier of the device to replace the twin on.</param>
	/// <param name="twin">The new twin for the device. This replaces all properties on the old twin, even if the new twin doesn't have the same properties.</param>
	/// <param name="ifMatch">A string representing a weak ETag for this twin, as per RFC7232. The replace operation is performed
	/// only if this ETag matches the value maintained by the server, indicating that the twin has not been modified since it was last retrieved.
	/// To force an unconditional replace, set If-Match to the wildcard character (*).</param>
	/// <param name="cancellationToken"></param>
	/// <returns>The server's new representation of the device twin.</returns>
	public virtual Response<TwinData> ReplaceTwin(string deviceId, TwinData twin, string ifMatch = "*", CancellationToken cancellationToken = default)

	/// <summary>
	/// Update a device's twin.
	/// </summary>
	/// <param name="deviceId">The unique identifier of the device to update the twin on.</param>
	/// <param name="twinPatch">The properties to update. Any existing properties not referenced by this patch will be unaffected by this patch.</param>
	/// <param name="ifMatch">A string representing a weak ETag for this twin, as per RFC7232. The update operation is performed
	/// only if this ETag matches the value maintained by the server, indicating that the twin has not been modified since it was last retrieved.
	/// To force an unconditional update, set If-Match to the wildcard character (*).</param>
	/// <param name="cancellationToken">The cancellation token</param>
	/// <returns>The server's new representation of the device twin.</returns>
	public virtual Response<TwinData> UpdateTwin(string deviceId, TwinData twinPatch, string ifMatch = "*", CancellationToken cancellationToken = default)

	/// <summary>
	/// Update multiple devices' twins.
	/// </summary>
	/// <param name="twinUpdates">The new twins to replace the twins on existing devices</param>
	/// <param name="force">If true, all the update operations will ignore the provided twin ETags and will
	/// force the update. If false, each update operation will fail if the provided ETag for the update is out of date.</param>
	/// <param name="cancellationToken">The cancellation token</param>
	/// <returns>The result of the bulk operation.</returns>
	public virtual Response<BulkRegistryOperationResult> UpdateTwins(IEnumerable<TwinData> twinUpdates, bool force, CancellationToken cancellationToken = default)

	/// <summary>
	/// Invoke a method on a device.
	/// </summary>
	/// <param name="deviceId">The unique identifier of the device to invoke the method on.</param>
	/// <param name="directMethodRequest">The details of the method to invoke.</param>
	/// <param name="cancellationToken">The cancellation token.</param>
	/// <returns>The result of the method invocation.</returns>
	public virtual Response<CloudToDeviceMethodResult> InvokeMethod(string deviceId, CloudToDeviceMethod directMethodRequest, CancellationToken cancellationToken = default)
}
```
</details>

<details><summary><b>Modules</b></summary>
APIs for managing module identities, module twins, and querying modules

```csharp

```
</details>

<details><summary><b>Jobs</b></summary>
APIs for using IotHub v2 jobs

```csharp

```
</details>

<details><summary><b>Messages</b></summary>
Feedback messages, sending cloud to device messages (missing from current swagger), and purging cloud to device message queue
```csharp

```
</details>

<details><summary><b>Files</b></summary>
APIs for getting file upload notifications (missing from current swagger)

```csharp

```
</details>

<details><summary><b>Fault Injection</b></summary>
Not sure if we'll expose these

```csharp

```
</details>

<details><summary><b>Query</b></summary>
APIs for querying on device or module identities

```csharp

```
</details>
