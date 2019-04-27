---
title: Azure veri kutusu Edge sorunlarını gidermek için Azure portalını kullanma | Microsoft Docs
description: Azure veri kutusu Edge sorunlarını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/15/2019
ms.author: alkohli
ms.openlocfilehash: 3a8d1f93930d2b298eeb7d10a73624b9a19bcc0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60756262"
---
# <a name="troubleshoot-your-azure-data-box-edge-issues"></a>Azure veri kutusu Edge sorunlarını giderme 

Bu makalede, Azure veri kutusu edge'de sorunlarını açıklar. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Tanılama çalıştırma
> * Destek paketi toplama
> * Sorun gidermek için günlükleri kullanma


## <a name="run-diagnostics"></a>Tanılama çalıştırma

Cihaz hatalarını tanılamak ve gidermek için tanılama testlerini çalıştırabilirsiniz. Tanılama testlerini çalıştırmak için cihazınızın yerel web arabiriminde aşağıdaki adımları gerçekleştirin.

1. Yerel web arabiriminde **Sorun giderme > Tanılama testleri** sayfasına gidin. Çalıştırmak istediğiniz testi seçip **Testi çalıştır** öğesine tıklayın. Bunu yaptığınızda ağ, cihaz, web proxy, saat veya bulut ayarlarınızdaki sorunların tanılanmasına yönelik testler çalıştırılır. Cihazın testleri çalıştırdığı bildirilir.

    ![Testleri seçin](media/data-box-edge-troubleshoot/run-diag-1.png)
 
2. Testler tamamlandıktan sonra sonuçlar görüntülenir. 

    ![Test sonuçlarını gözden geçirme](media/data-box-edge-troubleshoot/run-diag-2.png)

    Test başarısız olursa önerilen eylem URL'si gösterilir. URL'ye tıklayarak önerilen eylemi görüntüleyebilirsiniz.
 
    ![Başarısız testler için uyarıları gözden geçirin](media/data-box-edge-troubleshoot/run-diag-3.png)


## <a name="collect-support-package"></a>Destek paketi toplama

Günlük paketi, Microsoft Desteğinin cihaz sorunlarını giderme konusunda yardımcı olabilmesi için ihtiyaç duyabileceği günlüklerin tamamını içerir. Günlük paketi oluşturmak için yerel web arabirimini kullanabilirsiniz.

Destek paketi toplamak için aşağıdaki adımları gerçekleştirin. 

1. Yerel web arabiriminde **Sorun giderme > Destek** sayfasına gidin. **Destek paketi oluştur** öğesine tıklayın. Sistem, destek paketini toplamaya başlar. Paketin toplanması birkaç dakika sürebilir.

    ![Kullanıcı ekle'ye tıklayın](media/data-box-edge-troubleshoot/collect-logs-1.png)
 
2. Destek paketi oluşturulduktan sonra **Destek paketini indir**'e tıklayın. Seçtiğiniz yola .zip uzantılı bir paket indirilir. Paketi açıp sistem günlüğü dosyalarını görüntüleyebilirsiniz.

    ![Kullanıcı ekle'ye tıklayın](media/data-box-edge-troubleshoot/collect-logs-2.png)

## <a name="use-logs-to-troubleshoot"></a>Sorun gidermek için günlükleri kullanma

Yükleme ve yenileme işlemlerinde karşılaşılan hatalar ilgili hata dosyalarına yazılır.

1. Hata dosyalarını görüntülemek için paylaşımınıza gidin ve içeriği görüntülemek için paylaşıma tıklayın. 

      ![Bağlanın ve Paylaşım içeriğini görüntüleyin](media/data-box-edge-troubleshoot/troubleshoot-logs-1.png)

2. Tıklayın _Microsoft Veri kutusu Edge klasör_. Bu klasör iki alt klasör içerir:

    - Karşıya yükleme hatalarının bulunduğu Upload klasörü.
    - Yenileme sırasında karşılaşılan hataların bulunduğu Refresh klasörü.

    Yenileme günlük dosyası örneği burada gösterilmiştir.

    ```
    <root container="test1" machine="VM15BS020663" timestamp="03/18/2019 00:11:10" />
    <file item="test.txt" local="False" remote="True" error="16001" />
    <summary runtime="00:00:00.0945320" errors="1" creates="2" deletes="0" insync="3" replaces="0" pending="9" />
    ``` 

3. Bu dosyada bir hata gördüğünüzde (örnekte vurgulanmıştır) hata kodunu yazın (burada 16001). Hata kodunun açıklamasını aşağıdaki hata başvurusu bölümünde bulabilirsiniz.

    [!INCLUDE [data-box-edge-edge-upload-error-reference](../../includes/data-box-edge-gateway-upload-error-reference.md)]


## <a name="next-steps"></a>Sonraki adımlar

- [Bu sürümdeki bilinen sorunlar](data-box-gateway-release-notes.md) hakkında daha fazla bilgi edinin.
