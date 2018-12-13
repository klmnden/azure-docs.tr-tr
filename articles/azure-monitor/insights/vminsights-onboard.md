---
title: Vm'leri Önizleme için Azure İzleyici dağıtma | Microsoft Docs
description: Bu makalede nasıl dağıtıp dağıtılmış uygulamanızı nasıl performans gösterdiğini anlamak başlatabilmeniz, VM'ler için Azure İzleyici yapılandırın ve hangi durumu sorunları belirlenmiştir açıklar.
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
ms.date: 12/07/2018
ms.author: magoedte
ms.openlocfilehash: 741288bd1a927b12705b3b31c5a1c60d6b94db5b
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53194258"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>Azure İzleyici Vm'leri Önizleme için dağıtma
Bu makalede, Azure İzleyici ' VM'ler için ayarlanacak açıklar. Hizmet, Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümeleri ve sanal makineleri ortamınızda işletim sistemi durumunu izler. Bu izleme, bulma ve bunlar üzerinde barındırılabilir uygulama bağımlılıkları eşleme içerir. 

VM'ler için Azure İzleyici aşağıdaki yöntemlerden birini kullanarak etkinleştirin:  

* Tek bir Azure sanal makine seçerek etkinleştirin **Insights (Önizleme)** doğrudan VM'den.
* İki veya daha fazla Azure sanal makine, Azure İlkesi'ni kullanarak etkinleştirin. Bu yöntem kullanılarak, mevcut ve yeni VM'lerin gerekli bağımlılıkları yüklü ve düzgün yapılandırılmış. Bunları etkinleştirmek ve düzeltmek istiyorsanız nasıl karar verebilmek için uyumlu olmayan Vm'leri raporlanır. 
* İki etkinleştirmek veya PowerShell kullanarak belirtilen abonelik veya kaynak grubu üzerinde daha fazla Azure sanal makineleri veya sanal makine ölçek kümeleri.

Her yöntem hakkında ek bilgi makalenin sonraki bölümlerinde sağlanır.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdaki bölümlerde yer alan bilgiler anladığınızdan emin olun.

### <a name="log-analytics"></a>Log Analytics 

Bir Log Analytics çalışma alanı şu anda şu bölgelerde desteklenir:

  - Batı Orta ABD  
  - Doğu ABD  
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

Azure portalında tek bir Azure VM için izlemeyi etkinleştirme, bu işlem sırasında bir çalışma alanı oluşturabilirsiniz. 

Ölçekli senaryosu için çözümü etkinleştirmek için Log Analytics çalışma alanınızda önce aşağıdakileri yapılandırın:

* ServiceMap ve InfrastructureInsights çözümlerini yükleyin. Bu yükleme, yalnızca bu makalede sağlanan bir Azure Resource Manager şablonu kullanarak tamamlayabilirsiniz.  
* Performans sayaçları toplamak için Log Analytics çalışma alanı yapılandırın.

Çalışma alanınız ölçekli senaryo için yapılandırmak üzere bkz [ölçekli dağıtımı için Log Analytics çalışma alanını ayarlama](#setup-log-analytics-workspace).

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda, VM'ler için Azure İzleyici ile desteklenen Windows ve Linux işletim sistemleri listelenmiştir. Büyük ve küçük Linux işletim sistemi sürüm ayrıntıları ve çekirdek sürümleriyle desteklenen tam listesi daha sonra bu bölümde sağlanır.

|İşletim sistemi sürümü |Performans |Haritalar |Durum |  
|-----------|------------|-----|-------|  
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|Red Hat Enterprise Linux (RHEL) 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |  
|CentOS Linux 7, 6 | X | X | X |  
|SUSE Linux Enterprise Server (SLES) 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4 sürümünden, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> VM'ler için Azure İzleyici performans özelliği yalnızca Azure İzleyici'deki kullanılabilir. Doğrudan Azure VM'nin sol bölmeden eriştiğinizde kullanılamaz. 

>[!NOTE]
>Aşağıdaki bilgiler, Linux işletim sistemini desteklemek için geçerlidir:  
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir. 
> - Fiziksel Adres Uzantısı (PAE) ve Xen, desteklenmeyen bir Linux dağıtımı için gibi standart olmayan çekirdek serbest bırakır. Örneğin, bir sürüm dizesi sistemiyle *2.6.16.21-0.8-xen* desteklenmiyor. 
> - Standart çekirdekleri yeniden derlemelerinin dahil olmak üzere özel çekirdekleri desteklenmez. 
> - CentOSPlus çekirdek desteklenmez. 


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

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency aracı
Vm'leri Haritası özelliği için Azure İzleyici verilerini Microsoft Dependency Aracıdan alır. Log Analytics aracısını Log Analytics bağlantısını için bağımlılık Aracısı'nı kullanır. Bu nedenle, sisteminizde yüklü ve yapılandırılmış bağımlılık aracısını Log Analytics aracısını olması gerekir. 

Azure İzleyici, VM'ler için tek bir Azure VM için etkinleştirmek ister ölçekli dağıtım yöntemini kullanmak, deneyiminin bir parçası aracıyı yüklemek için Azure VM bağımlılık Aracısı uzantısı kullanmanız gerekir. 

Karma bir ortamda, indirin ve iki yoldan biriyle bağımlılık aracısını yükleme: El ile veya sanal makineler için bir otomatik dağıtım yöntemi kullanarak, Azure'un dışında barındırılır. 

Aşağıdaki tabloda, karma bir ortamda, eşleme özelliğini destekleyen bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | Ek olarak [Windows için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Windows aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemlerinin tam listesi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | Ek olarak [Linux için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Linux aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemlerinin tam listesi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |  

Bağımlılık Aracısı'nı aşağıdaki konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Etkinleştirmek ve VM'ler için Azure İzleyici'nın özelliklerine erişmek için aşağıdaki erişim rolleri atanmış olması gerekir: 
  
- Çözümü etkinleştirmek için olmalıdır *Log Analytics katkıda bulunan* rol. 

- Performans, sistem durumu, görüntüleme ve harita verileri için olmalıdır *izleme okuyucusu* Azure VM rolü. Log Analytics çalışma alanı için Azure İzleyici VM'ler için yapılandırılmış olması gerekir.  

Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md).

## <a name="enable-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi etkinleştir
Azure portalında Azure sanal makinenizin izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) oturum açın. 
1. Seçin **sanal makineler**. 
1. Listeden bir VM seçin. 
1. VM sayfasında içinde **izleme** bölümünden **Insights (Önizleme)**.
1. Üzerinde **Insights (Önizleme)** sayfasında **şimdi deneyin**.

    ![Bir VM için sanal makineler için Azure İzleyici etkinleştir](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)

1. Üzerinde **Azure İzleyici İçgörüler ekleme** sayfasında mevcut bir Log Analytics varsa, aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  
    Listenin varsayılan çalışma alanı ve sanal makine abonelikte dağıtılmış konumunu belirler. 

    >[!NOTE]
    >VM izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md) daha önce desteklenen bölgelerden birinde listelenir.  

İzleme etkinleştirdikten sonra sanal makine için sistem durumu ölçümleri görmeden önce yaklaşık 10 dakika sürebilir. 

![Dağıtım işlemi izlemeyi VM'ler için Azure İzleyicisi'ni etkinleştirme](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="deploy-at-scale"></a>Uygun ölçekte dağıtın
Bu bölümde, Azure İzleyici VM'ler için uygun ölçekte Azure İlkesi veya Azure PowerShell kullanarak dağıtırsınız. 

Sanal makinelerinizi dağıtmadan önce aşağıdakileri yaparak Log Analytics çalışma alanınızın önceden yapılandırın:

1. Bir çalışma alanı yoksa sanal makineler için Azure İzleyicisi'ni destekleyen bir tane oluşturun.  
    Devam etmeden önce bkz [çalışma alanlarını yönetme](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json) maliyeti, yönetim ve uyumluluk için yapılacak değerlendirmeleri anlamaktır.      

1. Yeni bir tane zaten, mevcut değilse çalışma alanı, VM'ler için Azure İzleyici desteklemek için kullanılabilir. Gözden geçirme [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json) devam etmeden önce maliyeti, yönetim ve uyumluluk konuları anlamak için yeni bir çalışma alanı oluşturmadan önce.       

1. Çalışma alanı koleksiyonu Linux ve Windows Vm'leri için performans sayaçları sağlar.

1. Yükleme ve çalışma alanınızdaki ServiceMap ve InfrastructureInsights çözümü etkinleştirin. 

### <a name="set-up-a-log-analytics-workspace"></a>Bir Log Analytics çalışma alanını ayarlama
Bir Log Analytics çalışma alanınız yoksa, oluşturun, önerilen yöntemleri inceleyerek ["Önkoşullar"](#log-analytics) bölümü. 

#### <a name="enable-performance-counters"></a>Performans sayaçları sağlar
Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki yoldan biriyle yapabilirsiniz:
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* İndiriliyor ve kullanılabilir bir PowerShell betiğini çalıştırarak [Azure PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)
 
#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>ServiceMap ve InfrastructureInsights çözümleri yüklemesi
Bu yöntem, Log Analytics çalışma alanınızda çözüm bileşenlerini etkinleştirmek için yapılandırmasını belirten bir JSON şablonu içerir. 

Bir şablon kullanarak kaynakları dağıtma ile bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md) 

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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

1. Bu dosyayı farklı Kaydet *installsolutionsforvminsights.json* yerel bir klasöre.
1. Değerlerini düzenleyin *WorkspaceName*, *ResourceGroupName*, ve *WorkspaceLocation*. Değeri *WorkspaceName* çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği. Değeri *WorkspaceLocation* çalışma alanı içinde tanımlanan bölgedir.
1. Aşağıdaki PowerShell komutunu kullanarak bu şablonu dağıtmaya hazırsınız:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

    ```powershell
    provisioningState       : Succeeded
    ```

### <a name="enable-by-using-azure-policy"></a>Azure İlkesi'ni kullanarak etkinleştirme
Uygun ölçekte yardımcı tutarlı uyumluluk ve yeni sağlanan sanal makinelerin Otomatik etkinleştirme emin bir şekilde VM'ler için Azure İzleyicisi'ni etkinleştirmek için önerilir [Azure İlkesi](../../azure-policy/azure-policy-introduction.md). Bu ilkeler:

* Log Analytics aracısını ve bağımlılık aracısını dağıtın. 
* Uyumluluk sonuçları hakkında rapor oluşturun. 
* Uyumlu olmayan VM'ler için düzeltin.

Azure İzleyici VM'ler için kiracınızda Azure İlkesi kullanarak etkinleştirmek için: 

- Girişimin bir kapsama atayın: Yönetim grubu, abonelik veya kaynak grubu 
- Gözden geçirin ve uyumluluk sonuçlarını Düzelt  

Azure ilkesi atama hakkında daha fazla bilgi için bkz. [Azure ilkesine genel bakış](../../governance/policy/overview.md#policy-assignment) ve gözden geçirme [yönetim gruplarına genel bakış](../../governance/management-groups/index.md) devam etmeden önce. 

İlke tanımları aşağıdaki tabloda listelenmiştir: 

|Ad |Açıklama |Tür |  
|-----|------------|-----|  
|[Önizleme]: Sanal makineler için Azure İzleyicisi'ni etkinleştirme |Azure İzleyici, belirtilen kapsam (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler (VM) için etkinleştirin. Log Analytics çalışma alanı, bir parametre olarak alır. |Girişim |  
|[Önizleme]: Denetim bağımlılık Aracısı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı |Vm'leri uyumsuz olarak VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil bildirir. |İlke |  
|[Önizleme]: Denetim günlüğü analiz aracı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı |Vm'leri uyumsuz olarak VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil bildirir. |İlke |  
|[Önizleme]: Linux sanal makineleri için bağımlılık Aracısı dağıtma |Bağımlılık aracısını Linux Vm'leri için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |  
|[Önizleme]: Windows sanal makineleri için bağımlılık Aracısı dağıtma |Windows Vm'leri için bağımlılık Aracısı, aracı yüklü değil ve (OS) VM görüntü listesinde tanımlanan dağıtın. |İlke |  
|[Önizleme]: Linux Vm'leri için log Analytics aracısını dağıtmayı |Log Analytics aracısını Linux Vm'leri için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |  
|[Önizleme]: Windows Vm'leri için log Analytics aracısını dağıtmayı |VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil Windows Vm'leri için log Analytics aracısını dağıtın. |İlke |  

Tek başına ilke (girişimle yer almaz) aşağıda açıklanmıştır: 

|Ad |Açıklama |Tür |  
|-----|------------|-----|  
|[Önizleme]: VM - rapor uyuşmazlığı denetim Log Analytics çalışma alanı |Vm'lere ilke/girişim atamasını belirtilen Log Analytics çalışma alanına oturum olmayan uyumsuz olarak bildirin. |İlke |

#### <a name="assign-the-azure-monitor-initiative"></a>Azure İzleyici girişim Ata
Bu ilk sürümde, yalnızca Azure Portalı'nda ilke ataması oluşturabilirsiniz. Bu adımları tamamlamak nasıl anlamak için bkz: [Azure portalından bir ilke ataması oluşturma](../../governance/policy/assign-policy-portal.md). 

1. Azure portalında Azure İlkesi hizmetini başlatmak için **tüm hizmetleri**, arayın ve seçin **ilke**. 
1. Azure İlkesi sayfasının sol bölmesinde seçin **atamaları**.  
    Atama, belirli bir kapsamda gerçekleşmesi için atanmış olan bir ilkedir.
1. Üst kısmındaki **ilke - atamalar** sayfasında **girişim atamak**.
1. Üzerinde **girişim atamak** sayfasında **kapsam** göre üç nokta (…) tıklatarak ve bir yönetim grubu veya abonelik seçin.  
    Bizim örneğimizde, bir kapsam bir gruplandırma için ilke ataması için zorlama sanal makinelerin sınırlar.
1. Sayfanın alt kısmında **kapsam** seçerek değişikliklerinizi kaydedin sayfasının **seçin**.
1. (İsteğe bağlı) Kapsamdan bir veya daha fazla kaynakları kaldırmak için işaretleyin **dışlamaları**. 
1. Seçin **girişim tanımı** kullanılabilir tanımlar listesini görüntülemek için üç nokta (...) seçin  **[Önizleme] VM'ler için Azure İzleyici'ı etkinleştirme**ve ardından seçin **Seçin**.  
    **Atama adı** kutusu seçtiğiniz girişim adıyla otomatik olarak doldurulur, ancak bunu değiştirebilirsiniz. İsteğe bağlı bir açıklama da ekleyebilirsiniz. **Atayan** kutusu, açan göre otomatik olarak doldurulur ve bu değer isteğe bağlıdır.
1. İçinde **Log Analytics çalışma alanı** bir çalışma alanı açılır listesinde desteklenen bir bölge için seçin.

    >[!NOTE]
    >Çalışma alanı atama kapsamı dışındadır, verme *Log Analytics katkıda bulunan* izinleri ilke atama sorumlusu kimliği Bunu yapmazsanız, aşağıdaki gibi bir dağıtım hatası görebilirsiniz: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... ` Erişim vermek için gözden [el ile yönetilen kimlik yapılandırma](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
    >  
    **Yönetilen kimliği** onay kutusu seçiliyse, atanan girişim bir ilkeyle içerdiğinden *Deployıfnotexists* efekt. 
1. İçinde **yönetme kimlik konumu** aşağı açılan listesinde, uygun bölgeyi seçin. 
1. **Ata**'yı seçin.

#### <a name="review-and-remediate-the-compliance-results"></a>Gözden geçirin ve uyumluluk sonuçlarını Düzelt 

Okuyarak uyumluluk sonuçlarını gözden geçirmek öğrenebilirsiniz [uyumsuzluk sonuçları tanımlamak](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Sol bölmede seçin **Uyumluluk**ve bulun  **[Önizleme] VM'ler için Azure İzleyici'ı etkinleştirme** girişim ataması göre uyumlu olmayan VM'ler için oluşturduğunuz.

![Azure Vm'leri için Uyumluluk İlkesi](./media/vminsights-onboard/policy-view-compliance-01.png)

Girişimle dahil ilke sonuçları temelinde, uyumlu değil olarak aşağıdaki senaryolarda Vm'leri bildirilir:  
  
* Log Analytics veya bağımlılık aracısını dağıtılabilir değil. 
   Bu senaryo, var olan Vm'leri bir kapsamla tipik bir durumdur. Bunu azaltmak için gereken aracıları tarafından dağıtma [düzeltme görevler oluşturma](../../governance/policy/how-to/remediate-resources.md) uyumlu olmayan bir ilkesi üzerinde yapılamaz.   
 
    - [Önizleme]: Deploy Dependency Agent for Linux VMs   
    - [Önizleme]: Deploy Dependency Agent for Windows VMs  
    - [Önizleme]: Deploy Log Analytics Agent for Linux VMs  
    - [Önizleme]: Deploy Log Analytics Agent for Windows VMs  

* VM görüntüsü (OS) ilke tanımında tanımlanan değildir. 
   Dağıtım ilkesi ölçütlerini iyi bilinen bir Azure VM görüntülerinden dağıtılan Vm'leri içerir. VM işletim sistemi desteklenip desteklenmediğini görmek için belgelere bakın. Desteklenmiyorsa, güncelleştirme ve dağıtım ilkesi yinelenen veya görüntü uyumlu hale getirmek için değiştirebilirsiniz. 
  
    - [Önizleme]: Denetim bağımlılık Aracısı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı  
    - [Önizleme]: Denetim günlüğü analiz aracı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı

* Sanal makineleri belirtilen Log Analytics çalışma alanına oturum değildir.  
    Bazı VM'ler girişim kapsamda bir ilke atamasında belirtilen başka bir Log Analytics çalışma açtığı, mümkündür. Bu ilke, VM'ler, uyumlu olmayan bir çalışma alanına raporlama yapmayan tanımlamak için kullanılan bir araçtır. 
 
    - [Önizleme]: Audit Log Analytics Workspace for VM - Report Mismatch  

### <a name="enable-with-powershell"></a>PowerShell ile etkinleştirme
Azure İzleyici VM'ler için birden çok sanal makineleri veya sanal makine ölçek kümeleri için etkinleştirmek için PowerShell betiğini kullanabilirsiniz [yükleme VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), Azure PowerShell Galerisi kullanılabilir. Bu betik, aboneliğinizdeki tarafından belirtilen kapsamı belirlenmiş bir kaynak grubundaki her sanal makine ve sanal makine ölçek kümesi gezinir *ResourceGroup*, ya da tarafından belirtilen tek bir sanal makine veya sanal makine ölçek kümesi *Adı*. Her sanal makine veya sanal makine ölçek kümesi için betik VM uzantısı zaten yüklü olup olmadığını doğrular. VM uzantısı yüklü değilse, yeniden yüklemek betik çalışır. VM uzantısı yüklü değilse, betik Log Analytics ve bağımlılık Aracısı VM uzantılarını yükler.  

Bu betik, Azure PowerShell modülü sürüm 5.7.0 gerektirir veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

Betik bağımsız değişkeni ayrıntıları ve örnek kullanım listesini almak için çalıştırın `Get-Help`.  

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VMs and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VMs/VM Scale Sets that will apply to and let you confirm to continue.
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
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
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

    Install for all VMs in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to reinstall (move to a new workspace)
```

Aşağıdaki örnek, VM'ler için Azure İzleyici etkinleştirmek ve beklenen çıktıyı anlamak için klasörde PowerShell komutlarını kullanarak göstermektedir:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or VM ScaleSets matching criteria specified

VMs or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on above 2 VMs or VM Scale Sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example to update workspace

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

## <a name="enable-for-a-hybrid-environment"></a>Karma bir ortamınız için etkinleştir
Bu bölümde, sanal makine veya veri merkezinizde veya diğer bulut ortamları, izleme, VM'ler için Azure İzleyici tarafından barındırılan fiziksel bilgisayarları dağıtmak açıklanmaktadır. 

Azure İzleyici Vm'leri harita bağımlılık aracısı için hiçbir veri aktarır değil ve güvenlik duvarları veya bağlantı noktaları için herhangi bir değişiklik yapılması gerekmez. Harita verileri her zaman doğrudan Azure İzleyici'hizmetine veya üzerinden Log Analytics aracısını tarafından aktarılan [OMS ağ geçidi](../../azure-monitor/platform/gateway.md) BT güvenlik ilkeleriniz bilgisayarların internet'e bağlanmak için ağ üzerinde izin verme durumunda.

Dağıtım yöntemleri ve gereksinimleri gözden [Log Analytics Linux ve Windows Aracısı](../../log-analytics/log-analytics-agent-overview.md). 

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Adımları aşağıda özetlenmiştir:

1. Windows veya Linux için log Analytics aracısını yükleyin.
1. Vm'leri harita bağımlılık aracısı için Azure İzleyicisi'ni yükleyip [Windows](https://aka.ms/dependencyagentwindows) veya [Linux](https://aka.ms/dependencyagentlinux).
1. Performans sayaçlarını toplamayı etkinleştirin.
1. VM'ler için Azure İzleyici dağıtın.

### <a name="install-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık aracısını yükleme 
Bağımlılık aracısını el ile Windows bilgisayarlarda çalıştırarak yükleyebilirsiniz `InstallDependencyAgent-Windows.exe`. Bu yürütülebilir dosya hiçbir seçenek olmadan çalıştırırsanız, aracıyı etkileşimli olarak yüklemek için izlemeniz gereken bir Kurulum Sihirbazı başlar. 

>[!NOTE]
>*Yönetici* ayrıcalıkları yükleyin veya aracıyı kaldırmak için gereklidir.

Aşağıdaki tabloda kurulum tarafından desteklenen komut satırı aracı için parametreleri vurgular. 

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini döndürür. |
| /S | Kullanıcı etkileşimi olmadan Sessiz bir yükleme gerçekleştirir. |

Örneğin, ile yükleme programını çalıştırmak için `/?` parametresi, türü **InstallDependencyAgent Windows.exe /?**.

Windows bağımlılık aracısını için dosyalar yüklenir *C:\Program Files\Microsoft bağımlılık aracısını* varsayılan olarak. Kurulum tamamlandıktan sonra başlatmak bağımlılık Aracısı'nı başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Günlük dizini *%Programfiles%\Microsoft bağımlılık Agent\logs*. 

### <a name="install-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık aracısını yükleme
Bağımlılık Aracısı'nı Linux sunuculardan yüklü *InstallDependencyAgent Linux64.bin*, kendi kendine ayıklanan bir ikili içeren bir kabuk betiği. Kullanarak dosyayı çalıştırabilirsiniz `sh` veya yürütme izinleri dosya için.

>[!NOTE]
> Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.
> 

| Parametre | Açıklama |
|:--|:--|
| -Yardım | Komut satırı seçeneklerinin listesini alır. |
| -s | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| --denetleyin | İzinler ve işletim sistemini denetleyin, ancak aracıyı yüklemeyin. |

Örneğin, ile yükleme programını çalıştırmak için `-help` parametresi, türü **InstallDependencyAgent Linux64.bin-yardımcı**.

Aşağıdaki komutu çalıştırarak kök olarak Linux bağımlılık aracısını yükleme `sh InstallDependencyAgent-Linux64.bin`.
    
Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları, günlük dizindir */var/opt/microsoft/dependency-agent/log*.

Bağımlılık Aracısı'nı dosyaları aşağıdaki dizinlerde yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Performans sayaçları sağlar
Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki yoldan biriyle yapabilirsiniz: 
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* İndiriliyor ve kullanılabilir bir PowerShell betiğini çalıştırarak [Azure PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)
 
### <a name="deploy-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici dağıtma
Bu yöntem, Log Analytics çalışma alanınızda çözüm bileşenlerini etkinleştirmek için yapılandırmasını belirten bir JSON şablonu içerir. 

Bir şablon kullanarak kaynakları dağıtma ile bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md) 

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

1. Bu dosyayı farklı Kaydet *installsolutionsforvminsights.json* yerel bir klasöre.
1. Değerlerini düzenleyin *WorkspaceName*, *ResourceGroupName*, ve *WorkspaceLocation*. Değeri *WorkspaceName* çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği. Değeri *WorkspaceLocation* çalışma alanı içinde tanımlanan bölgedir.
1. Aşağıdaki PowerShell komutunu kullanarak bu şablonu dağıtmaya hazırsınız:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

    ```powershell
    provisioningState       : Succeeded
    ```
İzleme etkinleştirdikten sonra sistem durumunu ve karma bilgisayar için ölçümleri görmeden önce yaklaşık 10 dakika sürebilir. 

## <a name="performance-counters-enabled"></a>Performans sayaçları etkinleştirildi
VM'ler için Azure İzleyici, çözüm tarafından kullanılan performans sayaçları toplamak için bir Log Analytics çalışma alanı yapılandırır. Aşağıdaki tabloda, 60 saniyede toplanan çözüm tarafından yapılandırılan sayaçlarını ve nesneleri listeler.

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
