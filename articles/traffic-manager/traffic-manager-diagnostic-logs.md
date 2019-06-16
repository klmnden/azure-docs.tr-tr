---
title: Azure Traffic Manager'da tanılama günlük kaydını etkinleştirme
description: Traffic Manager profilinizin için tanılama günlüğünü etkinleştirme ve sonuç olarak oluşturulan günlük dosyalarına erişmek hakkında bilgi edinin.
services: traffic-manager
author: asudbring
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: allensu
ms.openlocfilehash: b2ebeb41e69b7edfd43c38cc3b828069a1b3401a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071240"
---
# <a name="enable-diagnostic-logging-in-azure-traffic-manager"></a>Azure Traffic Manager'da tanılama günlük kaydını etkinleştirme

Bu makalede, bir Traffic Manager profili için tanılama günlük kaydı ve erişim günlüğü verileri etkinleştirmeyi açıklar.

Azure Traffic Manager tanılama günlükleri, Traffic Manager profili kaynak davranışını Öngörüler sağlayabilir. Örneğin, profilin günlük verilerini neden ayrı bir araştırmalarla bir uç noktasına karşı zaman aşımına uğradı belirlemek için kullanabilirsiniz.

## <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell bilgisayarınızdan çalıştırırsanız, Azure PowerShell modülü, 1.0.0 gerekir veya üzeri. Çalıştırabileceğiniz `Get-Module -ListAvailable Az` yüklü sürümü bulmak için. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Login-AzAccount` için Azure'da oturum açın.

1. **Traffic Manager profilini Al:**

    Tanılama günlük kaydını etkinleştirmek için bir Traffic Manager profili kimliği gerekir. Tanılama ile günlük etkinleştirmek istediğiniz bir Traffic Manager profili almak [Get-AzTrafficManagerProfile](/powershell/module/az.TrafficManager/Get-azTrafficManagerProfile). Çıktı, Traffic Manager profilinin kimlik bilgilerini içerir.

    ```azurepowershell-interactive
    Get-AzTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Traffic Manager profili için tanılama günlüğüne kaydetmeyi etkinleştir:**

    Traffic Manager profili önceki adımda elde edilen kimliği kullanarak için tanılama günlüğünü etkinleştirme [kümesi AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting?view=latest). Aşağıdaki komut, belirtilen Azure depolama hesabı için Traffic Manager profili için ayrıntılı günlükleri depolar. 

      ```azurepowershell-interactive
    Set-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Tanılama ayarlarını doğrulayın:**

      Traffic Manager profilini kullanarak tanılama ayarlarını doğrulayın [Get-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/get-azdiagnosticsetting?view=latest). Aşağıdaki komut, bir kaynak için oturum açmış olan kategorileri görüntüler.

     ```azurepowershell-interactive
     Get-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
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
|EndpointName|String|Sistem durumu kaydedilen Traffic Manager uç noktası adı.|*myPrimaryEndpoint*|
|Durum|String|Araştırılan Traffic Manager uç nokta sistem durumu. Durum ya da olabilir **yukarı** veya **aşağı**.|**Ayarlama**|
|||||

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Traffic Manager izleme](traffic-manager-monitoring.md)

