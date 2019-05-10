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
ms.openlocfilehash: b04bd57c66b52e9a2c14d43c9e89d7c54fc48ae2
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415830"
---
# <a name="azure-monitor-application-change-analysis-public-preview"></a>Azure İzleyici uygulama değişikliği analizi (genel Önizleme)

Canlı site sorun/kesinti oluştuğunda, kök nedenini kolayca belirlemek önemlidir. Standart izleme çözümleri hızlı bir şekilde bir sorun yoktur ve genellikle bile hangi bileşeni belirlemenize yardımcı. Ancak bu her zaman hata oluşup neden için hemen bir açıklama neden olmaz. Şimdi bunu bozduğunu siteniz beş dakika önce eşitlendi, çalışmıştır. Son beş dakika içinde değişiklikler? Azure İzleyici'de uygulama değişikliği analizi karşılamak üzere tasarlanmış yeni özellik soru budur. Gücünü oluşturma tarafından [Azure kaynak Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) uygulama değişikliği analizi observability artırmak ve MTTR (ortalama süresi için onarım) azaltmak için Azure App Service uygulama değişikliklerinizi Öngörüler sağlar.

> [!IMPORTANT]
> Azure İzleyici uygulama değişikliği analizi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-do-i-use-application-change-analysis"></a>Uygulama değişikliği analizi nasıl kullanabilirim?

Azure İzleyici uygulama değişikliği analizi şu anda Self Servis yerleşik **Tanıla ve problemleri çözmenize** deneyimi, gelen erişilebilen **genel bakış** bölümde, Azure App Service Uygulama:

![Ekran Azure App Service'e genel bakış genel bakış düğmeyi etrafında kırmızı kutuları sayfası ve tanılamada ve sorunları düğmesi](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis"></a>Değişiklik analizini etkinleştirme

1. Seçin **kullanılabilirlik ve performans**

    ![Kullanılabilirlik ve performans sorunlarını giderme seçenekleri ekran görüntüsü](./media/change-analysis/availability-and-performance.png)

2. Tıklayın **uygulama kilitlenmeleri** Döşe.

   ![Uygulama kilitlenmeleri kutucuk içeren ekran görüntüsü](./media/change-analysis/application-crashes-tile.png)

3. Etkinleştirmek için **değişiklik analiz** seçin **şimdi etkinleştirmek**.

   ![Kullanılabilirlik ve performans sorunlarını giderme seçenekleri ekran görüntüsü](./media/change-analysis/application-crashes.png)

4. Tam avantajlarından yararlanmak için analiz işlevsellik kümesini değiştirmek **değiştirme analiz**, **kod değişikliklerini tara**, ve **her zaman** için **üzerinde** seçip **Kaydet**.

    ![Azure App Service etkinleştir değişiklik analiz kullanıcı arabiriminin ekran görüntüsü](./media/change-analysis/change-analysis-on.png)

    Varsa **değişiklik analiz** olan etkin, kaynak düzeyi değişikliklerini algılamak mümkün olmayacak. Varsa **kod değişikliklerini tara** olan etkin, ayrıca dağıtım dosyaları görebilir ve site yapılandırma değişikliklerini. Etkinleştirme **her zaman** performans tarama değişiklik en iyi duruma getirir, ancak fatura açısından ek ücrete neden.

5.  Her şeyi etkinleştirildikten sonra seçerek **Tanıla ve problemleri çözmenize** > **kullanılabilirliğini ve performansını** > **uygulama kilitlenmeleri** değişiklik analizi deneyimine erişmenize olanak sağlar. Grafiğin zaman ayrıntılarıyla birlikte bu değişikliklerle ilgili gerçekleşen değişikliklerin türünü summerize:

     ![Değişiklik fark görünümünün ekran görüntüsü](./media/change-analysis/change-view.png)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unable-to-fetch-change-analysis-information"></a>Değişiklik çözümleme bilgisi alınamıyor.

Bu, geçerli Önizleme ekleme deneyimi ile geçici bir sorundur. Geçici çözüm, gizli etiket web uygulamanızı ayarlama ve sonra sayfayı yenilemeyi oluşur:

   ![Değişiklik gizli etiket ekran görüntüsü](./media/change-analysis/hidden-tag.png)

PowerShell kullanarak gizli etiket ayarlamak için:

1. Azure Cloud Shell'i açın:

    ![Azure Cloud Shell değişimin ekran görüntüsü](./media/change-analysis/cloud-shell.png)

2. Kabuk türü için PowerShell değiştirin:

   ![Azure Cloud Shell değişimin ekran görüntüsü](./media/change-analysis/choose-powershell.png)

3. Şu komutu çalıştırın:

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
