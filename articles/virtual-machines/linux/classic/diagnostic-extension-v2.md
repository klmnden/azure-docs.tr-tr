---
title: Bir Linux VM VM uzantısı ile izleme | Microsoft Docs
description: Azure'da bir Linux sanal makinesi tanılama verilerini ve performansı izlemek için Linux tanılama uzantısı'nı kullanmayı öğrenin.
services: virtual-machines-linux
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: 13d7594c15959661f3f9c3ab2165739719beac07
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308230"
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a>Linux Diagnostic Extension’ı kullanarak bir Linux VM’nin performansını ve tanılama verilerini izleme

Bu belgede Linux tanılama uzantısı 2.3 sürümü açıklanmaktadır.

> [!IMPORTANT]
> Bu sürüm kullanım dışıdır ve 30 Haziran 2018'den sonra istediğiniz zaman yayımdan kaldırılmış olabilir. Sürüm 3.0 ile değiştirilmiştir. Daha fazla bilgi için [Linux tanılama uzantısının 3.0 sürümü belgelerine](../diagnostic-extension.md).

## <a name="introduction"></a>Giriş

(**Not**: Linux tanılama uzantısı üzerinde açık kaynaklı [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) nerede uzantısında en güncel bilgiler ilk yayımlanır. Denetlemek isteyebilirsiniz [GitHub sayfasına](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) ilk.)

Linux tanılama uzantısı, Microsoft Azure üzerinde çalışan Linux sanal makineleri bir kullanıcı izleme yardımcı olur. Bunu aşağıdaki özellikleri içerir:

* Toplar ve kullanıcının depolama tablosu, tanılama ve syslog bilgiler dahil olmak üzere Linux VM'den sistem performans bilgilerine yükler.
* Toplanan ve karşıya veri ölçümlerini özelleştirme olanağı sağlar.
* Belirtilen depolama tablosu için belirtilen günlük dosyalarını karşıya yüklemek kullanıcıların sağlar.

Geçerli sürümde 2.3 veriler içerir:

* Sistem, güvenlik ve uygulama günlükleri de dahil olmak üzere tüm Linux Rsyslog günlükleri.
* Belirtilen tüm sistem verilerini [System Center Çapraz Platform çözümleri site](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Kullanıcı tarafından belirtilen günlük dosyaları.

Bu uzantı, hem Klasik hem de Resource Manager dağıtım modeli ile çalışır.

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a>Geçerli sürümünü uzantısı ve eski sürümlerinin kullanımdan kaldırma

Uzantının en son sürüm **2.3**, ve **(2.0, 2.1 ve 2.2) eski sürümler kullanım dışı ve bu yılın sonuna kadar (2017) tarafından yayımlanmamış**. Devre dışı otomatik ikincil sürüm yükseltme ile Linux tanılama uzantısının yüklü değilse, bu uzantının yüklemesini kaldırmak ve otomatik ikincil sürüm yükseltme işlemine etkin yeniden önemle tavsiye edilir. Klasik (ASM) Vm'lerinde Azure XPLAT CLI veya Powershell aracılığıyla uzantı yüklüyorsanız '2.*' sürüm olarak belirterek bunu gerçekleştirebilirsiniz. ARM Vm'leri üzerinde ekleyerek bunu gerçekleştirebilirsiniz ' "autoUpgradeMinorVersion": true' VM dağıtımı şablonunda. Ayrıca, uzantının yeni bir yükleme, otomatik ikincil sürüm yükseltme seçeneği açık olmalıdır.

## <a name="enable-the-extension"></a>Uzantıyı etkinleştir

Bu uzantıyı kullanarak etkinleştirebilirsiniz [Azure portalında](https://portal.azure.com/#), Azure PowerShell veya Azure CLI betikleri.

Görüntülemek ve sistem ve performans verileri doğrudan Azure portalından yapılandırmak için izleyin [Azure blogundaki adımları](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

Bu makalede, etkinleştirmek ve Azure CLI komutlarını kullanarak uzantısını yapılandırmak nasıl odaklanır. Bu, okuma ve doğrudan depolama tablosundan verileri görüntülemek sağlar.

Burada açıklanan yapılandırma yöntemleri Azure portalı için işe yaramaz unutmayın. Görüntülemek ve sistem ve performans verileri doğrudan Azure portalından yapılandırmak için portal üzerinden uzantısı etkinleştirilmelidir.

## <a name="prerequisites"></a>Önkoşullar

* **Azure Linux Aracısı 2.0.6 sürümüne veya daha sonra**.

  Çoğu Azure VM Linux galeri görüntüleri 2.0.6 sürümüne içerdiğine dikkat edin veya üzeri. Çalıştırabileceğiniz **WAAgent-sürüm** VM'de hangi sürümünün yüklü olduğunu doğrulamak için. VM 2.0.6 önceki bir sürümünü çalıştırıyorsa, izleyebilirsiniz [github'da bu yönergeleri](https://github.com/Azure/WALinuxAgent "yönergeleri") güncelleştirmek için.
* **Azure CLI**. İzleyin [CLI yüklemek için bu Kılavuzu](../../../cli-install-nodejs.md) makinenizde Azure CLI ortamı ayarlamak için. Azure CLI'yı yükledikten sonra kullanabileceğiniz **azure** komut satırı arabiriminizde Azure CLI komutlarına erişip (Bash, Terminal veya komut istemi) komutu. Örneğin:

  * Çalıştırma **azure vm uzantısı set--help** ayrıntılı yardım bilgisi için.
  * Çalıştırma **azure oturum açma** için Azure'da oturum açın.
  * Çalıştırma **azure vm listesini** Azure'da sahip tüm sanal makineler listesi.
* Verileri depolamak için bir depolama hesabı. Önceden oluşturulmuş bir depolama hesabı adı ve depolama verileri yüklemek için bir erişim anahtarı gerekir.

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a>Linux Diagnostic Extension'ı etkinleştirmek için Azure CLI komutunu kullanın

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a>Senaryo 1. Varsayılan veri kümesiyle uzantısını etkinleştirme

Sürüm 2.3 veya üstü, toplanacak varsayılan veri içerir:

* Tüm Rsyslog bilgi (Sistem, güvenlik ve uygulama günlüklerini dahil).  
* Bir dizi temel sistem verileri. Tam veri kümesi üzerinde açıklanmıştır Not [System Center Çapraz Platform çözümleri site](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Ek veriler etkinleştirmek istiyorsanız, Senaryo 2 ve 3'te adımlarla devam edin.

1. Adım Aşağıdaki içerikle PrivateConfig.json adlı bir dosya oluşturun:

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

2. Adım Çalıştırma **azure vm uzantısı vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2 olarak ayarlayın.\* --private-config-path PrivateConfig.json**.

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a>Senaryo 2. Performans İzleyicisi ölçümlerini özelleştirme

Bu bölümde, performans ve tanılama veri tablosu özelleştirmeyi açıklar.

1. Adım Senaryo 1'de tanımlanan içeriği ile PrivateConfig.json adlı bir dosya oluşturun. Ayrıca PublicConfig.json adlı bir dosya oluşturun. Toplamak istediğiniz belirli verileri belirtin.

Tüm desteklenen sağlayıcılar ve değişkenleri için başvuru [System Center Çapraz Platform çözümleri site](https://scx.codeplex.com/wikipage?title=xplatproviders). Birden çok sorgu sahip ve daha fazla sorgu komut dosyasına ekleyerek birden çok tablodan depolayabilirsiniz.

Varsayılan olarak, her zaman Rsyslog verileri toplanır.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


2. Adım Çalıştırma **azure vm uzantısı ayarlamak vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*'--private-config-path PrivateConfig.json--public-config-path PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>Senaryo 3. Kendi günlük dosyalarını karşıya yükleme

Bu bölümde, toplamak ve belirli günlük dosyalarını depolama hesabınıza yükleme işlemini açıklar. Yolunu günlük dosyanız ve günlüğünüzün depolamak istediğiniz tablonun adını belirtmeniz gerekir. Birden çok dosya/tablo girişleri komut dosyasına ekleyerek, birden çok günlük dosyası oluşturabilirsiniz.

1. Adım Senaryo 1'de tanımlanan içeriği ile PrivateConfig.json adlı bir dosya oluşturun. Ardından aşağıdaki içerikle PublicConfig.json adlı başka bir dosya oluşturun:

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

2. Adım `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json` öğesini çalıştırın.

2.3 önce uzantı sürümlerinde bu ayar, tüm günlükleri yazılan Not `/var/log/mysql.err` için çoğaltılmaya `/var/log/syslog` (veya `/var/log/messages` Linux distro bağlı olarak) de. Bu yinelenen bir günlük önlemek istiyorsanız, günlüğe kaydedilmesini hariç tutabilirsiniz `local6` tesis rsyslog yapılandırmanızda günlüğe kaydeder. Üzerinde Linux distro bağlıdır, ancak bir Ubuntu 14.04 sistemde değiştirmek için dosya `/etc/rsyslog.d/50-default.conf` ve satır değiştirebilirsiniz `*.*;auth,authpriv.none -/var/log/syslog` için `*.*;auth,authpriv,local6.none -/var/log/syslog`. 2.3 (2.3.9007) en son düzeltme sürümde bu sorun düzeltilene 2.3 Uzantı sürümü varsa, bu sorunu değil olacağını şekilde. Bile, sanal Makinenizin yeniden başlattıktan sonra hala varsa lütfen bizimle iletişim kurun ve en son düzeltme sürümü otomatik olarak yüklenmez neden sorun giderme yardımcı olun.

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a>Senaryo 4. Uzantı'nın tüm günlükleri toplamayı Durdur

Bu bölümde, günlükleri toplama gelen uzantısı durdurmak açıklar. İzleme Aracısı işlemi hala çalışır duruma bile bu yeniden yapılandırmaya olacağını unutmayın. İzleme Aracısı işlemi tamamen durdurmak istiyorsanız, uzantıyı devre dışı bırakarak bunu yapabilirsiniz. Uzantı devre dışı bırakmak için bu komut `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

1. Adım Senaryo 1'de tanımlanan içeriği ile PrivateConfig.json adlı bir dosya oluşturun. Aşağıdaki içerikle PublicConfig.json adlı başka bir dosya oluşturun:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


2. Adım Çalıştırma **azure vm uzantısı ayarlamak vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*'--private-config-path PrivateConfig.json--public-config-path PublicConfig.json**.

## <a name="review-your-data"></a>Verilerinizi gözden geçirin

Performans ve Tanılama verileri bir Azure depolama tablosunda depolanır. Gözden geçirme [Azure tablo Depolama'yı Ruby kullanma](../../../cosmos-db/table-storage-how-to-use-ruby.md) Azure CLI betiklerini kullanarak depolama tablosundaki verilere erişmek öğrenin.

Ayrıca, verilere erişmek için UI araçları kullanabilirsiniz:

1. Visual Studio Sunucu Gezgini. Depolama hesabınıza gidin. VM yaklaşık beş dakika boyunca çalıştıktan sonra dört varsayılan tablolar görürsünüz: "LinuxCpu", "LinuxDisk", "LinuxMemory" ve "Linuxsyslog". Tablo adları, verileri görüntülemek için çift tıklayın.
1. [Azure Depolama Gezgini](https://azurestorageexplorer.codeplex.com/ "Azure Depolama Gezgini").

![image](./media/diagnostic-extension/no1.png)

FileCfg veya (senaryoları 2 ve 3'te açıklandığı gibi) perfCfg etkinleştirdiyseniz, varsayılan olmayan verileri görüntülemek için Visual Studio sunucu Gezgini'nde ve Azure Depolama Gezgini'ni kullanabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

* Rsyslog bilgileri ve müşteri tarafından belirtilen günlük dosyası yalnızca komut dosyası aracılığıyla erişilebilir.
