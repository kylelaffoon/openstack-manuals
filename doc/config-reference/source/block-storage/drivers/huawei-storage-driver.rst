====================
Huawei volume driver
====================

Huawei volume driver can be used to provide functions such as the logical
volume and snapshot for virtual machines (VMs) in the OpenStack Block Storage
driver that supports iSCSI and Fibre Channel protocols.

Version mappings
~~~~~~~~~~~~~~~~

The following table describes the version mappings among the Block Storage
driver, Huawei storage system and OpenStack:

.. list-table:: **Version mappings among the Block Storage driver and Huawei
   storage system**
   :widths: 30 35 10
   :header-rows: 1

   * - Description (Volume Driver Version)
     - Storage System Version
     - Volume Driver Version
   * - Create, delete, expand, attach, detach, manage, and unmanage volumes.

       Create, delete, manage and unmanage a snapshot.

       Copy an image to a volume

       Copy a volume to an image

       Create a volume from a snapshot

       Clone a volume

       QoS
     - OceanStor T series V1R5 C02/C30

       OceanStor T series V2R2 C00/C20/C30

       OceanStor V3 V3R1C10/C20 V3R2C10 V3R3C00

       OceanStor 2200V3 V300R005C00

       OceanStor 2600V3 V300R005C00

       OceanStor 18500/18800 V1R1C00/C20/C30 V3R3C00
     - 1.1.0

       1.1.1
   * - Volume Migration(version 1.1.1 or later)

       Auto zoning(version 1.1.1 or later)

       SmartTier(version 1.1.1 or later)

       SmartCache(version 1.1.1 or later)

       Smart Thin/Thick(version 1.1.1 or later)

       Replication V2.1(version 1.1.1 or later)
     - OceanStor T series V2R2 C00/C20/C30

       OceanStor V3 V3R1C10/C20 V3R2C10 V3R3C00

       OceanStor 2200V3 V300R005C00

       OceanStor 2600V3 V300R005C00

       OceanStor 18500/18800V1R1C00/C20/C30
     - 1.1.1
   * - SmartPartition(version 1.1.1 or later)
     - OceanStor T series V2R2 C00/C20/C30

       OceanStor V3 V3R1C10/C20 V3R2C10 V3R3C00

       OceanStor 2600V3 V300R005C00

       OceanStor 18500/18800V1R1C00/C20/C30
     - 1.1.1

Block Storage driver installation and deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ubuntu environment deployment
-----------------------------

The OpenStack standard deployment steps are as follows:

#. Before installation, delete all the installation files of Huawei OpenStack
   Driver. The default path may be:
   ``/usr/lib/python2.7/disk-packages/cinder/volume/drivers/huawei``.

   .. note::

      In this example, the version of Python is 2.7. If other version is
      used, make corresponding changes to the Driver path.

#. Copy OpenStack Cinder Driver to the Cinder Driver installation directory.
   Refer to step 1 to find the default directory.

#. Refer to chapter :ref:`huawei-driver-configuration` to complete the
   configuration.

#. After configuration, restart the ``cinder-volume`` service:

   .. code-block:: console

      service cinder-volume restart

#. Check the status of services using the :command:`cinder service-list`
   command. If the "State" of ``cinder-volume`` is ``up``, that means
   Cinder-Volume is OK.

   .. code-block:: console

    root@ubuntuL004:/# cinder service-list

    +---------------+---------------+------+-------+-----+--------------------------+---------------+
    |Binary         |Host           | Zone |Status |State|Updated_at                |Disabled Reason|
    +---------------+---------------+------+-------+-----+--------------------------+---------------+
    |cinderscheduler|ubuntuL004     | nova |enabled|up   |2016-02-01T16:26:00.000000|-              |
    +---------------+---------------+------+-------+-----+--------------------------+---------------+
    |cindervolume   |ubuntuL004@v3r3| nova |enabled|up   |2016-02-01T16:25:53.000000|-              |
    +---------------+---------------+------+-------+-----+--------------------------+---------------+

Red Hat OpenStack deployment
----------------------------

Red Hat OpenStack deployment steps are as follows:

#. Before installation, delete all the installation files of Huawei OpenStack
   Driver. The default path may be:
   ``/usr/lib/python2.7/disk-packages/cinder/volume/drivers/huawei``.

   .. note::

      In this example, the version of Python is 2.7. If other version is used,
      make corresponding changes to the Driver path.

#. Copy OpenStack Cinder Driver to Cinder Driver installation directory. Refer
   to step 1 to find the default directory.

#. Refer to chapter :ref:`huawei-driver-configuration` for the configurations.

#. After configuration, restart the ``cinder-volume`` service:

   .. code-block:: console

      systemctl start openstack-cinder-volume.service

#. Check the status of services using the :command:`cinder service-list`
   command. If the "State" of ``cinder-volume`` is ``up``, that means
   Cinder-Volume is OK.

   .. code-block:: console

    root@ubuntuL004:/# cinder service-list

    +------------------+---------------+------+-------+-----+--------------------------+----------------+
    |Binary            |Host           | Zone |Status |State|Updated_at                | Disabled Reason|
    +------------------+---------------+------+-------+-----+--------------------------+----------------+
    |cinderscheduler   |ubuntuL004     | nova |enabled|up   |2016-02-01T16:26:00.000000|-               |
    +------------------+---------------+------+-------+-----+--------------------------+----------------+
    |cindervolume      |ubuntuL004@v3r3| nova |enabled|up   |2016-02-01T16:25:53.000000|-               |
    +------------------+---------------+------+-------+-----+--------------------------+----------------+

.. _huawei-driver-configuration:

Volume driver configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section describes how to configure the Huawei volume driver for either
iSCSI storage or Fibre Channel storage.

**Pre-requisites**

When creating a volume from image, install the ``multipath`` tool and add the
following configuration keys in the ``[DEFAULT]`` configuration group of
the ``/etc/cinder/cinder.conf`` file:

.. code-block:: xml

   use_multipath_for_image_xfer = True
   enforce_multipath_for_image_xfer = True

To configure the volume driver, follow the steps below:

#. In ``/etc/cinder``, create a Huawei-customized driver configuration file.
   The file format is XML.
#. Change the name of the driver configuration file based on the site
   requirements, for example, ``cinder_huawei_conf.xml``.
#. Configure parameters in the driver configuration file.

   Each product has its own value for the ``Product`` parameter under the
   ``Storage`` xml block. The full xml file with the appropriate ``Product``
   parameter is as below:

   .. code-block:: xml

      <?xml version="1.0" encoding="UTF-8"?>
         <config>
            <Storage>
               <Product>PRODUCT</Product>
               <Protocol>iSCSI</Protocol>
               <ControllerIP1>x.x.x.x</ControllerIP1>
               <UserName>xxxxxxxx</UserName>
               <UserPassword>xxxxxxxx</UserPassword>
            </Storage>
            <LUN>
               <LUNType>xxx</LUNType>
               <StripUnitSize>xxx</StripUnitSize>
               <WriteType>xxx</WriteType>
               <MirrorSwitch>xxx</MirrorSwitch>
               <Prefetch Type="xxx" Value="xxx" />
               <StoragePool Name="xxx" />
               <StoragePool Name="xxx" />
            </LUN>
            <iSCSI>
               <DefaultTargetIP>x.x.x.x</DefaultTargetIP>
               <Initiator Name="xxxxxxxx" TargetIP="x.x.x.x"/>
            </iSCSI>
            <Host OSType="Linux" HostIP="x.x.x.x, x.x.x.x"/>
         </config>

    The corresponding ``Product`` values for each product are as below:

   * **For T series V1**

     .. code-block:: xml

        <Product>T</Product>

   * **For T series V2**

     .. code-block:: xml

        <Product>TV2</Product>

   * **For V3**

     .. code-block:: xml

        <Product>V3</Product>

   * **For OceanStor 18000 series**

     .. code-block:: xml

        <Product>18000</Product>

   The ``Protocol`` value to be used is ``iSCSI`` for iSCSI and ``FC`` for
   Fibre Channel as shown below:

   .. code-block:: xml

      # For iSCSI
      <Protocol>iSCSI</Protocol>

      # For Fibre channel
      <Protocol>FC</Protocol>

   .. note::

      For details about the parameters in the configuration file, see the
      `Configuration file parameters`_ section.

#. Configure the ``cinder.conf`` file.

   In the ``[default]`` block of ``/etc/cinder/cinder.conf``, add the following
   contents:

   * ``volume_driver`` indicates the loaded driver.

   * ``cinder_huawei_conf_file`` indicates the specified Huawei-customized
     configuration file.

   * ``hypermetro_devices`` indicates the list of remote storage devices for
     which Hypermetro is to be used.

   The added content in the ``[default]`` block of ``/etc/cinder/cinder.conf``
   with the appropriate ``volume_driver`` and the list of
   ``remote storage devices`` values for each product is as below:

   .. code-block:: ini

      volume_driver = VOLUME_DRIVER
      cinder_huawei_conf_file = /etc/cinder/cinder_huawei_conf.xml
      hypermetro_devices = {STORAGE_DEVICE1, STORAGE_DEVICE2....}

   .. note::

      By default, the value for ``hypermetro_devices`` is ``None``.


   The ``volume-driver`` values for each iSCSI product is as below:

   * **For T series V1**

     .. code-block:: ini

        # For iSCSI
        volume_driver = cinder.volume.drivers.huawei.huawei_t.HuaweiTISCSIDriver

        # For FC
        volume_driver = cinder.volume.drivers.huawei.huawei_t.HuaweiTFCDriver

   * **For T series V2**

     .. code-block:: ini

        # For iSCSI
        volume_driver = cinder.volume.drivers.huawei.huawei_driver.HuaweiTV2ISCSIDriver

        # For FC
        volume_driver = cinder.volume.drivers.huawei.huawei_driver.HuaweiTV2FCDriver

   * **For V3**

     .. code-block:: ini

        # For iSCSI
        volume_driver = cinder.volume.drivers.huawei.huawei_driver.HuaweiV3ISCSIDriver

        # For FC
        volume_driver = cinder.volume.drivers.huawei.huawei_driver.HuaweiV3FCDriver

   * **For OceanStor 18000 series**

     .. code-block:: ini

        # For iSCSI
        volume_driver = cinder.volume.drivers.huawei.huawei_driver.HuaweiISCSIDriver

        # For FC
        volume_driver = cinder.volume.drivers.huawei.huawei_driver.HuaweiFCDriver

     .. note::

        In Mitaka, ``Huawei18000ISCSIDriver`` and ``Huawei18000FCDriver`` have
        been renamed to ``HuaweiISCSIDriver`` and ``HuaweiFCDriver``.

#. Run the service :command:`cinder-volume restart` command to restart the
   Block Storage service.

Configuring iSCSI Multipathing
------------------------------

To configure iSCSI Multipathing, follow the steps below:

#. Create a port group on the storage device using the ``DeviceManager`` and add
   service links that require multipathing into the port group.

#. Log in to the storage device using CLI commands and enable the multiport
   discovery switch in the multipathing.

   .. code-block:: console

      developer:/>change iscsi discover_multiport switch=on

#. Add the port group settings in the Huawei-customized driver configuration
   file and configure the port group name needed by an initiator.

   .. code-block:: xml

      <iSCSI>
         <DefaultTargetIP>x.x.x.x</DefaultTargetIP>
         <Initiator Name="xxxxxx" TargetPortGroup="xxxx" />
      </iSCSI>

#. Enable the multipathing switch of the OpenStack Nova module.

   If the version of OpenStack is Havana or IceHouse, add
   ``libvirt_iscsi_use_multipath = True`` in ``[default]`` of
   ``/etc/nova/nova.conf``.

   If the version of OpenStack is Juno, Kilo, Liberty or Mitaka, add
   ``iscsi_use_multipath = True`` in ``[libvirt]`` of ``/etc/nova/nova.conf``.

#. Run the service :command:`nova-compute restart` command to restart the
   ``nova-compute`` service.

Configuring CHAP and ALUA
-------------------------

On a public network, any application server whose IP address resides on the
same network segment as that of the storage systems iSCSI host port can access
the storage system and perform read and write operations in it. This poses
risks to the data security of the storage system. To ensure the storage
systems access security, you can configure ``CHAP`` authentication to control
application servers access to the storage system.

Adjust he driver configuration file as follows:

.. code-block:: xml

   <Initiator ALUA="xxx" CHAPinfo="xxx" Name="xxx" TargetIP="x.x.x.x"/>

``ALUA`` indicates a multipathing mode. 0 indicates that ``ALUA`` is disabled.
1 indicates that ``ALUA`` is enabled. ``CHAPinfo`` indicates the user name and
password authenticated by ``CHAP``. The format is ``mmuser; mm-user@storage``.
The user name and password are separated by semicolons (;).

Configuring multi-storage support
---------------------------------

Example for configuring multiple storage systems:

.. code-block:: ini

   enabled_backends = t_fc, 18000_fc
   [t_fc]
   volume_driver = cinder.volume.drivers.huawei.huawei_t.HuaweiTFCDriver
   cinder_huawei_conf_file = /etc/cinder/cinder_huawei_conf_t_fc.xml
   volume_backend_name = HuaweiTFCDriver
   [18000_fc]
   volume_driver = cinder.volume.drivers.huawei.huawei_driver.HuaweiFCDriver
   cinder_huawei_conf_file = /etc/cinder/cinder_huawei_conf_18000_fc.xml
   volume_backend_name = HuaweiFCDriver

Configuration file parameters
-----------------------------

This section describes mandatory and optional configuration file parameters
of the Huawei volume driver.

.. list-table:: **Mandatory parameters**
   :widths: 10 10 50 10
   :header-rows: 1

   * - Parameter
     - Default value
     - Description
     - Applicable to
   * - Product
     - -
     - Type of a storage product. Possible values are ``T``, ``18000`` and
       ``V3``.
     - All
   * - Protocol
     - -
     - Type of a connection protocol. The possible value is either ``'iSCSI'``
       or ``'FC'``.
     - All
   * - ControllerIP0
     - -
     - IP address of the primary controller on an OceanStor T series V100R005
       storage device.
     - T series V1
   * - ControllerIP1
     - -
     - IP address of the secondary controller on an OceanStor T series V100R005
       storage device.
     - T series V1
   * - RestURL
     - -
     - Access address of the REST interface,
       ``https://x.x.x.x/devicemanager/rest/``. The value ``x.x.x.x`` indicates
       the management IP address. OceanStor 18000 uses the preceding setting,
       and V2 and V3 requires you to add port number ``8088``, for example,
       ``https://x.x.x.x:8088/deviceManager/rest/``. If you need to configure
       multiple RestURL, separate them by semicolons (;).
     - T series V2

       V3 18000
   * - UserName
     - -
     - User name of a storage administrator.
     - All
   * - UserPassword
     - -
     - Password of a storage administrator.
     - All
   * - StoragePool
     - -
     - Name of a storage pool to be used. If you need to configure multiple
       storage pools, separate them by semicolons (;).
     - All

.. note::

   The value of ``StoragePool`` cannot contain Chinese characters.

.. list-table:: **Optional parameters**
   :widths: 20 10 50 15
   :header-rows: 1

   * - Parameter
     - Default value
     - Description
     - Applicable to
   * - LUNType
     - Thin
     - Type of the LUNs to be created. The value can be ``Thick`` or ``Thin``.
     - All
   * - StripUnitSize
     - 64
     - Stripe depth of a LUN to be created. The unit is KB. This parameter is
       invalid when a thin LUN is created.
     - T series V1
   * - WriteType
     - 1
     - Cache write type, possible values are: ``1`` (write back), ``2``
       (write through), and ``3`` (mandatory write back).
     - All
   * - MirrorSwitch
     - 1
     - Cache mirroring or not, possible values are: ``0`` (without mirroring)
       or ``1`` (with mirroring).
     - All
   * - Prefetch Type
     - 3
     - Cache prefetch policy, possible values are: ``0`` (no prefetch), ``1``
       (fixed prefetch), ``2`` (variable prefetch) or ``3``
       (intelligent prefetch).
     - T series V1
   * - Prefetch Value
     - 0
     - Cache prefetch value.
     - T series V1
   * - LUNcopyWaitInterval
     - 5
     - After LUN copy is enabled, the plug-in frequently queries the copy
       progress. You can set a value to specify the query interval.
     - T series V2 V3

       18000
   * - Timeout
     - 432000
     - Timeout interval for waiting LUN copy of a storage device to complete.
       The unit is second.
     - T series V2 V3

       18000
   * - Initiator Name
     - -
     - Name of a compute node initiator.
     - All
   * - Initiator TargetIP
     - -
     - IP address of the iSCSI port provided for compute nodes.
     - All
   * - Initiator TargetPortGroup
     - -
     - IP address of the iSCSI target port that is provided for compute
       nodes.
     - T series V2 V3

       18000
   * - DefaultTargetIP
     - -
     - Default IP address of the iSCSI target port that is provided for
       compute nodes.
     - All
   * - OSType
     - Linux
     - Operating system of the Nova compute node's host.
     - All
   * - HostIP
     - -
     - IP address of the Nova compute node's host.
     - All

.. important::

   The ``Initiator Name``, ``Initiator TargetIP``, and
   ``Initiator TargetPortGroup`` are ``ISCSI`` parameters and therefore not
   applicable to ``FC``.
