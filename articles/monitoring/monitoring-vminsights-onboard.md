---
title: Sanal makineler için yerleşik Azure İzleyici | Microsoft Docs
description: Bu makalede nasıl eklemenize ve dağıtılmış uygulamanızı nasıl performans gösterdiğini ve sistem durumu sorunları belirlenen anlamak başlatabilmeniz VM'ler için Azure İzleyicisi'ni yapılandırabilirsiniz.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: 2f0568064eed556429675ffb34c84d588ac670d5
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064365"
---
# <a name="how-to-onboard-the-azure-monitor-for-vms"></a>Nasıl eklenirim Azure izleme VM'ler için 
Bu makalede, Azure sanal makinelerinizin işletim sistemi durumunu izleyin ve keşfedin ve bunlar üzerinde barındırılabilir uygulama bağımlılıkları eşleme VM'ler için Azure İzleyici'kurmak açıklar.  

VM'ler için Azure İzleyicisi'ni etkinleştirmek aşağıdaki yöntemlerden birini kullanarak elde edilir ve her yöntemle ilgili ayrıntılar, makalenin sonraki bölümlerinde sağlanır.  

* Tek bir Azure sanal seçerek makine **Insights (Önizleme)** doğrudan VM'den.
* Birden çok varolan sağlamanın Azure İlkesi'ni kullanarak Azure Vm'leri ve hesaplanan yeni VM'ler yüklü gerekli bağımlılıkları sahiptir ve düzgün şekilde yapılandırılır.  Uyumlu olmayan Vm'leri, göre iyileştirmek istediğiniz nasıl uyumlu olmadığını üzerinde karar verebilmek için raporlanır.  
* Belirtilen abonelik veya PowerShell kullanarak bir kaynak grubu arasında birden fazla Azure sanal makineleri veya sanal makine ölçek ayarlar.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdaki alt bölümlerde açıklandığı gibi aşağıdaki sahip olduğunuzdan emin olun.

### <a name="log-analytics"></a>Log Analytics 

Şu anda bir Log Analytics çalışma alanı aşağıdaki bölgelerde desteklenir:

  - Batı Orta ABD  
  - Doğu ABD  
  - Batı Avrupa  
  - Güneydoğu Asya<sup>1</sup>  

<sup>1</sup> bu bölgede Azure İzleyici'nin sistem durumu özelliği şu anda VM'ler için desteklemiyor   

Bir çalışma alanı yoksa, şunları yapabilirsiniz üzerinden oluşturabilirsiniz [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md)temellidir [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../log-analytics/log-analytics-quick-create-workspace.md).  

Çözümü etkinleştirmek için Log Analytics katkıda bulunan rolünün bir üyesi olmanız gerekir. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda, VM'ler için Azure İzleyici ile desteklenen Windows ve Linux işletim sistemleri listelenmiştir.  Büyük ve küçük Linux işletim sistemi sürüm gerçekleşen bir tam listesi ve çekirdek sürümleri desteklenir, bu bölümde ileride verilmiştir.

|İşletim sistemi sürümü |Performans |Haritalar |Durum |  
|-----------|------------|-----|-------|  
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|RHEL 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |  
|Sent OS Linux 7, 6 | X | X | X |  
|SLES 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4 sürümünden, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> Azure VM'nin sol bölmesinden doğrudan erişim kullanılabilir değilse, VM'ler için Azure İzleyici performans özelliği, yalnızca Azure İzleyicisi'nden kullanılabilir.  

>[!NOTE]
>Aşağıdaki bilgiler, Linux işletim sistemini desteklemek için geçerlidir:  
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.  
> - PAE ve Xen gibi standart dışı çekirdek sürümleri hiçbir Linux dağıtımında desteklenmez. Örneğin, "2.6.16.21-0.8-xen" sürüm dizesi sistemiyle desteklenmiyor.  
> - Standart çekirdeklerin yeniden derlemeleri de dahil olmak üzere özel çekirdekler desteklenmez.  
> - CentOSPlus çekirdeği desteklenmez.  


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| Ubuntu 18.04 | Çekirdek 4.15. * |
| Ubuntu 16.04.3 | Çekirdek 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="oracle-enterprise-linux-6-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 6 kesilemeyen Enterprise Çekirdeği
| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-enterprise-linux-5-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 5 kesilemeyen Enterprise Çekirdeği

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 kuruluş sunucusu

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |

### <a name="hybrid-environment-connected-sources"></a>Hibrit ortamı bağlı kaynakları
Azure İzleyici Vm'leri harita verilerini Microsoft Dependency Aracıdan alır. Log Analytics aracısını Log Analytics bağlantısını için bağımlılık Aracısı'nı kullanır. Başka bir deyişle, bir sistemde yüklenmiş ve yapılandırılmış bağımlılık aracısını Log Analytics aracısını olması gerekir.  Aşağıdaki tabloda, karma bir ortamda, eşleme özelliğini destekleyen bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | Ek olarak [Windows için Log Analytics aracısını](../log-analytics/log-analytics-concept-hybrid.md), Windows aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemi sürümlerinin tam listesi için bkz. [Desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | Ek olarak [Linux için Log Analytics aracısını](../log-analytics/log-analytics-concept-hybrid.md), Linux aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemi sürümlerinin tam listesi için bkz. [Desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |  

Windows üzerinde Microsoft Monitoring Agent (MMA) hem System Center Operations Manager hem de Log Analytics tarafından toplayın ve göndermek için kullanılan izleme verileri. System Center Operations Manager ve Log Analytics Aracısı'nın farklı çıkış-hazır sürümlerini sağlar. Bu sürümlerin her biri System Center Operations Manager'a, Log Analytics'e veya her ikisine birden raporlayabilir.  

Linux üzerinde Log Analytics aracısını Linux toplar ve izleme verilerini Log analytics'e gönderir.   

Windows veya Linux bilgisayarlarınızın doğrudan hizmetine bağlanamıyor, OMS ağ geçidi kullanarak Log Analytics'e bağlanmak için Log Analytics aracısını yapılandırmanız gerekir. OMS ağ geçidi yapılandırmak ve dağıtmak hakkında daha fazla bilgi için bkz. [OMS ağ geçidi kullanarak Internet erişimi bilgisayarları bağlama](../log-analytics/log-analytics-oms-gateway.md).  

Bağımlılık Aracısı'nı şu konumdan indirilebilir.

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.1 | 55030ABF553693D8B5112569FB2F97D7C54B66E9990014FC8CC43EFB70DE56C6 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.1 | 43C75EF0D34471A0CBCE5E396FFEEF4329C9B5517266108FA5D6131A353D29FE |

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri
Microsoft, Azure İzleyici hizmeti kullanımınız vasıtasıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft, kalite, güvenlik ve bütünlüğünü hizmeti geliştirmek için bu verileri kullanır. Doğru ve etkili sorun giderme özellikleri sağlamak üzere eşleme özelliğini verilerden işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi yazılımınızın yapılandırması hakkında bilgiler içerir. Microsoft, ad, adres veya diğer iletişim bilgilerinizi toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="performance-counters-enabled"></a>Performans sayaçları etkinleştirildi
VM'ler için Azure İzleyici, bir çözüm tarafından kullanılan performans sayaçları toplamak için Log Analytics çalışma alanı yapılandırır.  Aşağıdaki tabloda, 60 saniyede toplanan çözüm tarafından yapılandırılan sayaçlarını ve nesneleri listeler.

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

## <a name="enable-from-the-azure-portal"></a>Azure portalından etkinleştirme
Azure portalında Azure sanal makinenizin izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında **sanal makineler**. 
2. Listeden bir VM seçin. 
3. VM sayfasında içinde **izleme** bölümünden **Insights (Önizleme)**.
4. Üzerinde **Insights (Önizleme)** sayfasında **şimdi deneyin**.

    ![Bir VM için sanal makineler için Azure İzleyici etkinleştir](./media/monitoring-vminsights-onboard/enable-vminsights-vm-portal-01.png)

5. Üzerinde **Azure İzleyici İçgörüler ekleme** sayfasında mevcut bir Log Analytics varsa, aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  Listenin varsayılan çalışma alanı ve sanal makine abonelikte dağıtılmış konumunu belirler. 

    >[!NOTE]
    >VM izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md) daha önce desteklenen bölgelerden birinde listelenir.   

İzleme etkinleştirdikten sonra sanal makine için sistem durumu ölçümleri görmeden önce yaklaşık 10 dakika sürebilir. 

![Dağıtım işlemi izlemeyi VM'ler için Azure İzleyicisi'ni etkinleştirme](./media/monitoring-vminsights-onboard/onboard-vminsights-vm-portal-status.png)

## <a name="enable-using-azure-policy"></a>Azure İlkesi'ni kullanarak etkinleştirme
Tutarlı uyumluluk ve sağlanan, yeni sanal makineler için Otomatik etkinleştirme sağlayan birden çok Azure Vm'leri için çözümü etkinleştirmek için [Azure İlkesi](../azure-policy/azure-policy-introduction.md) önerilir.  Azure İlkesi ile sağlanan ilkeleri kullanarak yeni sanal makineler için aşağıdaki avantajları sunar:

* Azure İzleyici tanımlı kapsam içindeki her VM için sanal makineler için etkinleştirme
* Log Analytics aracısını dağıtmayı 
* Uygulama bağımlılıklarını keşfedin ve haritada göstermek için bağımlılık Aracısı dağıtma
* Önceden tanımlanmış bir ilke tanımı listesinde, Azure sanal makine işletim sistemi görüntüsüdür olup olmadığını denetle  
* Azure VM çalışma belirtilen dışındaki oturum olup olmadığını denetle
* Uyumluluk sonuçları raporu 
* Uyumlu olmayan VM'ler için Düzeltme desteği

Kiracınız için etkinleştirmek için bu işlem gerektirir:

- Log Analytics çalışma alanı aşağıda listelenen adımları kullanarak yapılandırın
- (Düzeyinde yönetim grubuna veya aboneliğe) kiracınız için girişim tanımı Al
- İlkeyi istenen kapsama atayın
- Uyumluluk sonuçlarını gözden geçirin

### <a name="add-the-policies-and-initiative-to-your-subscription"></a>İlke ve girişim aboneliğinize ekleyin
İlkeleri kullanmak için sağlanan bir PowerShell Betiği - kullanabilirsiniz [Ekle VMInsightsPolicy.ps1](https://www.powershellgallery.com/packages/Add-VMInsightsPolicy/1.2) bu görevi tamamlamak için Azure PowerShell Galerisi kullanılabilir. Betik, aboneliğiniz için ilke ve girişim ekler.  Aboneliğinizde Azure İlkesi yapılandırmak için aşağıdaki adımları gerçekleştirin. 

1. PowerShell betiğini yerel dosya sisteminize indirin.

2. Aşağıdaki PowerShell komutunu klasöründe ilke eklemek için kullanın. Betik, aşağıdaki isteğe bağlı parametreleri destekler: 

    ```powershell
    -UseLocalPolicies [<SwitchParameter>]
      <Optional> Load the policies from a local folder instead of https://raw.githubusercontent.com/dougbrad/OnBoardVMInsights/Policy/Policy/

    -SubscriptionId <String>
      <Optional> SubscriptionId to add the Policies/Initiatives to
    -ManagementGroupId <String>
      <Optional> Management Group Id to add the Policies/Initiatives to

    -Approve [<SwitchParameter>]
      <Optional> Gives the approval to add the Policies/Initiatives without any prompt
    ```  

    >[!NOTE]
    >Not: birden fazla aboneliğe girişim/ilke atamayı düşünüyorsanız, tanımları yönetim grubundaki ilkeyi atayacağınız abonelikleri içeren depolanmalıdır. Bu nedenle - Managementgroupıd parametresini kullanmanız gerekir.
    >
   
    Parametresiz örnek:  `.\Add-VMInsightsPolicy.ps1`

### <a name="create-a-policy-assignment"></a>İlke ataması oluşturma
Çalıştırdıktan sonra `Add-VMInsightsPolicy.ps1` PowerShell Betiği, aşağıdaki girişim ve ilkeleri eklenir:

* **Windows Vm'leri - Preview için log Analytics aracısını dağıtmayı**
* **Linux Vm'leri - Preview için log Analytics aracısını dağıtmayı**
* **Windows sanal makineler - Preview için bağımlılık Aracısı dağıtma**
* **Linux Vm'leri - Preview için bağımlılık Aracısı dağıtma**
* **Denetim Log Analytics aracısını dağıtım - VM görüntüsü (OS) listelenmemiş - Önizleme**
* **Denetim bağımlılık Aracısı dağıtımı - VM görüntüsü (OS) listelenmemiş - Önizleme**

Aşağıdaki girişim parametresi eklendi:

- **Analytice çalışma oturum** (PowerShell veya CLI kullanarak bir atama uygulama çalışma alanının ResourceId sağlamanız gerekir)

    Vm'leri denetim ilkelerinden uyumsuz olarak bulunduğunda **VM'lerin işletim sistemi kapsam içinde değil...**  dağıtım ilkesi ölçütlerini yalnızca iyi bilinen bir Azure VM görüntülerinden dağıtılan Vm'leri içerir. VM işletim sistemi veya destekleniyorsa belgelerine bakın.  Yüklü değilse, dağıtım ilkesi yinelenen ve güncelleştirme/kapsamda görüntüyü oluşturmaya değiştirmek gerekir.

Aşağıdaki tek başına isteğe bağlı İlkesi eklenir:

- **VM yapılandırılan eşleşmeyen Log Analytics çalışma alanı için - Önizleme**

    Bu, önceden yapılandırılmış sanal makineleri belirlemek için kullanılabilir [Log Analytics VM uzantısını](../virtual-machines/extensions/oms-windows.md), ancak farklı bir çalışma alanıyla (ilke ataması tarafından belirtildiği şekilde) istenenden yapılandırılır. Bu parametre için çalışma alanı kimliği alır.

Bu ilk sürümde, yalnızca Azure portalından ilke ataması oluşturabilirsiniz. Bu adımları tamamlamak nasıl anlamak için bkz: [Azure portalından bir ilke ataması oluşturma](../azure-policy/assign-policy-definition.md).

## <a name="enable-with-powershell"></a>PowerShell ile etkinleştirme
Birden çok VM veya VM ölçek kümeleri Azure İzleyici VM'ler için etkinleştirmek üzere, sağlanan bir PowerShell Betiği - kullanabilirsiniz [yükleme VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) bu görevi tamamlamak için Azure PowerShell Galerisi kullanılabilir.  Bu betik her sanal makine ve sanal makine yineleme ölçek kümesi tarafından belirtilen kapsamı belirlenmiş bir kaynak grubundaki aboneliğinizdeki *ResourceGroup*, tek bir VM veya ölçek kümesi tarafından belirtilen *adı*.  Her VM veya VM ölçek kümesi betik VM uzantısı zaten yüklü değilse ve yeniden yüklemek için değil denemesi olursa doğrular.  Aksi takdirde, Log Analytics ve bağımlılık Aracısı VM uzantıları yüklemeye devam eder.   

Bu betik, Azure PowerShell modülü sürüm 5.7.0 gerektirir veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

Komut dosyası hakkında Yardım almak için çalıştırabileceğiniz `Get-Help` bağımsız değişken ayrıntıları ve örnek kullanım listesini almak için.   

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and Dependency Agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VM's and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VM's/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VM's are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong to

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VM's to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VM's/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Suported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VM's in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to ReInstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to ReInstall (move to a new workspace)
```

Aşağıdaki örnek, VM'ler için Azure İzleyici etkinleştirmek ve beklenen çıktıyı anlamak için klasörde PowerShell komutlarını kullanarak göstermektedir:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VM's or VM ScaleSets matching criteria specified

VM's or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency Agent extensions on above 2 VM's or VM Scale Sets.
VM's in a non-running state will be skipped.
Extension will not be re-installed if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="enable-for-hybrid-environment"></a>Hibrit ortamı için etkinleştir
Bu bölümde, nasıl sanal makine veya fiziksel bilgisayarlar veri merkezinizi veya VM'ler için Azure İzleyici ile izleme için başka bir bulut ortamında barındırılan açıklanmaktadır.  

Azure İzleyici Vm'leri harita bağımlılık aracısı için tüm veriler aktarmaz ve güvenlik duvarları ya da bağlantı noktalarını herhangi bir değişiklik gerektirmez. Harita verileri her zaman doğrudan Azure İzleyici'hizmetine veya üzerinden Log Analytics aracısını tarafından aktarılan [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md) BT güvenlik ilkeleriniz bilgisayarların Internet'e bağlanmak için ağ üzerinde izin vermiyorsa.

Dağıtım yöntemleri ve gereksinimleri gözden [Log Analytics Linux ve Windows Aracısı](../log-analytics/log-analytics-concept-hybrid.md).

Özetlenen adımları:

1. Windows veya Linux için log Analytics aracısını yükleme
2. Azure İzleyici için Vm'leri harita bağımlılık aracısını yükleme
3. Performans sayaçlarını toplamayı etkinleştir
4. Sanal makineler için yerleşik Azure izleme

### <a name="install-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık aracısını yükleme 
Bağımlılık aracısını el ile Windows bilgisayarlarda çalıştırarak yüklenebilir `InstallDependencyAgent-Windows.exe`. Bu yürütülebilir dosya hiçbir seçenek olmadan çalıştırırsanız, etkileşimli bir şekilde yüklemek için izlemeniz gereken bir Kurulum Sihirbazı başlar.  

>[!NOTE]
>Aracıyı yüklemek veya kaldırmak için yönetici ayrıcalıkları gereklidir.

Aşağıdaki tabloda kurulum tarafından desteklenen komut satırı aracı için belirli parametreleri vurgular.  

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini döndürür. |
| /S | Kullanıcı etkileşimi olmadan Sessiz bir yükleme gerçekleştirin. |

Örneğin, ile yükleme programını çalıştırmak için `/?` parametresi, türü `InstallDependencyAgent-Windows.exe /?`

Windows bağımlılık aracısını için dosyalar yüklenir `C:\Program Files\Microsoft Dependency Agent` varsayılan olarak.  Kurulum tamamlandıktan sonra başlatmak bağımlılık Aracısı'nı başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Günlük dizini `%Programfiles%\Microsoft Dependency Agent\logs`. 

### <a name="install-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık aracısını yükleme
Bağımlılık Aracısı'nı Linux sunuculardan yüklü `InstallDependencyAgent-Linux64.bin`, kendi kendine ayıklanan bir ikili içeren bir kabuk betiği. Kullanarak dosyayı çalıştırabilirsiniz `sh` veya yürütme izinleri dosya için.

>[!NOTE]
> Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.
> 

| Parametre | Açıklama |
|:--|:--|
| -Yardım | Komut satırı seçeneklerinin listesini alır. |
| -s | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| --denetleyin | İzinleri ve işletim sistemini denetleyin ama aracıyı yüklemeyin. |

Örneğin, ile yükleme programını çalıştırmak için `-help` parametresi, türü `InstallDependencyAgent-Linux64.bin -help`.

Linux bağımlılık aracısını aşağıdaki komutu çalıştırarak kök olarak yükleme `sh InstallDependencyAgent-Linux64.bin`
    
Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları, günlük dizindir `/var/opt/microsoft/dependency-agent/log`.

Bağımlılık Aracısı'nı dosyaları aşağıdaki dizinlerde yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Performans sayaçları sağlar
Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, etkinleştirilmesi gerekir. Bu açıklandığı şekilde el ile gerçekleştirilebilir [burada](../log-analytics/log-analytics-data-sources-performance-counters.md), veya tarafından yükleme ve kullanılabilir bir PowerShell Betiği çalıştırma [Azure Powershell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1).
 
### <a name="onboard-azure-monitor-for-vms"></a>Sanal makineler için yerleşik Azure izleme
Bu yöntem, Log Analytics çalışma alanınıza çözüm bileşenlerini yapılandırmasını belirten bir JSON şablonu içerir.  

Bir şablon kullanarak kaynakları dağıtma kavramıyla alışkın değilseniz, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md) 

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Oluşturma ve bir şablonu yürütme

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Bu dosyayı farklı Kaydet **installsolutionsforvminsights.json** yerel bir klasöre.
3. Değerlerini düzenleyin **WorkspaceName**, **ResourceGroupName**, ve **WorkspaceLocation**.  Değeri **WorkspaceName** olan çalışma alanı adı ve değeri içeren Log Analytics çalışma alanınızın tam kaynak kimliği **WorkspaceLocation** çalışma tanımlanan bölgesi .
4. Aşağıdaki PowerShell komutunu kullanarak bu şablonu dağıtmaya hazırsınız:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

    ```powershell
    provisioningState       : Succeeded
    ```
İzleme etkinleştirdikten sonra sistem durumunu ve karma bilgisayar için ölçümleri görmeden önce yaklaşık 10 dakika sürebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineniz için etkin izleme ile bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir.  Sistem durumu özelliği kullanmayı öğrenmek için bkz [görünümü VM sistem durumu için Azure İzleyici](monitoring-vminsights-health.md), veya bulunan Uygulama bağımlılıklarını görüntülemek için bkz. [Vm'leri harita görünümü Azure İzleyici](monitoring-vminsights-maps.md).  