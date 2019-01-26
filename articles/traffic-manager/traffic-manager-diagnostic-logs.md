---
title: Azure Traffic Manager'da tanılama günlük kaydını etkinleştirme
description: Traffic Manager profilinizin için tanılama günlüğünü etkinleştirme ve sonuç olarak oluşturulan günlük dosyalarına erişmek hakkında bilgi edinin.
services: traffic-manager
author: KumudD
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: kumud
ms.openlocfilehash: abdc50d6d3d27ab7611994089345a997afc72cae
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55082560"
---
# <a name="enable-diagnostic-logging-in-azure-traffic-manager"></a>Azure Traffic Manager'da tanılama günlük kaydını etkinleştirme

Bu makalede, bir Traffic Manager profili için tanılama günlük kaydı ve erişim günlüğü verileri etkinleştirmeyi açıklar.

Azure Traffic Manager tanılama günlükleri, Traffic Manager profili kaynak davranışını Öngörüler sağlayabilir. Örneğin, profilin günlük verilerini neden ayrı bir araştırmalarla bir uç noktasına karşı zaman aşımına uğradı belirlemek için kullanabilirsiniz.

## <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell bilgisayarınızdan çalıştırırsanız, gereksinim duyduğunuz *AzureRM* PowerShell modülü, 6.13.1 veya üzeri. Çalıştırabileceğiniz `Get-Module -ListAvailable AzureRM` yüklü sürümü bulmak için. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/azurerm/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Login-AzureRmAccount` için Azure'da oturum açın.

1. **Traffic Manager profilini Al:**

    Tanılama günlük kaydını etkinleştirmek için bir Traffic Manager profili kimliği gerekir. Tanılama ile günlük etkinleştirmek istediğiniz bir Traffic Manager profili almak [Get-AzureRmTrafficManagerProfile](/powershell/module/AzureRM.TrafficManager/Get-AzureRmTrafficManagerProfile). Çıktı, Traffic Manager profilinin kimlik bilgilerini içerir.

    ```azurepowershell-interactive
    Get-AzureRmTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Traffic Manager profili için tanılama günlüğüne kaydetmeyi etkinleştir:**

    Traffic Manager profili önceki adımda elde edilen kimliği kullanarak için tanılama günlüğünü etkinleştirme [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/set-azurermdiagnosticsetting?view=latest). Aşağıdaki komut, belirtilen Azure depolama hesabı için Traffic Manager profili için ayrıntılı günlükleri depolar. 

      ```azurepowershell-interactive
    Set-AzureRmDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Tanılama ayarlarını doğrulayın:**

      Traffic Manager profilini kullanarak tanılama ayarlarını doğrulayın [Get-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermdiagnosticsetting?view=latest). Aşağıdaki komut, bir kaynak için oturum açmış olan kategorileri görüntüler.

     ```azurepowershell-interactive
     Get-AzureRmDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
     ```  
      Tüm kategoriler etkin olarak Traffic Manager profili kaynak görüntü ile ilişkili oturum emin olun. Ayrıca, depolama hesabı düzgün şekilde ayarlandığını doğrulayın.

## <a name="access-log-files"></a>Erişim günlük dosyaları
1. [Azure Portal](https://portal.azure.com) oturum açın. 
1. Portalda Azure depolama hesabınıza gidin.
2. Üzerinde **genel bakış** sayfa, Azure depolama hesabı, altında **Hizmetleri** seçin **Blobları**.
3. İçin **kapsayıcıları**seçin **ınsights günlükleri probehealthstatusevents**, aşağı PT1H.json dosyasına gidin ve tıklayın **indirin** indirin ve bu günlük bir kopyasını kaydedin dosya.

    ![Traffic Manager profilinizin blob depolamadan erişim günlük dosyaları](./media/traffic-manager-logs/traffic-manager-logs.png)


## <a name="traffic-manager-log-schema"></a>Traffic Manager günlüğü şeması

Azure İzleyici kullanılabilir tüm tanılama günlükleri esneklik her hizmet kendi olaylar için benzersiz özellikler yaymak için ortak bir üst düzey şema paylaşın. İçin şemayı en üst düzey tanılama günlükleri, [desteklenen hizmetler, şemalar ve kategoriler için Azure tanılama günlükleri](../azure-monitor/platform/tutorial-dashboards.md).

Aşağıdaki tablo, Azure Traffic Manager profili kaynağa özel günlükleri şema içerir.

|||||
|----|----|---|---|
|**Alan adı**|**Alan türü**|**Tanım**|**Örnek**|
|EndpointName|Dize|Sistem durumu kaydedilen Traffic Manager uç noktası adı.|*myPrimaryEndpoint*|
|Durum|Dize|Araştırılan Traffic Manager uç nokta sistem durumu. Durum ya da olabilir **yukarı** veya **aşağı**.|**Ayarlama**|
|||||

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Traffic Manager izleme](traffic-manager-monitoring.md)

