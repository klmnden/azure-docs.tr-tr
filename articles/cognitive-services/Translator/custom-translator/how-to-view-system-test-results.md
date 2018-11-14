---
title: Sistemin test sonuçlarını ve dağıtım - özel Translator görüntüleyin
titleSuffix: Azure Cognitive Services
description: Eğitim başarılı olduğunda, eğitim sonuçlarınızı çözümlemek için sistem testleri gözden geçirin. Dağıtım isteği eğitim modeli için eğitim Sonuçlardan memnun kalırsanız yerleştirin.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 61d3869559d88e14c0b9a3c3e23cd8a1f9c2b6c4
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627815"
---
# <a name="view-system-test-results"></a>Sistemin test sonuçlarını görüntüle

Eğitim başarılı olduğunda, eğitim sonuçlarınızı çözümlemek için sistem testleri gözden geçirin. Dağıtım isteği eğitim modeli için eğitim Sonuçlardan memnun kalırsanız yerleştirin. 

## <a name="system-test-results-page"></a>Sistem test sonuçları sayfası

Bir proje seçin ve ardından bu projenin modelleri sekmesini seçin, son olarak test sekmesini seçin ve kullanmak istediğiniz modelini bulun.

Test sekmesi, gösterir:

1.  Sistem Test sonuçları: Eğitimleri test işleminin sonucu. Test işleminin BLEU puanı üretir.

    **Tümce sayısı:** kaç paralel cümleler test kümesi kullanıldı.

     **BLEU puanı:** BLEU puan modeli için eğitim tamamlandıktan sonra oluşturulur.

    **Durum:** test işlemi tamamlandığında veya ilerleme durumunu gösterir.

    ![Sistem test sonuçları](media/how-to/how-to-system-test-results.png)

2.  Sistemin test sonuçlarına tıklayın ve test sonuç Ayrıntıları sayfası sayfasına yönlendirileceksiniz. Bu sayfa, makine çevirisi test veri kümesini parçası olan tümcelerin gösterir.

3.  Test sonuç ayrıntıları sayfasındaki tablonun - çiftindeki her dil için iki sütun vardır. Kaynak dili için sütun çevrilemeyen cümlenin gösterir. Her bir satırdaki iki cümle hedef dil için sütun içerir.

    **Ref:** şu cümleyi test veri kümesinde belirtilen kaynak cümlenin başvuru çevrilmesidir.

    **MT:** şu cümleyi eğitim yürütülen sonra oluşturulan model tarafından yapılan kaynak cümle otomatik çevrilmesidir.

    ![Sistemin test sonuçları karşılaştırma](media/how-to/how-to-system-test-results-2.png)

## <a name="download-test"></a>Test yükle

Zip dosyasını indirmek için indirme çevirilerini bağlantıya tıklayın. Makine çevirileri sınama veri kümesi kaynağı tümcelerin zip içerir.

![Test yükle](media/how-to/how-to-system-test-download.png)

Bu indirilen ZIP arşivini üç dosyalarını içerir.

1.  Custom.MT.txt: Bu dosya kaynak dili cümle hedef kullanıcının verileriyle eğitilen model tarafından yapılan dilde makine çevirileri içerir.

2.  Ref.txt: kullanıcı tarafından sağlanan Çeviriler kaynak dili cümle hedef dilde bu dosya içerir.

3.  Source.txt: Bu dosya kaynak dili cümlelerde içerir.

    ![İndirilen sistem test sonuçları](media/how-to/how-to-download-system-test.png)

## <a name="deploy-a-model"></a>Model dağıtma

Bir dağıtım talep için:

1.  Bir proje seçin, modelleri sekmesine gidin.

2. Başarıyla eğitilen bir modeli için "Dağıt" düğmesini gösterir, yoksa dağıtılmış.

    ![Model dağıtma](media/how-to/how-to-deploy-model.png)

3.  Tıklayarak dağıtma.
4.  Dağıtılacak modelinizi istediğiniz olduğu bölgelerin seçin ve Kaydet'e tıklayın. Birden çok bölgede seçebilirsiniz.

    ![Model dağıtma](media/how-to/how-to-deploy-model-regions.png)

5.  Model durumu "Durum" sütununda görüntüleyebilirsiniz.

>[!Note]
>Bir model zaten dağıttıysanız, bu model için bir "Undeploy" düğmesi görürsünüz. Model dağıtımı geri için "Undeploy" tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- Dağıtılan özel çeviri modeliniz aracılığıyla kullanmaya başlama [Microsoft Translator Text API v3 sürümüne](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
- Bilgi [ayarlarının nasıl yönetileceğini](how-to-manage-settings.md) çalışma alanınızı paylaşmak için abonelik anahtarınızı yönetme.
- Bilgi [çalışma ve projeyi geçirmek nasıl](how-to-migrate.md) gelen [Microsoft Translator Hub](https://hub.microsofttranslator.com)