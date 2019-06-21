---
title: Azure İzleyici etkinleştirmek için Vm'leri (Önizleme) genel bakış | Microsoft Docs
description: Dağıtma ve VM'ler için Azure İzleyici yapılandırma hakkında bilgi edinin. Sistem gereksinimlerini bulabilirsiniz.
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
ms.date: 06/19/2019
ms.author: magoedte
ms.openlocfilehash: 2d4e49b4f7c1aa244b59ef17716c90369a0d3339
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273385"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>VM'ler (Önizleme) genel bakış için Azure İzleyicisi'ni etkinleştirme

Bu makalede, VM'ler için Azure İzleyici ' ayarlamak için kullanılabilir seçenekleri'ne genel bakış sağlar. Azure İzleyici durumunu izleme ve performansı için Vm'leri için kullanın. Azure sanal makinelerinde (VM'ler) çalıştıran Uygulama bağımlılıklarını keşfedin ve sanal makine ölçek ayarlar, Vm'leri ya da başka bir bulut ortamında barındırılan sanal makineleri şirket içinde.  

VM'ler için Azure İzleyici ' ayarlamak için:

* Seçim yaparak tek bir Azure sanal makine veya sanal makine ölçek etkinleştirme **Insights (Önizleme)** doğrudan sanal makine veya sanal makine ölçek kümesinden.
* İki etkinleştirmek veya Azure İlkesi'ni kullanarak daha fazla Azure sanal makineleri ve sanal makine ölçek kümeleri. Bu yöntem, mevcut ve yeni VM'ler ve ölçek kümeleri, gerekli bağımlılıkları yüklenmiş ve düzgün yapılandırılmış olduğunu sağlar. Bunları etkinleştirmek ve bunları düzeltmek için karar verebilir böylece uyumsuz VM'ler ve ölçek kümeleri raporlanır.
* İki etkinleştirmek veya PowerShell kullanarak belirtilen abonelik veya kaynak grubu üzerinde daha fazla Azure sanal makineleri veya sanal makine ölçek kümeleri.
* Sanal makinelerin sanal makineleri veya fiziksel şirket ağınızda veya başka bir bulut ortamında barındırılan izlemek Azure İzleyici etkinleştirin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki bölümlerde yer alan bilgiler anladığınızdan emin olun.

### <a name="log-analytics"></a>Log Analytics

VM'ler için Azure İzleyici, bir Log Analytics çalışma alanı şu bölgelerde destekler:

- Batı Orta ABD
- Batı ABD 2<sup>1</sup>
- Doğu ABD
- Orta Kanada
- Birleşik Krallık Güney
- Batı Avrupa
- Güneydoğu Asya

<sup>1</sup> VM'ler için bu bölgede şu anda Azure İzleyici'nın sistem durumu özelliğini desteklemiyor.

>[!NOTE]
>Azure Vm'leri bir bölgeden dağıtabilirsiniz. Bu VM'ler, Log Analytics çalışma alanı tarafından desteklenen bölgeler sınırlı değildir.
>

Bir çalışma alanınız yoksa, bu kaynakları birini kullanarak bir tane oluşturabilirsiniz:
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure portalı](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Azure portalında tek Azure sanal makine veya sanal makine ölçek kümesi için izleme mümkün kılıyoruz olsa da bir çalışma alanı oluşturabilirsiniz.

Azure İlkesi, Azure PowerShell veya Azure Resource Manager şablonları, Log Analytics çalışma alanınızda kullanan bir ölçekli senaryo ayarlamak için:

* ServiceMap ve InfrastructureInsights çözümlerini yükleyin. Bu yükleme, sağlanan bir Azure Resource Manager şablonu kullanarak tamamlayabilirsiniz. Veya **Başlarken** sekmesinde **yapılandırma çalışma alanı**.
* Performans sayaçları toplamak için Log Analytics çalışma alanı yapılandırın.

Çalışma alanınız ölçekli senaryo için yapılandırmak için aşağıdaki yöntemlerden birini kullanın:

* Kullanım [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace).
* Sanal makineler için Azure İzleyicisi [ **ilke kapsamı** ](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) sayfasında **yapılandırma çalışma alanı**. 

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda sanal makineler için Azure İzleyici destekleyen Windows ve Linux işletim sistemleri listelenmiştir. Daha sonra bu bölümde ayrıntılı olarak birincil ve ikincil Linux işletim sistemi sürüm ve çekirdek sürümleriyle desteklenen tam listesini bulabilirsiniz.

|İşletim sistemi sürümü |Performans |Haritalar |Durum |
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

<sup>1</sup> VM'ler için Azure İzleyici performans özelliği yalnızca Azure İzleyici'deki kullanılabilir. Doğrudan Azure VM'nin sol bölmeden kullanılamaz.

>[!NOTE]
>VM'ler için Azure İzleyici'nın sistem durumu özelliğini desteklemediği [iç içe sanallaştırma](../../virtual-machines/windows/nested-virtualization.md) Azure VM'deki.
>

>[!NOTE]
>Linux işletim sisteminde:
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
> - Fiziksel Adres Uzantısı (PAE) ve Xen, desteklenmeyen bir Linux dağıtımı için gibi standart olmayan çekirdek serbest bırakır. Örneğin, bir sürüm dizesi sistemiyle *2.6.16.21-0.8-xen* desteklenmiyor.
> - Standart çekirdekler, yeniden dahil olmak üzere özel çekirdekleri desteklenmez.
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

#### <a name="centosplus"></a>CentOSPlus
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

VM'ler için Azure İzleyici'de eşleme özelliğini verilerini Microsoft Dependency Aracıdan alır. Log Analytics aracısını Log Analytics bağlantısını için bağımlılık Aracısı'nı kullanır. Bu nedenle, sisteminizi yüklenmiş ve yapılandırılmış bağımlılık aracısını Log Analytics aracısını olması gerekir.

Azure İzleyici, VM'ler için tek bir Azure VM için etkinleştirmek ister ölçekli dağıtım yöntemini kullanmak, deneyiminin bir parçası aracıyı yüklemek için Azure VM bağımlılık Aracısı uzantısı'nı kullanın.

Karma bir ortamda, indirin ve bağımlılık aracısını el ile yükleyin. Vm'lerinizi Azure dışında barındırılıyorsa bir otomatik dağıtım yöntemini kullanın.

Aşağıdaki tabloda, karma bir ortamda, eşleme özelliğini destekleyen bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | İle birlikte [Windows için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), bağımlılık aracısını Windows Aracısı gerekir. Daha fazla bilgi için [desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | İle birlikte [Linux için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), bağımlılık Aracısı'nı Linux aracıları gerekir. Daha fazla bilgi için [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |

Bağımlılık Aracısı'nı bu konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Etkinleştirme ve VM'ler için Azure İzleyici'nın özelliklerine erişmek için olmalıdır *Log Analytics katkıda bulunan* rol. Performans, sistem durumu, görüntüleme ve harita verileri için olmalıdır *izleme okuyucusu* Azure VM rolü. Log Analytics çalışma alanı için Azure İzleyici VM'ler için yapılandırılmış olması gerekir.

Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>Azure İzleyici (Önizleme) VM'ler için etkinleştirme

Azure İzleyici, bu tabloda açıklanan yöntemlerden birini kullanarak VM'ler için etkinleştirin:

| Dağıtım durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Tek Azure sanal makine veya sanal makine ölçek kümesi | [Sanal makineden etkinleştir](vminsights-enable-single-vm.md) | Tek bir Azure VM seçerek etkinleştirebilirsiniz **Insights (Önizleme)** doğrudan sanal makine veya sanal makine ölçek kümesinden ayarlayın. |
| Birden fazla Azure sanal makineleri veya sanal makine ölçek kümeleri | [Azure İlkesi aracılığıyla etkinleştirme](vminsights-enable-at-scale-policy.md) | Azure İlkesi ve kullanılabilir ilke tanımlarını kullanarak birden fazla Azure VM'yi etkinleştirebilirsiniz. |
| Birden fazla Azure sanal makineleri veya sanal makine ölçek kümeleri | [Azure PowerShell veya Azure Resource Manager şablonları etkinleştirme](vminsights-enable-at-scale-powershell.md) | Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak, belirtilen abonelik veya kaynak grubu arasında birden fazla Azure sanal makineleri veya sanal makine ölçek kümeleri etkinleştirebilirsiniz. |
| Karma bulut | [Hibrit ortamı için etkinleştir](vminsights-enable-hybrid-cloud.md) | Veri merkezinizde veya diğer bulut ortamları sanal makineleri veya barındırılan fiziksel bilgisayarlara dağıtabilirsiniz. |

## <a name="performance-counters-enabled"></a>Performans sayaçları etkinleştirildi 

VM'ler için Azure İzleyici kullandığı performans sayaçları toplamak için bir Log Analytics çalışma alanı yapılandırır. Aşağıdaki tablolar, 60 saniyede toplanan sayaçlarını ve nesneleri listeler.

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

Microsoft, Azure İzleyici hizmeti kullanımınız vasıtasıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft, kalite, güvenlik ve bütünlüğünü hizmeti geliştirmek için bu verileri kullanır. 

Doğru ve etkili sorun giderme özellikleri sağlamak üzere yazılımınızın yapılandırması hakkında daha fazla veri eşleme özelliğini içerir. Veriler işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi bilgileri sağlar. Microsoft, ad, adres veya diğer iletişim bilgilerinizi toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

Sanal Makineniz için izleme etkinleştirdikten sonra izleme bilgileri analiz VM'ler için Azure İzleyici'de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).
