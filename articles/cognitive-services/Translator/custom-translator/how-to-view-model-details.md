---
title: Model - özel Translator ayrıntılarına bakın
titleSuffix: Azure Cognitive Services
description: Modelleri sekmesinde herhangi bir projeyi altında model adı, model durumu, eğitim, ayarlama, test cümle sayısı BLEU puanı, her model ayrıntılarını gösterir.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 80ee99fc4490f1fdc591093f357c8db55a97abb9
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627816"
---
# <a name="view-model-details"></a>Model ayrıntıları görüntüle

Proje modelleri sekmesinde tüm modelleri projede gösterir. Tüm modeller için proje bu sekmede listelenen eğitim.

Projedeki her model için model aşağıdaki ayrıntıları görüntülenir.

1.  Model adı: belirli bir model model adını gösterir.

2.  Durum: belirli bir model durumunu gösterir. Kabul edilinceye kadar yeni eğitim gönderildi durumuna sahip. Belgelerinizi içeriğini hizmet tarafından değerlendirilir olsa veri işleme durumu değişir. Belgelerinizi değerlendirmesi tamamlandıktan sonra çalışan durumu değişir ve mümkün olacaktır, eğitim ve sınama ayarlama dahil, bir parçası olan cümleler sayısını ayarlar bakın sizin için otomatik olarak oluşturulur. Model durumunu açıklayan model durumu listesi aşağıdadır.

    -  Gönderilen: arka uç bu modeli belgeleri işliyor belirtir.

    -  TrainingQueued: eğitim MT sisteme bu model için sıraya alınıyor olduğunu belirtir.

    -  Çalıştırılıyor: MT sisteminde bu modeli için eğitim çalıştığını belirtir.

    -  Başarılı: eğitim MT sistemde başarılı ve bir modeli kullanılabilir olduğunu belirtir. Bu durumda, bu modeli BLEU puanı görüntülenir.

    -  Dağıtılan: başarılı eğitilen model MT sistemi dağıtımı için gönderildiği belirtir.

    -  Undeploying: dağıtılan model undeploying olduğunu belirtir.

    -  Dağıtılmamış: model undeployment işlemi başarıyla tamamlandığını belirtir.

    -  Eğitim başarısız oldu: eğitim başarısız olduğunu belirtir. Bir eğitim hata oluşursa, eğitim işini yeniden deneyin. Hata devam ederse bizimle iletişime geçin. Başarısız modeli silmeyin.

    - DataProcessingFailed: veri işleme modele ait bir veya daha fazla belge için başarısız olduğunu belirtir.

    - DeploymentFailed: model dağıtımı başarısız olduğunu belirtir.

    - MigratedDraft: model özel Translator Hub'ından geçişten sonra taslak durumunda olduğunu belirtir.

4.  BLEU puanı: Çeviri sisteminizi kalitesini belirten modelin BLEU (iki dilli değerlendirme Understudy) puan gösterilmektedir. Bu puan ne kadar yakın bu eğitimleri kaynaklanan çeviri sistemi tarafından gerçekleştirilen çevirileri sınama veri kümesi başvurusu cümleleri eşleşen söyler. Eğitim başarıyla tamamlandıysa BLEU puanı görünür. Eğitim değilse tam / başarısız herhangi BLEU puanı görmezsiniz.

5.  Tümce sayısı eğitim: cümleler Eğitim kümesi olarak kullanılan toplam sayısını gösterir.

6.  Tümce sayısı ayarlama: cümleler kümesi ayarlama olarak kullanılan toplam sayısını gösterir.

7.  Tümce sayısı eğitim: test kümesi olarak kullanılan cümle toplam sayısını gösterir.

8.  Mono cümle sayısı: mono kümesi olarak kullanılan cümle toplam sayısını gösterir.

9.  Dağıtım eylem düğmesi: "Dağıt" düğmesini yoksa dağıtılmış başarıyla eğitilen bir modelin için bunu gösterir. Bir model dağıttıysanız, "Undeploy" düğmesi gösterilir.

10. Sil: model silmek istiyorsanız bu düğmesini kullanabilirsiniz. Bir model siliniyor, bu modeli oluşturmak için kullanılan belgeler hiçbirini silinmez.

    ![Model ayrıntıları görüntüle](media/how-to/how-to-view-model-details.png)

>[!Note]
>Aynı sistemler için ardışık eğitimleri karşılaştırmak için bu ayar ayarlanmış tutmak önemlidir ve test sabit ayarlayın.

## <a name="view-model-training-details"></a>Model eğitimi ayrıntılarını görüntüle

Eğitim tamamlandığında Eğitim ayrıntıları sayfasında ilgili ayrıntıları gözden geçirebilirsiniz. Bir proje seçin, bulun ve modelleri sekmesini seçin ve bir modeli seçin.

Modeli sayfasına iki sekme bulunur: Eğitim ayrıntıları ve Test.

1.  **Ayrıntılar eğitim:** listesi eğitim kullanılan belgeler, bu sekme gösterir:

    -  Belge adı: Bu alan belge adını gösterir.

    -  Belge türü: Bu alan, bu belge, varsa paralel / mono gösterir.

    -  Tümce kaynak dili sayısı: Bu alan sayısı gösterilmektedir cümleler var olan kaynak dili bir parçası olarak.

    -  Cümle hedef dilde sayısı: Bu alan sayısı gösterilmektedir cümle hedef dil bir parçası olarak var olan.

    -  Hizalanmış cümleler: işlem sırasında özel Translator tarafından cümleler sayısı gösterilmektedir hizalı Bu alan hizalayın.

    -  Kullanılan cümle: cümleler sayısı gösterilmektedir özel Translator tarafından bu eğitim sırasında bu alan kullanıldığını.

    ![Model eğitimi ayrıntıları](media/how-to/how-to-model-training-details.png)

2.  **Test:** Bu sekme başarılı bir eğitim test ayrıntılarını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [test sonuçları](how-to-view-system-test-results.md) ve eğitim sonuçları analiz edin.
