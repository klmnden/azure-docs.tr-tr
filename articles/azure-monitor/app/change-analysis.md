---
title: Uygulama değişikliği analizi, web uygulaması sorunlarını bulmak için Azure İzleyici'de kullanın. | Microsoft Docs
description: Uygulama değişikliği analizi, Azure İzleyici'de canlı sitelerde Azure App Service üzerindeki uygulama sorunlarını gidermek için kullanın.
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: 45df8f9e57223ea60a11c6af2187d362184cae2b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443363"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Azure İzleyici'de uygulama değişikliği analizi (Önizleme) kullanma

Canlı site sorun veya kesinti oluştuğunda, hızlı bir şekilde kök nedeni belirlemek önemlidir. Standart izleme çözümleri için sorun uyarı gönderebilir. Bunlar bile hangi bileşenin başarısız olduğunu gösteriyor olabilir. Ancak, bu uyarı, hemen her zaman hata'nın neden açıklayan olmaz. Siteniz beş dakika önce çalışan ve artık bozduğunu bildiğiniz. Son beş dakika içinde değişiklikler? Bu soru ise, uygulama değişikliği analizi, Azure İzleyici'de yanıtlamak için tasarlanmıştır.

Gücünü oluşturmaya [Azure kaynak Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), değişiklik analiz observability artırmak ve MTTR (onarmak için saati) azaltmak için Azure uygulama değişikliklerinizi dair Öngörüler sağlar.

> [!IMPORTANT]
> Değişiklik analizi şu anda Önizleme aşamasındadır. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi sağlanmaktadır. Bu sürüm, üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor veya kısıtlı yeteneklere sahip. Daha fazla bilgi için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Genel Bakış

Değişiklik analiz, altyapı katmanını uygulama dağıtımı için tüm değişikliklerden çeşitli türlerde algılar. Kaynak abonelik değişikliklerde abonelik düzeyinde Azure kaynak sağlayıcısıdır. Kullanıcıların ne değişiklikler anlamalarına yardımcı olmak çeşitli tanılama araçları için veri sorunları neden olabilecek değişiklik analizini sağlar.

Aşağıdaki diyagramda, değişiklik analiz mimarisi gösterilmektedir:

![Değiştirme analizi nasıl değişiklik verilerini alır ve istemci araçları sağlar mimarisi diyagramı](./media/change-analysis/overview.png)

Şu anda değişiklik analiz içinde tümleşik olarak **Tanıla ve problemleri çözmenize** App Service web uygulamasında deneyimi. Değişiklik algılama etkinleştirmek ve web uygulamasında değişiklikleri görüntülemek için bkz: *Web Apps özelliğini değiştirmek analize* bu makalenin devamındaki bölümüne.

### <a name="azure-resource-manager-deployment-changes"></a>Azure Resource Manager dağıtım değişiklikleri

Kullanarak [Azure kaynak Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), uygulamanızı barındıran Azure kaynaklarını zamanla nasıl değiştiğini geçmiş kaydını değişiklik analizini sağlar. Değişiklik analiz algılayabilir, örneğin, IP yapılandırma kuralları, yönetilen kimlikleri ve SSL ayarları değiştirir. Bu nedenle bir web uygulaması için bir etiket eklenirse, analiz değiştirme değişikliği yansıtır. Bu bilgi kullanılabilir olduğu sürece `Microsoft.ChangeAnalysis` kaynak sağlayıcısı, Azure aboneliğinde etkinleştirilir.

### <a name="changes-in-web-app-deployment-and-configuration"></a>Web uygulaması dağıtma ve yapılandırma değişiklikleri

Değişiklik analiz 4 saatte bir uygulama dağıtımını ve yapılandırmasını durumunu yakalar. Bu, örneğin, uygulama ortam değişkenleri içindeki değişiklikleri algılayabilir. Araç farkları hesaplar ve nelerin değiştiğini gösterir. Resource Manager değişiklikleri, kod dağıtım bilgilerini değiştirme aracı hemen kullanılamayabilir. Değişiklik analizi en son değişiklikleri görüntülemek için seçin **tarama değişiklikleri şimdi**.

!["Artık tarama değişiklikleri" düğmesinin Ekran görüntüsü](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Bağımlılık değişiklikleri

Kaynak bağımlılıkları değişiklikler bir web uygulamasında sorunlara neden olabilir. Örneğin, bir web uygulamasını Redis önbelleğine çağırırsa, Redis önbelleği SKU web uygulaması performansını etkileyebilir. Bağımlılıkları değişikliklerini algılamak için web uygulamanızın DNS kaydı değişiklik analiz denetler. Bu şekilde, bu değişiklikleri sorunlarına neden olabileceği tüm uygulama bileşenleri tanımlar.

## <a name="change-analysis-for-the-web-apps-feature"></a>Web Apps özelliği için değişiklik analizi

Azure İzleyici'de değişiklik analiz Self Servis şu anda yerleşik **tanılayın ve sorunlarını çözmek** deneyimi. Bu deneyimden erişim **genel bakış** App Service uygulamanızı sayfası.

!["Genel bakış" düğmesinin Ekran görüntüsü ve "tanılama ve sorun çözme" düğmesi](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Tanılama içinde değişiklik analizini etkinleştirme ve aracı sorunları çözün

1. Seçin **kullanılabilirliğini ve performansını**.

    !["Kullanılabilirlik ve sorun giderme seçenekleri performans" ekran görüntüsü](./media/change-analysis/availability-and-performance.png)

1. Seçin **uygulama değişiklikleri**. Bu özellik ayrıca içinde kullanılabilir değil **uygulama kilitlenmeleri**.

   !["Uygulama kilitlenmeleri" düğmesinin Ekran görüntüsü](./media/change-analysis/application-changes.png)

1. Değişiklik analiz etkinleştirmek için seçin **şimdi etkinleştirmek**.

   !["Uygulama kilitlenmeleri" seçeneklerinin ekran görüntüsü](./media/change-analysis/enable-changeanalysis.png)

1. Açma **değişiklik analiz** seçip **Kaydet**.

    !["Değişiklik analizini etkinleştir" kullanıcı arabiriminin ekran görüntüsü](./media/change-analysis/change-analysis-on.png)


1. Değişiklik analiz erişmek için seçin **Tanıla ve problemleri çözmenize** > **kullanılabilirliğini ve performansını** > **uygulama kilitlenmeleri**. Bu değişiklikler hakkında ayrıntılı bilgi ile birlikte zaman içinde tür değişiklikler özetleyen bir grafik görürsünüz:

     ![Değişiklik fark görünümünün ekran görüntüsü](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Uygun ölçekte değişiklik analizini etkinleştirme

Aboneliğinizi çok sayıda web uygulaması içeriyorsa, web uygulamasının düzeyinde hizmetini etkinleştirme verimsiz olabilir. Bu durumda, bu diğer yönergeleri izleyin.

### <a name="register-the-change-analysis-resource-provider-for-your-subscription"></a>Aboneliğiniz için değişiklik analiz kaynak sağlayıcısını kaydetme

1. Değişiklik analiz özellik bayrağını (Önizleme) kaydedin. Özellik bayrağı Önizleme aşamasında olduğundan, aboneliğinize görebilmesi için kaydetmeniz gerekir:

   1. [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)'i açın.

      ![Cloud Shell değişimin ekran görüntüsü](./media/change-analysis/cloud-shell.png)

   1. Kabuk türe çeviremezsiniz **PowerShell**.

      ![Cloud Shell değişimin ekran görüntüsü](./media/change-analysis/choose-powershell.png)

   1. Aşağıdaki PowerShell komutunu çalıştırın:

        ``` PowerShell
        Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to enable
        Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
        Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag
        ```

1. Abonelik için değişiklik analiz kaynak sağlayıcısını kaydedin.

   - Git **abonelikleri**ve değişiklik hizmeti etkinleştirmek istediğiniz aboneliği seçin. Ardından kaynak sağlayıcılarını seçin:

        ![Değişiklik analiz kaynak sağlayıcısını kaydetme gösteren ekran görüntüsü](./media/change-analysis/register-rp.png)

       - Seçin **Microsoft.ChangeAnalysis**. Sayfanın üst kısmında seçip **kaydetme**.

       - Kaynak sağlayıcısı etkinleştirildikten sonra dağıtım düzeyinde değişikliklerini algılamak için web uygulaması gizli etiket ayarlayabilirsiniz. Gizli bir etiket ayarlamak için yönergeleri uygulayın. **değişiklik analiz bilgileri getirilemiyor**.

   - Alternatif olarak, kaynak sağlayıcısını kaydetmek için bir PowerShell Betiği kullanabilirsiniz:

        ```PowerShell
        Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
        ```

        Bir web uygulaması gizli bir etiket ayarlamak için PowerShell kullanmak için aşağıdaki komutu çalıştırın:

        ```powershell
        $webapp=Get-AzWebApp -Name <name_of_your_webapp>
        $tags = $webapp.Tags
        $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
        Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
        ```

     > [!NOTE]
     > Gizli etiket ekledikten sonra değişiklikleri görmesini başlamadan önce en fazla 4 saat beklemeniz gerekebilir. Değişiklik analiz 4 saatte bir yalnızca web uygulamanızı taradığından sonuçları gecikir. 4 saatlik zamanlama taramanın performans etkisini sınırlar.

## <a name="next-steps"></a>Sonraki adımlar

- App Service izlemek daha etkili bir şekilde göre [Application Insights özelliklerini etkinleştirme](azure-web-apps.md) Azure İzleyici'de.
- Daha fazla bilgi edinin [Azure kaynak Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), güç değişiklik analiz yardımcı olur.
