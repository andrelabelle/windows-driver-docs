---
title: Multifunction device driver design guide
author: windows-driver-content
description: Multifunction device driver design guide
MS-HAID:
- 'mf-supp\_5055ff9d-981b-4d05-8d10-6e1a084e0d11.xml'
- 'multifunc.multifunction\_devices'
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 110c0b9b-4870-4853-8fbf-a9faf0c5f2ca
keywords: ["multifunction devices WDK", "multifunction devices WDK , about multifunction devices", "combined function devices WDK", "multiple function devices WDK", "printer multiple functions WDK", "DVD/CD-ROM multiple functions WDK", "multifunction devices WDK , installing", "parent buses WDK multifunction devices", "INF files WDK multifunction devices"]
---

# Multifunction device driver design guide


## <a href="" id="ddk-supporting-multifunction-devices-dg"></a>


A *multifunction device* occupies one location on its parent bus but contains more than one function. Combination printer/scanner/fax devices and modem/network cards are common multifunction devices.

In a multifunction device, the individual functions are independent. This means the functions must have the following characteristics:

-   The functions cannot have start-order dependencies.

-   The resource requirements for one function cannot be expressed in terms of the resources of another function (for example, *function1* uses I/O port *x* and *function2* uses port *x* + 200).

-   Each function must be able to operate as a separate device, even if it is serviced by the same drivers as another function.

Additionally, the following requirements must be met so that a multifunction device can be correctly configured on an NT-based platform:

-   Each function on the device must be enumerated.

-   Resource requirements for each function must be communicated to the PnP manager.

-   There must be INF files and drivers for each function.

The component responsible for each of these tasks depends on the multifunction standard for the device's parent bus, the extent to which the device conforms to the standard, and the capabilities of the parent bus driver.

If the device complies with the multifunction standards for its bus, your driver requirements are significantly reduced. Industry-wide multifunction standards have been defined for the PC Card and PCI buses. For the applicable hardware standards and guidelines, see the [multifunction devices](http://go.microsoft.com/fwlink/p/?linkid=8758) Web site.

If you are designing a multifunction printer, please follow the hardware, firmware and software recommendations available in the [Multifunction Printer Design Recommendations](http://go.microsoft.com/fwlink/p/?linkid=38442) white paper.

If you are working with a multifunction DVD/CD-ROM device used for data storage (not for audio/video playback) on a Windows XP or later operating system, you should use the system-supplied WDM DVD class driver, which treats the device as a single logical unit. For Windows 2000 and Windows 98, you should treat the device as two logical units (so it will appear with two drive letters). If the DVD capabilities are combined with some other type of function, you should treat the device as a single logical unit and supply a class driver that implements common command sets for all the features of the device. For more information, see the [DVD technology](http://go.microsoft.com/fwlink/p/?linkid=8754) Web site.

For a multifunction device that combines other functionality, you can use a system-supplied driver and INF file if the device complies with the multifunction standards for its bus. The system supplied multifunction driver (mf.sys) can handle the bus-level enumeration and resource allocation requirements for the device, and the system-supplied INF (mf.sys) can install the multifunction device. You need to supply only a function driver and INF file for each of the individual device functions.

If the device does not comply with the standard for its bus, you might need to supply a driver equivalent to mf.sys in functionality, in addition to function drivers and INF files for the device functions.

To install a multifunction device, you typically provide a base INF file for the device and an additional INF file for each of the device's functions. The base INF file typically copies the INF files for the device's individual functions. For information about how to accomplish this, see [Copying INFs](https://msdn.microsoft.com/library/windows/hardware/ff540117).

The following sections describe driver and installation requirements for various types of multifunction devices:

[Supporting Multifunction PC Card Devices](supporting-multifunction-pc-card-devices.md)

[Supporting Multifunction PCI Devices](supporting-multifunction-pci-devices.md)

[Supporting Multifunction Devices On Other Buses](supporting-multifunction-devices-on-other-buses.md)

[Using the System-Supplied Multifunction Bus Driver](using-the-system-supplied-multifunction-bus-driver.md)

[Creating Resource Maps for a Multifunction Device](creating-resource-maps-for-a-multifunction-device.md)

See [INF File Sections and Directives](https://msdn.microsoft.com/library/windows/hardware/ff547433) for information about INF file syntax.

The rest of this section describes how to support multifunction devices on Windows 2000 and later NT-based platforms only.

The Windows Driver Kit (WDK) includes a separate section that describes how to support [multifunction audio devices](https://msdn.microsoft.com/library/windows/hardware/ff537574).

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bmultifunc\multifunc%5D:%20Multifunction%20device%20driver%20design%20guide%20%20RELEASE:%20%288/29/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")


