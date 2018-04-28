---
title: Bir Linux VM VM uzantısı ile izleme | Microsoft Docs
description: Linux tanılama uzantı performans ve Tanılama verileri azure'da bir Linux VM izlemek için nasıl kullanılacağını öğrenin.
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
ms.openlocfilehash: f1415e2cfbe48b287db5851bb8ebef1ff9251280
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a>Linux Diagnostic Extension’ı kullanarak bir Linux VM’nin performansını ve tanılama verilerini izleme

Bu belge Linux tanılama uzantısını 2.3 sürümünü açıklar.

> [!IMPORTANT]
> Bu sürüm kullanım dışıdır ve 30 Haziran 2018 sonra her zaman yayımdan olabilir. 3.0 sürümünde değiştirilmiştir. Daha fazla bilgi için bkz: [Linux tanılama uzantısı sürüm 3.0 belgelerine](../diagnostic-extension.md).

## <a name="introduction"></a>Giriş

(**Not**: Linux tanılama uzantısını açık kaynaklıdır [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) burada uzantısına en güncel bilgiler ilk yayımlanır. Denetlemek isteyebilirsiniz [GitHub sayfası](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) ilk.)

Linux tanılama uzantısını kullanıcı İzleyicisi Microsoft Azure üzerinde çalışan Linux sanal makineleri yardımcı olur. Aşağıdaki özellikleri içerir:

* Toplar ve kullanıcının depolama tablosu tanılama ve syslog bilgiler dahil olmak üzere, Linux VM'den sistem performans bilgilerine yükler.
* Kullanıcıların, toplanan ve karşıya veri ölçümlerini özelleştirme olanak tanır.
* Belirtilen günlük dosyalarını karşıya yüklemek için atanmış depolama tablosu olanak tanır.

Geçerli sürümde 2.3 verileri şunları içerir:

* Sistem, güvenlik ve uygulama günlükleri de dahil olmak üzere tüm Linux Rsyslog günlükleri.
* Belirtilen tüm sistem verileri [System Center Çapraz Platform çözümlerini site](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Kullanıcı tarafından belirtilen günlük dosyaları.

Bu uzantı, Klasik ve Resource Manager dağıtım modelleri ile çalışır.

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a>Geçerli sürümünü uzantısı ve eski sürümleri kullanımdan kaldırma

Uzantısı'nın en son sürüm **2.3**, ve **eski sürümlerini (2.0, 2.1 ve 2.2) kullanım dışı ve, bu yılın sonuna (2017) tarafından yayımlanmamış**. Linux tanılama uzantısını devre dışı otomatik alt sürüm yükseltme işlemine yüklediyseniz, uzantı kaldırıp etkin otomatik alt sürüm yükseltme işlemine yeniden yüklemeniz önerilir. Klasik (ASM) Vm'lerinde, bu Azure XPLAT CLI veya Powershell ile uzantısı yüklüyorsanız '2.*' sürüm olarak belirterek elde edebilirsiniz. ARM Vm'leri üzerinde bu dahil ederek elde edebilirsiniz ' "olan": true' VM Dağıtım şablonu olarak. Ayrıca, herhangi bir yeni yüklemesi uzantısının seçeneği açık yükseltme otomatik alt sürüm olmalıdır.

## <a name="enable-the-extension"></a>Uzantı etkinleştirilemedi

Bu uzantıyı kullanarak etkinleştirebilirsiniz [Azure portal](https://portal.azure.com/#), Azure PowerShell veya Azure CLI komut dosyaları.

Görüntülemek ve sistem ve performans verileri doğrudan Azure Portalı'nı yapılandırmak için izleyin [adımları Azure blogunda](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

Bu makalede etkinleştirmek ve Azure CLI komutları kullanarak uzantısını yapılandırmak nasıl odaklanır. Bu, okuma ve verileri doğrudan depolama tablosu görüntülemenize olanak sağlar.

Burada açıklanan yapılandırma yöntemleri için Azure portalı çalışmaz unutmayın. Görüntülemek ve sistem ve performans verileri doğrudan Azure Portalı'nı yapılandırmak için dahili olarak portalı üzerinden etkinleştirilmesi gerekir.

## <a name="prerequisites"></a>Önkoşullar

* **Azure Linux Aracısı sürüm 2.0.6 veya daha sonra**.

  Çoğu Azure VM Linux galeri görüntüleri sürümü 2.0.6 dahil Not veya sonraki bir sürümü. Çalıştırabilirsiniz **WAAgent-sürüm** VM hangi sürümünün yüklü olduğunu doğrulamak için. VM 2.0.6 önceki bir sürümünü çalıştırıyorsa, takip edebilirsiniz [github'daki bu yönergeleri](https://github.com/Azure/WALinuxAgent "yönergeleri") güncelleştirin.
* **Azure CLI**. İzleyin [CLI yüklemek için bu Kılavuzu](../../../cli-install-nodejs.md) makinenizi Azure CLI ortamda ayarlamak için. Azure CLI yüklendikten sonra kullanabileceğiniz **azure** arabiriminden, komut satırı Azure CLI komutlara erişmek için (Bash, Terminal veya komut istemi) komutu. Örneğin:

  * Çalıştırma **azure vm uzantısı kümesi--Yardım** ayrıntılı yardım bilgi.
  * Çalıştırma **azure oturum açma** Azure'a oturum açmak için.
  * Çalıştırma **azure vm listesi** Azure üzerinde sahip tüm sanal makineler listesi.
* Verileri depolamak için bir depolama hesabı. Daha önce oluşturulmuş bir depolama hesabı adı ve verileri depolama alanına yüklemek için bir erişim anahtarı gerekir.

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a>Linux tanılama uzantısını etkinleştirmek için Azure CLI komutunu kullanın

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a>Senaryo 1. Varsayılan veri kümesi uzantısını etkinleştirme

2.3 veya sonraki sürümü toplanacağını varsayılan veri içerir:

* Tüm Rsyslog bilgileri (Sistem, güvenlik ve uygulama günlükleri dahil).  
* Temel sistem verileri çekirdek kümesi. Tam veri kümesi üzerinde açıklanan Not [System Center Çapraz Platform çözümlerini site](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Ek veriler etkinleştirmek istiyorsanız, Senaryo 2 ve 3'te adımlarla devam edin.

1. Adım Aşağıdaki içerik ile PrivateConfig.json adlı bir dosya oluşturun:

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

2. Adım Çalıştırma **azure vm uzantısı vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2 ayarlayın.* --Özel yapılandırma yolu PrivateConfig.json**.

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a>Senaryo 2. Performans İzleyicisi ölçümlerini özelleştirme

Bu bölümde, performans ve tanı veri tablosu nasıl özelleştirileceği açıklanmaktadır.

1. Adım Senaryo 1'de açıklanan içeriğe sahip PrivateConfig.json adlı bir dosya oluşturun. Ayrıca PublicConfig.json adlı bir dosya oluşturun. Toplamak istediğiniz belirli verileri belirtin.

Desteklenen tüm sağlayıcıları ve değişkenleri için başvuru [System Center Çapraz Platform çözümlerini site](https://scx.codeplex.com/wikipage?title=xplatproviders). Birden çok sorgu sahip ve birden fazla tabloya daha fazla sorguları komut dosyasına ekleyerek depolayabilirsiniz.

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


2. Adım Çalıştırma **azure vm uzantısı ayarlamak vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*'--özel yapılandırma yolu PrivateConfig.json--ortak yapılandırma yolu PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>Senaryo 3. Kendi günlük dosyalarını karşıya yükleme

Bu bölümde, toplamak ve belirli günlük dosyalarını depolama hesabınıza yüklemeniz açıklar. Yolu, günlük dosyası ve günlüğünüzü depolamak istediğiniz tablonun adını belirtmeniz gerekir. Birden çok dosya/tablosu girdileri komut dosyasına ekleyerek, birden çok günlük dosyası oluşturabilirsiniz.

1. Adım Senaryo 1'de açıklanan içeriğe sahip PrivateConfig.json adlı bir dosya oluşturun. Ardından aşağıdaki içerik ile PublicConfig.json adlı başka bir dosya oluşturun:

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

2.3 önce uzantısı sürümlerinde bu ayar, tüm günlükleri yazılan Not `/var/log/mysql.err` için yinelenen `/var/log/syslog` (veya `/var/log/messages` Linux distro bağlı olarak) de. Bu yinelenen günlük önlemek istiyorsanız, günlük kaydını dışlayabilirsiniz `local6` tesis rsyslog yapılandırmanızda günlüğe kaydeder. Linux distro bağlıdır, ancak bir Ubuntu 14.04 sistemde değiştirmek için dosyasıdır `/etc/rsyslog.d/50-default.conf` ve satır değiştirebilirsiniz `*.*;auth,authpriv.none -/var/log/syslog` için `*.*;auth,authpriv,local6.none -/var/log/syslog`. 2.3 (2.3.9007), en son düzeltme sürümde bu sorun düzeltilene 2.3 uzantısı sürümüne sahipseniz, bu sorun değil olacağını şekilde. Hala bile VM'yi yeniden başlatıldıktan sonra varsa, lütfen bizimle iletişime geçin ve en son düzeltme sürümü otomatik olarak yüklenmez neden gidermenize yardımcı.

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a>Senaryo 4. Herhangi bir günlük toplama gelen uzantısı Durdur

Bu bölümde, günlükleri toplamayı gelen uzantısı durdurmayı açıklar. İzleme Aracısı işlemi hala hazır ve çalışır bile bu yeniden yapılandırmaya olacağını unutmayın. İzleme Aracısı işlemi tamamen durdurmak istiyorsanız, uzantıyı devre dışı bırakarak bunu yapabilirsiniz. Uzantı devre dışı bırakmak için komut `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

1. Adım Senaryo 1'de açıklanan içeriğe sahip PrivateConfig.json adlı bir dosya oluşturun. Aşağıdaki içerik ile PublicConfig.json adlı başka bir dosya oluşturun:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


2. Adım Çalıştırma **azure vm uzantısı ayarlamak vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*'--özel yapılandırma yolu PrivateConfig.json--ortak yapılandırma yolu PublicConfig.json**.

## <a name="review-your-data"></a>Verilerinizi gözden geçirin

Performans ve tanılama verilerini bir Azure Storage tablosunda depolanır. Gözden geçirme [Ruby Azure tablo depolamasından kullanmayı](../../../cosmos-db/table-storage-how-to-use-ruby.md) Azure CLI komut dosyalarını kullanarak veri depolama tablosundaki erişim hakkında bilgi edinmek için.

Ayrıca, verilere erişmek için kullanıcı Arabirimi araçlarını kullanabilirsiniz:

1. Visual Studio Sunucu Gezgini. Depolama hesabınıza gidin. VM yaklaşık beş dakika boyunca çalıştıktan sonra dört varsayılan tabloları görürsünüz: "LinuxCpu", "LinuxDisk", "LinuxMemory" ve "Linuxsyslog". Tablo adları verileri görüntülemek için çift tıklayın.
1. [Azure Storage Gezgini](https://azurestorageexplorer.codeplex.com/ "Azure Storage Gezgini").

![görüntü](./media/diagnostic-extension/no1.png)

FileCfg veya (senaryoları 2 ve 3'te açıklandığı gibi) perfCfg etkinleştirdiyseniz, varsayılan olmayan verileri görüntülemek için Visual Studio Sunucu Gezgini ve Azure Storage Gezgini kullanabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

* Müşteri tarafından belirtilen günlük dosyası ve Rsyslog bilgi yalnızca komut dosyası aracılığıyla erişilebilir.
