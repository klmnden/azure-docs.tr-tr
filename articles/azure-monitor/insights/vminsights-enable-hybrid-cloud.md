---
title: Karma bir ortamınız için Azure İzleyicisi'ni (Önizleme) etkinleştirme | Microsoft Docs
description: Bu makalede, nasıl Azure İzleyici VM'ler için bir veya daha fazla sanal makine içeren bir karma bulut ortamı olanak açıklanır.
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
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: 6b8870f0a6f14536fdf3a1ff675f2fbe3ce8aeec
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524198"
---
# <a name="enable-azure-monitor-for-vms-preview-for-a-hybrid-environment"></a>Azure İzleyici (Önizleme) VM'ler için karma bir ortamınız için etkinleştirin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, sanal makine veya veri merkezinizi veya başka bir bulut ortamında barındırılan fiziksel bilgisayarlar için Azure İzleyici (Önizleme) VM'ler için etkinleştirme açıklanmaktadır. Ortamınızdaki sanal makinelerinizi, başarıyla başladı, bu işlem sonunda izleme ve performans veya kullanılabilirlik sorunları yaşıyorsanız öğrenin. 

Alma başlatıldı, gözden geçirdiğinizden emin olun önce [önkoşulları](vminsights-enable-overview.md) ve aboneliğiniz ve kaynak gereksinimlerini karşılamak doğrulayın. Dağıtım yöntemleri ve gereksinimleri gözden [Log Analytics Linux ve Windows Aracısı](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

>[!NOTE]
>Azure İzleyici Vm'leri harita bağımlılık aracısı için hiçbir veri aktarır değil ve güvenlik duvarları veya bağlantı noktaları için herhangi bir değişiklik yapılması gerekmez. Harita verileri her zaman doğrudan Azure İzleyici'hizmetine veya üzerinden Log Analytics aracısını tarafından aktarılan [OMS ağ geçidi](../../azure-monitor/platform/gateway.md) BT güvenlik ilkeleriniz bilgisayarların internet'e bağlanmak için ağ üzerinde izin verme durumunda.

Bu görevi tamamlamak için görevleri aşağıda özetlenmiştir:

1. Windows veya Linux için log Analytics aracısını yükleyin.

2. Vm'leri harita bağımlılık aracısı için Azure İzleyicisi'ni yükleyip [Windows](https://aka.ms/dependencyagentwindows) veya [Linux](https://aka.ms/dependencyagentlinux).

3. Performans sayaçlarını toplamayı etkinleştirin.

4. VM'ler için Azure İzleyici dağıtın.

## <a name="install-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık aracısını yükleme
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

## <a name="install-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık aracısını yükleme
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

## <a name="enable-performance-counters"></a>Performans sayaçları sağlar
Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki yoldan biriyle yapabilirsiniz:
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* İndiriliyor ve kullanılabilir bir PowerShell betiğini çalıştırarak [Azure PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

## <a name="deploy-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici dağıtma
Bu yöntem, Log Analytics çalışma alanınızda çözüm bileşenlerini etkinleştirmek için yapılandırmasını belirten bir JSON şablonu içerir.

Bir şablon kullanarak kaynakları dağıtma ile bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-and-execute-a-template"></a>Oluşturma ve bir şablonu yürütme

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
    }
    ```

1. Bu dosyayı farklı Kaydet *installsolutionsforvminsights.json* yerel bir klasöre.

1. Değerleri yakalama *WorkspaceName*, *ResourceGroupName*, ve *WorkspaceLocation*. Değeri *WorkspaceName* Log Analytics çalışma alanınızın adıdır. Değeri *WorkspaceLocation* çalışma alanı içinde tanımlanan bölgedir.

1. Aşağıdaki PowerShell komutunu kullanarak bu şablonu dağıtmaya hazırsınız:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

    ```powershell
    provisioningState       : Succeeded
    ```
   İzleme etkinleştirdikten sonra sistem durumunu ve karma bilgisayar için ölçümleri görmeden önce yaklaşık 10 dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için izlenmesi etkin olduğunda, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir. Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). Performans sorunlarını ve Vm'leri performansınızı ile genel kullanımı belirlemek için bkz: [Azure VM performansını görüntüleme](vminsights-performance.md), ya da bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).