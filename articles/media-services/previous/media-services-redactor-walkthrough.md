---
title: Azure medya analizi gözden geçirme ile yüzleri özgürlüğü | Microsoft Docs
description: Bu konuda, Azure Media Services Gezgini (AMSE) ve Azure Media Redactor Görselleştirici (açık kaynak aracı) kullanarak tam Redaksiyon iş akışı çalıştırma hakkında adım adım yönergeler gösterilmektedir.
services: media-services
documentationcenter: ''
author: Lichard
manager: femila
editor: ''
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: rli; juliako;
ms.openlocfilehash: 3e4844c3174e41ca7f6f5667a2777aba11f70f11
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60875081"
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Azure medya analizi gözden geçirme ile yüzleri özgürlüğü

## <a name="overview"></a>Genel Bakış

**Azure Media Redactor** olduğu bir [Azure medya analizi](media-services-analytics-overview.md) medya işlemci (MP) bulutta ölçeklenebilir yüz flulaştırma sunar. Yüz flulaştırma seçilen kişilerin yüzlerini bulanıklaştıran için videonuzu değiştirmenize olanak sağlar. Yüz flulaştırma hizmet kamu güvenliği ve haber medya senaryolarında kullanmak isteyebilirsiniz. El ile özgürlüğü saat birden fazla yüzeye içeren görüntülerini, birkaç dakika sürebilir, ancak bu hizmet ile yalnızca birkaç basit adımda yüz flulaştırma işlemi gerektirir. Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

Hakkındaki ayrıntılar için **Azure Media Redactor**, bkz: [yüz flulaştırma genel bakış](media-services-face-redaction.md) konu.

Bu konuda, Azure Media Services Gezgini (AMSE) ve Azure Media Redactor Görselleştirici (açık kaynak aracı) kullanarak tam Redaksiyon iş akışı çalıştırma hakkında adım adım yönergeler gösterilmektedir.

Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/redaction-preview-available-globally) blogu.

## <a name="azure-media-services-explorer-workflow"></a>Azure Media Services Gezgini iş akışı

Redactor ile başlamanın en kolay yolu, GitHub üzerinde açık kaynak AMSE aracını kullanmaktır. Basitleştirilmiş bir iş akışı aracılığıyla çalıştırabileceğiniz **birleştirilmiş** ek açıklama json ya da yüz jpg görüntüleri erişim gerekmiyorsa modu.

### <a name="download-and-setup"></a>Karşıdan yükleme ve Kurulum

1. AMSE aracını indirin [burada](https://github.com/Azure/Azure-Media-Services-Explorer).
1. Hizmet anahtarınızı kullanarak Media Services hesabınız için oturum açın.

    Hesap adını ve anahtar bilgilerini almak için [Azure portalına](https://portal.azure.com/) gidin ve AMS hesabınızı seçin. Sonra ayarlarını seçin > anahtarlar. Anahtarları yönet pencerelerinde hesap adı gösterilir ve birincil anahtar ile ikincil anahtar görüntülenir. Hesap adı ve birincil anahtar değerlerini kopyalayın.

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>İlk başarılı – modu analiz edin

1. Medya dosyası aracılığıyla varlığını karşıya yükleme –> karşıya yükleme, ya da sürükle ve bırak ile. 
1. Sağ tıklayın ve medya analizi kullanarak medya dosyanızı işlem Azure Media Redactor –> –> Çözümle modu. 


![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

Çıkış algılanan her yüz jpg yanı sıra yüz konumu verilerle bir ek açıklamaları json dosyası içerir. 

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

### <a name="second-pass--redact-mode"></a>İkinci geçirmek – modu özgürlüğü

1. Özgün video Varlığınızı çıkışa ilk pass karşıya yükleyin ve bir birincil varlık ayarlayın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (İsteğe bağlı) Özgürlüğü istiyor kimliğinin bir yeni satır virgülle ayrılmış listesini içeren bir 'Dance_idlist.txt' dosyasını karşıya yükleyin. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (İsteğe bağlı) Annotations.json dosyanın sınırlama kutusu sınırlarını artırma gibi tüm düzenlemeleri yapın. 
4. İlk geçişinde çıkış varlığından sağ tıklayın, Redactor seçin ve çalıştırın **Redact** modu. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. İndirme veya Nihai sonuç kısaltılmıştır çıktı varlığı paylaşın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

## <a name="azure-media-redactor-visualizer-open-source-tool"></a>Azure Media Redactor görselleştiricisi açık kaynak aracı

Bir açık kaynak [görselleştiricisi araç](https://github.com/Microsoft/azure-media-redactor-visualizer) ayrıştırma ve çıkış kullanarak ek açıklamalar biçimiyle yeni başlayan geliştiricilerin yardımcı olmak için tasarlanmıştır.

Projeyi çalıştırmak için depo kopyalama sonra gelen FFMPEG indirmek gerekir, [resmi sitesi](https://ffmpeg.org/download.html).

JSON ek açıklama verilerini Ayrıştırılmaya çalışılırken bir geliştiriciyseniz, örnek kod örnekleri için Models.MetaData içinde arayın.

### <a name="set-up-the-tool"></a>Aracı'nı ayarlama

1.  İndirin ve tüm Çözümü derleyin. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  FFMPEG'nden indirin [burada](https://ffmpeg.org/download.html). Bu proje, ilk statik bağlama ile sürüm be1d324 (2016-10-04) ile geliştirilmiştir. 
3.  Ffmpeg.exe ve ffprobe.exe AzureMediaRedactor.exe aynı çıktı klasöre kopyalayın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. AzureMediaRedactor.exe çalıştırın. 

### <a name="use-the-tool"></a>Aracını kullanma

1. Videonuzu analiz modunu Redactor MP ile Azure Media Services hesabınızda işlem. 
2. Hem özgün video dosyası ve Redaksiyon çıktısını indirin - iş analiz edin. 
3. Görselleştirici uygulamayı çalıştırın ve yukarıdaki dosyaları seçin. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Dosyanızı önizlemesini görüntüleyin. Sağ kenar aracılığıyla bulanıklaştıran istediğiniz hangi yüzleri seçin. 
    
    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  Alt metin alanı kimlikleri yüz tanıma güncelleştirir. "İdlist.txt" adlı bir dosya ile bu kimliklerinin yeni satır virgülle ayrılmış liste olarak oluşturun. 

    >[!NOTE]
    > ANSI idlist.txt kaydedilmesi gerekir. Not Defteri'ni ANSI kaydetmek için kullanabilirsiniz.
    
6.  Bu dosya, adım 1'den çıktı varlığına karşıya yükleyin. Bu varlık için özgün videoyu karşıya yükleme ve birincil varlık ayarlayın. 
7.  En son Redaksiyon video almak için bu varlık üzerinde "Redact" moduyla Redaksiyon işi çalıştırın. 

## <a name="next-steps"></a>Sonraki adımlar 

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md)

[Azure medya analizi tanıtımları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Azure medya analizi için yüz Flulaştırma ile tanışın](https://azure.microsoft.com/blog/azure-media-redactor/)
