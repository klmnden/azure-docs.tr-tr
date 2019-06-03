---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/28/2019
ms.author: alkohli
ms.openlocfilehash: f5b60d862be0d71f71f770c47d88ad39f2fc6ac7
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419576"
---
Son adım cihazı göndermeye hazırlamaktır. Bu adımda tüm cihaz paylaşımları çevrimdışı duruma getirilir. Bu işlemi başlattıktan sonra paylaşımları erişilemez.

> [!IMPORTANT]
> Göndermeye hazırlama Azure adlandırma kurallarına uymuyor veri bayrakları olarak gereklidir. Bu adımı atlarsanız olası veri sonucunda hatalar nedeniyle veri DSCP karşıya yüklenemedi.

1. **Göndermeye hazırlama** sayfasına gidip **Hazırlamayı başlat**'a tıklayın. Veriler kopyalanırken sırasında varsayılan olarak, sağlama toplamları hesaplanır. Göndermeye hazırlama sağlama toplamı hesaplama tamamlar ve dosyaların listesini oluşturur (olarak da bilinen *BOM dosyaları* veya bildirim). Sağlama toplamı hesaplaması, verilerinizin boyutuna bağlı olarak gün saat sürebilir.
   
    ![Göndermeye hazırlama 1](media/data-box-heavy-prepare-to-ship/prepare-to-ship1.png)

    Herhangi bir nedenden dolayı cihaz hazırlığı durdurmak istiyorsanız, tıklayın **hazırlığı Durdur**. Daha sonra göndermeye hazırlama devam edebilir.
        
    ![2 göndermeye hazırlama](media/data-box-heavy-prepare-to-ship/prepare-to-ship2.png)
    
2. Göndermeye hazırlama başlatılır ve cihaz paylaşımları çevrimdışı. Cihazın hazır olduktan sonra iade sevkiyat etiketini indirmek için bir anımsatıcı görürsünüz.

    ![Sevkiyat Etiketi anımsatıcı indirin](media/data-box-heavy-prepare-to-ship/download-shipping-label-reminder.png)

3. Cihaz durumu güncelleştirmeleri *gönderilmeye hazır* ve cihaz hazırlığı tamamlandıktan sonra cihaz kilitlenir.
        
    ![3 göndermeye hazırlama](media/data-box-heavy-prepare-to-ship/prepare-to-ship3.png)

    Cihaza daha fazla veri kopyalamak istiyorsanız, cihazın kilidini açmak, daha fazla veri kopyalama ve çalıştırma hazırlama yeniden dağıtmayı.

    Bu adımda bir hata varsa, hata günlüğünü indirin ve hataları giderin. Hataları çözümlendikten sonra Çalıştır **göndermeye hazırlama**.

4. Göndermeye hazırlama sonra indirme başarıyla tamamlandı (hatasız), dosya listesi (olarak da bilinen *BOM dosyaları* veya bildirim) Bu işlemde kopyalanır. 

    ![Veya ürün reçetesi dosyaları listesini indirme](media/data-box-heavy-prepare-to-ship/download-list-of-files.png)

   Daha sonra bu listeyi kullanarak Azure'a yüklenen dosyaları doğrulayabilirsiniz. Daha fazla bilgi için [İnceleme BOM dosyaları göndermeye hazırlama sırasında](../articles/databox/data-box-logs.md#inspect-bom-during-prepare-to-ship).
        
    ![Örnek Ürün reçetesi dosyası](media/data-box-heavy-prepare-to-ship/sample-bom-file.png)

5. Cihazı kapatın. **Kapat veya yeniden başlat** sayfasına gidip **Kapat**'a tıklayın. Onayınız istendiğinde devam etmek için **Tamam**'a tıklayın.

    ![İlk cihaz düğümü kapatın](media/data-box-heavy-prepare-to-ship/shut-down-device-node.png)

6. Bir aygıt düğümü için yukarıdaki adımları yineleyin.
7. Cihazı tamamen kapatıldı sonra cihazın arkasına tüm LED'lerini kapatmış. Sonraki adım, kabloların kaldırın ve cihazı Microsoft'a gönderin sağlamaktır.
