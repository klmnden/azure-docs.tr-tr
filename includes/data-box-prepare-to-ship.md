---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/25/2019
ms.author: alkohli
ms.openlocfilehash: 1d52117440028c75b249f469f2b3576c2ab1c5c5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65815485"
---
Son adım cihazı göndermeye hazırlamaktır. Bu adımda tüm cihaz paylaşımları çevrimdışı duruma getirilir. Bu işlemi başlattıktan sonra paylaşımları erişilemez.

> [!IMPORTANT]
> Göndermeye hazırlama Azure adlandırma kurallarına uymuyor veri bayrakları olarak gereklidir. Bu adımı atlarsanız olası veri sonucunda hatalar nedeniyle veri DSCP karşıya yüklenemedi.

1. **Göndermeye hazırlama** sayfasına gidip **Hazırlamayı başlat**'a tıklayın. Veriler kopyalanırken sırasında varsayılan olarak, sağlama toplamları hesaplanır. Göndermeye hazırlama sağlama toplamı hesaplama tamamlar ve dosyaların listesini oluşturur ( *- BOM dosyaları*). Sağlama toplamı hesaplaması, verilerinizin boyutuna bağlı olarak gün saat sürebilir. 
   
    ![Göndermeye hazırlama 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

    Herhangi bir nedenden dolayı cihaz hazırlığı durdurmak istiyorsanız, tıklayın **hazırlığı Durdur**. Daha sonra göndermeye hazırlama devam edebilir.
        
    ![2 göndermeye hazırlama](media/data-box-prepare-to-ship/prepare-to-ship2.png)
    
2. Göndermeye hazırlama başlatılır ve cihaz paylaşımları çevrimdışı. Cihazın hazır olduktan sonra iade sevkiyat etiketini indirmek için bir anımsatıcı görürsünüz.

    ![Sevkiyat Etiketi anımsatıcı indirin](media/data-box-prepare-to-ship/download-shipping-label-reminder.png)

3. Cihaz durumu güncelleştirmeleri *gönderilmeye hazır* ve cihaz hazırlığı tamamlandıktan sonra cihaz kilitlenir.
        
    ![3 göndermeye hazırlama](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    Cihaza daha fazla veri kopyalamak istiyorsanız, cihazın kilidini açmak, daha fazla veri kopyalama ve çalıştırma hazırlama yeniden dağıtmayı.

    Bu adımda bir hata varsa, hata günlüğü indir ve hataları çözmek gerekir. Hataları çözümlendikten sonra Çalıştır **göndermeye hazırlama**.

4. Göndermeye hazırlama (hatasız) başarıyla tamamlandıktan sonra bu işlemde kopyalanan dosyaların (bildirim olarak da bilinir) listesini indirin. Daha sonra bu listeyi kullanarak Azure'a yüklenen dosyaları doğrulayabilirsiniz. Daha fazla bilgi için [İnceleme BOM dosyaları göndermeye hazırlama sırasında](../articles/databox/data-box-logs.md#inspect-bom-during-prepare-to-ship).
        
    ![Göndermeye hazırlama 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

5. Cihazı kapatın. **Kapat veya yeniden başlat** sayfasına gidip **Kapat**'a tıklayın. Onayınız istendiğinde devam etmek için **Tamam**'a tıklayın.

6. Kabloları sökün. Bir sonraki adım cihazı Microsoft'a göndermektir.
