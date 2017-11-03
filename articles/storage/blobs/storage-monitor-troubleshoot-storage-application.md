---
title: Azure bulut depolama uygulamada izleyebilir ve | Microsoft Docs
description: "Tanılama araçları, ölçümleri ve uyarı sorun giderme ve bir bulut uygulama izlemek için kullanın."
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: db88c331f79d83e0124519f8b6dbb34514b456dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-troubleshoot-a-cloud-storage-application"></a>İzleme ve bulut depolama uygulama sorunlarını giderme

Bu öğretici dört ve bir dizi son bölümü parçasıdır. İzleme ve bulut depolama uygulama sorunlarını giderme hakkında bilgi edinin.

Bölümünde dizisinin dört öğrenin nasıl yapılır:

> [!div class="checklist"]
> * Günlüğe kaydetme ve ölçümleri Aç
> * Yetkilendirme hataları için uyarıları etkinleştir
> * Test trafiğini yanlış SAS belirteci ile Çalıştır
> * Karşıdan yükle ve günlüklerini analiz edin

[Azure depolama çözümlemeleri](../common/storage-analytics.md) için bir depolama hesabı oturum açma ve ölçüm verilerini sağlar. Bu veri depolama hesabınızın durumu fikir sağlar. Depolama hesabınızda görünürlük kullanılabilmesi için öncelikle, veri toplama ayarlamanız gerekir. Bu işlem günlüğü, ölçümleri yapılandırma ve Uyarıları etkinleştirme kapatma içerir.

Günlüğe kaydetme ve depolama hesapları arasından ölçümleri etkin gelen **tanılama** Azure portalında sekmesi. Ölçümleri iki tür vardır. **Birleşik** ölçümleri toplamak giriş/çıkış, kullanılabilirlik, gecikme ve başarı yüzdeleri. Bu ölçümler blob, kuyruk, tablo ve Dosya Hizmetleri için toplanır. **API başına** Azure depolama hizmeti API'si her depolama işlem ölçümlerini aynı kümesini toplar. Depolama günlüğe kaydetme, depolama hesabınızdaki hem başarılı hem başarısız istekler için kayıt ayrıntıları sağlar. Bu günlükler, okuma, yazma ve silme işlemlerinin Azure tabloları, kuyrukları ve blobları karşı ayrıntılarını görmek etkinleştirin. Bunlar ayrıca başarısız olan istekler zaman aşımları, azaltma ve yetkilendirme hataları gibi nedenlerle görmek etkinleştirin.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure portalı](https://portal.azure.com)’nda oturum açın

## <a name="turn-on-logging-and-metrics"></a>Günlüğe kaydetme ve ölçümleri Aç

Sol menüden seçin **kaynak grupları**seçin **myResourceGroup**ve ardından kaynak listesinde depolama hesabınızı seçin.

Altında **tanılama** ayarlamak **durum** için **üzerinde**. Olun **Blob toplama ölçümleri**, **API ölçümleri başına Blob**, ve **Blob günlükleri** etkinleştirilir.

Tamamlandığında, tıklayın **Kaydet**

![Tanılama bölmesi](media/storage-monitor-troubleshoot-storage-application/figure1.png)

## <a name="enable-alerts"></a>Uyarılarını etkinleştir

Uyarıları Yöneticiler e-posta veya bir eşik ihlal bir ölçümü tabanlı bir Web kancası tetiklemek için bir yol sağlar. Bu örnekte, bir uyarı için etkinleştirme `SASClientOtherError` ölçüm.

### <a name="navigate-to-the-storage-account-in-the-azure-portal"></a>Azure Portal'da depolama hesabı gidin

Sol menüden seçin **kaynak grupları**seçin **myResourceGroup**ve ardından kaynak listesinde depolama hesabınızı seçin.

Altında **izleme** bölümünde, select **uyarı kuralları**.

Seçin **+ uyarı Ekle**altında **uyarı kuralı eklemek**, gerekli bilgileri doldurun. Seçin `SASClientOtherError` gelen **ölçüm** açılır.

![Tanılama bölmesi](media/storage-monitor-troubleshoot-storage-application/figure2.png)

## <a name="simulate-an-error"></a>Hata benzetimi

Geçerli bir uyarı benzetimini yapmak için mevcut olmayan blob depolama hesabınızdan isteği deneyebilirsiniz. Bunu yapmak için yerini `<incorrect-blob-name>` var olmayan bir değerle. Aşağıdaki kod örneği birkaç kez çalıştırmak benzetimini yapmak için blob isteği başarısız oldu.

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <incorrect-blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<incorrect-blob-name>?$sasToken
```

Aşağıdaki resimde, önceki örnekle benzetimli hatası dayalı bir örnek uyarı çalıştırıldı ' dir.

 ![Örnek Uyarısı](media/storage-monitor-troubleshoot-storage-application/alert.png)

## <a name="download-and-view-logs"></a>Karşıdan yükleme ve görünüm günlükleri

Depolama günlükleri bir dizi blobun gruplandırılmasını adlı blob kapsayıcısında veri deposunda **$logs** depolama hesabınızdaki. Bu kapsayıcıdaki tüm blob kapsayıcıları hesabınızda listesinde ancak doğrudan erişim içeriğini görebilirsiniz gösterilmez.

Bu senaryoda, kullandığınız [Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) Azure storage hesabıyla etkileşime geçmek için.

### <a name="download-microsoft-message-analyzer"></a>Microsoft Message Analyzer'ı karşıdan yükle

Karşıdan [Microsoft Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) ve uygulamayı yükleyin.

Uygulamayı başlatın ve seçin **dosya** > **açık** > **diğer dosya kaynaklardan**.

İçinde **dosya Seçici** iletişim kutusunda **+ Azure Bağlantısı Ekle**. Girin, **depolama hesabı adı** ve **hesap anahtarı** tıklatıp **Tamam**.

![Microsoft Message Analyzer - Azure depolama bağlantı iletişim kutusu ekleme](media/storage-monitor-troubleshoot-storage-application/figure3.png)

Bağlandıktan sonra depolama ağaç kapsayıcılarında görünümünü için günlük BLOB'ları genişletin. En son günlük tıklatıp **Tamam**.

![Microsoft Message Analyzer - Azure depolama bağlantı iletişim kutusu ekleme](media/storage-monitor-troubleshoot-storage-application/figure4.png)

Üzerinde **yeni oturum** iletişim kutusunda, tıklatın **Başlat** , günlüğünü görüntülemek için.

Oturum açan sonra depolama olayları görüntüleyebilirsiniz. Aşağıdaki görüntüden gördüğünüz oluştu bir `SASClientOtherError` depolama hesabında tetiklendi. Depolama günlüğe kaydetme hakkında ek bilgi için ziyaret [depolama çözümlemeleri](../common/storage-analytics.md).

![Microsoft Message Analyzer - olayları görüntüleme](media/storage-monitor-troubleshoot-storage-application/figure5.png)

[Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) dahil olmak üzere, depolama hesapları ile etkileşim kurmak için kullanılan bir diğer araç **$logs** kapsayıcı ve içinde bulunan günlükleri.

## <a name="next-steps"></a>Sonraki adımlar

Dört bölümü ve seri son bölümü, izlemek ve nasıl gibi depolama hesabınız ilgili sorunları giderme hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Günlüğe kaydetme ve ölçümleri Aç
> * Yetkilendirme hataları için uyarıları etkinleştir
> * Test trafiğini yanlış SAS belirteci ile Çalıştır
> * Karşıdan yükle ve günlüklerini analiz edin

Önceden oluşturulmuş depolama örnekleri görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure depolama kod örnekleri](storage-samples-blobs-cli.md)