---
title: Azure İzleyici etkinleştirmek için Vm'leri (Önizleme) genel bakış | Microsoft Docs
description: Bu makalede nasıl dağıtma ve Vm'leri ve gerekli sistem gereksinimleri için Azure İzleyici yapılandırma açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2019
ms.author: magoedte
ms.openlocfilehash: 76d18b6a942ed9b8c6871b0ff7cbc1c83917ada4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66130476"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>VM'ler (Önizleme) genel bakış için Azure İzleyicisi'ni etkinleştirme

Bu makalede, sanal makinelerin durumunu, performansını izleyin ve Azure sanal makineler ve sanal makine ölçek kümeleri çalıştıran Uygulama bağımlılıklarını bulmak Azure İzleyici kurulumu, şirket içi Vm'leri için kullanılabilir seçenekleri'ne genel bakış sağlar ve barındırılan sanal makineleri başka bir bulut ortamı.  

VM'ler için Azure İzleyici aşağıdaki yöntemlerden birini kullanarak etkinleştirin:

* Tek bir Azure sanal makine veya sanal makine ölçek kümesi seçerek etkinleştirme **Insights (Önizleme)** doğrudan sanal makine veya sanal makine ölçek kümesinden.
* İki etkinleştirmek veya Azure İlkesi'ni kullanarak daha fazla Azure sanal makineleri ve sanal makine ölçek kümeleri. Bu yöntem kullanılarak, mevcut ve yeni VM'lerin ve ölçek kümeleri gerekli bağımlılıkları yüklü ve düzgün yapılandırılmış. Bunları etkinleştirmek ve düzeltmek istiyorsanız nasıl karar verebilmek için uyumlu VM'ler ve ölçek kümeleri raporlanır.
* İki etkinleştirmek veya PowerShell kullanarak belirtilen abonelik veya kaynak grubu üzerinde daha fazla Azure sanal makineleri veya sanal makine ölçek kümeleri.
* Sanal makineleri veya fiziksel şirket ağınızda veya başka bir bulut ortamında barındırılan izlemek için bu seçeneği etkinleştirin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki bölümlerde yer alan bilgiler anladığınızdan emin olun.

### <a name="log-analytics"></a>Log Analytics

VM'ler için Azure İzleyici, bir Log Analytics çalışma alanı şu bölgelerde destekler:

- Orta Batı ABD
- Doğu ABD
- Kanada orta<sup>1</sup>
- UK Güney<sup>1</sup>
- Batı Avrupa
- Güneydoğu Asya<sup>1</sup>

<sup>1</sup> VM'ler için bu bölgede şu anda Azure İzleyici'nın sistem durumu özelliğini desteklemiyor.

>[!NOTE]
>Azure sanal makineleri, herhangi bir bölgeden dağıtılabilir ve Log Analytics çalışma alanı için desteklenen bölgelerin sınırlı değildir.
>

Bir çalışma alanınız yoksa, aşağıdaki yöntemlerden biriyle oluşturabilirsiniz:
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure portalı](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Tek bir Azure VM veya Azure portalında sanal makine ölçek kümesi için izleme mümkün kılıyoruz, bu işlem sırasında bir çalışma alanı oluşturabilirsiniz.

Azure İlkesi, Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak ölçekli senaryosu için Log Analytics çalışma alanınızı ilk yapılandırılmış aşağıdakiler gerekir:

* ServiceMap ve InfrastructureInsights çözümlerini yükleyin. Sağlanan bir Azure Resource Manager şablonu kullanarak veya kullanarak bu yüklemeyi tamamlayacak **yapılandırma çalışma alanı** seçeneği sonucuna **Get Started** sekmesi.
* Performans sayaçları toplamak için Log Analytics çalışma alanı yapılandırın.

Çalışma alanınız ölçekli senaryo için yapılandırmak için aşağıdaki yöntemlerden birini kullanarak yapılandırın:

* [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace)
* Gelen **yapılandırma çalışma alanı** VM'ler için Azure İzleyici seçeneği [ilke kapsamı](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) sayfası

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda, VM'ler için Azure İzleyici ile desteklenen Windows ve Linux işletim sistemleri listelenmiştir. Büyük ve küçük Linux işletim sistemi sürüm ayrıntıları ve çekirdek sürümleriyle desteklenen tam listesi daha sonra bu bölümde sağlanır.

|İşletim sistemi sürümü |Performans |Haritalar |Sağlık |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | X |
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | X |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| X |
|Ubuntu 14.04, 16.04, 18.04 | X | X | X |
|CentOS Linux 6, 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 11, 12 | X | X | X |
|Debian 8, 9.4 sürümünden | X<sup>1</sup> | | X |

<sup>1</sup> VM'ler için Azure İzleyici performans özelliği yalnızca Azure İzleyici'deki kullanılabilir. Doğrudan Azure VM'nin sol bölmeden eriştiğinizde kullanılamaz.

>[!NOTE]
>Aşağıdaki bilgiler, Linux işletim sistemini desteklemek için geçerlidir:
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
> - Fiziksel Adres Uzantısı (PAE) ve Xen, desteklenmeyen bir Linux dağıtımı için gibi standart olmayan çekirdek serbest bırakır. Örneğin, bir sürüm dizesi sistemiyle *2.6.16.21-0.8-xen* desteklenmiyor.
> - Standart çekirdekleri yeniden derlemelerinin dahil olmak üzere özel çekirdekleri desteklenmez.
> - CentOSPlus çekirdek desteklenir.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus
| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| Ubuntu 18.04 | Çekirdek 4.15. * |
| Ubuntu 16.04.3 | Çekirdek 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
|11 SP4 | 3.0.* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 kuruluş sunucusu

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency aracı

Vm'leri Haritası özelliği için Azure İzleyici verilerini Microsoft Dependency Aracıdan alır. Log Analytics aracısını Log Analytics bağlantısını için bağımlılık Aracısı'nı kullanır. Bu nedenle, sisteminizde yüklü ve yapılandırılmış bağımlılık aracısını Log Analytics aracısını olması gerekir.

Azure İzleyici, VM'ler için tek bir Azure VM için etkinleştirmek ister ölçekli dağıtım yöntemini kullanmak, deneyiminin bir parçası aracıyı yüklemek için Azure VM bağımlılık Aracısı uzantısı kullanmanız gerekir.

Karma bir ortamda, indirin ve iki yoldan biriyle bağımlılık Aracısı'nı yükleyin: el ile veya sanal makine Azure dışından barındırılan için bir otomatik dağıtım yöntemi kullanarak.

Aşağıdaki tabloda, karma bir ortamda, eşleme özelliğini destekleyen bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | Ek olarak [Windows için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Windows aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemlerinin tam listesi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | Ek olarak [Linux için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Linux aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemlerinin tam listesi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |

Bağımlılık Aracısı'nı aşağıdaki konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Etkinleştirmek ve VM'ler için Azure İzleyici'nın özelliklerine erişmek için aşağıdaki erişim rolleri atanmış olması gerekir:

- Bunu etkinleştirmek için aşağıdakiler gereklidir *Log Analytics katkıda bulunan* rol.

- Performans, sistem durumu, görüntüleme ve harita verileri için olmalıdır *izleme okuyucusu* Azure VM rolü. Log Analytics çalışma alanı için Azure İzleyici VM'ler için yapılandırılmış olması gerekir.

Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>Azure İzleyici (Önizleme) VM'ler için etkinleştirme

VM'ler için Azure İzleyici aşağıdaki tabloda açıklanan aşağıdaki yöntemlerden birini kullanarak etkinleştirin.

| Dağıtım durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Tek Azure sanal makine veya sanal makine ölçek kümesi | [Sanal makineden doğrudan](vminsights-enable-single-vm.md) | Tek bir Azure sanal makine seçerek etkinleştirebilirsiniz **Insights (Önizleme)** doğrudan sanal makine veya sanal makine ölçek kümesinden ayarlayın. |
| Birden fazla Azure sanal makineleri veya sanal makine ölçek kümeleri | [Azure İlkesi](vminsights-enable-at-scale-policy.md) | Azure İlkesi ve kullanılabilir ilke tanımlarını kullanarak birden çok Azure Vm'leri etkinleştirebilirsiniz. |
| Birden fazla Azure sanal makineleri veya sanal makine ölçek kümeleri | [Azure PowerShell veya Azure Resource Manager şablonları](vminsights-enable-at-scale-powershell.md) | Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak bir belirtilen abonelik veya kaynak grubu arasında birden fazla Azure sanal makineleri veya sanal makine ölçek kümeleri etkinleştirebilirsiniz. |
| Karma bulut | [Hibrit ortamı için etkinleştir](vminsights-enable-hybrid-cloud.md) | Veri merkezinizde veya diğer bulut ortamları sanal makineler veya barındırılan fiziksel bilgisayarlara dağıtabilirsiniz. |

## <a name="performance-counters-enabled"></a>Performans sayaçları etkinleştirildi

VM'ler için Azure İzleyici kullandığı performans sayaçları toplamak için bir Log Analytics çalışma alanı yapılandırır. Aşağıdaki tabloda, 60 saniyede toplanan sayaçlarını ve nesneleri listeler.

### <a name="windows-performance-counters"></a>Windows performans sayaçları

|Nesne adı |Sayaç adı |
|------------|-------------|
|MantıksalDisk |% Boş alan |
|MantıksalDisk |Ort. Disk sn/Okuma |
|MantıksalDisk |Ort. Disk sn/Aktarım |
|MantıksalDisk |Ort. Disk sn/yazma |
|MantıksalDisk |Disk Bayt/sn |
|MantıksalDisk |Disk Okuma Bayt/sn |
|MantıksalDisk |Disk Okuma/sn |
|MantıksalDisk |Disk aktarımı/sn |
|MantıksalDisk |Disk Yazma Bayt/sn |
|MantıksalDisk |Disk Yazma/sn |
|MantıksalDisk |Boş megabayt |
|Bellek |Kullanılabilir MBayt |
|Ağ bağdaştırıcısı |Alınan Bayt/sn |
|Ağ bağdaştırıcısı |Gönderilen bayt/sn |
|İşlemci |% İşlemci zamanı |

### <a name="linux-performance-counters"></a>Linux performans sayaçları

|Nesne adı |Sayaç adı |
|------------|-------------|
|Mantıksal Disk |% Kullanılan alan |
|Mantıksal Disk |Disk Okuma Bayt/sn |
|Mantıksal Disk |Disk Okuma/sn |
|Mantıksal Disk |Disk aktarımı/sn |
|Mantıksal Disk |Disk Yazma Bayt/sn |
|Mantıksal Disk |Disk Yazma/sn |
|Mantıksal Disk |Boş megabayt |
|Mantıksal Disk |Mantıksal Disk Bayt/sn |
|Bellek |Kullanılabilir MBayt belleği |
|Ağ |Alınan toplam bayt sayısı |
|Ağ |Aktarılan toplam bayt |
|İşlemci |% İşlemci zamanı |

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Azure İzleyici hizmeti kullanımınız vasıtasıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft, kalite, güvenlik ve bütünlüğünü hizmeti geliştirmek için bu verileri kullanır. Doğru ve etkili sorun giderme özellikleri sağlamak üzere eşleme özelliğini verilerden işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi yazılımınızın yapılandırması hakkında bilgiler içerir. Microsoft, ad, adres veya diğer iletişim bilgilerinizi toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

İzleme sanal makineniz için etkinleştirilir, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir. Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).
