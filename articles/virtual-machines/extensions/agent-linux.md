---
title: Azure Linux VM Aracısı genel bakış | Microsoft Docs
description: Yükleme ve sanal makinenizin etkileşimi Azure yapı denetleyicisi ile yönetmek için Linux aracısını (waagent) yapılandırma hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
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
ms.author: roiyz
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1defa08b0eb9ede2adec3b7ac12c873522dd6c37
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60800220"
---
# <a name="understanding-and-using-the-azure-linux-agent"></a>Anlama ve Azure Linux Aracısı'nı kullanma

Microsoft Azure Linux aracısını (waagent), Linux ve FreeBSD sağlama ve Azure yapı denetleyicisi ile VM etkileşimi yönetir. Azure Linux sağlama işlevsellik sağlayan Aracısı ek olarak, bazı Linux işletim sistemi ortamları için cloud-init kullanma seçeneği sunar. Linux aracısı aşağıdaki işlevleri, Linux ve FreeBSD Iaas dağıtımları sağlar:

> [!NOTE]
> Daha fazla bilgi için [Benioku](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
> 
> 

* **Görüntü sağlama**
  
  * Bir kullanıcı hesabı oluşturma
  * SSH kimlik doğrulaması türlerini yapılandırma
  * SSH ortak anahtarları ve anahtar çiftlerini dağıtımı
  * Ana bilgisayar adını ayarlama
  * Ana bilgisayar adı platformuna DNS yayımlama
  * SSH konak anahtarı parmak izi platformuna raporlamaya
  * Kaynak Disk Yönetimi
  * Biçimlendirme ve kaynak disk takılamadı
  * Takas alanı yapılandırma
* **Ağ**
  
  * Platform DHCP sunucuları ile uyumluluğu artırmak için rotalar yönetir
  * Ağ arabirimi adı kararlılığını sağlar
* **Çekirdek**
  
  * Sanal NUMA'yı yapılandırır (çekirdek için devre dışı bırakın <`2.6.37`)
  * Hyper-V entropi /dev/random için kullanır.
  * SCSI zaman aşımları (uzak olabilir) kök cihaz yapılandırır
* **Tanılama**
  
  * Konsol yönlendirmesi seri bağlantı noktası
* **SCVMM dağıtımları**
  
  * Algılar ve Linux için VMM Aracısı, System Center Virtual Machine Manager 2012 R2 ortamında çalıştırırken bootstraps
* **VM uzantısı**
  
  * Microsoft ve iş ortakları tarafından yazılım etkinleştirmek için Linux VM (Iaas) ve yapılandırma Otomasyonu yazılan bileşeni ekleme
  * VM uzantısı başvuru uygulaması üzerinde [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>İletişim
Aracı platformundan bilgi akışına iki kanalı gerçekleşir:

* Önyükleme zamanı DVD Iaas dağıtımları için eklenmiş. Bu DVD gerçek SSH keypairs dışındaki tüm sağlama bilgilerini içeren bir OVF uyumlu bir yapılandırma dosyası içerir.
* REST API'sini kullanıma sunan bir TCP uç noktası, dağıtım ve topoloji yapılandırması elde etmek için kullanılır.

## <a name="requirements"></a>Gereksinimler
Aşağıdaki sistemleri edilmiş ve Azure Linux Aracısı ile çalışmak için bilinmektedir:

> [!NOTE]
> Bu liste, resmi Microsoft Azure platformunda desteklenen sistemleri listesinden burada açıklandığı değişebilir: [https://support.microsoft.com/kb/2805216](https://support.microsoft.com/kb/2805216)
> 
> 

* CoreOS
* CentOS 6.3+
* Red Hat Enterprise Linux 6.7+
* Debian 7.0 +
* Ubuntu 12.04+
* openSUSE 12.3 +
* SLES 11 SP3+
* Oracle Linux 6.4+

Desteklenen diğer sistemler:

* FreeBSD 10 + (Azure Linux Aracısı v2.0.10 +)

Linux Aracısı düzgün çalışması için bazı sistem paketleri bağlıdır:

* Python 2.6 +
* OpenSSL 1.0+
* OpenSSH 5.3+
* Dosya sistemi yardımcı programları: sfdisk fdisk, mkfs, parted
* Parola araçları: chpasswd, sudo
* Metin araçları işleme: sed, grep
* Ağ araçları: IP yönlendirme
* Çekirdek UDF dosya sistemleri bağlamak için destek.

## <a name="installation"></a>Yükleme
Dağıtımınıza ait bir paket deposundaki bir RPM veya DEB paketini kullanarak yükleme, Azure Linux Aracısı yükseltme ve yükleme için tercih edilen yöntemdir. Tüm [dağıtım sağlayıcıları onaylı](../linux/endorsed-distros.md) Azure Linux Aracısı paketi, görüntüleri ve depoları tümleştirme.

Belgeye başvurun [GitHub üzerindeki Azure Linux Aracısı depo](https://github.com/Azure/WALinuxAgent) kaynak veya özel konumları veya ön ekleri için yükleme gibi gelişmiş yükleme seçenekleri için.

## <a name="command-line-options"></a>Komut satırı seçenekleri
### <a name="flags"></a>bayrakları
* ayrıntılı: Belirtilen komut ayrıntı düzeyini artırın
* Zorla: Bazı komutlar için etkileşimli onayı atlayın

### <a name="commands"></a>Komutlar
* Yardım: Desteklenen komutlar ve bayrakları listeler.
* sağlamayı kaldırma: Sistem temizleyin ve çıkış için uygun hale getiren girişimi. Aşağıdaki işlem siler:
  
  * (Provisioning.regeneratesshhostkeypair değerini 'y' yapılandırma dosyası ise) tüm SSH ana bilgisayar anahtarları
  * /Etc/resolv.conf ad sunucusu yapılandırması
  * Kök parolasını (Provisioning.deleterootpassword değerini 'y' yapılandırma dosyası ise) eşitleme
  * Önbelleğe alınan istemci kiralamalarını
  * Ana bilgisayar adını localhost.localdomain adına sıfırlar

> [!WARNING]
> Sağlama kaldırmayı görüntü tüm hassas bilgilerin temizlenir ve yeniden dağıtım için uygun olduğunu garanti etmez.
> 
> 

* sağlamayı kaldırma + kullanıcı: Her şeyi sağlamayı kaldırma-(yukarıda) gerçekleştirir ve de (/var/lib/waagent alınan) son sağlanan kullanıcı hesabı siler ve ilişkili verileri. Bu parametre, yakalanan ve yeniden, daha önce Azure'da sağlama bir görüntü sağlamayı zaman olur.
* Sürüm: Waagent sürümünü görüntüler
* serialconsole: GRUB ttyS0 işaretlemek için yapılandırır (ilk seri bağlantı noktası) Önyükleme Konsolu. Bu, çekirdek önyükleme günlüklerini seri bağlantı noktasına gönderilen ve hata ayıklama için kullanılabilir hale sağlar.
* arka plan programı: Waagent platformuyla etkileşim yönetmek için bir arka plan olarak çalıştırın. Bu bağımsız değişken waagent waagent init komut dosyasında belirtilir.
* Başlat: Waagent bir arka plan işlemi Çalıştır

## <a name="configuration"></a>Yapılandırma
Bir yapılandırma dosyası (/ etc/waagent.conf) waagent eylemlerini denetler. Aşağıdaki örnek bir yapılandırma dosyası gösterir:

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

Aşağıdaki çeşitli yapılandırma seçenekleri açıklanmaktadır. Yapılandırma seçenekleri üç türleridir; Boole, dize veya tamsayı. Boole yapılandırma seçenekleri, "y" veya "n" olarak belirtilebilir. Özel anahtar sözcüğü için bazı dize türü yapılandırma girdileri "None" aşağıdaki ayrıntıları kullanılabilir:

**Provisioning.Enabled:**  
```
Type: Boolean  
Default: y
```
Bu kullanıcının etkinleştirme veya devre dışı sağlama aracı işlevselliği sağlar. Geçerli değerler "y" veya "n" olmalı. Sağlama devre dışıysa, SSH ana bilgisayar ve kullanıcı anahtarları görüntüde korunur ve API sağlama Azure içinde belirtilen herhangi bir yapılandırma yok sayılır.

> [!NOTE]
> `Provisioning.Enabled` Ubuntu bulut görüntülerindeki sağlamak için cloud-init kullanma "n" parametresi varsayılan değeri.
> 
> 

**Provisioning.deleterootpassword değerini:**  
```
Type: Boolean  
Default: n
```
Kök parolasını/etc/shadow dosyasındaki kümesi sağlama işlemi sırasında silinmesi durumunda.

**Provisioning.regeneratesshhostkeypair değerini:**  
```
Type: Boolean  
Default: y
```
Gelen/etc/ssh/sağlama işlemi sırasında tüm SSH konak anahtar çiftleri (ecdsa, dsa ve rsa), küme silindiğinde. Ve tek bir yeni anahtar çifti oluşturulur.

Şifreleme türü için yeni bir anahtar çifti Provisioning.SshHostKeyPairType giriş tarafından yapılandırılabilir. SSH arka plan programı (örneğin, bir yeniden başlatma) yeniden başlatıldığında, bazı dağıtımları eksik herhangi bir şifreleme türü için SSH anahtar çiftleri yeniden oluşturun.

**Provisioning.SshHostKeyPairType:**  
```
Type: String  
Default: rsa
```
Bu sanal makineye SSH arka plan programı tarafından desteklenen bir şifreleme algoritması türü için ayarlanabilir. Genellikle desteklenen değerler şunlardır: "rsa", "dsa" ve "ecdsa". Windows üzerinde "putty.exe", "ecdsa" desteklemez. Bir Linux dağıtımına bağlanmak için Windows üzerinde putty.exe kullanmak istiyorsanız, bu nedenle, "rsa" veya "dsa" kullanın.

**Provisioning.MonitorHostName:**  
```
Type: Boolean  
Default: y
```
Ana bilgisayar adı değişiklikleri ("ana bilgisayar adı" komutu tarafından döndürülen gibi) ve otomatik olarak Linux sanal makine kümesi, waagent izler görüntüde değişimi yansıtmak için ağ yapılandırmasını güncelleştirin. Ad değişikliği DNS sunucularına göndermek için ağ sanal makine yeniden başlatılır. Bu kısaca Internet bağlantı kaybı ile sonuçlanır.

**Provisioning.DecodeCustomData**  
```
Type: Boolean  
Default: n
```
Kümesi, waagent Base64 gelen CustomData çözerse.

**Provisioning.ExecuteCustomData**  
```
Type: Boolean  
Default: n
```
Kümesi, waagent sağladıktan sonra CustomData çalıştırırsa.

**Provisioning.AllowResetSysUser**
```
Type: Boolean
Default: n
```
Bu seçenek sıfırlanması sys kullanıcının parolasını sağlar; Varsayılan devre dışı bırakıldı.

**Provisioning.PasswordCryptId**  
```
Type: String  
Default: 6
```
Parola Karması oluştururken crypt tarafından kullanılan algoritma.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
```
Type: String  
Default: 10
```
Parola Karması oluştururken kullanılan rastgele salt uzunluğu.

**ResourceDisk.Format:**  
```
Type: Boolean  
Default: y
```
Ayarlayın, platform tarafından sağlanan kaynak disk biçimlendirilmiş ve "ResourceDisk.Filesystem" kullanıcı tarafından istenen dosya sistemi türü "ntfs" dışında herhangi bir şey ise waagent tarafından oluşturulmuş. Linux (83) türünde tek bir bölüm disk üzerindeki kullanılabilir hale getirilir. Bu bölüm, başarılı bir şekilde bağlanabiliyorsa biçimlendirilmemiş.

**ResourceDisk.Filesystem:**  
```
Type: String  
Default: ext4
```
Bu, kaynak diskin dosya sistemi türü belirtir. Desteklenen değerler Linux dağıtımına göre değişir. Bir dize X, ardından mkfs ise. X, Linux görüntüsü üzerinde mevcut olmalıdır. SLES 11 görüntüleri genellikle 'ext3' kullanmanız gerekir. FreeBSD görüntüleri 'ufs2' burada kullanmanız gerekir.

**ResourceDisk.MountPoint:**  
```
Type: String  
Default: /mnt/resource 
```
Bu, kaynak disk bağlı olduğu yerin yolu belirtir. Kaynak disk bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir.

**ResourceDisk.MountOptions**  
```
Type: String  
Default: None
```
Bağlama -o komuta geçirilecek disk bağlama seçeneklerini belirtir. Değerleri virgülle ayrılmış bir listesini ex budur. 'nodev, nosuid'. Mount(8) Ayrıntılar için bkz.

**ResourceDisk.EnableSwap:**  
```
Type: Boolean  
Default: n
```
Varsa bir takas dosyası ayarı (/ swapfile) kaynak disk üzerinde oluşturulur ve sistem takas alanı için eklendi.

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
Kümesi, günlük ayrıntı boosted durumunda. Waagent /var/log/waagent.log için günlükleri ve günlükleri döndürmek için sistem logrotate işlevselliğini kullanır.

**OS.EnableRDMA**  
```
Type: Boolean  
Default: n
```
Kümesi, aracıyı yükleyin ve ardından bir RDMA çekirdek sürücüsü yükleme girişiminde bulunursa, temel alınan donanım bellenimi sürümüyle eşleşmiyor.

**İŞLETİM SİSTEMİ. RootDeviceScsiTimeout:**  
```
Type: Integer  
Default: 300
```
Bu ayarı, işletim sistemi diski ve veri sürücülerinde saniye SCSI zaman aşımı yapılandırır. Aksi durumda ayarlamak, sistem varsayılan değerler kullanılır.

**OS.OpensslPath:**  
```
Type: String  
Default: None
```
Bu ayar, openssl şifreleme işlemleri için kullanılacak ikili için alternatif bir yol belirtmek için kullanılabilir.

**HttpProxy.Host, HttpProxy.Port**  
```
Type: String  
Default: None
```
Kümesi, aracı, İnternet'e erişmek için bu proxy sunucusu kullanıyorsa. 

**AutoUpdate.Enabled**
```
Type: Boolean
Default: y
```
Etkinleştirmek veya hedef durumunu işleme için otomatik güncelleştirme devre dışı; Varsayılan olarak etkindir.



## <a name="ubuntu-cloud-images"></a>Ubuntu Cloud Images
Ubuntu bulut görüntüleri kullanan [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) Azure Linux aracısı tarafından yönetilmesi çok sayıda yapılandırma görevlerini gerçekleştirmek için. Aşağıdaki farklar geçerlidir:

* **Provisioning.Enabled** Ubuntu bulut görüntülerindeki, hazırlama görevleri gerçekleştirmek için cloud-init kullanma "n" varsayılan değeri.
* Aşağıdaki yapılandırma parametrelerini Ubuntu bulut görüntüleri, kaynak disk yönetme ve değiştirme alanı için cloud-init kullanma üzerinde hiçbir etkisi yoktur:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**

* Daha fazla bilgi için kaynak disk bağlama noktası yapılandırın ve sağlama sırasında değiştirme Ubuntu bulut görüntülerindeki alanı için aşağıdaki kaynaklara bakın:
  
  * [Ubuntu Wiki: Takas bölümlerini yapılandırma](https://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Bir Azure sanal makinesine özel veri ekleme](../windows/classic/inject-custom-data.md)

