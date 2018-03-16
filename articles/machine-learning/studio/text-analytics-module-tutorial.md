---
title: "Metin analiz modelleriniz Azure Machine Learning Studio'da oluşturun. | Microsoft Docs"
description: "Ön işleme metin, N-gram veya özellik karma modülleri kullanarak Azure Machine Learning Studio'daki analiz modelleriniz metin oluşturma"
services: machine-learning
documentationcenter: 
author: rastala
manager: jhubbard
editor: 
ms.assetid: 08cd6723-3ae6-4e99-a924-e650942e461b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: roastala
ms.openlocfilehash: 0c2dc3bad6973b7b00f0ed44231e78298f4422fb
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio’da metin analiz modelleri oluşturma
Azure Machine Learning, yapı ve metin analiz modelleriniz faaliyete geçirmek için kullanabilirsiniz. Bu modeller, örneğin, belge sınıflandırma veya düşünceleri çözümleme sorunlarını çözmenize yardımcı olabilir.

Metin analiz denemesi genellikle gerekir:

1. Temizleme ve metin dataset önişle
2. Sayısal özellik vektörlerinin önceden işlenmiş metin ayıklamanızı
3. Sınıflandırma veya regresyon modelini eğitme
4. Puanlama ve model doğrulama
5. Model üretime dağıtma

Bu öğreticide, biz Amazon Kitap incelemeleri veri kümesini kullanarak düşünceleri analiz modeli aracılığıyla yol olarak bu adımları öğrenin (Bu araştırma incelemesine bakın "Biyografileri, Bollywood, müzik kutuları ve Blenders: etki alanı uyarlama düşünceleri sınıflandırma" John Blitzer, işareti Dredze ve Fernando Pereira; İlişkilendirmesini hesaplama Linguistics (ACL) 2007.) Bu veri kümesi gözden geçirme puanları (1-2 veya 4-5) ve serbest biçimli metin oluşur. Gözden geçirme puan tahmin etmek için belirtilir: düşük (1 - 2) veya yüksek (4-5).

Bu öğreticide Azure AI Galerisi kapsamdaki denemeler bulabilirsiniz:

[Kitap incelemeleri tahmin etme](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Kitap incelemeleri - Tahmine dayalı denemeye tahmin etme](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>1. adım: Temizleyin ve metin dataset önişle
Biz, denemeyi iki sınıflı sınıflandırma sorunu formüle için kategorik düşük ve yüksek demet gözden geçirme puanları bölerek başlayın. Kullanırız [Düzenle meta veri](https://msdn.microsoft.com/library/azure/dn905986.aspx) ve [Grup kategorik değerleri](https://msdn.microsoft.com/library/azure/dn906014.aspx) modüller.

![Etiket oluşturma](./media/text-analytics-module-tutorial/create-label.png)

Ardından, metin kullanarak temiz [Önişle metin](https://msdn.microsoft.com/library/azure/mt762915.aspx) modülü. Temizleme kümesindeki en önemli özellikleri bulmak ve son modelin doğruluğunu artırmanıza yardımcı gürültü azaltır. Biz Durma sözcükleri - "" veya "a" - gibi sık kullanılan sözcük ve sayılar, özel karakterler, yinelenen karakterler, e-posta adreslerini ve URL'leri kaldırın. Biz de küçük harf, sözcük lemmatize metne dönüştürmek ve ardından belirtilir cümle sınırları algılamak "|||" simgesi önceden işlenmiş metin.

![Metin önişle](./media/text-analytics-module-tutorial/preprocess-text.png)

Ne özel Durma sözcükleri listesini kullanmak istediğiniz? İçinde isteğe bağlı giriş olarak geçirebilirsiniz. Ayrıca, alt dizeler değiştirmek için özel C# sözdizimi normal ifade kullanın ve konuşma parçası sözcükler kaldırın: isimleri, fiilleri veya sıfatları.

Ön işleme tamamlandıktan sonra biz tren veri bölme ve kümelerini test.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>2. adım: sayısal özellik vektörlerinin önceden işlenmiş metin ayıklar.
Metin verileri için bir model oluşturmak için genellikle sayısal özellik vektörlerinin serbest biçimli metin dönüştürmeniz gerekir. Bu örnekte, kullandığımız [N-Gram özelliklerinden ayıklamak metin](https://msdn.microsoft.com/library/azure/mt762916.aspx) metin verileri gibi biçimine dönüştürmek için modülü. Bu modül boşluk ayrılmış sözcükler sütunu alır ve bir sözlük sözcüklerin veya N-gram dataset içinde görünen sözcüklerin hesaplar. Ardından, her kez sözcük veya N-gram, her bir kayıtta görüntülenir ve bu özellik vektörler oluşturur kaç sayıları sayar. Tek tek sözcükleri ve iki sonraki sözcük birleşimlerini bizim özelliği vektörlerinin dahil şekilde Bu öğreticide, N-gram boyutu 2'ye ayarlarız.

![N-gram Ayıkla](./media/text-analytics-module-tutorial/extract-ngrams.png)

Biz TF uygulamak * N-gram ağırlığı IDF (terim sıklığı ters belge sıklığı) sayar. Bu yaklaşım, tek bir kayıtta sık görünür, ancak tüm veri kümesi arasında nadiren sözcükler ağırlığını ekler. Diğer seçenekler ve başvuracaklarını grafik ikili, TF, içerir.

Metin özellikleri genellikle yüksek boyut sahiptir. Gövde 100.000 benzersiz sözcükler varsa, N-gram kullanılıyorsa, örneğin, özellik alanınızı 100.000 boyutları ya da daha fazla gerekir. N-Gram özellikleri ayıklamak modülü bir dizi boyut azaltmak için seçenekler sağlar. Kısa veya uzun veya çok seyrek veya çok sık önemli Tahmine dayalı değerine sahip olacak şekilde sözcükler dışlamayı seçebilirsiniz. Bu öğreticide, 5'ten az kayıtları veya kayıtları % 80'den fazla görünür N-gram dışlayın.

Ayrıca, en bağıntılı tahmin hedefe sahip olan özellikleri seçmek için özellik seçimi kullanabilirsiniz. Kikare özellik seçimi 1000 özellikleri seçmek için kullanırız. Seçili sözcükleri veya N-gram sözlüğünü Extract N-gram modülü sağ çıktısı tıklayarak görüntüleyebilirsiniz.

Alternatif bir yaklaşım ayıklamak N-Gram özelliklerini kullanmak için bilgi, özellik karma modülünü kullanabilirsiniz. Unutmayın ancak [özellik karma](https://msdn.microsoft.com/library/azure/dn906018.aspx) yapı içinde özellik seçimi özelliklerini veya TF yok * IDF başvuracaklarını.

## <a name="step-3-train-classification-or-regression-model"></a>3. adım: sınıflandırma veya regresyon modelini eğitme
Artık metin sayısal özellik sütunları dönüştürülmüş. Hala veri kümesi önceki aşamaları dize sütunlarından içerdiğinden, Sütunları Seç kümesinde dışında tutmak için kullanırız.

Ardından kullanırız [iki sınıflı Lojistik regresyon](https://msdn.microsoft.com/library/azure/dn905994.aspx) bizim hedef tahmin etmek için: yüksek veya düşük gözden geçirme puanı. Bu noktada, metin analytics sorun bir normal sınıflandırma sorunla dönüştürülmüş. Modeli geliştirmek için Azure Machine Learning ile kullanılabilen araçlar kullanabilirsiniz. Örneğin, verdikleri nasıl doğru sonuçlar bulmak için farklı sınıflandırıcı denemeniz veya doğruluğunu artırmak için ayarlama hyperparameter kullanın.

![Tren ve puanı](./media/text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>4. adım: Puanlama ve model doğrulama
Nasıl eğitilen model doğrulamak? Biz karşı test veri Puanlama ve doğruluk değerlendirin. Ancak, model N-gram ve ağırlıklarını eğitim kümesinden sözlüğünü öğrendiniz. Bu nedenle, biz o dağarcığı ve bu ağırlıkları özellikleri sözlüğünü mı başlatacağı oluşturma aksine test verilerden çıkarılırken kullanmanız gerekir. Bu nedenle, biz deneme Puanlama dala ayıklamak N-Gram özellikleri Modül Ekle, çıktı sözlük eğitim daldan bağlanmak ve sözlük modu salt okunur olarak ayarlayın. Biz de en az 1 örneği ve en fazla % 100 ayarlanarak filtreleme N-gram frekans tarafından devre dışı bırakın ve özellik seçimi devre dışı bırakma.

Test verileri metin sütununda sayısal özellik sütunları dönüştürüldükten sonra biz dize sütunlarındaki önceki aşamada eğitim şube gibi dışında bırakın. Ardından tahminlerde için Score Model modülü ve Evaluate Model modülünün doğruluğu değerlendirmek için kullanırız.

## <a name="step-5-deploy-the-model-to-production"></a>5. adım: model üretime dağıtabilirsiniz.
Üretim için neredeyse hazır modelidir. Web hizmeti olarak dağıtıldığında, serbest biçimli metin dizesini giriş olarak alır ve "Yüksek" veya "Düşük" Tahmin Döndür Öğrenilen N-gram sözlük özellikleri ve bu özelliklerinden tahminde bulunmak için eğitilmiş Lojistik regresyon modeli metne dönüştürmek için kullanır. 

Tahmine dayalı denemeye ayarlamak için biz ilk veri kümesi olarak N-gram dağarcığı ve eğitilen Lojistik regresyon modeli deneme eğitim daldan kaydedin. Ardından, Tahmine dayalı denemeye için bir deneme grafiği oluşturmak için "Farklı Kaydet" kullanarak deneme kaydedin. Biz, deneme bölünmüş veri modülü ve eğitim dalı kaldırın. Biz sonra önceden kaydedilmiş N-gram dağarcığı ve model N-Gram özellikleri ayıklamak ve Score Model modüller, sırasıyla bağlanır. Biz de Evaluate Model modülünü Kaldır.

Biz Önişle metin modülü etiket sütunu kaldırmak için önce veri kümesi modülünde Sütunları Seç takın ve puanı modülünde "Veri kümesine ekleme puan sütunu" seçeneğinin işaretini kaldırın. Böylece, web hizmeti tahmin etmek için çalışıyor etiketi ve mu yanıtta giriş özellikleri Yankı istemez.

![Tahmine dayalı denemeye](./media/text-analytics-module-tutorial/predictive-text.png)

Artık bir web hizmeti olarak yayınlanan ve istek-yanıt ya da toplu iş yürütme API'lerini kullanarak adlı bir denemeyi vardır.

## <a name="next-steps"></a>Sonraki Adımlar
Metin analizi modüllerden hakkında bilgi edinin [MSDN belgelerine](https://msdn.microsoft.com/library/azure/dn905886.aspx).

