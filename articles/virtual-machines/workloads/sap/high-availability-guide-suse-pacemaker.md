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
ms.date: 07/13/2018
ms.author: sedusch
ms.openlocfilehash: 9ce95bcf15d0186c1baea3df407d0fc0c4200f45
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115485"
---
# <a name="setting-up-pacemaker-on-suse-linux-enterprise-server-in-azure"></a>SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[virtual-machines-linux-maintenance]:../../linux/maintenance-and-updates.md#memory-preserving-maintenance
[virtual-machines-windows-maintenance]:../../windows/maintenance-and-updates.md#memory-preserving-maintenance

Azure'da Pacemaker kümeyi ayarlamak için iki seçenek vardır. Azure API'leri aracılığıyla başarısız bir düğümü yeniden başlatmak üstlenir bir sınır Aracısı ya da kullanabilir veya SBD cihaz kullanabilirsiniz.

Bir ek sanal SBD cihaz sağlar ve bir iSCSI hedef sunucusu olarak davranan makineyi SBD cihaz gerektirir. Bu iSCSI hedef sunucusu ancak olabilir diğer Pacemaker kümeleriyle paylaşılan. SBD cihaz kullanmanın avantajı, daha hızlı yük devretme zaman alan bir işlemdir ve SBD cihazlar şirket içinde kullanıyorsanız nasıl pacemaker küme çalıştırıyorsanız üzerinde herhangi bir değişiklik gerektirmez. SBD çitlemek iSCSI hedef sunucusu kullanılamıyor durumda mekanizması çitlemek yedek olarak Azure sınır Aracısı'nı kullanmaya devam edebilirsiniz.

Bir ek sanal makine yatırımını yapmak istemiyorsanız, Azure sınır Aracısı'nı kullanabilirsiniz. Dezavantajı, bir yük devretme kaynak durdurma başarısız olursa veya küme düğümleri, birbirine artık iletişim kuramıyor 10-15 dakika arasında sürebilir ' dir.

![SLES genel SLES üzerinde pacemaker](./media/high-availability-guide-suse-pacemaker/pacemaker.png)

>[!IMPORTANT]
> Düğümleri ve SBD cihazları planlama ve dağıtma Linux Pacemaker kümelenmiş, söz konusu sanal makineler arasında yönlendirme ve SBD onları barındıran VM üzerinden geçmiyor tam küme yapılandırması genel güvenilirliği için gereklidir herhangi bir cihaza ister [nva'ları](https://azure.microsoft.com/solutions/network-appliances/). Aksi takdirde, sorunları ve NVA ile bakım olayları kararlılık ve güvenilirlik genel küme yapılandırması üzerinde olumsuz bir etkiye sahip olabilir. Tür engelleri önlemek için yönlendirme kuralları nva belirtmiyor veya [kullanıcı tanımlı yönlendirme kuralları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) kümelenmiş düğümler ve Nva üzerinden SBD cihazları ve planlama ve Linux dağıtırken benzer cihazlar arasında trafiği yönlendirme Kümelenmiş pacemaker düğümleri ve SBD cihazlar. 
>


## <a name="sbd-fencing"></a>SBD çitlemek

SBD cihaz için sınır kullanmak istiyorsanız aşağıdaki adımları izleyin.

### <a name="set-up-an-iscsi-target-server"></a>Bir iSCSI hedef sunucusu ayarlama

Henüz sahip değil, bir iSCSI hedef sanal makine oluşturmak önce gerekir. iSCSI hedef sunucularına ile birden çok Pacemaker küme paylaşılabilir.

1. Yeni SLES 12 SP1 veya daha yüksek bir sanal makine dağıtın ve makinesine ssh bağlanın. Makine büyük olması gerekmez. Bir sanal makine boyutu Standard_E2s_v3 veya Standard_D2s_v3 gibi büyük/küçük harf yeterlidir.

1. Güncelleştirme SLES

   <pre><code>
   sudo zypper update
   </code></pre>

1. Paketleri kaldırın

   Targetcli ve SLES 12 SP3 ile bilinen bir sorunu önlemek için aşağıdaki paketleri kaldırın. Nebyla nalezena paketlerle ilgili hataları yoksayabilirsiniz.
   
   <pre><code>
   sudo zypper remove lio-utils python-rtslib python-configshell targetcli
   </code></pre>
   
1. İSCSI hedef paketlerini yükleyin

   <pre><code>
   sudo zypper install targetcli-fb dbus-1-python
   </code></pre>

1. İSCSI hedef hizmeti etkinleştirme

   <pre><code>   
   sudo systemctl enable targetcli
   sudo systemctl start targetcli
   </code></pre>

### <a name="create-iscsi-device-on-iscsi-target-server"></a>İSCSI hedef sunucuda iSCSI cihazı oluşturma

Bu küme için kullanılabilir iSCSI hedef sanal makine için yeni bir veri diski ekleyin. Veri diski 1 GB kadar küçük olabilir ve bir Premium depolama hesabı veya bir Premium yönetilen Disk yararlanmasını yerleştirilmelidir [tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines).

Aşağıdaki komutu şurada çalıştırın **iSCSI hedef VM** yeni küme için bir iSCSI diski oluşturmak için. Aşağıdaki örnekte, **cl1** yeni kümeye tanımlamak için kullanılır ve **prod cl1 0** ve **prod cl1 1** küme düğümlerinin konak adları. Bunları, Küme düğümlerinizi konak adı ile değiştirin.

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

### <a name="set-up-sbd-device"></a>SBD cihazı ayarlama

Son adımda kümeden oluşturulduğu iSCSI cihazı bağlayın.
Oluşturmak istediğiniz yeni küme düğümlerinde aşağıdaki komutları çalıştırın.
Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  İSCSI cihazları Bağlan

   İlk olarak iSCSI ve SBD hizmetlerini etkinleştirin.

   <pre><code>
   sudo systemctl enable iscsid
   sudo systemctl enable iscsi
   sudo systemctl enable sbd
   </code></pre>

1. **[1]**  İlk düğümü üzerinde Başlatıcı adını değiştirin

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   İSCSI hedef sunucuda iSCSI cihazı oluşturulurken kullanılan ACL'leri eşleştirilecek dosya içeriğini değiştirme

   <pre><code>   
   InitiatorName=<b>iqn.2006-04.prod-cl1-0.local:prod-cl1-0</b>
   </code></pre>

1. **[2]**  İkinci düğümü Başlatıcı adını değiştirin

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   İSCSI hedef sunucuda iSCSI cihazı oluşturulurken kullanılan ACL'leri eşleştirilecek dosya içeriğini değiştirme

   <pre><code>
   InitiatorName=<b>iqn.2006-04.prod-cl1-1.local:prod-cl1-1</b>
   </code></pre>

1. **[A]**  İSCSI Hizmeti yeniden başlatın

   Değişikliği uygulamak için iSCSI hizmetini şimdi yeniden Başlat
   
   <pre><code>
   sudo systemctl restart iscsid
   sudo systemctl restart iscsi
   </code></pre>

   İSCSI cihazları bağlayın. Aşağıdaki örnekte, 10.0.0.17 iSCSI hedef sunucusunun IP adresi ve 3260'ın varsayılan bağlantı noktasıdır. <b>iqn.2006 04.cl1.local:cl1</b> ilk komutu çalıştırdığınızda listelenen hedef adı değil.

   <pre><code>
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.17:3260</b>
   
   sudo iscsiadm -m node -T <b>iqn.2006-04.cl1.local:cl1</b> --login --portal=<b>10.0.0.17:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.17:3260</b> --op=update --name=node.startup --value=automatic
   </code></pre>

   İSCSI cihazı kullanılabilir olduğundan emin olun ve Not cihaz adı (Aşağıdaki örnek/dev/sde) bitti

   <pre><code>
   lsscsi
   
   # [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
   # [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
   # [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
   # [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
   # <b>[6:0:0:0]    disk    LIO-ORG  cl1              4.0   /dev/sde</b>
   </code></pre>

   Şimdi, iSCSI cihaz Kimliğini alın.

   <pre><code>
   ls -l /dev/disk/by-id/scsi-* | grep <b>sde</b>
   
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-1LIO-ORG_cl1:3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   # <b>lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df -> ../../sde</b>
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-SLIO-ORG_cl1_3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   </code></pre>

   Üç cihaz kimlikleri Listele komutu. SCSI-3, bu Yukarıdaki örnekteki ile başlayan kimliği kullanılması önerilir
   
   **/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df**

1. **[1]**  SBD cihaz oluşturma

   Cihazın kimliği iSCSI ilk küme düğümüne SBD yeni bir cihaz oluşturmak için kullanın.

   <pre><code>
   sudo sbd -d <b>/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df</b> -1 10 -4 20 create
   </code></pre>

1. **[A]**  SBD config uyum

   SBD yapılandırma dosyasını aç

   <pre><code>
   sudo vi /etc/sysconfig/sbd
   </code></pre>

   SBD cihaz özelliğini değiştirin, pacemaker entegrasyon sağlayın ve SBD başlangıç modunu değiştirin.

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

   Artık modülünü yükleme

   <pre><code>
   sudo modprobe -v softdog
   </code></pre>

## <a name="cluster-installation"></a>Küme yükleme

Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  Güncelleştirme SLES

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Ssh erişimi etkinleştir

   <pre><code>
   sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

2. **[2]**  Ssh erişimi etkinleştir

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

1. **[1]**  Ssh erişimi etkinleştir

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Dilimi yükleme aracıları
   
   <pre><code>
   sudo zypper install fence-agents
   </code></pre>

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi   

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin. / Etc/hosts kullanmanın avantajı, kümenizi tek hata noktası çok olabilecek DNS bağımsız olmasıdır.

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.   
   
   <pre><code>
   # IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[1]**  Küme yükleme
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> Press ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> Press ENTER
   # Multicast port [5405] -> Press ENTER
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Küme düğümü Ekle
   
   <pre><code> 
   sudo ha-cluster-join
   
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.14
   # /root/.ssh/id_rsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Aynı parolayı hacluster parolasını değiştirme

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Corosync diğer aktarım kullanın ve bir düğüm listesine eklemek için yapılandırın. Aksi takdirde, küme çalışmaz.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   Değerler var. ya da farklı değilse kalın aşağıdaki içeriği dosyaya ekleyin. Bakımı koruma bellek izin vermek için 30000 belirteç değiştirdiğinizden emin olun. Bkz: [Linux'a yönelik bu makaleyi] [ virtual-machines-linux-maintenance] veya [Windows] [ virtual-machines-windows-maintenance] daha fazla ayrıntı için.
   
   <pre><code> 
   [...]
     <b>token:          30000
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

STONITH cihaz, Microsoft Azure karşı korunmasına yetki vermek için bir hizmet sorumlusu kullanır. Bir hizmet sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Şuraya gidin: <https://portal.azure.com>
1. Azure Active Directory dikey penceresini açın  
   Özellikler bölümüne gidin ve dizin kimliği yazma Bu **Kiracı kimliği**.
1. Uygulama kayıtları tıklayın
1. Ekle'ye tıklayın.
1. Bir ad girin, "Web uygulaması/API'si" uygulama türünü seçin, bir oturum açma URL'sini girin (örneğin http://localhost) ve Oluştur'a tıklayın
1. Oturum açma URL'si kullanılmaz ve geçerli bir URL olabilir
1. Yeni uygulamayı seçin ve ayarları sekmesini anahtarları
1. Yeni bir anahtar için bir açıklama girin, "Her zaman geçerli olsun"'i seçin ve Kaydet'e tıklayın
1. Değeri yazın. Olarak kullanılan **parola** için hizmet sorumlusu
1. Uygulama Kimliği yazma Kullanıcı adı olarak kullanılır (**oturum açma kimliği** sonraki adımlarda), hizmet sorumlusu

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]**  Sınır aracısı için özel bir rol oluşturun

Hizmet sorumlusu kullanarak Azure kaynaklarınızı varsayılan olarak erişim izni yok. Başlatmak ve durdurmak için hizmet sorumlusu izinleri vermeniz gerekir (serbest bırakın) kümenin tüm sanal makineler. Özel rol zaten oluşturmadıysanız, bunu kullanarak oluşturabilirsiniz [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell#create-a-custom-role) veya [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli#create-a-custom-role)

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

### <a name="1-assign-the-custom-role-to-the-service-principal"></a>**[1]**  Hizmet sorumlusuna özel rolü atama

Özel rol "Linux sınır aracısı hizmet sorumlusuna son bölümde oluşturduğunuz rolü" atayın. Sahip rolü artık kullanmayın!

1. Şuraya gidin: https://portal.azure.com
1. Tüm kaynaklar dikey penceresini açın
1. İlk küme düğümüne sanal makinesini seçin
1. Erişim denetimi (IAM)'ye tıklayın.
1. Ekle'ye tıklayın.
1. "Linux sınır Aracısı rolü" rolü seçin
1. Yukarıda oluşturduğunuz uygulamanın adını girin
1. Tamam'a tıklayın

İkinci küme düğümü için yukarıdaki adımları yineleyin.

### <a name="1-create-the-stonith-devices"></a>**[1]**  STONITH cihazları oluşturun

Sanal makineler için izinleri düzenleme sonra kümedeki STONITH cihazları yapılandırabilirsiniz.

<pre><code>
# replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
sudo crm configure property stonith-timeout=900

sudo crm configure primitive rsc_st_azure stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

</code></pre>

### <a name="1-create-fence-topology-for-sbd-fencing"></a>**[1]**  SBD sınır için sınır topolojisi oluşturma

Bir SBD cihazı kullanmak istiyorsanız, iSCSI hedef sunucusu kullanılabilir değilse, bir Azure sınır Aracısı bir yedek olarak kullanarak yine de öneririz.

<pre><code>
sudo crm configure fencing_topology \
  stonith-sbd rsc_st_azure

</code></pre>
### **[1] ** STONITH cihaz kullanımını etkinleştir

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>


## <a name="next-steps"></a>Sonraki adımlar
* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* Yüksek kullanılabilirlik ve Azure Vm'leri üzerinde SAP hana olağanüstü durum kurtarma planı oluşturma hakkında bilgi almak için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
