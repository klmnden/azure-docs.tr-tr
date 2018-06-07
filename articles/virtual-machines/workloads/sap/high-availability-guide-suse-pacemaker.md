---
title: SUSE Linux Enterprise Server Azure üzerinde Pacemaker ayarlama | Microsoft Docs
description: SUSE Linux Enterprise Server Azure üzerinde Pacemaker ayarlama
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
ms.date: 03/20/2018
ms.author: sedusch
ms.openlocfilehash: ba44a8988c4af68abf4d155a2b9cb490b6122d39
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656423"
---
# <a name="setting-up-pacemaker-on-suse-linux-enterprise-server-in-azure"></a>SUSE Linux Enterprise Server Azure üzerinde Pacemaker ayarlama

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md

Azure Pacemaker kümedeki ayarlamak için iki seçenek vardır. Azure API'leri aracılığıyla başarısız bir düğümü yeniden başlatmak mvc'deki bir yalıtma Aracısı ya da kullanabilir veya bir SBD cihazı kullanabilirsiniz.

SBD aygıt bir ek sanal bir iSCSI hedef sunucusu gibi davranır ve SBD aygıt sağlayan machine gerektirir. Bu iSCSI hedef sunucusu ancak olabilir diğer Pacemaker kümeleriyle paylaşılan. SBD aygıt kullanmanın avantajı, daha hızlı yük devretme zaman ve SBD cihazlar şirket kullanıyorsanız pacemaker küme nasıl çalıştığını üzerinde herhangi bir değişiklik gerektirmez. SBD yalıtma iSCSI hedef sunucusu kullanılamaz durumda mekanizması eskrim yedek olarak Azure dilimi Aracısı'nı kullanmaya devam edebilirsiniz.

Bir ek sanal makine yatırım istemiyorsanız, Azure dilimi Aracısı'nı da kullanabilirsiniz. Dezavantajı, bir yük devretme kaynak durdurma başarısız veya küme düğümlerini hangi birbirine artık iletişim kuramıyor, 10-15 dakika arasında sürebilir ' dir.

![Pacemaker üzerinde SLES genel bakış](./media/high-availability-guide-suse-pacemaker/pacemaker.png)

## <a name="sbd-fencing"></a>SBD yalıtma

SBD aygıt yalıtma için kullanmak istiyorsanız, aşağıdaki adımları izleyin.

### <a name="set-up-an-iscsi-target-server"></a>Bir iSCSI hedef sunucusu ayarlamanız

Önce bir zaten yoksa bir iSCSI hedef sanal makine oluşturmanız gerekir. iSCSI hedef sunucularına ile birden çok Pacemaker küme paylaşılabilir.

1. Yeni SLES 12 SP1 ya da daha yüksek sanal makineyi dağıtmak ve aracılığıyla makine ssh bağlanın. Makine büyük olması gerekmez. Bir sanal makine boyutu Standard_E2s_v3 veya Standard_D2s_v3 gibi yeterli olur.

1. SLES güncelleştir

   <pre><code>
   sudo zypper update
   </code></pre>

1. Paketlerini kaldırın

   Targetcli ve SLES 12 SP3 ile ilgili bilinen bir sorun önlemek için aşağıdaki paketleri kaldırın. Bulunamayan paketlerle ilgili hatalar yoksayabilirsiniz
   
   <pre><code>
   sudo zypper remove lio-utils python-rtslib python-configshell targetcli
   </code></pre>
   
1. İSCSI hedef paket yüklemek için

   <pre><code>
   sudo zypper install targetcli-fb dbus-1-python
   </code></pre>

1. İSCSI hedef hizmeti etkinleştirme

   <pre><code>   
   sudo systemctl enable targetcli
   sudo systemctl start targetcli
   </code></pre>

### <a name="create-iscsi-device-on-iscsi-target-server"></a>İSCSI hedef sunucuda iSCSI cihaz oluşturma

Bu küme için kullanılabilir iSCSI hedef sanal makine için yeni bir veri diski ekleyin. Veri diski 1 GB kadar küçük olabilir ve bir Premium depolama hesabı veya yönetilen bir Premium Disk yararlanmasını yerleştirilmelidir [tek VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines).

Aşağıdaki komutu çalıştırın **iSCSI hedef VM** yeni küme için bir iSCSI diski oluşturmak için. Aşağıdaki örnekte, **cl1** yeni küme tanımlamak için kullanılır ve **üretim cl1 0** ve **üretim cl1 1** konak küme düğümlerinin adları. Bunları, Küme düğümlerinizi ana bilgisayar adı ile değiştirin.

<pre><code>
# List all data disks with the following command
sudo ls -al /dev/disk/azure/scsi1/

# total 0
# drwxr-xr-x 2 root root  80 Mar 26 14:42 .
# drwxr-xr-x 3 root root 160 Mar 26 14:42 ..
# lrwxrwxrwx 1 root root  12 Mar 26 14:42 lun0 -> ../../../<b>sdc</b>
# lrwxrwxrwx 1 root root  12 Mar 26 14:42 lun1 -> ../../../sdd

# Then use the disk name to list the disk id
sudo ls -l /dev/disk/by-id/scsi-* | grep sdc

# lrwxrwxrwx 1 root root  9 Mar 26 14:42 /dev/disk/by-id/scsi-14d53465420202020a50923c92babda40974bef49ae8828f0 -> ../../sdc
# lrwxrwxrwx 1 root root  9 Mar 26 14:42 <b>/dev/disk/by-id/scsi-360022480a50923c92babef49ae8828f0 -> ../../sdc</b>

# Use the data disk that you attached for this cluster to create a new backstore
sudo targetcli backstores/block create <b>cl1</b> <b>/dev/disk/by-id/scsi-360022480a50923c92babef49ae8828f0</b>

sudo targetcli iscsi/ create iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/luns/ create /backstores/block/<b>cl1</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/acls/ create iqn.2006-04.<b>prod-cl1-0.local:prod-cl1-0</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/acls/ create iqn.2006-04.<b>prod-cl1-1.local:prod-cl1-1</b>

# save the targetcli changes
sudo targetcli saveconfig
</code></pre>

### <a name="set-up-sbd-device"></a>SBD aygıtı kurma

Son adımda kümeden oluşturulduğu iSCSI aygıtı bağlayın.
Oluşturmak istediğiniz yeni küme düğümleri üzerinde aşağıdaki komutları çalıştırın.
Aşağıdaki öğeler ile ya da önek **[A]** - tüm düğümleri için geçerli **[1]** - 1 düğümü yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  İSCSI aygıtlarını Bağlan

   İlk olarak, iSCSI ve SBD hizmetleri etkinleştirin.

   <pre><code>
   sudo systemctl enable iscsid
   sudo systemctl enable iscsi
   sudo systemctl enable sbd
   </code></pre>

1. **[1]**  İlk düğümde Başlatıcı adını değiştirin

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Dosya içeriği, iSCSI aygıtı iSCSI hedef sunucuda oluşturulurken kullanılan ACL'ler eşleşecek şekilde değiştirin

   <pre><code>   
   InitiatorName=<b>iqn.2006-04.prod-cl1-0.local:prod-cl1-0</b>
   </code></pre>

1. **[2]**  İkinci düğüm üzerindeki Başlatıcı adını değiştirin

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Dosya içeriği, iSCSI aygıtı iSCSI hedef sunucuda oluşturulurken kullanılan ACL'ler eşleşecek şekilde değiştirin

   <pre><code>
   InitiatorName=<b>iqn.2006-04.prod-cl1-1.local:prod-cl1-1</b>
   </code></pre>

1. **[A]**  İSCSI Hizmeti yeniden başlatın

   Şimdi değişikliği uygulamak için iSCSI Hizmeti yeniden başlatın
   
   <pre><code>
   sudo systemctl restart iscsid
   sudo systemctl restart iscsi
   </code></pre>

   İSCSI aygıtları bağlayın. Aşağıdaki örnekte, 10.0.0.17 iSCSI hedef sunucusunun IP adresi ve 3260 varsayılan bağlantı noktasıdır. <b>iqn.2006 04.cl1.local:cl1</b> ilk komut çalıştırdığınızda listelenen hedef adı değil.

   <pre><code>
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.17:3260</b>
   
   sudo iscsiadm -m node -T <b>iqn.2006-04.cl1.local:cl1</b> --login --portal=<b>10.0.0.17:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.17:3260</b> --op=update --name=node.startup --value=automatic
   </code></pre>

   İSCSI aygıtı kullanılabilir olduğundan emin olun ve cihaz adını (Aşağıdaki örnek/dev/sde) yapılan Not

   <pre><code>
   lsscsi
   
   # [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
   # [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
   # [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
   # [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
   # <b>[6:0:0:0]    disk    LIO-ORG  cl1              4.0   /dev/sde</b>
   </code></pre>

   Şimdi, iSCSI aygıtın Kimliğini alır.

   <pre><code>
   ls -l /dev/disk/by-id/scsi-* | grep <b>sde</b>
   
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-1LIO-ORG_cl1:3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   # <b>lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df -> ../../sde</b>
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-SLIO-ORG_cl1_3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   </code></pre>

   Üç aygıt kimlikleri Listele komutu. SCSI-3, bu yukarıdaki örnekte ile başlayan kimliği kullanılması önerilir
   
   **/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df**

1. **[1]**  SBD cihaz oluşturma

   İlk küme düğümünde yeni bir SBD cihaz oluşturmak için iSCSI aygıtın aygıt Kimliğini kullanın.

   <pre><code>
   sudo sbd -d <b>/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df</b> -1 10 -4 20 create
   </code></pre>

1. **[A]**  SBD config uyarlama

   SBD yapılandırma dosyasını açın

   <pre><code>
   sudo vi /etc/sysconfig/sbd
   </code></pre>

   SBD aygıt özelliğini değiştirin, pacemaker tümleştirmesini etkinleştirmek ve SBD başlangıç modunu değiştirmek.

   <pre><code>
   [...]
   <b>SBD_DEVICE="/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df"</b>
   [...]
   <b>SBD_PACEMAKER="yes"</b>
   [...]
   <b>SBD_STARTMODE="always"</b>
   </code></pre>

   Softdog yapılandırma dosyası oluşturma

   <pre><code>
   echo softdog | sudo tee /etc/modules-load.d/softdog.conf
   </code></pre>

   Şimdi yük Modülü

   <pre><code>
   sudo modprobe -v softdog
   </code></pre>

## <a name="cluster-installation"></a>Küme yükleme

Aşağıdaki öğeler ile ya da önek **[A]** - tüm düğümleri için geçerli **[1]** - 1 düğümü yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  SLES güncelleştir

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Ssh erişimini etkinleştirme

   <pre><code>
   sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

2. **[2]**  Ssh erişimini etkinleştirme

   <pre><code>
   sudo ssh-keygen
   
   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[1]**  Ssh erişimini etkinleştirme

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Yükleme HA uzantısı
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi   

   Bir DNS sunucusu kullanın veya tüm düğümlerde/etc/hosts değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda ana bilgisayar adını değiştirin. / Etc/hosts kullanmanın faydası, kümenizi hataları tek bir noktadan çok olabilen DNS bağımsız olmasıdır.

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   / Etc/hosts aşağıdaki satırları ekleyin. Ortamınıza uyum sağlaması için ana bilgisayar adı ve IP adresini değiştirme   
   
   <pre><code>
   # IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[1]**  Küme yükleyin
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> Press ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> Press ENTER
   # Multicast port [5405] -> Press ENTER
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Küme düğümüne Ekle
   
   <pre><code> 
   sudo ha-cluster-join
   
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.14
   # /root/.ssh/id_rsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Hacluster parola aynı parolayı Değiştir

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Diğer aktarım kullanın ve listesi eklemek için corosync yapılandırın. Aksi takdirde, küme çalışmaz.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   Değerler yok ya da farklı değilse aşağıdaki kalın içeriği dosyaya ekleyin.
   
   <pre><code> 
   [...]
     <b>token:          5000
     token_retransmits_before_loss_const: 10
     join:           60
     consensus:      6000
     max_messages:   20</b>
     
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
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

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[1]**  Pacemaker varsayılan ayarlarını değiştirme

   <pre><code>
   sudo crm configure rsc_defaults resource-stickiness="1"   
   </code></pre>

## <a name="create-stonith-device"></a>STONITH cihaz oluşturma

STONITH aygıt bir hizmet sorumlusu Microsoft Azure karşı yetkilendirmek için kullanır. Bir hizmet sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Şuraya gidin: <https://portal.azure.com>
1. Azure Active Directory dikey penceresini açın  
   Özellikleri'ne gidin ve dizin kimliği yazma Bu **kimliği Kiracı**.
1. Uygulama kayıtlar'ı tıklatın
1. Ekle'ye tıklayın.
1. Bir ad girin, uygulama türü "Web uygulaması/API" seçin, bir oturum açma URL'sini girin (örneğin http://localhost) Oluştur'u tıklatın
1. Oturum açma URL'si kullanılmaz ve geçerli bir URL olabilir
1. Yeni uygulama seçin ve anahtarları Ayarlar sekmesini
1. Yeni bir anahtar için bir açıklama girin, "Her zaman geçerli olsun" seçin ve Kaydet
1. Değeri yazın. Olarak kullanılan **parola** için hizmet sorumlusu
1. Uygulama Kimliği yazma Kullanıcı adı olarak kullanılır (**oturum açma kimliği** aşağıdaki adımlarda), hizmet sorumlusu

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]**  Dilimi aracı için özel bir rol oluşturun

Hizmet sorumlusu Azure kaynaklarınızı varsayılan olarak erişim izni yok. Başlatmak ve durdurmak için hizmet asıl izinleri vermeniz gerekir (serbest bırakma) kümenin tüm sanal makineler. Özel rol zaten oluşturmadıysanız kullanarak oluşturabilirsiniz [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell#create-a-custom-role) veya [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli#create-a-custom-role)

Aşağıdaki içerik giriş dosyası için kullanın. İhtiyacınız olan içeriği, aboneliklere uyum, c276fc76-9cd4-44c9-99a7-4fd71546436e ve e91d47c4-76f3-4271-a796-21b4ecfe3624 aboneliğinizi kimlikleri ile değiştirin. Yalnızca bir aboneliğiniz varsa, ikinci giriş içinde AssignableScopes kaldırın.

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

### <a name="1-assign-the-custom-role-to-the-service-principal"></a>**[1]**  Hizmet sorumlusu özel rol atama

"Linux dilimi aracı son bölüm için hizmet sorumlusu oluşturulan rol" özel rol atayın. Sahip rolü artık kullanmayın!

1. Şuraya gidin: https://portal.azure.com
1. Tüm kaynaklar dikey penceresini açın
1. İlk küme düğümüne sanal makinesini seçin
1. Erişim denetimi (IAM) tıklatın
1. Ekle'ye tıklayın.
1. "Linux dilimi Aracısı rol" rolü seçin
1. Yukarıda oluşturduğunuz uygulamanın adını girin
1. Tamam'ı tıklatın

İkinci küme düğümü için yukarıdaki adımları yineleyin.

### <a name="1-create-the-stonith-devices"></a>**[1]**  STONITH aygıtları oluşturun

Sanal makineler için izinleri düzenlenebilir sonra kümede STONITH cihazları yapılandırabilirsiniz.

<pre><code>
# replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
sudo crm configure property stonith-timeout=900

sudo crm configure primitive rsc_st_azure stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

</code></pre>

### <a name="1-create-fence-topology-for-sbd-fencing"></a>**[1]**  SBD yalıtma dilimi topolojisi oluştur

Bir SBD aygıtı kullanmak istiyorsanız, iSCSI hedef sunucusu kullanılamaz durumda yedek olarak Azure dilimi Aracısı'nı kullanma hala öneririz.

<pre><code>
sudo crm configure fencing_topology \
  stonith-sbd rsc_st_azure

</code></pre>
### **[1] ** STONITH aygıt kullanımını etkinleştirme

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>


## <a name="next-steps"></a>Sonraki adımlar
* [Azure sanal makineleri planlama ve uygulama SAP için][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik ve Azure vm'lerinde SAP HANA olağanüstü durum kurtarma planı oluşturmak öğrenmek için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
