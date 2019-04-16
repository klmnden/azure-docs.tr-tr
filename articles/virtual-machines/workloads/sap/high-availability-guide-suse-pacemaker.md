---
title: SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama | Microsoft Docs
description: SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: sedusch
ms.openlocfilehash: 62356ee35631373b5a5d38ed356bbb2fb489807b
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59577804"
---
# <a name="setting-up-pacemaker-on-suse-linux-enterprise-server-in-azure"></a>SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[virtual-machines-linux-maintenance]:../../linux/maintenance-and-updates.md#maintenance-not-requiring-a-reboot
[virtual-machines-windows-maintenance]:../../windows/maintenance-and-updates.md#maintenance-not-requiring-a-reboot
[sles-nfs-guide]:high-availability-guide-suse-nfs.md
[sles-guide]:high-availability-guide-suse.md

Azure'da Pacemaker kümeyi ayarlamak için iki seçenek vardır. Azure API'leri aracılığıyla başarısız bir düğümü yeniden başlatmak üstlenir bir sınır Aracısı ya da kullanabilir veya SBD cihaz kullanabilirsiniz.

En az bir ek sanal SBD cihaz sağlar ve bir iSCSI hedef sunucusu olarak davranan makineyi SBD cihaz gerektirir. Bu iSCSI hedef sunucularına ancak olabilir diğer Pacemaker kümeleriyle paylaşılan. SBD cihaz kullanmanın avantajı, daha hızlı yük devretme zaman alan bir işlemdir ve SBD cihazlar şirket içinde kullanıyorsanız nasıl pacemaker küme çalıştırıyorsanız üzerinde herhangi bir değişiklik gerektirmez. Örneğin iSCSI hedef sunucusunun işletim sistemi düzeltme eki uygulama sırasında kullanılamaz hale SBD aygıtının izin vermek için en fazla üç SBD cihazlar Pacemaker küme için kullanabilirsiniz. Birden fazla SBD cihaz başına Pacemaker kullanmak istiyorsanız, birden fazla iSCSI hedef sunucularına dağıtmak ve her iSCSI hedef sunucudan bir SBD bağlanmak emin olun. Bir SBD cihaz veya üç kullanmanızı öneririz. Pacemaker yalnızca iki SBD cihazları yapılandırmak ve bunlardan biri kullanılabilir değilse, bir küme düğümünde otomatik olarak Çit mümkün olmayacaktır. Bir iSCSI hedef sunucusu kullanılamaz durumdayken Çit istiyorsanız, üç SBD cihazlar ve bu nedenle üç iSCSI hedef sunucusu kullanmanız gerekir.

Bir ek sanal makine yatırımını yapmak istemiyorsanız, Azure sınır Aracısı'nı kullanabilirsiniz. Dezavantajı, bir yük devretme kaynak durdurma başarısız olursa veya küme düğümleri, birbirine artık iletişim kuramıyor 10-15 dakika arasında sürebilir ' dir.

![SLES genel SLES üzerinde pacemaker](./media/high-availability-guide-suse-pacemaker/pacemaker.png)

>[!IMPORTANT]
> Düğümleri ve SBD cihazları planlama ve dağıtma Linux Pacemaker kümelenmiş, söz konusu sanal makineler arasında yönlendirme ve SBD onları barındıran VM üzerinden geçmiyor tam küme yapılandırması genel güvenilirliği için gereklidir herhangi bir cihaza ister [nva'ları](https://azure.microsoft.com/solutions/network-appliances/). Aksi takdirde, sorunları ve NVA ile bakım olayları kararlılık ve güvenilirlik genel küme yapılandırması üzerinde olumsuz bir etkiye sahip olabilir. Tür engelleri önlemek için yönlendirme kuralları nva belirtmiyor veya [kullanıcı tanımlı yönlendirme kuralları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) kümelenmiş düğümler ve Nva üzerinden SBD cihazları ve planlama ve Linux dağıtırken benzer cihazlar arasında trafiği yönlendirme Kümelenmiş pacemaker düğümleri ve SBD cihazlar. 
>

## <a name="sbd-fencing"></a>SBD çitlemek

SBD cihaz için sınır kullanmak istiyorsanız aşağıdaki adımları izleyin.

### <a name="set-up-iscsi-target-servers"></a>İSCSI hedef sunucusu ayarlayabilir

Önce iSCSI hedef sanal makineler oluşturmak gerekir. iSCSI hedef sunucularına ile birden çok Pacemaker küme paylaşılabilir.

1. Yeni SLES 12 SP1 ya da daha yüksek sanal makineleri dağıtmak ve bunları ssh bağlanın. Makineleri büyük olması gerekmez. Bir sanal makine boyutu Standard_E2s_v3 veya Standard_D2s_v3 gibi büyük/küçük harf yeterlidir. Premium depolama işletim sistemi diskini kullandığınızdan emin olun.

Tüm aşağıdaki komutları çalıştırın **iSCSI hedef sanal makinelere**.

1. Güncelleştirme SLES

   <pre><code>sudo zypper update
   </code></pre>

1. Paketleri kaldırın

   Targetcli ve SLES 12 SP3 ile bilinen bir sorunu önlemek için aşağıdaki paketleri kaldırın. Nebyla nalezena paketlerle ilgili hataları yoksayabilirsiniz.

   <pre><code>sudo zypper remove lio-utils python-rtslib python-configshell targetcli
   </code></pre>

1. İSCSI hedef paketlerini yükleyin

   <pre><code>sudo zypper install targetcli-fb dbus-1-python
   </code></pre>

1. İSCSI hedef hizmeti etkinleştirme

   <pre><code>sudo systemctl enable targetcli
   sudo systemctl start targetcli
   </code></pre>

### <a name="create-iscsi-device-on-iscsi-target-server"></a>İSCSI hedef sunucuda iSCSI cihazı oluşturma

Tüm aşağıdaki komutları çalıştırın **iSCSI hedef sanal makinelere** iSCSI disklerini, SAP sistemlerini tarafından kullanılan kümeler için oluşturulacak. Aşağıdaki örnekte, SBD cihazlar birden fazla küme için oluşturulur. Bir iSCSI hedef sunucusu birden fazla küme için nasıl kullanacağınız gösterilmektedir. SBD cihazlar, işletim sistemi diskinde yerleştirilir. Yeterli alana sahip olduğunuzdan emin olun.

**`nfs`** NFS küme tanımlamak için kullanılan **ascsnw1** ASCS kümesini tanımlamak için kullanılan **NW1**, **dbnw1** veritabanı kümesini tanımlamak için kullanılan **NW1** , **nfs 0** ve **nfs 1** konak adları NFS küme düğümlerinin **nw1 xscs 0** ve **nw1 xscs 1**konak adları'nın **NW1** ASCS küme düğümlerini ve **nw1-db-0** ve **nw1-db-1** veritabanının ana bilgisayar adları olan küme düğümleri. Bunları, Küme düğümlerinizi ana bilgisayar adlarını ve SAP sisteminizin SID ile değiştirin.

<pre><code># Create the root folder for all SBD devices
sudo mkdir /sbd

# Create the SBD device for the NFS server
sudo targetcli backstores/fileio create sbdnfs /sbd/sbdnfs 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.nfs.local:nfs
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/luns/ create /backstores/fileio/sbdnfs
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/acls/ create iqn.2006-04.<b>nfs-0.local:nfs-0</b>
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/acls/ create iqn.2006-04.<b>nfs-1.local:nfs-1</b>

# Create the SBD device for the ASCS server of SAP System NW1
sudo targetcli backstores/fileio create sbdascs<b>nw1</b> /sbd/sbdascs<b>nw1</b> 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/luns/ create /backstores/fileio/sbdascs<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-xscs-0.local:nw1-xscs-0</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-xscs-1.local:nw1-xscs-1</b>

# Create the SBD device for the database cluster of SAP System NW1
sudo targetcli backstores/fileio create sbddb<b>nw1</b> /sbd/sbddb<b>nw1</b> 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/luns/ create /backstores/fileio/sbddb<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-db-0.local:nw1-db-0</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-db-1.local:nw1-db-1</b>

# save the targetcli changes
sudo targetcli saveconfig
</code></pre>

Her şeyin doğru şekilde ile ayarlanmıştır, kontrol edebilirsiniz

<pre><code>sudo targetcli ls

o- / .......................................................................................................... [...]
  o- backstores ............................................................................................... [...]
  | o- block ................................................................................... [Storage Objects: 0]
  | o- fileio .................................................................................. [Storage Objects: 3]
  | | o- <b>sbdascsnw1</b> ................................................ [/sbd/sbdascsnw1 (50.0MiB) write-thru activated]
  | | | o- alua .................................................................................... [ALUA Groups: 1]
  | | |   o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | | o- <b>sbddbnw1</b> .................................................... [/sbd/sbddbnw1 (50.0MiB) write-thru activated]
  | | | o- alua .................................................................................... [ALUA Groups: 1]
  | | |   o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | | o- <b>sbdnfs</b> ........................................................ [/sbd/sbdnfs (50.0MiB) write-thru activated]
  | |   o- alua .................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | o- pscsi ................................................................................... [Storage Objects: 0]
  | o- ramdisk ................................................................................. [Storage Objects: 0]
  o- iscsi ............................................................................................. [Targets: 3]
  | o- <b>iqn.2006-04.ascsnw1.local:ascsnw1</b> .................................................................. [TPGs: 1]
  | | o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  | |   o- acls ........................................................................................... [ACLs: 2]
  | |   | o- <b>iqn.2006-04.nw1-xscs-0.local:nw1-xscs-0</b> ............................................... [Mapped LUNs: 1]
  | |   | | o- mapped_lun0 ............................................................ [lun0 fileio/<b>sbdascsnw1</b> (rw)]
  | |   | o- <b>iqn.2006-04.nw1-xscs-1.local:nw1-xscs-1</b> ............................................... [Mapped LUNs: 1]
  | |   |   o- mapped_lun0 ............................................................ [lun0 fileio/<b>sbdascsnw1</b> (rw)]
  | |   o- luns ........................................................................................... [LUNs: 1]
  | |   | o- lun0 .......................................... [fileio/sbdascsnw1 (/sbd/sbdascsnw1) (default_tg_pt_gp)]
  | |   o- portals ..................................................................................... [Portals: 1]
  | |     o- 0.0.0.0:3260 ...................................................................................... [OK]
  | o- <b>iqn.2006-04.dbnw1.local:dbnw1</b> ...................................................................... [TPGs: 1]
  | | o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  | |   o- acls ........................................................................................... [ACLs: 2]
  | |   | o- <b>iqn.2006-04.nw1-db-0.local:nw1-db-0</b> ................................................... [Mapped LUNs: 1]
  | |   | | o- mapped_lun0 .............................................................. [lun0 fileio/<b>sbddbnw1</b> (rw)]
  | |   | o- <b>iqn.2006-04.nw1-db-1.local:nw1-db-1</b> ................................................... [Mapped LUNs: 1]
  | |   |   o- mapped_lun0 .............................................................. [lun0 fileio/<b>sbddbnw1</b> (rw)]
  | |   o- luns ........................................................................................... [LUNs: 1]
  | |   | o- lun0 .............................................. [fileio/sbddbnw1 (/sbd/sbddbnw1) (default_tg_pt_gp)]
  | |   o- portals ..................................................................................... [Portals: 1]
  | |     o- 0.0.0.0:3260 ...................................................................................... [OK]
  | o- <b>iqn.2006-04.nfs.local:nfs</b> .......................................................................... [TPGs: 1]
  |   o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  |     o- acls ........................................................................................... [ACLs: 2]
  |     | o- <b>iqn.2006-04.nfs-0.local:nfs-0</b> ......................................................... [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ................................................................ [lun0 fileio/<b>sbdnfs</b> (rw)]
  |     | o- <b>iqn.2006-04.nfs-1.local:nfs-1</b> ......................................................... [Mapped LUNs: 1]
  |     |   o- mapped_lun0 ................................................................ [lun0 fileio/<b>sbdnfs</b> (rw)]
  |     o- luns ........................................................................................... [LUNs: 1]
  |     | o- lun0 .................................................. [fileio/sbdnfs (/sbd/sbdnfs) (default_tg_pt_gp)]
  |     o- portals ..................................................................................... [Portals: 1]
  |       o- 0.0.0.0:3260 ...................................................................................... [OK]
  o- loopback .......................................................................................... [Targets: 0]
  o- vhost ............................................................................................. [Targets: 0]
  o- xen-pvscsi ........................................................................................ [Targets: 0]
</code></pre>

### <a name="set-up-sbd-device"></a>SBD cihazı ayarlama

Son adımda kümeden oluşturulduğu iSCSI cihazı bağlayın.
Oluşturmak istediğiniz yeni küme düğümlerinde aşağıdaki komutları çalıştırın.
Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  İSCSI cihazları Bağlan

   İlk olarak iSCSI ve SBD hizmetlerini etkinleştirin.

   <pre><code>sudo systemctl enable iscsid
   sudo systemctl enable iscsi
   sudo systemctl enable sbd
   </code></pre>

1. **[1]**  İlk düğümü üzerinde Başlatıcı adını değiştirin

   <pre><code>sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Örneğin, NFS sunucusu için iSCSI hedef sunucusundaki iSCSI cihazı oluşturulurken kullanılan ACL'leri eşleştirilecek dosya içeriğini değiştirin.

   <pre><code>InitiatorName=<b>iqn.2006-04.nfs-0.local:nfs-0</b>
   </code></pre>

1. **[2]**  İkinci düğümü Başlatıcı adını değiştirin

   <pre><code>sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   İSCSI hedef sunucuda iSCSI cihazı oluşturulurken kullanılan ACL'leri eşleştirilecek dosya içeriğini değiştirme

   <pre><code>InitiatorName=<b>iqn.2006-04.nfs-1.local:nfs-1</b>
   </code></pre>

1. **[A]**  İSCSI Hizmeti yeniden başlatın

   Değişikliği uygulamak için iSCSI hizmetini şimdi yeniden Başlat

   <pre><code>sudo systemctl restart iscsid
   sudo systemctl restart iscsi
   </code></pre>

   İSCSI cihazları bağlayın. Aşağıdaki örnekte, 10.0.0.17 iSCSI hedef sunucusunun IP adresi ve 3260'ın varsayılan bağlantı noktasıdır. <b>iqn.2006 04.nfs.local:nfs</b> (iscsiadm -m bulma) aşağıdaki ilk komutunu çalıştırdığınızda listelenen hedef adları biridir.

   <pre><code>sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.17:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.17:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.17:3260</b> --op=update --name=node.startup --value=automatic
   
   # If you want to use multiple SBD devices, also connect to the second iSCSI target server
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.18:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.18:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.18:3260</b> --op=update --name=node.startup --value=automatic
   
   # If you want to use multiple SBD devices, also connect to the third iSCSI target server
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.19:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.19:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.19:3260</b> --op=update --name=node.startup --value=automatic
   </code></pre>

   İSCSI cihazlar kullanılabilir ve cihaz adı (Aşağıdaki örnek/dev/sde) not emin olun

   <pre><code>lsscsi
   
   # [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
   # [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
   # [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
   # [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
   # <b>[6:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sdd</b>
   # <b>[7:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sde</b>
   # <b>[8:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sdf</b>
   </code></pre>

   Şimdi, iSCSI cihazların kimlikleri alma.

   <pre><code>ls -l /dev/disk/by-id/scsi-* | grep <b>sdd</b>
   
   # lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-1LIO-ORG_sbdnfs:afb0ba8d-3a3c-413b-8cc2-cca03e63ef42 -> ../../sdd
   # <b>lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03 -> ../../sdd</b>
   # lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-SLIO-ORG_sbdnfs_afb0ba8d-3a3c-413b-8cc2-cca03e63ef42 -> ../../sdd
   
   ls -l /dev/disk/by-id/scsi-* | grep <b>sde</b>
   
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-1LIO-ORG_cl1:3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   # <b>lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df -> ../../sde</b>
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-SLIO-ORG_cl1_3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   
   ls -l /dev/disk/by-id/scsi-* | grep <b>sdf</b>
   
   # lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-1LIO-ORG_sbdnfs:f88f30e7-c968-4678-bc87-fe7bfcbdb625 -> ../../sdf
   # <b>lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf -> ../../sdf</b>
   # lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-SLIO-ORG_sbdnfs_f88f30e7-c968-4678-bc87-fe7bfcbdb625 -> ../../sdf
   </code></pre>

   Her SBD cihaz için üç cihaz kimlikleri Listele komutu. SCSI-3, bu Yukarıdaki örnekteki ile başlayan kimliği kullanılması önerilir

   * **/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03**
   * **/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df**
   * **/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf**

1. **[1]**  SBD cihaz oluşturma

   Cihaz kimliği iSCSI cihazların ilk küme düğümüne yeni SBD cihazları oluşturmak için kullanın.

   <pre><code>sudo sbd -d <b>/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03</b> -1 60 -4 120 create

   # Also create the second and third SBD devices if you want to use more than one.
   sudo sbd -d <b>/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df</b> -1 60 -4 120 create
   sudo sbd -d <b>/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf</b> -1 60 -4 120 create
   </code></pre>

1. **[A]**  SBD config uyum

   SBD yapılandırma dosyasını aç

   <pre><code>sudo vi /etc/sysconfig/sbd
   </code></pre>

   SBD cihaz özelliğini değiştirin, pacemaker entegrasyon sağlayın ve SBD başlangıç modunu değiştirin.

   <pre><code>[...]
   <b>SBD_DEVICE="/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03;/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df;/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf"</b>
   [...]
   <b>SBD_PACEMAKER="yes"</b>
   [...]
   <b>SBD_STARTMODE="always"</b>
   [...]
   <b>SBD_WATCHDOG="yes"</b>
   </code></pre>

   Oluşturma `softdog` yapılandırma dosyası

   <pre><code>echo softdog | sudo tee /etc/modules-load.d/softdog.conf
   </code></pre>

   Artık modülünü yükleme

   <pre><code>sudo modprobe -v softdog
   </code></pre>

## <a name="cluster-installation"></a>Küme yükleme

Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  Güncelleştirme SLES

   <pre><code>sudo zypper update
   </code></pre>

1. **[A]**  İşletim sistemini Yapılandır

   Bazı durumlarda, Pacemaker birçok süreçleri oluşturuyor ve böylece izin verilen işlem sayısını tükettiğinde. Böyle bir durumda, küme düğümleri arasında bir sinyal başarısız ve kaynaklarınızı yük devretmesi için neden. En fazla izin verilen işlem aşağıdaki parametresini ayarlayarak artırma öneririz.

   <pre><code># Edit the configuration file
   sudo vi /etc/systemd/system.conf
   
   # Change the DefaultTasksMax
   #DefaultTasksMax=512
   DefaultTasksMax=4096
   
   #and to activate this setting
   sudo systemctl daemon-reload
   
   # test if the change was successful
   sudo systemctl --no-pager show | grep DefaultTasksMax
   </code></pre>

   Kirli önbellek boyutunu küçültün. Daha fazla bilgi için [düşük performans SLES 11/12 üzerinde yazma büyük RAM sunucularıyla](https://www.suse.com/support/kb/doc/?id=7010287).

   <pre><code>sudo vi /etc/sysctl.conf

   # Change/set the following settings
   vm.dirty_bytes = 629145600
   vm.dirty_background_bytes = 314572800
   </code></pre>

1. **[A]**  Bulut netconfig azure için yüksek kullanılabilirlik kümesi yapılandırma

   Bulut ağ eklentisi sanal IP adresi (VIP atama Pacemaker denetim gerekir) kaldırmasını önlemek için aşağıda gösterildiği gibi ağ arabiriminin yapılandırma dosyasını değiştirin. Daha fazla bilgi için [SUSE KB 7023633](https://www.suse.com/support/kb/doc/?id=7023633). 

   <pre><code># Edit the configuration file
   sudo vi /etc/sysconfig/network/ifcfg-eth0 
   
   # Change CLOUD_NETCONFIG_MANAGE
   # CLOUD_NETCONFIG_MANAGE="yes"
   CLOUD_NETCONFIG_MANAGE="no"
   </code></pre>

1. **[1]**  Ssh erişimi etkinleştir

   <pre><code>sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[2]**  Ssh erişimi etkinleştir

   <pre><code># insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   sudo ssh-keygen

   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[1]**  Ssh erişimi etkinleştir

   <pre><code># insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Dilimi yükleme aracıları
   
   <pre><code>sudo zypper install fence-agents
   </code></pre>

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin. / Etc/hosts kullanmanın avantajı, kümenizin bir tek hata noktası çok olabilir DNS bağımsız olur.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.   

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[1]**  Küme yükleme

   <pre><code>sudo ha-cluster-init
   
   # ! NTP is not configured to start at system boot.
   # Do you want to continue anyway (y/n)? <b>y</b>
   # /root/.ssh/id_rsa already exists - overwrite (y/n)? <b>n</b>
   # Network address to bind to (e.g.: 192.168.1.0) [10.0.0.0] <b>Press ENTER</b>
   # Multicast address (e.g.: 239.x.x.x) [239.232.97.43] <b>Press ENTER</b>
   # Multicast port [5405] <b>Press ENTER</b>
   # SBD is already configured to use /dev/disk/by-id/scsi-36001405639245768818458b930abdf69;/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03;/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf - overwrite (y/n)? <b>n</b>
   # Do you wish to configure an administration IP (y/n)? <b>n</b>
   </code></pre>

1. **[2]**  Küme düğümü Ekle

   <pre><code>sudo ha-cluster-join
   
   # ! NTP is not configured to start at system boot.
   # Do you want to continue anyway (y/n)? <b>y</b>
   # IP address or hostname of existing node (e.g.: 192.168.1.1) []<b>10.0.0.6</b>
   # /root/.ssh/id_rsa already exists - overwrite (y/n)? <b>n</b>
   </code></pre>

1. **[A]**  Aynı parolayı hacluster parolasını değiştirme

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]**  Corosync diğer aktarım kullanın ve bir düğüm listesine eklemek için yapılandırın. Aksi takdirde, küme çalışmaz.

   <pre><code>sudo vi /etc/corosync/corosync.conf
   </code></pre>

   Değerler var. ya da farklı değilse kalın aşağıdaki içeriği dosyaya ekleyin. Bakımı koruma bellek izin vermek için 30000 belirteç değiştirdiğinizden emin olun. Daha fazla bilgi için [Linux'a yönelik bu makaleyi] [ virtual-machines-linux-maintenance] veya [Windows][virtual-machines-windows-maintenance]. Ayrıca parametre mcastaddr kaldırdığınızdan emin olun.

   <pre><code>[...]
     <b>token:          30000
     token_retransmits_before_loss_const: 10
     join:           60
     consensus:      36000
     max_messages:   20</b>
     
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
     # remove parameter mcastaddr
     <b># mcastaddr: IP</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-cl1-0</b>
      ring0_addr:10.0.0.6
     }
     node {
      # IP address of <b>prod-cl1-1</b>
      ring0_addr:10.0.0.7
     } 
   }</b>
   logging {
     [...]
   }
   quorum {
        # Enable and configure quorum subsystem (default: off)
        # see also corosync.conf.5 and votequorum.5
        provider: corosync_votequorum
        <b>expected_votes: 2</b>
        <b>two_node: 1</b>
   }
   </code></pre>

   Corosync hizmetini durdurup yeniden başlatın

   <pre><code>sudo service corosync restart
   </code></pre>

## <a name="create-azure-fence-agent-stonith-device"></a>Azure sınır Aracısı STONITH cihaz oluşturma

STONITH cihaz, Microsoft Azure karşı korunmasına yetki vermek için bir hizmet sorumlusu kullanır. Bir hizmet sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Git [https://portal.azure.com](https://portal.azure.com)
1. Azure Active Directory dikey penceresini açın  
   Özellikler bölümüne gidin ve dizin kimliği yazma Bu **Kiracı kimliği**.
1. Uygulama kayıtları tıklayın
1. Ekle'ye tıklayın.
1. Bir ad girin, "Web uygulaması/API'si" uygulama türünü seçin, bir oturum açma URL'sini girin (örneğin http\://localhost) ve Oluştur'a tıklayın
1. Oturum açma URL'si kullanılmaz ve geçerli bir URL olabilir
1. Yeni uygulamayı seçin ve ayarları sekmesini anahtarları
1. Yeni bir anahtar için bir açıklama girin, "Her zaman geçerli olsun"'i seçin ve Kaydet'e tıklayın
1. Değeri yazın. Olarak kullanılan **parola** için hizmet sorumlusu
1. Uygulama Kimliği yazma Kullanıcı adı olarak kullanılır (**oturum açma kimliği** sonraki adımlarda), hizmet sorumlusu

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]**  Sınır aracısı için özel bir rol oluşturun

Hizmet sorumlusu kullanarak Azure kaynaklarınızı varsayılan olarak erişim izni yok. Başlatmak ve durdurmak için hizmet sorumlusu izinleri vermeniz gerekir (serbest bırakın) kümenin tüm sanal makineler. Özel rol zaten oluşturmadıysanız, bunu kullanarak oluşturabilirsiniz [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell) veya [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)

Giriş dosyası için aşağıdaki içeriği kullanın. İhtiyacınız olan içeriği için aboneliklerinizi uyum, c276fc76-9cd4-44c9-99a7-4fd71546436e ve e91d47c4-76f3-4271-a796-21b4ecfe3624 aboneliğinizin kimliği ile değiştirin. İkinci girdi, yalnızca bir aboneliğiniz varsa, ın AssignableScopes içinde kaldırın.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to deallocate and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="a-assign-the-custom-role-to-the-service-principal"></a>**[A]**  Hizmet sorumlusuna özel rolü atama

Özel rol "Linux sınır aracısı hizmet sorumlusuna son bölümde oluşturduğunuz rolü" atayın. Sahip rolü artık kullanmayın!

1. Git [https://portal.azure.com](https://portal.azure.com)
1. Tüm kaynaklar dikey penceresini açın
1. İlk küme düğümüne sanal makinesini seçin
1. Erişim denetimi (IAM)'ye tıklayın.
1. Ekle rol ataması
1. "Linux sınır Aracısı rolü" rolü seçin
1. Yukarıda oluşturduğunuz uygulamanın adını girin
1. Kaydet’e tıklayın.

İkinci küme düğümü için yukarıdaki adımları yineleyin.

### <a name="1-create-the-stonith-devices"></a>**[1]**  STONITH cihazları oluşturun

Sanal makineler için izinleri düzenleme sonra kümedeki STONITH cihazları yapılandırabilirsiniz.

<pre><code># replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
sudo crm configure primitive rsc_st_azure stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

sudo crm configure property stonith-timeout=900
sudo crm configure property stonith-enabled=true
</code></pre>

## <a name="default-pacemaker-configuration-for-sbd"></a>SBD için varsayılan Pacemaker yapılandırma

1. **[1]**  STONITH cihaz kullanımını etkinleştirmek ve sınır gecikme ayarlama

<pre><code>sudo crm configure property stonith-timeout=144
sudo crm configure property stonith-enabled=true

# List the resources to find the name of the SBD device
sudo crm resource list
sudo crm resource stop stonith-sbd
sudo crm configure delete <b>stonith-sbd</b>
sudo crm configure primitive <b>stonith-sbd</b> stonith:external/sbd \
   params pcmk_delay_max="15" \
   op monitor interval="15" timeout="15"
</code></pre>

## <a name="pacemaker-configuration-for-azure-scheduled-events"></a>Azure için pacemaker yapılandırma zamanlanmış olaylar

Azure tekliflerini [zamanlanmış olaylar](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/scheduled-events). Zamanlanmış olaylar, meta veri hizmeti sağlanır ve uygulamanın olayları VM kapatma, yeniden dağıtım VM, vb. için hazırlamak zaman tanıyın. Kaynak Aracısı **[azure etkinlikleri](https://github.com/ClusterLabs/resource-agents/pull/1161)** izleyiciler için zamanlanmış Azure etkinlikleri. Olayları algılanmazsa, tüm kaynaklar üzerinde etkilenen sanal Makineyi durdurun ve kümedeki başka bir düğüme taşımak aracı deneyecek. Bu ek Pacemaker kaynaklara ulaşmak için yapılandırılmalıdır. 

1. **[A]**  Yükleme **azure etkinlikleri** aracı. 

<pre><code>sudo zypper install resource-agents
</code></pre>

2. **[1]**  Pacemaker kaynakları yapılandırırsınız. 

<pre><code>
#Place the cluster in maintenance mode
sudo crm configure property maintenance-mode=true

#Create Pacemaker resources for the Azure agent
sudo crm configure primitive rsc_azure-events ocf:heartbeat:azure-events op monitor interval=10s
sudo crm configure clone cln_azure-events rsc_azure-events

#Take the cluster out of maintenance mode
sudo crm configure property maintenance-mode=false
</code></pre>

   > [!NOTE]
   > Küme içinde veya bakım modundan yerleştirdiğinizde Pacemaker kaynaklar azure etkinlikleri aracı için yapılandırdıktan sonra uyarı iletileri gibi alabilirsiniz:  
     Uyarı: cib önyükleme seçenekleri: Bilinmeyen öznitelik ' hostName_  <strong>hostname</strong>'  
     Uyarı: cib önyükleme seçenekleri: Bilinmeyen öznitelik 'azure-events_globalPullState'  
     Uyarı: cib önyükleme seçenekleri: Bilinmeyen öznitelik ' hostName_ <strong>hostname</strong>'  
   > Bu uyarı iletilerini yoksayılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik][sles-nfs-guide]
* [SAP uygulamaları için SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik][sles-guide]
* Yüksek kullanılabilirlik ve Azure Vm'leri üzerinde SAP hana olağanüstü durum kurtarma planı oluşturma hakkında bilgi almak için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
