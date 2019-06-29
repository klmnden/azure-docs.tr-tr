---
title: Sistemin test sonuçlarını ve dağıtım - özel Translator görüntüleyin
titleSuffix: Azure Cognitive Services
description: Eğitim başarılı olduğunda, eğitim sonuçlarınızı çözümlemek için sistem testleri gözden geçirin. Dağıtım isteği eğitim modeli için eğitim Sonuçlardan memnun kalırsanız yerleştirin.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: ec15851ae7ff59a752fbf0d823d87aa6e68f10e9
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442491"
---
# <a name="view-system-test-results"></a>Sistem test sonuçlarını görüntüleme

Eğitim başarılı olduğunda, eğitim sonuçlarınızı çözümlemek için sistem testleri gözden geçirin. Dağıtım isteği eğitim modeli için eğitim Sonuçlardan memnun kalırsanız yerleştirin.

## <a name="system-test-results-page"></a>Sistem test sonuçları sayfası

Bir proje seçin ve ardından bu projenin modelleri sekmesini seçin, son olarak test sekmesini seçin ve kullanmak istediğiniz modelini bulun.

Test sekmesi, gösterir:

1.  **Sistem Test sonuçları:** Eğitimleri test işleminin sonucu. Test işleminin BLEU puanı üretir.

    **Tümce sayısı:** Kaç paralel cümleler test kümesi kullanıldı.

     **BLEU puan:** BLEU puan modeli için eğitim tamamlandıktan sonra oluşturulur.

    **Durum:** Test işlemi tamamlandığında veya ilerleme durumunu gösterir.

    ![Sistem test sonuçları](media/how-to/how-to-system-test-results.png)

2.  Sistemin test sonuçlarına tıklayın ve test sonuç Ayrıntıları sayfası sayfasına yönlendirileceksiniz. Bu sayfa, makine çevirisi test veri kümesini parçası olan tümcelerin gösterir.

3.  Test sonuç ayrıntıları sayfasındaki tablonun - çiftindeki her dil için iki sütun vardır. Kaynak dili için sütun çevrilemeyen cümlenin gösterir. Her bir satırdaki iki cümle hedef dil için sütun içerir.

    **Ref:** Bu cümleyi test veri kümesinde belirtilen kaynak cümlenin başvuru çevrilmesidir.

    **MT:** Bu cümleyi eğitim yürütülen sonra oluşturulan model tarafından yapılan kaynak cümle otomatik çevrilmesidir.

    ![Sistemin test sonuçları karşılaştırma](media/how-to/how-to-system-test-results-2.png)

## <a name="download-test"></a>Test yükle

Zip dosyasını indirmek için indirme çevirilerini bağlantıya tıklayın. Makine çevirileri sınama veri kümesi kaynağı tümcelerin zip içerir.

![Test yükle](media/how-to/how-to-system-test-download.png)

Bu indirilen ZIP arşivini üç dosyalarını içerir.

1.  **custom.mt.txt:** Bu dosya, kaynak dili cümle hedef kullanıcının verileriyle eğitilen model tarafından yapılan dilde makine çevirileri içerir.

2.  **Ref.txt:** Bu dosya, kullanıcı tarafından sağlanan Çeviriler kaynak dili cümle hedef dilde içerir.

3.  **Source.txt:** Bu dosya, kaynak dili cümlelerde içerir.

    ![İndirilen sistem test sonuçları](media/how-to/how-to-download-system-test.png)

## <a name="deploy-a-model"></a>Model dağıtma

Bir dağıtım talep için:

1.  Bir proje seçin, modelleri sekmesine gidin.

2. Başarıyla eğitilen bir modeli için "Dağıt" düğmesini gösterir, yoksa dağıtılmış.

    ![Model dağıtma](media/how-to/how-to-deploy-model.png)

3.  Tıklayarak dağıtma.
4.  Seçin **dağıtıldı** için olduğu bölgelerin istediğiniz modelinizin dağıtılması ve Kaydet'e tıklayın. Seçebileceğiniz **dağıtıldı** birden çok bölgeye yönelik.

    ![Model dağıtma](media/how-to/how-to-deploy-model-regions.png)

5.  Model durumu "Durum" sütununda görüntüleyebilirsiniz.

>[!Note]
>Özel Translator, zaman içinde herhangi bir noktada bir çalışma alanı 10 dağıtılan modelleri destekler.

## <a name="update-deployment-settings"></a>Güncelleştirme dağıtım ayarları

Dağıtım ayarları güncelleştirmek için:

1.  Bir projeyi seçin ve Git **modelleri** sekmesi.

2. Başarıyla dağıtılan bir model için bunu gösterir bir **güncelleştirme** düğmesi.

    ![Model dağıtma](media/how-to/how-to-update-undeploy-model.png)

3.  Seçin **güncelleştirme**.
4.  Seçin **dağıtıldı** veya **Undeployed** dağıtılan veya dağıtımı geri modelinizi istediğiniz olduğu bölgelerin için ardından **Kaydet**.

    ![Model dağıtma](media/how-to/how-to-undeploy-model.png)

>[!Note]
>Seçerseniz **Undeployed** tüm bölgeler için model tüm bölgelerden dağıtımı geri ve dağıtılmamış bir duruma getirir. Kullanım için artık kullanılamıyor.

## <a name="next-steps"></a>Sonraki adımlar

- Dağıtılan özel çeviri modeliniz aracılığıyla kullanmaya başlama [Microsoft Translator Text API v3 sürümüne](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
- Bilgi [ayarlarının nasıl yönetileceğini](how-to-manage-settings.md) çalışma alanınızı paylaşmak için abonelik anahtarınızı yönetme.
- Bilgi [çalışma ve projeyi geçirmek nasıl](how-to-migrate.md) gelen [Microsoft Translator Hub](https://hub.microsofttranslator.com)
