---
title: Azure Data Factory ile LastModifiedDate yeni ve değiştirilen dosyaları kopyalama | Microsoft Docs
description: Azure Data Factory ile LastModifiedDate tarafından yeni ve değiştirilen dosyaları kopyalamak için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 3/8/2019
ms.openlocfilehash: cae75f4d64c8b3f74cc40e94a675c0f10a6bd9ec
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60312857"
---
# <a name="copy-new-and-changed-files-by-lastmodifieddate-with-azure-data-factory"></a>Azure Data Factory ile LastModifiedDate tarafından yeni ve değiştirilen dosyaları Kopyala

Bu makalede, yeni ve değiştirilen dosyalar LastModifiedDate tarafından yalnızca bir dosya tabanlı depolama alanından hedef deposuna kopyalamak için kullanabileceğiniz bir çözüm şablonu açıklanır. 

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, yeni ve değiştirilmiş dosyalar ilk yalnızca özniteliklerini seçer. **LastModifiedDate**ve ardından seçili dosyaları kaynak veri deposundan hedef veri deposuna kopyalar.

Bu şablon bir etkinlik içerir:
- **Kopyalama** yeni ve değiştirilen dosyalar LastModifiedDate tarafından yalnızca bir dosya deposundan hedef deposuna kopyalamak için.

Şablon dört parametre tanımlar:
-  *FolderPath_Source* nerede okuyabilir dosyaları kaynak deposundan klasör yoludur. Varsayılan değer kendi klasör yolu ile değiştirmeniz gerekir.
-  *FolderPath_Destination* istediğiniz dosyaları hedef deposuna kopyalamak için klasör yolu. Varsayılan değer kendi klasör yolu ile değiştirmeniz gerekir.
-  *LastModified_From* bu tarih/saat değerine eşit veya olan LastModifiedDate özniteliktir sonra dosyalar seçmek için kullanılır.  Yalnızca son kez kopyalanır değil, yeni dosyalar seçmek için bu tarih saat değeri zaman işlem hattının son tetiklenme zamanı olabilir. Varsayılan değer 2019 ' değiştirebilirsiniz-02-01T00:00:00Z', beklenen LastModifiedDate UTC saat dilimi içinde için. 
-  *LastModified_To* olan LastModifiedDate özniteliği bu tarih saat değeri önce dosya seçmek için kullanılır. Yalnızca son kez kopyalanır değil, yeni dosyalar seçmek için bu tarih saat değeri saatten olabilir.  Varsayılan değer 2019 ' değiştirebilirsiniz-02-01T00:00:00Z', beklenen LastModifiedDate UTC saat dilimi içinde için. 

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu kullanma

1. Şablon Git **LastModifiedDate tarafından yalnızca yeni bir dosya kopyalama**. Oluşturma bir **yeni** kaynak depolama deponuza bağlantı. Kaynak depolama dosyalarını kopyalamak istediğiniz deposudur.

    ![Kaynak için yeni bir bağlantı oluşturun](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate1.png)
    
2. İlk Depolama seçin **türü**. Depolama giriş sonra **hesap adı** ve **hesap anahtarı**. Son olarak, seçin **son**.

    ![Bir bağlantı dizesi girin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate2.png)
    
3. Oluşturma bir **yeni** hedef deponuza bağlantı. Dosyaları kopyalamak istediğiniz hedef deposudur. 2 adımda gibi veri deposunun hedef benzer bağlantı bilgilerini de giriş.

    ![Hedef için yeni bir bağlantı oluşturun](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate3.png)

4. Seçin **bu şablonu kullan**.

    ![Bu şablonu kullanın.](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate4.png)
    
5. Aşağıdaki örnekte gösterildiği gibi panelinde kullanılabilir işlem hattı görürsünüz:

    ![İşlem hattı Göster](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate5.png)

6. Seçin **hata ayıklama**, değeri yazma **parametreleri** seçip **son**.  Aşağıdaki resimde, biz parametreleri aşağıdaki gibi ayarlayın.
   - **FolderPath_Source** =  **/source/**
   - **FolderPath_Destination** =  **/destination/**
   - **LastModified_From** =  **2019-02-01T00:00:00Z**
   - **LastModified_To** = **2019-03-01T00:00:00Z**
    
     Örneğin son arasında geçen süreyi içindeki değiştirilmiş dosyaları gösteren *2019-02-01T00:00:00Z* ve *2019-03-01T00:00:00Z* bir klasörden kopyalanır */source/*  bir klasöre */destination/* .  Bunları kendi parametrelerini değiştirebilirsiniz.
    
     ![İşlem hattını çalıştırma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate6.png)

7. Sonucu gözden geçirin. Yalnızca son yapılandırılmış içinde değiştirilen dosyaları göreceğiniz timespan hedef depoya kopyalandı.

    ![Sonucu gözden geçir](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate7.png)
    
8. Artık işlem hattı her zaman yeni ve değiştirilen dosyalar yalnızca LastModifiedDate tarafından düzenli aralıklarla böylelikle bu işlem hattı otomatikleştirmek için bir atlayan windows tetikleyicisi ekleyebilirsiniz.  Seçin **tetikleyicisi Ekle**seçip **yeni/Düzenle**.

    ![Sonucu gözden geçir](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate8.png)
    
9. İçinde **tetikleyici Ekle** penceresinde **+ yeni**.

    ![Sonucu gözden geçir](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate9.png)

10. Seçin **atlayan pencere** tetikleyici türü için **her 15 dakikada bir** (dilediğiniz zaman aralığını değiştirebilirsiniz), yineleme olarak seçip **sonraki**.

    ![Tetikleyici oluşturma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate10.png)    
    
11. Değeri yazma **tetikleyici çalıştırma parametreleri** seçin ve aşağıdaki **son**.
    - **FolderPath_Source** =  **/source/** .  Kaynak veri deposu klasörünüze ile değiştirebilirsiniz.
    - **FolderPath_Destination** =  **/destination/** .  Hedef veri deposuna klasörünüze ile değiştirebilirsiniz.
    - **LastModified_From** =   **@trigger(). outputs.windowStartTime**.  Tetikleyici, işlem hattı son tetiklenme zamanı belirleyen bir sistem değişkenidir.
    - **LastModified_To** =  **@trigger(). outputs.windowEndTime**.  Bu süre işlem hattı tetiklendiğinde süreyi belirlemede tetikleyiciden bir sistem değişkenidir.
    
    ![Giriş parametreleri](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate11.png)
    
12. **Tümünü Yayımla**.
    
    ![Tümünü Yayımla](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate12.png)

13. Veri kaynağı deposu, kaynak klasördeki yeni dosyalar oluşturun.  Artık otomatik olarak tetiklenen işlem hattının atanmasını bekliyor ve yalnızca yeni dosyalar hedef deposuna kopyalanacak.

14. Seçin **izleme** sekmesinde sol gezinti panelinde ve 15 dakikada tetikleyicisinin yinelenmesi ayarlarsanız yaklaşık 15 dakika bekleyin. 

    ![İzleme seçin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate14.png)

15. Sonucu gözden geçirin. İşlem hattınızı otomatik olarak 15 dakikada bir tetiklenir ve her işlem hattı çalıştırması hedef depoda yalnızca yeni veya değiştirilmiş dosyaları kaynak deposundan kopyalanacağı görürsünüz.

    ![Sonucu gözden geçir](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate15.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)