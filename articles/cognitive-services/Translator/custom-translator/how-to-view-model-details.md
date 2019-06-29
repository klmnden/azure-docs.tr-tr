---
title: Model - özel Translator ayrıntılarına bakın
titleSuffix: Azure Cognitive Services
description: Modelleri sekmesinde herhangi bir projeyi altında model adı, model durumu, eğitim, ayarlama, test cümle sayısı BLEU puanı, her model ayrıntılarını gösterir.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 92f5275b85b4a257e8841f43424d356db702cacd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442463"
---
# <a name="view-model-details"></a>Model ayrıntılarını görüntüleme

Proje modelleri sekmesinde tüm modelleri projede gösterir. Tüm modeller için proje bu sekmede listelenen eğitim.

Projedeki her model için bu ayrıntılar görüntülenir.

1.  Model adı: Belirli bir model model adını gösterir.

2.  Durum: Belirli bir model durumunu gösterir. Kabul edilinceye kadar yeni eğitim gönderildi durumuna sahip. Belgelerinizi içeriğini hizmet tarafından değerlendirilir olsa veri işleme durumu değişir. Belgelerinizi değerlendirmesi tamamlandıktan sonra çalışan durumu değişir ve mümkün olacaktır, eğitim ve sınama ayarlama dahil, bir parçası olan cümleler sayısını ayarlar bakın sizin için otomatik olarak oluşturulur. Model durumunu açıklayan model durumu listesi aşağıdadır.

    -  Gönderilme tarihi: Arka uç bu modeli belgeleri işliyor belirtir.

    -  TrainingQueued: Eğitim MT sisteme bu model için sıraya alınıyor olduğunu belirtir.

    -  Çalıştıran: MT sisteminde bu modeli için eğitim çalıştığını belirtir.

    -  Başarılı: MT sistemde eğitim başarılı ve bir modeli kullanılabilir olduğunu belirtir. Bu durumda, bu modeli BLEU puanı görüntülenir.

    -  Dağıtıldı: Başarılı eğitilen model MT sistemi dağıtımı için gönderildiği belirtir.

    -  Undeploying: Dağıtılmış bir modelinin undeploying olduğunu belirtir.

    -  Dağıtımı geri: Bir modelin undeployment işlemi başarıyla tamamlandığını belirtir.

    -  Eğitim başarısız oldu: Eğitim başarısız olduğunu belirtir. Bir eğitim hata oluşursa, eğitim işini yeniden deneyin. Hata devam ederse bizimle iletişime geçin. Başarısız modeli silmeyin.

    - DataProcessingFailed: Veri işleme modele ait bir veya daha fazla belge için başarısız olduğunu belirtir.

    - DeploymentFailed: Model dağıtımı başarısız olduğunu belirtir.

    - MigratedDraft: Modeli özel Translator Hub'ından geçişten sonra taslak durumunda olduğunu belirtir.

4.  BLEU puanı: Çeviri sisteminizi kalitesini belirten modelin BLEU (iki dilli değerlendirme Understudy) puan gösterilmektedir. Bu puan ne kadar yakın bu eğitimleri kaynaklanan çeviri sistemi tarafından gerçekleştirilen çevirileri sınama veri kümesi başvurusu cümleleri eşleşen söyler. Eğitim başarıyla tamamlandıysa BLEU puanı görünür. Eğitim değilse tam / başarısız herhangi BLEU puanı görmezsiniz.

5.  Eğitim cümle sayısı: Eğitim kümesi olarak kullanılan cümle toplam sayısı gösterilmektedir.

6.  Ayarlama cümle sayısı: Cümleleri kümesi ayarlama olarak kullanılan toplam sayısı gösterilmektedir.

7.  Eğitim cümle sayısı: Test kümesi olarak kullanılan cümle toplam sayısı gösterilmektedir.

8.  Mono cümle sayısı: Mono kümesi olarak kullanılan cümle toplam sayısı gösterilmektedir.

9.  Eylem düğmesi dağıtın: Başarıyla eğitilen bir modeli için "Dağıt" düğmesini gösteren yoksa dağıtılmış. Bir model dağıttıysanız, "Undeploy" düğmesi gösterilir.

10. Sil: Model silmek istiyorsanız bu düğmesini kullanabilirsiniz. Bir model siliniyor, bu modeli oluşturmak için kullanılan belgeler hiçbirini silinmez.

    ![Model ayrıntılarını görüntüleme](media/how-to/how-to-view-model-details.png)

>[!Note]
>Aynı sistemler için ardışık eğitimleri karşılaştırmak için bu ayar ayarlanmış tutmak önemlidir ve test sabit ayarlayın.

## <a name="view-model-training-details"></a>Model eğitimi ayrıntılarını görüntüle

Eğitim tamamlandığında Eğitim ayrıntıları sayfasında ilgili ayrıntıları gözden geçirebilirsiniz. Bir proje seçin, bulun ve modelleri sekmesini seçin ve bir modeli seçin.

Modeli sayfasına iki sekme bulunur: Eğitim ayrıntıları ve Test.

1.  **Eğitim ayrıntıları:** Bu sekme, eğitim kullanılan belgelerin listesini gösterir:

    -  Belge adı: Bu alan belge adını gösterir.

    -  Belge türü: Bu alan, bu belge, varsa paralel / mono olduğunu gösterir.

    -  Kaynak dili cümle sayısı: Bu alan sayısı gösterilmektedir cümleler var olan kaynak dili bir parçası olarak.

    -  Hedef dilde cümle sayısı: Bu alan sayısı gösterilmektedir cümle hedef dil bir parçası olarak var olan.

    -  Hizalanmış cümleler: Bu alan cümleler sayısı gösterilmektedir sırasında özel Translator tarafından hizalı işlem hizalayın.

    -  Cümleleri kullanılır: Cümleleri sayısı gösterilmektedir kullanılmıştı özel Translator tarafından bu eğitim sırasında bu alan.

    ![Model eğitimi ayrıntıları](media/how-to/how-to-model-training-details.png)

2.  **Test:** Bu sekme başarılı bir eğitim test ayrıntılarını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [test sonuçları](how-to-view-system-test-results.md) ve eğitim sonuçları analiz edin.
