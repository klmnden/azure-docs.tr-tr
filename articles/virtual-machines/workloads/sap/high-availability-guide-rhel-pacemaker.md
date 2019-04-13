---
title: SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama | Microsoft Docs
description: SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/17/2018
ms.author: sedusch
ms.openlocfilehash: b844c93a1f3e83d682b51db6f9854f11b24d82e7
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543757"
---
# <a name="setting-up-pacemaker-on-red-hat-enterprise-linux-in-azure"></a>SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[virtual-machines-linux-maintenance]:../../linux/maintenance-and-updates.md#maintenance-not-requiring-a-reboot

> [!NOTE]
> SLES Red Hat Enterprise Linux üzerinde pacemaker Azure sınır Aracısı gerekirse bir küme düğümü Çit için kullanır. Bir yük devretme, bir kaynak durdurma başarısız olursa veya küme düğümleri, birbirine artık iletişim kuramıyor 15 dakika kadar sürebilir. Daha fazla bilgi için okuma [Azure RHEL yüksek kullanılabilirlik kümesi üyesi olarak çalışan VM çevrelenmiş olacak şekilde çok uzun zaman alabilir veya çitlemek başarısız / kez genişletme VM kapatılmadan önce](https://access.redhat.com/solutions/3408711)

Öncelikle aşağıdaki SAP notları ve incelemeleri okuyun:

* SAP notu [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen bir Azure VM boyutlarını listesi.
  * Azure VM boyutları için önemli kapasite bilgileri.
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimleri.
  * Windows ve Linux'ta Microsoft Azure için gerekli SAP çekirdek sürümü.
* SAP notu [2015553] azure'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP notu [2002167] Red Hat Enterprise Linux işletim sistemi ayarlarını önerilir
* SAP notu [2009879] Red Hat Enterprise Linux için SAP HANA yönergeleri içeriyor
* SAP notu [2178632] ayrıntılı azure'da SAP için bildirilen tüm izlenen ölçümler hakkında bilgi içerir.
* SAP notu [2191498] azure'da Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP notu [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi içeriyor.
* SAP notu [1999351] Azure Gelişmiş izleme uzantısı için SAP için ek bilgiler.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tüm SAP notları Linux için zorunludur.
* [Azure sanal makineleri planlama ve uygulama için Linux üzerinde SAP][planning-guide]
* [(Bu makale) Linux'ta SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* [SAP HANA sistem çoğaltması pacemaker kümedeki](https://access.redhat.com/articles/3004101)
* Genel RHEL belgeleri
  * [Yüksek kullanılabilirlik eklentilere genel bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek kullanılabilirlik eklenti Yönetim](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek kullanılabilirlik eklenti başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Azure özel RHEL belgeleri:
  * [RHEL yüksek kullanılabilirlik kümelerini - Microsoft Azure sanal makineleri küme üyeleri olarak ilkeleri desteği](https://access.redhat.com/articles/3131341)
  * [Yükleme ve Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve üzeri) yüksek kullanılabilirlik kümesi yapılandırma](https://access.redhat.com/articles/3252491)

## <a name="cluster-installation"></a>Küme yükleme

![SLES RHEL genel bakış üzerinde pacemaker](./media/high-availability-guide-rhel-pacemaker/pacemaker-rhel.png)

Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  Kaydetme

   Sanal makinelerinizi kaydedin ve RHEL 7 için depoları içeren bir havuz ekleme.

   <pre><code>sudo subscription-manager register
   # List the available pools
   sudo subscription-manager list --available --matches '*SAP*'
   sudo subscription-manager attach --pool=&lt;pool id&gt;
   </code></pre>

   Bir Azure Market PAYG RHEL görüntüsü için bir havuz ekleyerek, RHEL kullanımınız için etkili bir şekilde çift faturalandırılır olacağını unutmayın: PAYG görüntü için bir kez ve bir kez eklediğiniz havuzundaki RHEL yetkilendirme için. Bunu azaltmak için Azure artık görüntüleri BYOS RHEL sağlar. Daha fazla bilgi edinilebilir [burada](https://aka.ms/rhel-byos).

1. **[A]**  SAP depoları için RHEL etkinleştir

   Gerekli paketleri yüklemek için aşağıdaki depolardan etkinleştirin.

   <pre><code>sudo subscription-manager repos --disable "*"
   sudo subscription-manager repos --enable=rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable="rhel-sap-for-rhel-7-server-rpms"
   </code></pre>

1. **[A]**  Yükleme RHEL HA eklentisi

   <pre><code>sudo yum install -y pcs pacemaker fence-agents-azure-arm nmap-ncat
   </code></pre>

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin. / Etc/hosts kullanmanın avantajı, kümenizi tek hata noktası çok olabilecek DNS bağımsız olmasıdır.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[A]**  Aynı parolayı hacluster parolasını değiştirme

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]**  Pacemaker için güvenlik duvarı kuralları ekleme

   Küme düğümleri arasında tüm küme iletişimi için aşağıdaki güvenlik duvarı kuralları ekleyin.

   <pre><code>sudo firewall-cmd --add-service=high-availability --permanent
   sudo firewall-cmd --add-service=high-availability
   </code></pre>

1. **[A]**  Temel küme Hizmetleri'ni etkinleştirme

   Pacemaker hizmetini etkinleştirmek ve başlatmak için aşağıdaki komutları çalıştırın.

   <pre><code>sudo systemctl start pcsd.service
   sudo systemctl enable pcsd.service
   </code></pre>

1. **[1]**  Oluşturma Pacemaker küme

   Düğümleri kimliğini doğrulamak ve kümeyi oluşturmak için aşağıdaki komutları çalıştırın. Belirteç 30000 Bakımı koruma bellek izin vermek için'olarak ayarlayın. Daha fazla bilgi için [Linux'a yönelik bu makaleyi][virtual-machines-linux-maintenance].

   <pre><code>sudo pcs cluster auth <b>prod-cl1-0</b> <b>prod-cl1-1</b> -u hacluster
   sudo pcs cluster setup --name <b>nw1-azr</b> <b>prod-cl1-0</b> <b>prod-cl1-1</b> --token 30000
   sudo pcs cluster start --all

   # Run the following command until the status of both nodes is online
   sudo pcs status

   # Cluster name: nw1-azr
   # WARNING: no stonith devices and stonith-enabled is not false
   # Stack: corosync
   # Current DC: <b>prod-cl1-1</b> (version 1.1.18-11.el7_5.3-2b07d5c5a9) - partition with quorum
   # Last updated: Fri Aug 17 09:18:24 2018
   # Last change: Fri Aug 17 09:17:46 2018 by hacluster via crmd on <b>prod-cl1-1</b>
   #
   # 2 nodes configured
   # 0 resources configured
   #
   # Online: [ <b>prod-cl1-0</b> <b>prod-cl1-1</b> ]
   #
   # No resources
   #
   #
   # Daemon Status:
   #   corosync: active/disabled
   #   pacemaker: active/disabled
   #   pcsd: active/enabled
   </code></pre>

1. **[A]**  Beklenen oyları ayarlayın

   <pre><code>sudo pcs quorum expected-votes 2
   </code></pre>

## <a name="create-stonith-device"></a>STONITH cihaz oluşturma

STONITH cihaz, Microsoft Azure karşı korunmasına yetki vermek için bir hizmet sorumlusu kullanır. Bir hizmet sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Şuraya gidin: <https://portal.azure.com>
1. Özellikler ve dizin kimliği azaltma Git Azure Active Directory dikey penceresini açın Bu **Kiracı kimliği**.
1. Uygulama kayıtları tıklayın
1. Ekle'ye tıklayın.
1. Bir ad girin, "Web uygulaması/API'si" uygulama türünü seçin, bir oturum açma URL'sini girin (örneğin http:\//localhost) ve Oluştur'a tıklayın
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

1. Şuraya gidin: https://portal.azure.com
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

<pre><code>
sudo pcs property set stonith-timeout=900
</code></pre>

Sınır cihazı yapılandırmak için aşağıdaki komutu kullanın.

> [!NOTE]
> RHEL ana bilgisayar adları ve Azure düğüm adları değilse seçeneği 'pcmk_host_map' komut içinde yalnızca gerekli aynı. Komutta kalın bölümüne bakın.

<pre><code>sudo pcs stonith create rsc_st_azure fence_azure_arm login="<b>login ID</b>" passwd="<b>password</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" subscriptionId="<b>subscription id</b>" <b>pcmk_host_map="prod-cl1-0:10.0.0.6;prod-cl1-1:10.0.0.7"</b> power_timeout=240 pcmk_reboot_timeout=900</code></pre>

### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]**  STONITH cihaz kullanımını etkinleştir

<pre><code>sudo pcs property set stonith-enabled=true
</code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* Yüksek kullanılabilirlik ve Azure Vm'leri üzerinde SAP hana olağanüstü durum kurtarma planı oluşturma hakkında bilgi almak için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
