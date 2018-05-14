---
title: Dış izleme çözümü Azure yığın ile tümleştirme | Microsoft Docs
description: Dış bir izleme çözümü, veri merkezinizdeki Azure yığın tümleştirileceğini öğrenin.
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
ms.date: 05/10/2018
ms.author: jeffgilb
ms.reviewer: thoroet
ms.openlocfilehash: d7c8520602132722fd0c7138de4a276b9ac2208a
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Dış izleme çözümü Azure yığın ile tümleştirme

Dış Azure yığın altyapı izleme için Azure yığın yazılım, fiziksel bilgisayarları ve fiziksel ağ anahtarları izlemeniz gerekir. Bu alanların her biri, sistem durumu ve uyarı bilgilerini almak için bir yöntem sunar:

- Azure yığın yazılım sistem durumu ve uyarıları almak için REST tabanlı bir API sunar. Depolama alanları doğrudan, depolama durumunu ve Uyarıları gibi yazılım tanımlı teknolojileri kullanan yazılım izleme bölümü.
- Fiziksel bilgisayar sistem durumunu ve uyarı bilgilerini temel kart yönetim denetçileri (BMC) aracılığıyla kullanılabilir hale getirebilirsiniz.
- Fiziksel ağ aygıtlarını sistem durumu ve uyarı bilgileri SNMP protokolü aracılığıyla kullanılabilir hale getirebilirsiniz.

Her Azure yığın çözüm donanım yaşam döngüsü ana bilgisayar ile birlikte gelir. Bu ana bilgisayar özgün donanım üreticisi (OEM) donanım satıcısının izleme yazılım fiziksel sunucuları ve ağ aygıtları için çalışır. İsterseniz, bu çözümleri izleme atlayabilir ve doğrudan, veri merkezinizde mevcut izleme çözümleriyle tümleştirin.

> [!IMPORTANT]
> Kullandığınız dış izleme çözümü aracısız olması gerekir. Azure yığın bileşenleri içindeki üçüncü taraf aracılar yükleyemezsiniz.

Aşağıdaki diyagramda bir Azure tümleşik yığını sistem, donanım yaşam döngüsü konak, dış bir izleme çözümü ve bir dış raporlama/veri toplama sistemi arasındaki trafik akışını gösterir.

![İzleme ve raporlama çözümü yığını, Azure arasındaki trafiği gösteren diyagram.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

Bu makalede, Azure yığın System Center Operations Manager ve Nagios gibi dış izleme çözümü ile tümleştirmek açıklanmaktadır. Ayrıca, uyarılarla programlı olarak REST API çağrıları aracılığıyla veya PowerShell kullanarak nasıl çalışılacağı içerir.

## <a name="integrate-with-operations-manager"></a>Operations Manager ile tümleştirme

Operations Manager dış Azure yığınını izlemek için kullanabilirsiniz. Microsoft Azure yığın için System Center Yönetim Paketi tek bir Operations Manager örneği birden çok Azure yığın dağıtım izlemenize olanak sağlar. Yönetim Paketi, Azure yığın ile iletişim kurmak için sistem durumu kaynak sağlayıcısı ve güncelleştirme kaynak sağlayıcısı REST API'lerini kullanır. Donanım yaşam döngüsü konakta çalışan yazılım izleme OEM geçiş yapmayı planlıyorsanız, fiziksel sunucuları izlemek için satıcı yönetim paketleri yükleyebilirsiniz. Operations Manager ağ aygıtı bulma, ağ anahtarları izlemek için de kullanabilirsiniz.

Azure yığını için yönetim paketi aşağıdaki yetenekleri sağlar:

- Birden çok Azure yığın dağıtımlarını yönetebilir
- Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) için destek
- Alabilir ve Uyarıları Kapat
- Bir sistem durumu ve kapasite Panosu
- Düzeltme eki ve güncelleştirme (P & U) olduğunda ediyor için otomatik Bakım modu algılama içerir
- Dağıtım ve bölge için zorla güncelleştirme görevleri içerir
- Bir bölgeye özel bilgiler ekleyebilirsiniz.
- Destekler bildirim ve Raporlama

System Center Yönetim Paketi için Microsoft Azure yığın ve ilişkili indirebilirsiniz [Kullanıcı Kılavuzu](https://www.microsoft.com/en-us/download/details.aspx?id=55184), ya da Operations Manager'dan doğrudan.

Bilet çözümü için Operations Manager System Center Service Manager ile tümleştirebilirsiniz. Tümleşik ürün Bağlayıcısı'nı bir hizmet isteği Hizmet Yöneticisi'nde çözdükten sonra Azure yığını ve Operations Manager bir uyarıyı kapatmak izin veren çift yönlü iletişim sağlar.

Aşağıdaki diyagram, mevcut bir System Center dağıtım ile Azure yığınının tümleştirme gösterir. Hizmet Yöneticisi daha fazla ile System Center Orchestrator veya Service Management Automation (SMA) Azure yığınında işlemlerini çalıştırmak için otomatik hale getirebilirsiniz.

![OM, Service Manager ve SMA ile tümleştirme gösteren diyagram.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Nagios ile tümleştirme

Eklentisi izleme Nagios izin veren ücretsiz yazılım lisansı altında – MIT (Massachusetts Teknoloji Enstitüsü'nün) kullanılabilir olan iş ortağı Cloudbase çözümleriyle birlikte geliştirilmiştir.

Eklenti Python içinde yazılmış ve sistem durumu kaynak sağlayıcısı REST API kullanır. Almak ve Azure yığın içindeki uyarıları Kapat temel işlevleri sunar. System Center Yönetim Paketi gibi birden çok Azure yığın dağıtımları eklemek ve bildirimleri göndermek için sağlar.

Eklenti Nagios Enterprise ve Nagios çekirdek ile çalışır. Karşıdan yükleyebileceğiniz [burada](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). İndirme site ayrıca yükleme ve yapılandırma ayrıntılarını içerir.

### <a name="plugin-parameters"></a>Eklentisi parametreleri

Eklenti dosyası "Azurestack_plugin.py" aşağıdaki parametrelerle yapılandırın:

| Parametre | Açıklama | Örnek |
|---------|---------|---------|
| *arm_endpoint* | Azure Kaynak Yöneticisi (Yönetici) uç noktası |https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Azure Kaynak Yöneticisi (Yönetici) uç noktası  | https://adminmanagement.local.azurestack.external |
| *Tenant_id* | Yönetici abonelik kimliği | Yönetici portalı veya PowerShell aracılığıyla Al |
| *User_name* | İşleç abonelik kullanıcı adı | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | İşleç abonelik parola | İstanbul |
| *client_id* | İstemci | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *Bölge* |  Azure yığın bölge adı | yerel |
|  |  |

* PowerShell sağlanan Evrensel GUID'dir. Her dağıtım için kullanabilirsiniz.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>İzleyici sistem durumu ve Uyarılar için PowerShell kullanın

Operations Manager, Nagios veya Nagios tabanlı bir çözümü kullanmıyorsanız, Azure yığın ile tümleştirmek için çözümlerini izleme geniş etkinleştirmek için PowerShell kullanabilirsiniz.

1. PowerShell kullanmak için bilgisayarınızda yüklü olduğundan emin olun [PowerShell yüklenmiş ve yapılandırılmış](azure-stack-powershell-configure-quickstart.md) Azure yığın işleci ortamı için. PowerShell Kaynak Yöneticisi (Yönetici) uç noktası ulaşabilir yerel bir bilgisayara yükleme (https://adminmanagement. [ Bölge]. [External_FQDN]).

2. Bir Azure yığın işleç olarak Azure yığın ortama bağlanmak için aşağıdaki komutları çalıştırın:

   ```PowerShell  
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Uyarılarla çalışma için aşağıdaki örneklerde olduğu gibi komutları kullanın:
   ```PowerShell
    #Retrieve all alerts
    Get-AzsAlert

    #Filter for active alerts
    $Active=Get-AzsAlert | Where {$_.State -eq "active"}
    $Active

    #Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    Get-AzsRPHealth

    #Retrieve infrastructure role instance health
    $FRPID=Get-AzsRPHealth|Where-Object {$_.DisplayName -eq "Capacity"}
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId

    ```

## <a name="learn-more"></a>Daha fazla bilgi edinin

Yerleşik sistem durumu izleme hakkında daha fazla bilgi için bkz: [sistem durumu ve Uyarıları Azure yığınında izleme](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik tümleştirme](azure-stack-integrate-security.md)
