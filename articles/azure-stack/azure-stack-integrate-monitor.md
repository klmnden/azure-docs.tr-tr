---
title: Azure Stack ile dış izleme çözümünü tümleştirmek | Microsoft Docs
description: Azure Stack, veri merkezinizdeki dış bir izleme çözümü ile tümleştirmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/06/2019
ms.author: jeffgilb
ms.reviewer: thoroet
ms.lastreviewed: 02/06/2019
ms.openlocfilehash: 77dda80e538c8b742a96e7b7f81abe8650ee6b5d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59257306"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Dış izleme çözümü, Azure Stack ile tümleştirin

Azure Stack altyapısını dış izleme için Azure Stack yazılımı, fiziksel bilgisayarları ve fiziksel ağ anahtarları izlemeniz gerekir. Bu alanların her biri, durum ve uyarı bilgilerini almak için bir yöntem sunar:

- Azure Stack yazılımı sistem durumu ve uyarıları almak için REST tabanlı bir API sunar. Depolama alanları doğrudan, depolama durumunu ve uyarılar gibi yazılım tanımlı teknolojilerinin kullanılması yazılım izleme bir parçası olan.
- Fiziksel bilgisayar sistem durumu ve uyarı bilgilerini temel kart yönetim denetleyicileri (Bmc'ler) aracılığıyla kullanılabilir hale getirir.
- Fiziksel ağ aygıtlarının durumunu ve uyarı bilgileri SNMP protokolü aracılığıyla kullanılabilir hale getirebilirsiniz.

Her Azure Stack çözüm donanım yaşam döngüsü konak ile birlikte gelir. Bu konak fiziksel sunucuları ve ağ cihazlarını izleme yazılımı orijinal ekipman üreticisi (OEM) donanım satıcısının çalıştırır. Lütfen izleme çözümleri, veri merkezinizdeki mevcut izleme çözümleriyle tümleştirilebilir, OEM sağlayıcınıza başvurun.

> [!IMPORTANT]
> Kullandığınız dış izleme çözümü aracısız olması gerekir. Azure Stack bileşenleri içindeki üçüncü taraf aracılar yükleyemezsiniz.

Aşağıdaki diyagramda bir Azure Stack tümleşik sistemi, donanım yaşam döngüsü konak, dış izleme çözümünü ve bir dış bilet oluşturma/veri toplama sistem arasındaki trafik akışını gösterir.

![Azure Stack, izleme ve bilet oluşturma çözümü arasındaki trafiği gösteren diyagram.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

> [!NOTE]
> Fiziksel sunucuları ile doğrudan dış Monitoring Integration değil izin ve erişim denetim listeleri tarafından (ACL'ler) etkin bir şekilde engellendi.  Dış izleme tümleştirmesiyle doğrudan fiziksel ağ aygıtlarını desteklenmiyor, lütfen bu özelliği etkinleştirmek nasıl OEM sağlayıcınızla denetleyin.

Bu makalede, Azure Stack, System Center Operations Manager ve Nagios gibi dış izleme çözümleriyle tümleştirmek açıklanmaktadır. Ayrıca uyarılarla PowerShell kullanarak veya REST API çağrıları üzerinden programlı bir şekilde çalışmanın nasıl içerir.

## <a name="integrate-with-operations-manager"></a>Operations Manager ile tümleştirme

Azure Stack dış izlemek için Operations Manager kullanabilirsiniz. System Center Yönetim Paketi Microsoft Azure Stack için tek bir Operations Manager örneği ile birden çok Azure Stack dağıtımı izlemenizi sağlar. Yönetim Paketi, Azure Stack ile iletişim kurmak için sistem durumu kaynak sağlayıcısı ve güncelleştirme kaynak sağlayıcısı REST API'leri kullanır. Donanım yaşam döngüsü konakta çalışan yazılım izleme OEM atlama planlıyorsanız, fiziksel sunucuları izlemek için satıcı yönetim paketlerini yükleyebilirsiniz. Operations Manager ağ cihazı bulma, ağ anahtarlarını izlemek için de kullanabilirsiniz.

Azure Stack için yönetim paketi aşağıdaki özellikleri sağlar:

- Birden çok Azure Stack dağıtımı yönetebilirsiniz.
- Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) için destek
- Alabilir ve Uyarıları Kapat
- Bir sistem durumu ve kapasite Panosu
- Düzeltme eki ve güncelleştirme (P & U) olduğunda ediyor için otomatik Bakım modu algılama dahildir
- Dağıtım ve bölge için zorla güncelleştirme görevleri içerir
- Bir bölgeye özel bilgiler ekleyebilirsiniz
- Bildirim destekler ve Raporlama

System Center Yönetim Paketi, Microsoft Azure Stack ve ilişkili indirebilirsiniz [Kullanıcı Kılavuzu](https://www.microsoft.com/en-us/download/details.aspx?id=55184), veya doğrudan Operations Manager.

Bilet oluşturma çözümü için Operations Manager, System Center Service Manager ile tümleştirebilirsiniz. Tümleşik bir ürün bağlayıcısının Service Manager'da hizmet isteği çözdükten sonra Azure Stack ve Operations Manager bir uyarı kapatmak izin veren iki yönlü iletişimi sağlar.

Aşağıdaki diyagram, mevcut bir System Center dağıtım ile Azure Stack tümleştirme gösterir. Service Manager daha fazla Azure Stack'te işlemleri çalıştırmak için System Center Orchestrator'ı veya Service Management Automation (SMA) ile otomatik hale getirebilirsiniz.

![SMA OM ve Service Manager ile tümleştirme gösteren diyagram.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Nagios ile tümleştirin

Eklenti izleme bir Nagios İhtiyari ücretsiz yazılım lisansı – MIT (Massachusetts Teknoloji Enstitüsü'nün) altında kullanılabilir olan iş ortağı Cloudbase çözümleriyle birlikte sunulmuştur geliştirilmiştir.

Eklenti Python'da yazılmıştır ve sistem durumu kaynak sağlayıcısı REST API'si yararlanır. Alma ve Azure stack'teki uyarıları kapatmak için temel işlevleri sunar. System Center Yönetim Paketi gibi birden fazla Azure Stack dağıtım eklemek ve bildirim göndermek için sağlar.

Eklenti Nagios Kurumsal ve Nagios Core ile çalışır. İndirebilirsiniz [burada](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). İndirme sitesinde ayrıca yükleme ve yapılandırma ayrıntılarını içerir.

### <a name="plugin-parameters"></a>Eklenti parametreleri

"Azurestack_plugin.py" eklenti dosyası aşağıdaki parametrelerle yapılandırın:

| Parametre | Açıklama | Örnek |
|---------|---------|---------|
| *arm_endpoint* | Azure Resource Manager (Yönetici) uç noktası | https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Azure Resource Manager (Yönetici) uç noktası  | https://adminmanagement.local.azurestack.external |
| *Kiracı* | Yönetim abonelik kimliği | Yönetici portalı veya PowerShell üzerinden alın |
| *User_name* | İşleç abonelik kullanıcı adı | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | İşleç abonelik parolası | parolam |
| *Client_id* | İstemci | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *bölge* |  Azure Stack bölge adı | yerel |
|  |  |

* PowerShell sağlanan Evrensel GUID'dir. Her dağıtım için kullanabilirsiniz.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>İzleyici sistem durumu ve uyarıları için PowerShell kullanma

Operations Manager, Nagios ve Nagios tabanlı bir çözümü kullanmıyorsanız, Azure Stack ile tümleştirme çözümlerini izleme geniş etkinleştirmek için PowerShell kullanabilirsiniz.

1. PowerShell kullanmak için bilgisayarınızda yüklü olduğundan emin olun [PowerShell yüklenmiş ve yapılandırılmış](azure-stack-powershell-configure-quickstart.md) Azure Stack operatörü ortam için. PowerShell'i Resource Manager (Yönetici) uç noktası ulaşan yerel bir bilgisayara yükleme (https://adminmanagement. [ Bölge]. [External_FQDN]).

2. Azure Stack operatör olarak Azure Stack ortamınıza bağlanmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Aşağıdaki örnekler gibi uyarılarla çalışma için komutları kullanın:
   ```powershell
    #Retrieve all alerts
    $Alerts = Get-AzsAlert
    $Alerts

    #Filter for active alerts
    $Active = $Alerts | Where-Object { $_.State -eq "active" }
    $Active

    #Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    $RPHealth = Get-AzsRPHealth
    $RPHealth

    #Retrieve infrastructure role instance health
    $FRPID = $RPHealth | Where-Object { $_.DisplayName -eq "Capacity" }
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId
    ```

## <a name="learn-more"></a>Daha fazla bilgi edinin

Yerleşik sistem durumu izleme hakkında daha fazla bilgi için bkz: [izleme sistem durumu ve Uyarıları Azure Stack'te](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik tümleştirmesi](azure-stack-integrate-security.md)
