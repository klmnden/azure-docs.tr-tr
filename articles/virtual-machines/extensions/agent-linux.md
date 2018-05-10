---
title: Azure Linux VM Aracısı genel bakış | Microsoft Docs
description: Azure yapı denetleyicisi ile sanal makinenizin etkileşimi yönetmek için Linux Aracısı (waagent) yükleyip öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: danis
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: danis
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2fe93cba2c8b295925ce4cfa8c3017ee1373261
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="understanding-and-using-the-azure-linux-agent"></a>Anlama ve Azure Linux Aracısı'nı kullanma

Microsoft Azure Linux Aracısı'nı (waagent), Linux ve FreeBSD sağlama ve Azure yapı denetleyicisi VM etkileşim yönetir. Linux sağlama işlevsellik sağlayan Aracısı yanı sıra Azure bulut init bazı Linux işletim sistemleri için kullanma seçeneğini de sağlar. Linux aracısı aşağıdaki işlevleri Linux ve FreeBSD Iaas dağıtımları sağlar:

> [!NOTE]
> Daha fazla bilgi için bkz: [Benioku](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
> 
> 

* **Görüntü sağlama**
  
  * Kullanıcı hesabı oluşturma
  * SSH kimlik doğrulama türlerini yapılandırma
  * SSH ortak anahtarları ve anahtar çiftleri dağıtımı
  * Ana bilgisayar adı ayarlama
  * Ana bilgisayar adı platformuna DNS yayımlama
  * Raporlama SSH ana anahtar parmak izi platformu
  * Kaynak Disk Yönetimi
  * Biçimlendirme ve kaynak diski takma
  * Takas alanı yapılandırma
* **Ağ**
  
  * Platform DHCP sunucularıyla uyumluluğu artırmak için yollar yönetir
  * Ağ arabirimi adı kararlılığını sağlar
* **Çekirdek**
  
  * Sanal NUMA yapılandırır (çekirdek için devre dışı <`2.6.37`)
  * Hyper-V entropi /dev/random için kullanır
  * SCSI zaman aşımı (uzak olabilir) kök cihazı için yapılandırır
* **Tanılama**
  
  * Seri bağlantı noktası konsol yeniden yönlendirme
* **SCVMM dağıtımları**
  
  * Algılar ve Linux için VMM aracısının System Center Virtual Machine Manager 2012 R2 ortamında çalıştırırken bootstraps
* **VM uzantısı**
  
  * Linux yazılım etkinleştirmek için VM (Iaas) ve yapılandırma Otomasyon halinde Microsoft ve iş ortakları tarafından yazılan Bileşen Ekle
  * VM uzantısı başvurusu mantığınız [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>İletişim
Aracı platformdan bilgi akışına iki kanallar oluşur:

* Önyükleme zamanında DVD Iaas dağıtımları için eklendi. Bu DVD gerçek SSH keypairs dışındaki tüm sağlama bilgilerini içeren bir OVF uyumlu yapılandırma dosyası içerir.
* Bir REST API gösterme TCP uç noktası, dağıtım ve topoloji yapılandırması elde etmek için kullanılır.

## <a name="requirements"></a>Gereksinimler
Aşağıdaki sistemleri test edilmiş ve Azure Linux Aracısı ile çalışmak için bilinen:

> [!NOTE]
> Bu liste desteklenen Microsoft Azure platformu sistemlerinde resmi listesi burada açıklandığı gibi farklı olabilir: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)
> 
> 

* CoreOS
* CentOS 6.3 +
* Red Hat Enterprise Linux 6.7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Desteklenen diğer sistemleri:

* FreeBSD 10 + (Azure Linux Aracısı v2.0.10 +)

Linux Aracısı düzgün çalışması için bazı sistem paketleri bağlıdır:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Dosya sistemi yardımcı programları: sfdisk, fdisk, mkfs, parted
* Parola araçları: chpasswd, sudo
* Araçlar işleme metin: azaltılabilir, grep
* Ağ araçları: IP yönlendirme
* Çekirdek desteği UDF bağlanan dosya sistemlerinin bağlanması için.

## <a name="installation"></a>Yükleme
Dağıtımınız ait paket depodan bir RPM veya DEB paketini kullanarak yükleme, Azure Linux Aracısı yükseltme ve yükleme için tercih edilen yöntemdir. Tüm [dağıtım sağlayıcıları destekli](../linux/endorsed-distros.md) Azure Linux Aracısı paketi depoları ve görüntüleri tümleştirin.

Belgelerde başvurmak [Azure Linux Aracısı bağlantıların github'da](https://github.com/Azure/WALinuxAgent) kaynağından ya da özel konumlar veya ön yükleme gibi gelişmiş yükleme seçenekleri için.

## <a name="command-line-options"></a>Komut satırı seçenekleri
### <a name="flags"></a>Bayrakları
* ayrıntılı: Belirtilen komut dosyasının ayrıntı düzeyini artırın
* Zorla: Bazı komutlar için etkileşimli Onayı Atla

### <a name="commands"></a>Komutlar
* Yardım: bayrakları ve desteklenen komutları listeler.
* deprovision: Sistem temizlemek ve sağlama işleminin için uygun hale girişimi. Aşağıdaki işlem siler:
  
  * (Provisioning.RegenerateSshHostKeyPair yapılandırma dosyasındaki ' y' ise) tüm SSH konak anahtarları
  * /Etc/resolv.conf ad sunucusu yapılandırma
  * Kök paroladan (Provisioning.DeleteRootPassword yapılandırma dosyasındaki ' y' ise) eşitleme
  * Önbelleğe alınan DHCP istemci kiraları
  * Ana bilgisayar adını localhost.localdomain adına sıfırlar

> [!WARNING]
> Sağlamayı kaldırmayı görüntü tüm hassas bilgilerin temizlenmiş ve yeniden dağıtım için uygun olduğunu garanti etmez.
> 
> 

* deprovision + kullanıcı: her şeyi (yukarıda)-deprovision gerçekleştirir ve ayrıca (/var/lib/waagent elde edilen) son sağlanan kullanıcının hesabını siler ve veri ilişkilendirilmiş. Bu parametre, yakalanan ve yeniden, böylece Azure üzerinde önceden sağlama bir görüntü XML'deki sağlamada olur.
* Sürüm: waagent sürümünü görüntüler
* serialconsole: ttyS0 işaretlemek için kaz yapılandırır (ilk seri bağlantı noktası) önyükleme konsolu olarak. Bu çekirdek önyükleme günlüklerini seri bağlantı noktasına gönderilen ve hata ayıklama için kullanılabilir hale sağlar.
* arka plan programı: waagent platformuyla etkileşim yönetmek için arka plan programı çalıştırın. Bu bağımsız değişken waagent waagent init komut dosyasında belirtilir.
* Başlat: waagent arka plan işlemi olarak çalıştır

## <a name="configuration"></a>Yapılandırma
Bir yapılandırma dosyası (/ etc/waagent.conf) waagent Eylemler denetler. Aşağıdaki örnek bir yapılandırma dosyası gösterir:

    ```
    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.AllowResetSysUser=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None
    AutoUpdate.Enabled=y
    ```

Aşağıdaki çeşitli yapılandırma seçenekleri açıklanmıştır. Yapılandırma seçenekleri üç tür içindedir; Boolean, String veya tamsayı. Boole yapılandırma seçenekleri "y" veya "n" belirtilebilir. "Hiçbiri" aşağıdaki ayrıntıları için bazı dize türü yapılandırma girdileri kullanılabilir özel anahtar sözcüğü:

**Provisioning.Enabled:**  
```
Type: Boolean  
Default: y
```
Bu kullanıcıya etkinleştirmek veya devre dışı aracı sağlama işlevleri sağlar. "Y" veya "n" değerler geçerlidir. Sağlama devre dışıysa, SSH ana bilgisayar ve kullanıcı anahtarları görüntüdeki korunur ve API sağlama Azure içinde belirtilen herhangi bir yapılandırma yok sayılır.

> [!NOTE]
> `Provisioning.Enabled` Parametresi varsayılan olarak "n" Ubuntu bulut görüntülerde, bulut init sağlamak için kullanın.
> 
> 

**Provisioning.DeleteRootPassword:**  
```
Type: Boolean  
Default: n
```
Kümesi, / etc/gölge dosyasında kök parolasını sağlama işlemi sırasında silinmesi durumunda.

**Provisioning.RegenerateSshHostKeyPair:**  
```
Type: Boolean  
Default: y
```
Gelen/etc/ssh/sağlama işlemi sırasında kümesi, tüm SSH ana anahtar çiftleri (ecdsa, dsa ve rsa) silinmişse. Ve tek bir yeni anahtar çifti oluşturulur.

Yeni anahtar çifti için şifreleme türü Provisioning.SshHostKeyPairType girişi tarafından yapılandırılabilir. SSH arka plan programı (örneğin, bir yeniden başlatma sırasında) yeniden başlatıldığında bazı dağıtımları eksik herhangi bir şifreleme türü için anahtar çiftleri SSH yeniden oluşturun.

**Provisioning.SshHostKeyPairType:**  
```
Type: String  
Default: rsa
```
Bu sanal makinenin SSH arka plan tarafından desteklenen bir şifreleme algoritması türü için ayarlanabilir. "Rsa", "dsa" ve "ecdsa" genellikle değerleri desteklenir. "putty.exe" Windows "ecdsa" desteklemez. Bir Linux dağıtımına bağlanmak için Windows putty.exe kullanmayı düşünüyorsanız, bu nedenle, "rsa" veya "dsa" kullanın.

**Provisioning.MonitorHostName:**  
```
Type: Boolean  
Default: y
```
Linux sanal makine ("ana bilgisayar adı" komutu tarafından döndürülen gibi) ana bilgisayar adı değişiklikleri ve otomatik olarak ayarlama, waagent izler değişikliği yansıtacak şekilde görüntüsündeki ağ yapılandırmasını güncelleştirin. Ad değişikliği DNS sunucularına göndermek için ağı içinde sanal makine yeniden başlatılır. Bu kısaca Internet bağlantı kaybı ile sonuçlanır.

**Provisioning.DecodeCustomData**  
```
Type: Boolean  
Default: n
```
Ayarlama, waagent Base64 gelen CustomData kodunu çözer.

**Provisioning.ExecuteCustomData**  
```
Type: Boolean  
Default: n
```
Ayarlama, waagent CustomData sağladıktan sonra yürütülürse.

**Provisioning.AllowResetSysUser**
```
Type: Boolean
Default: n
```
Bu seçenek sıfırlama sys kullanıcı için parola sağlar; Varsayılan devre dışıdır.

**Provisioning.PasswordCryptId**  
```
Type: String  
Default: 6
```
Parola karmasını oluştururken crypt tarafından kullanılan algoritma.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
```
Type: String  
Default: 10
```
Parola Karması oluşturulurken kullanılan rastgele salt uzunluğu.

**ResourceDisk.Format:**  
```
Type: Boolean  
Default: y
```
Ayarlama, platform tarafından sağlanan kaynak diskin biçimlendirildiğinden ve "ResourceDisk.Filesystem" kullanıcı tarafından istenen dosya sistemi türü "ntfs" dışında bir şey ise waagent tarafından oluşturulmuş varsa. Linux (83) türünde tek bir bölüm disk üzerinde kullanılabilir hale getirilir. Bu bölüm, başarılı bir şekilde bağlanabiliyorsa biçimlendirilmemiş.

**ResourceDisk.Filesystem:**  
```
Type: String  
Default: ext4
```
Bu kaynak disk dosya sistemi türünü belirtir. Desteklenen değerler Linux dağıtım göre farklılık gösterir. Dize X, ardından mkfs ise. X Linux görüntüde mevcut olması. SLES 11 görüntüleri genellikle 'ext3' kullanmanız gerekir. FreeBSD görüntüleri 'ufs2' burada kullanmanız gerekir.

**ResourceDisk.MountPoint:**  
```
Type: String  
Default: /mnt/resource 
```
Bu kaynak disk bağlı yolunu belirtir. Kaynak disk bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir.

**ResourceDisk.MountOptions**  
```
Type: String  
Default: None
```
Mount -o komuta geçirilecek disk bağlama seçeneklerini belirtir. Ex virgülle ayrılmış bir liste değerleri budur. 'nodev, nosuid'. Mount(8) Ayrıntılar için bkz.

**ResourceDisk.EnableSwap:**  
```
Type: Boolean  
Default: n
```
Varsa bir takas dosyası ayarı (/ swapfile) kaynak disk üzerinde oluşturulur ve sistem değiştirme alanına eklenir.

**ResourceDisk.SwapSizeMB:**  
```
Type: Integer  
Default: 0
```
Takas dosyası megabayt cinsinden boyutu.

**Logs.Verbose:**  
```
Type: Boolean  
Default: n
```
Kümesi, günlük ayrıntı boosted durumunda. Waagent /var/log/waagent.log için günlüğe kaydeder ve günlükleri döndürmek için sistem logrotate işlevselliğini kullanır.

**OS.EnableRDMA**  
```
Type: Boolean  
Default: n
```
Ayarlama, aracıyı yüklemek ve bir RDMA çekirdek sürücüsü yüklemek çalışırsa, temel alınan donanım üzerinde bellenim sürümünü eşleşir.

**İŞLETİM SİSTEMİ. RootDeviceScsiTimeout:**  
```
Type: Integer  
Default: 300
```
Bu ayarı, işletim sistemi diski ve veri sürücülerinde saniye cinsinden SCSI zaman aşımı yapılandırır. Aksi durumda ayarlanırsa, sistem varsayılan değerler kullanılır.

**OS.OpensslPath:**  
```
Type: String  
Default: None
```
Bu ayar, şifreleme işlemleri için ikili openssl için alternatif bir yol belirtmek için kullanılabilir.

**HttpProxy.Host, HttpProxy.Port**  
```
Type: String  
Default: None
```
Ayarlama, aracı Internet'e erişmek için bu proxy sunucusu kullanıyorsa. 

**AutoUpdate.Enabled**
```
Type: Boolean
Default: y
```
Etkinleştirmek veya hedef durumu işlenmesi için otomatik güncelleştirmeyi devre dışı; Varsayılan etkindir.



## <a name="ubuntu-cloud-images"></a>Ubuntu bulut görüntüleri
Ubuntu bulut görüntüleri kullanma [bulut init](https://launchpad.net/ubuntu/+source/cloud-init) Azure Linux aracısı tarafından yönetilmesini birçok yapılandırma görevlerini gerçekleştirmek için. Aşağıdaki farkları Uygula:

* **Provisioning.Enabled** varsayılan olarak "n" Ubuntu bulut görüntülerde, bulut init sağlama görevleri gerçekleştirmek için kullanın.
* Aşağıdaki yapılandırma parametreleri kaynak disk yönetmek ve değiştirme alanı için bulut init kullanın Ubuntu bulut görüntüleri üzerinde hiçbir etkisi vardır:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**

* Daha fazla bilgi için kaynak disk bağlama noktası yapılandırın ve sağlama işlemi sırasında değiştirme alanı Ubuntu bulut görüntüleri için aşağıdaki kaynaklara bakın:
  
  * [Ubuntu Wiki: Takas bölümlerini yapılandırma](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Bir Azure sanal makinesine özel veri injecting](../windows/classic/inject-custom-data.md)

