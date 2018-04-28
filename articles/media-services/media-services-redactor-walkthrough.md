---
title: Azure medya analizi kılavuza yüzeyleri Redaksiyon | Microsoft Docs
description: Bu konu Azure Media Services Gezgini (AMSE) ve Azure Media Redactor Görselleştirici (açık kaynak aracı) kullanarak tam Redaksiyon iş akışını çalıştırma hakkında adım adım yönergeler gösterir.
services: media-services
documentationcenter: ''
author: Lichard
manager: cfowler
editor: ''
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/03/2017
ms.author: rli; juliako;
ms.openlocfilehash: d287206d30c05289679c83636ba5f968a5fcbcbe
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Azure medya analizi kılavuza yüzeyleri Redaksiyon

## <a name="overview"></a>Genel Bakış

**Azure Media Redactor** olan bir [Azure medya analizi](media-services-analytics-overview.md) medya işlemcisi (MP) ölçeklenebilir yüz Redaksiyon bulutta sunar. Yüz Redaksiyon seçili kişiler yüzeyleri ölçeklendirilmelidir için videonuzu değiştirmenizi sağlar. Genel güvenlik ve haber medya senaryolarda yüz Redaksiyon hizmeti kullanmak isteyebilirsiniz. Birden çok yüzeyleri içeren çekimi birkaç dakika el ile Redaksiyon saat sürebilir, ancak bu hizmet ile birkaç basit adımla yüz Redaksiyon işlem gerektirir. Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

Hakkındaki ayrıntılar için **Azure medya Redactor**, bkz: [yüz Redaksiyon genel bakış](media-services-face-redaction.md) konu.

Bu konu Azure Media Services Gezgini (AMSE) ve Azure Media Redactor Görselleştirici (açık kaynak aracı) kullanarak tam Redaksiyon iş akışını çalıştırma hakkında adım adım yönergeler gösterir.

Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/redaction-preview-available-globally) blogu.

## <a name="azure-media-services-explorer-workflow"></a>Azure Media Services Gezgini iş akışı

Redactor ile çalışmaya başlamak için en kolay yolu, github'da açık kaynaklı AMSE aracını kullanmaktır. Basitleştirilmiş bir iş akışı aracılığıyla çalıştırabilirsiniz **birleştirilmiş** ek açıklama json veya yüz jpg görüntüleri erişim gerekmiyorsa modu.

### <a name="download-and-setup"></a>Karşıdan yükleme ve Kurulum

1. AMSE aracını indirin [burada](https://github.com/Azure/Azure-Media-Services-Explorer).
1. Media Services hesabınıza hizmet anahtarınızı kullanarak oturum açın.

    Hesap adını ve anahtar bilgilerini almak için [Azure portalına](https://portal.azure.com/) gidin ve AMS hesabınızı seçin. Ardından, ayarları seçin > anahtarları. Anahtarları yönet pencerelerinde hesap adı gösterilir ve birincil anahtar ile ikincil anahtar görüntülenir. Hesap adı ve birincil anahtar değerlerini kopyalayın.

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>İlk geçirmek – modu Çözümle

1. Karşıya yükleme, ortam dosyası aracılığıyla varlığını –> karşıya yükleme, ya da sürükle ve bırak aracılığıyla. 
1. Sağ tıklayın ve medya dosyanızı medya analizi kullanarak işlem Azure medya Redactor –> Çözümle modu –>. 


![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

Çıktı jpg algılanan her yazıtipinin yanı sıra, yüz konum verilerini içeren bir ek açıklamaları json dosyası içerir. 

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

### <a name="second-pass--redact-mode"></a>İkinci geçirmek – modu Redaksiyon

1. Özgün video Varlığınızı çıkışı ilk pass karşıya yükleyin ve birincil bir varlık ayarlayın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (İsteğe bağlı) Redaksiyon istediğiniz kimlikleri yeni satır ayrılmış listesini içeren bir 'Dance_idlist.txt' dosyasını karşıya yükleyin. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (İsteğe bağlı) Sınırlama kutusu sınırlarını artırma gibi annotations.json dosyaya tüm düzenlemeleri yapın. 
4. İlk geçişi çıkış varlığından sağ tıklayın, Redactor seçin ve çalıştırmalarına **Redact** modu. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Karşıdan yükleyin ya da son Redaksiyonu yapılmış çıkış varlığına paylaşabilirsiniz. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

## <a name="azure-media-redactor-visualizer-open-source-tool"></a>Azure Media Redactor Görselleştirici açık kaynak aracı

Bir açık kaynak [Görselleştirici aracı](https://github.com/Microsoft/azure-media-redactor-visualizer) ayrıştırma ve çıktı kullanarak ek açıklama biçimi yalnızca başlayarak geliştiricilere yardımcı olmak için tasarlanmıştır.

Projeyi çalıştırmak için depoyu kopyalama sonra gelen FFMPEG indirme gerekir kendi [resmi sitesi](https://ffmpeg.org/download.html).

JSON ek açıklama verilerini ayrıştırma çalışılırken bir geliştirici iseniz içinde Models.MetaData için örnek kod örnekleri arayın.

### <a name="set-up-the-tool"></a>Aracı ayarlamak

1.  İndirin ve tüm çözümü oluşturun. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Gelen FFMPEG karşıdan [burada](https://ffmpeg.org/download.html). Bu projede statik bağlama ile sürüm be1d324 (2016-10-04) ile ilk olarak geliştirilmiştir. 
3.  Ffmpeg.exe ve ffprobe.exe AzureMediaRedactor.exe aynı çıktı klasöre kopyalayın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. AzureMediaRedactor.exe çalıştırın. 

### <a name="use-the-tool"></a>Aracını kullanma

1. Azure Media Services hesabınızla Redactor MP Çözümle modunu, videoda işleyin. 
2. Özgün video dosyası ve Redaksiyon çıktısını yükle - iş analiz edin. 
3. Görselleştirici uygulamayı çalıştırın ve yukarıdaki dosyaları seçin. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Dosyanızı önizlemede. Sağ kenar aracılığıyla ölçeklendirilmelidir istediğiniz hangi yüzeyleri seçin. 
    
    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  Alt metin alanı kimlikleri yüz ile güncelleştirir. "İdlist.txt" adlı bir dosya Bu kimliklerle yeni satır ayrılmış liste olarak oluşturun. 

    >[!NOTE]
    > İdlist.txt ANSI kaydedilmesi gerekir. ANSI kaydetmek için Not Defteri'ni kullanabilirsiniz.
    
6.  Bu dosya, adım 1'den çıkış varlığına karşıya yükleyin. Bu varlık için özgün video karşıya yükleyin ve birincil varlık ayarlayın. 
7.  Redaksiyon işi Redaksiyon son video almak için bu varlık üzerinde "Redact" modu ile çalıştırın. 

## <a name="next-steps"></a>Sonraki adımlar 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Azure medya analizi için yüz Redaksiyon Duyurusu](https://azure.microsoft.com/blog/azure-media-redactor/)
