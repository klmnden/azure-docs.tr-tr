---
title: Yaklaşım analizi model oluşturma
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da metin ön işleme, N-gram veya özellik karma modüllerini kullanma, metin analiz modelleri oluşturma
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/14/2018
ms.openlocfilehash: 08d62e7a6c9503d415fe144da57eee72ce3bfafd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60636629"
---
# <a name="create-a-sentiment-analysis-model-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da bir yaklaşım analizi model oluşturma

Azure Machine Learning Studio, derleme ve metin analiz modelleri oluşturup kullanıma hazır hale getirmek için kullanabilirsiniz. Bu modeller, örneğin, belge sınıflandırma veya yaklaşım analizi sorunlarını çözmenize yardımcı olabilir.

Metin bir analiz denemesi genellikle gerekir:

1. Temiz ve metin veri kümesi için önceden işlenir
2. Önceden işlenmiş metin sayısal özellik vektör ayıklayın
3. Sınıflandırma veya regresyon modeli eğitme
4. Puanlama ve modelini doğrulama
5. Üretim için model dağıtma

Bu öğreticide, Amazon Kitap incelemeleri veri kümesini kullanarak bir yaklaşım analizi modeliyle inceleyeceğiz gibi adımları öğrenin (Bu araştırma incelemeye bakın "Biyografileri, Bollywood, müzik kutuları ve Blenders: Etki alanı uyarlaması için duygu sınıflandırmasını"John Blitzer tarafından Dredze ve Fernando Pereira işaretleyin; İlişkisini hesaplama Linguistics (ACL), 2007.) Bu veri kümesi (1-2 veya 4-5) Gözden geçirme puanları ve serbest biçimli metin oluşur. Gözden geçirme puanı tahmin olmaktır: düşük (1 - 2) veya yüksek (4-5).

Bu öğreticide Azure AI Gallery ele denemeleri bulabilirsiniz:

[Kitap incelemelerini tahmin edin](https://gallery.azure.ai/Experiment/Predict-Book-Reviews-1)

[Kitap incelemeleri - Tahmine dayalı denemeye tahmin edin](https://gallery.azure.ai/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>1\. adım: Temiz ve metin veri kümesi için önceden işlenir
Biz, denemeyi iki sınıflı sınıflandırma sorunlu formüle etmek için kategorik düşük ve yüksek demet gözden geçirme puanları bölerek başlayın. Kullandığımız [meta verileri Düzenle](https://msdn.microsoft.com/library/azure/dn905986.aspx) ve [grubu kategorik değerlere](https://msdn.microsoft.com/library/azure/dn906014.aspx) modüller.

![Etiket oluşturma](./media/text-analytics-module-tutorial/create-label.png)

Ardından, metin kullanarak temizleme [metin ön işleme](https://msdn.microsoft.com/library/azure/mt762915.aspx) modülü. Temizleme, en önemli özellikleri bulmak ve son modelin doğruluğunu artırmak Yardım kümesindeki gürültü azaltır. Biz stopword - "" veya "a" - gibi ortak kelimeler ve numaralarını, özel karakterler, yinelenen karakterler, e-posta adreslerini ve URL'leri kaldırın. Biz de küçük harf, sözcükleri lemmatize metne dönüştürün ve ardından tarafından belirtilen cümle sınırları algılamak "|||" önceden işlenmiş metin sembolü.

![Metin ön işleme](./media/text-analytics-module-tutorial/preprocess-text.png)

Peki özel stopword listesini kullanmak istiyorsunuz? İsteğe bağlı bir giriş içine geçirebilirsiniz. Özel kullanabilirsiniz C# alt dizeler değiştirin ve konuşma parçası tarafından sözcükleri kaldırmak için normal ifade söz dizimi: isimleri, fiil veya sıfat.

Ön işleme tamamlandıktan sonra biz verileri train bölme ve kümelerini test.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>2\. adım: Önceden işlenmiş metin sayısal özellik vektör ayıklayın
Metin verileri için bir model oluşturmak için genellikle sayısal özellik vektör serbest biçimli metin dönüştürmeniz gerekir. Bu örnekte [N-Gram özelliklerinden ayıklamak metin](https://msdn.microsoft.com/library/azure/mt762916.aspx) metin verileri gibi biçimine dönüştürmek için modülü. Bu modül, boşluk ayrılmış sözcüklerin bir sütun alır ve bir kelimelerin sözlük veya N-gram, veri kümesinde görünen sözcük hesaplar. Ardından, süreleri her sözcük veya N-gram, her kayıt görünür ve bu özellik vektörler oluşturur kaç sayıları sayar. Sunduğumuz özellik vektör tek sözcükler ve iki sonraki sözcük birleşimlerini içerir, böylece bu öğreticide, N-gram boyutu 2 olarak ayarladık.

![N-gram ayıklayın](./media/text-analytics-module-tutorial/extract-ngrams.png)

Biz TF uygulama * N-gram Ağırlıklandırma IDF (terimi sıklığı ters belge sıklık düzeyi) sayar. Bu yaklaşım, tek bir kayıtta sık görünür ancak veri kümesi genelinde nadir bir kelimelerin ağırlık ekler. Diğer Seçenekler ikili, TF, içerir ve ağırlığıyla grafik.

Bu metin özellikler genellikle yüksek işlenemez sahiptir. Gövde 100.000 benzersiz sözcük N-gram kullandıysanız Örneğin, özellik alanınızı 100.000 boyutları veya daha fazla varsa,. N-Gram özellikleri ayıklama modülü, boyut düzeyi azaltma seçenekleri kümesi sunar. Kısa veya uzun veya çok seyrek veya önemli Tahmine dayalı değerine sahip olacak şekilde çok sık kelimeler çıkarılacak seçebilirsiniz. Bu öğreticide, 5'ten az kayıtları veya % 80'birden fazla kayıt görünen N-gram tutarız.

Ayrıca, tahmin hedefi olan en bağıntılı olan özellikler seçmek için özellik seçimi kullanabilirsiniz. 1000 özellikleri seçmek için kikare özellik seçimi kullanın. Sözcüklerin veya N-gram sözlüğünü doğru Extract N-gram modülün çıkışına tıklayarak görüntüleyebilirsiniz.

Ayıklamak N-Gram özelliklerini kullanmak için alternatif bir yaklaşım, özellik karma modülünü kullanabilirsiniz. Ancak dikkat [özellik karma](https://msdn.microsoft.com/library/azure/dn906018.aspx) yerleşik özellik seçimi yetenekleri veya TF yok * IDF kolaylığı karşılaştırması.

## <a name="step-3-train-classification-or-regression-model"></a>3\. adım: Sınıflandırma veya regresyon modeli eğitme
Şimdi metin için sayısal özellik sütunu dönüştürülmüş. Sütunları Seç kümesinde dışında tutmak için kullanacağız veri kümesi önceki aşamalar dize sütunlarından yine de içerir.

Ardından kullandığımız [iki sınıflı Lojistik regresyon](https://msdn.microsoft.com/library/azure/dn905994.aspx) Hedefimiz tahmin etmek için: yüksek veya düşük gözden geçirme puanı. Bu noktada, metin analizi sorun, bir normal sınıflandırma sorunla dönüştürülmüş. Modeli geliştirmek için Azure Machine Learning Studio'da kullanılabilen araçları kullanabilirsiniz. Örneğin, farklı sınıflandırıcılar verdikleri nasıl doğru sonuçları bulmayı denemek veya hiper parametre ayarı doğruluğunu artırmak için kullanın.

![Eğitme ve puanı](./media/text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>4\. Adım: Puanlama ve modelini doğrulama
Nasıl eğitilen model doğrulama? Biz, test veri kümesinde puan ve doğruluğunu değerlendirin. Ancak, model N-gram ve eğitim kümesinden ağırlıkları sözlüğünü öğrendiniz. Bu nedenle, bu sözlük ve bu ağırlıkları özellikleri sözlük yenisini oluşturmak yerine test verileri ayıklanırken kullanmamız gereken. Bu nedenle, biz ayıklamak N-Gram özellikleri modülünü deneme Puanlama dalına ekleyin, çıkış sözlük eğitim daldan bağlanmak ve sözlük modunu salt okunur olarak ayarlayın. Biz de 1 örnek ve en fazla %100 için en düşük ayarlayarak N-gram sıklıkla filtresini devre dışı bırakın ve özellik seçimi devre dışı açın.

Sayısal özellik sütunu için test verilerini metin sütunu dönüştürüldükten sonra dize sütunlarındaki eğitim dal gibi önceki aşamada gelen kapsam dışında tutarız. Ardından Model Puanlama modülü tahminler elde etmeye ve doğruluğunu değerlendirilecek Model değerlendirme modülü kullanıyoruz.

## <a name="step-5-deploy-the-model-to-production"></a>5\. Adım: Üretim için model dağıtma
Model üretim ortamına dağıtılması neredeyse hazır. Web hizmeti olarak dağıtıldığında, serbest biçimli metin dizesi girdi olarak alır ve "Yüksek" veya "Düşük" Tahmin döndürür Öğrenilen N-gram sözlük, özellikler ve bu özelliği bir tahminde bulunmak için eğitilen Lojistik regresyon modeli metne dönüştürmek için kullanır. 

Tahmine dayalı deneme ayarlama, biz ilk N-gram sözlüğü veri kümesi ve eğitilen Lojistik regresyon modelini denemeyi eğitim daldan kaydedin. Ardından, deneme, Tahmine dayalı denemeye için bir deneme grafiğini oluşturmak için "Farklı Kaydet" kullanarak kaydedin. Biz, deneme verileri bölme modülü ve eğitim dal kaldırın. Biz sonra önceden kaydedilmiş N-gram sözlük ve model N-Gram özellikleri ayıklayın ve Score Model modülleri, sırasıyla bağlanır. Biz de Evaluate Model modülü kaldırın.

Biz Sütunları Seç metin ön işleme modülü etiket sütunu kaldırmak için önce Dataset modülü yerleştirin ve puan modülünde "Veri kümesine puanı sütun ekleme" seçeneğin işaretini kaldırın. Bu şekilde, web hizmeti, tahmin etmek için çalışıyor etiketi ve mu yanıtta giriş özellikleri echo istemez.

![Tahmine dayalı denemeye](./media/text-analytics-module-tutorial/predictive-text.png)

Artık bir web hizmeti olarak yayımlanan ve istek-yanıt ya da toplu iş yürütme API'leri kullanarak adlı bir denemeyi sahibiz.

## <a name="next-steps"></a>Sonraki Adımlar
Metin analiz modüllerini öğrenin [MSDN belgelerine](https://msdn.microsoft.com/library/azure/dn905886.aspx).

