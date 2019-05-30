---
title: Azure İzleyici uygulamasını değiştirin analizi - Canlı site sorunları/Azure İzleyici uygulama kesintilere etkileyebilecek bir değişiklik analiz keşfedin | Microsoft Docs
description: Azure İzleyici uygulama değişikliği Analizi ile Azure App Services'ta uygulama Canlı site sorunları giderme
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: 5bd3816e65398283de85b4551a137b3f97db4cc7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226323"
---
# <a name="application-change-analysis-public-preview"></a>Uygulama değişikliği analizi (genel Önizleme)

Canlı site sorun/kesinti oluştuğunda, kök nedenini kolayca belirlemek önemlidir. Standart izleme çözümleri hızlı bir şekilde bir sorun yoktur ve genellikle bile hangi bileşeni belirlemenize yardımcı. Ancak bu her zaman hata oluşup neden için hemen bir açıklama neden olmaz. Şimdi bunu bozduğunu siteniz beş dakika önce eşitlendi, çalışmıştır. Son beş dakika içinde değişiklikler? Azure İzleyici'de uygulama değişikliği analizi karşılamak üzere tasarlanmış yeni özellik soru budur. Gücünü oluşturma tarafından [Azure kaynak Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) uygulama değişikliği analizi observability artırmak ve MTTR (ortalama süresi için onarım) azaltmak için Azure uygulama değişikliklerinizi Öngörüler sağlar.

> [!IMPORTANT]
> Azure İzleyici uygulama değişikliği analizi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview-of-change-analysis-service"></a>Değişiklik analizi hizmetine genel bakış
Değiştirme analiz hizmeti, çeşitli uygulama dağıtımı için tüm altyapı katmandan değişiklikleri algılar. Bu Abonelikteki kaynak değişiklikleri içine arar ve sorunları neden olabilecek kullanıcıların ne değişiklikler anlamalarına yardımcı olmak çeşitli tanılama araçları için veri sağlayan bir abonelik düzeyi Azure kaynak sağlayıcısıdır.

Aşağıdaki diyagramda değiştirme analiz hizmeti mimarisi gösterilmektedir: ![Değiştirme analiz hizmeti nasıl alacağını için Mimari diyagramı verileri değiştiren ve istemci araçları verilerini sağlamak](./media/change-analysis/overview.png)

Şu anda aracı uygulaması web uygulaması deneyimi sorunları tanılayıp hizmetlerine tümleşiktir. Bkz: *App Services Web uygulaması için değişiklik analiz hizmetini* bölümünü etkinleştirmek ve bir web uygulaması için yapılan değişiklikleri görmek.

### <a name="azure-resource-manager-deployment-changes"></a>Azure Resource Manager dağıtım değişiklikleri
Yararlanarak [Azure kaynak Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) değişiklik analiz aracı uygulamanızı barındıran Azure kaynaklarını zamanla nasıl değiştiğini geçmiş kaydını sağlar. Örneğin, bir web uygulaması olarak eklenmiş bir etiket varsa değişiklik analiz Aracı'nda değişiklik yansıtılır.
Bu bilgileri her zaman kullanılabilir olduğu sürece `Microsoft.ChangeAnalysis` kaynak sağlayıcısı olan Azure aboneliğine eklenmedi.

### <a name="web-application-deployment-and-configuration-changes"></a>Web uygulama dağıtım ve yapılandırma değişiklikleri
Değişiklik analiz aracı farkları hesaplamak ve nelerin değiştiğini göstermek için 4 saatte bir uygulama dağıtımını ve yapılandırmasını durumunu yakalar. Uygulama ortam değişkeni değişiklikleri, IP yapılandırması kurallarında bir değişiklik, yönetilen hizmet kimliği değişiklikler, SSL ayarları değişiklikleri vb. değişiklikler örnekleridir.
Resource Manager değişiklikleri, bu tür değişiklik bilgiler aracı hemen kullanılamayabilir. En son değişiklikleri görüntülemek için Aracı'nda 'Değişiklikleri şimdi Tara' düğmesini kullanın.

![Ekran görüntüsü değişiklikler için tarama artık içinde Tanıla düğmesine ve app service web uygulaması için değişiklik analiz tümleştirmesi aracıyla sorunları çözün](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Bağımlılık değişiklikleri
Bağımlılıkları kaynak da sorunların neden olabilir. Örneğin, bir web uygulamasını Redis önbelleğine çağırırsa, web uygulaması performansını SKU Redis önbelleği tarafından etkilenebilir. Web uygulaması DNS kaydına bakarak hizmet sorunları neden olan bir uygulamanın tüm bileşenleri değişiklikleri belirlemek için değişiklik bilgileri de bağımlılıkları sunmak analiz değiştirin.


## <a name="change-analysis-service-for-app-services-web-app"></a>App Services Web uygulaması için Analiz Hizmetini değiştirin

Azure İzleyici uygulama değişikliği analizi şu anda Self Servis yerleşik **Tanıla ve problemleri çözmenize** deneyimi, gelen erişilebilen **genel bakış** bölümde, Azure App Service Uygulama:

![Ekran Azure App Service'e genel bakış genel bakış düğmeyi etrafında kırmızı kutuları sayfası ve tanılamada ve sorunları düğmesi](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-diagnose-and-solve-problems-tool"></a>Tanılama değişiklik analizi etkinleştirme ve aracı sorunları çözün

1. Seçin **kullanılabilirlik ve performans**

    ![Kullanılabilirlik ve performans sorunlarını giderme seçenekleri ekran görüntüsü](./media/change-analysis/availability-and-performance.png)

2. Tıklayın **uygulama kilitlenmeleri** Döşe.

   ![Uygulama kilitlenmeleri kutucuk içeren ekran görüntüsü](./media/change-analysis/application-crashes-tile.png)

3. Etkinleştirmek için **değişiklik analiz** seçin **şimdi etkinleştirmek**.

   ![Kullanılabilirlik ve performans sorunlarını giderme seçenekleri ekran görüntüsü](./media/change-analysis/application-crashes.png)

4. Tam avantajlarından yararlanmak için analiz işlevsellik kümesini değiştirmek **değiştirme analiz**, **kod değişikliklerini tara**, ve **her zaman** için **üzerinde** seçip **Kaydet**.

    ![Azure App Service etkinleştir değişiklik analiz kullanıcı arabiriminin ekran görüntüsü](./media/change-analysis/change-analysis-on.png)

    Varsa **değişiklik analiz** olan etkin, kaynak düzeyi değişikliklerini algılamak mümkün olmayacak. Varsa **kod değişikliklerini tara** olan etkin, ayrıca dağıtım dosyaları görebilir ve site yapılandırma değişikliklerini. Etkinleştirme **her zaman** performans tarama değişiklik en iyi duruma getirir, ancak fatura açısından ek ücrete neden.

5.  Her şeyi etkinleştirildikten sonra seçerek **Tanıla ve problemleri çözmenize** > **kullanılabilirliğini ve performansını** > **uygulama kilitlenmeleri** değişiklik analizi deneyimine erişmenize olanak sağlar. Grafiğin zaman ayrıntılarıyla birlikte bu değişikliklerle ilgili gerçekleşen değişikliklerin türünü özetlenir:

     ![Değişiklik fark görünümünün ekran görüntüsü](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-service-at-scale"></a>Uygun ölçekte değişiklik analiz hizmetini etkinleştirme
Çok sayıda web uygulamaları, aboneliğinizde varsa, Etkinleştirme hizmeti web uygulama düzeyi başına verimsiz olacaktır. Bazı alternatif ekleme yönergeleri aşağıda verilmiştir.

#### <a name="registering-change-analysis-resource-provider-for-your-subscription"></a>Aboneliğiniz için değişiklik analiz kaynak sağlayıcısı kaydediliyor

1. Değişiklik analiz Önizleme özellik bayrağı Kaydet

    Bu özellik Önizleme aşamasında olduğundan, ilk olarak aboneliğinize görünür olması için özellik bayrağı'ı kaydetmeniz gerekir.
    - [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)'i açın.

    ![Azure Cloud Shell değişimin ekran görüntüsü](./media/change-analysis/cloud-shell.png)

    - Kabuk türü için PowerShell değiştirin:

    ![Azure Cloud Shell değişimin ekran görüntüsü](./media/change-analysis/choose-powershell.png)

    - Aşağıdaki PowerShell komutunu çalıştırın:

    ``` PowerShell

    Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to onboard
    Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
    Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag

    ```

2. Abonelik için değişiklik analiz kaynak sağlayıcısını kaydetme

    - Aboneliklere Git, yerleşik değiştirme hizmeti istediğiniz aboneliği seçin ve ardından kaynak sağlayıcıları tıklayın:

        ![Abonelikler dikey penceresinden değişiklik analiz RP kaydetmek için ekran görüntüsü](./media/change-analysis/register-rp.png)

    - Seçin *Microsoft.ChangeAnalysis* tıklatıp *kaydetme* sayfanın üstündeki.

    - Kaynak sağlayıcısı eklendikten sonra yönergeleri izleyin *değiştirme analiz bilgileri getirilemiyor* aşağıda gizli etiket üzerinde web uygulaması dağıtımını etkinleştirmek için ayarlanacak düzeyini değiştirmek web uygulamasında algılama.

3. Bunun yerine 2. adıma üzerinde PowerShell Betiği aracılığıyla kaynak sağlayıcısı kaydedebilirsiniz:

    ```PowerShell
    Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
    ```

4. PowerShell kullanarak bir web uygulaması gizli bir etiket ayarlamak için aşağıdaki komutu çalıştırın:

    ```powershell
    $webapp=Get-AzWebApp -Name <name_of_your_webapp>
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
    ```

> [!NOTE]
> Gizli etiket eklendiğinde, başlangıçta ilk değişiklikleri görüntülemek için 4 saat beklemeniz gerekebilir. Bu, web uygulamanızı taramanın performans üzerindeki etkisini sınırlandırırken taramak için değişiklik analiz hizmetinin kullandığı 4 saat freqeuncy kaynaklanır.

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Services'ın İzleme özelliklerini geliştirip [Application Insights özelliklerini etkinleştirerek](azure-web-apps.md) Azure İzleyici.
- Anlayışınızı [Azure kaynak Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) power Azure İzleyici uygulama analizi değiştirme yardımcı olur.
