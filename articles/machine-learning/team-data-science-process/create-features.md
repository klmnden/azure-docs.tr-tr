---
title: Özellik Mühendisliği veri bilimi | Microsoft Docs
description: Özellik Mühendisliği amaçları açıklar ve makine öğrenme veri geliştirme işleminde rolü örnekleri sağlar.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 3fde69e8-5e7b-49ad-b3fb-ab8ef6503a4d
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: deguhath
ms.openlocfilehash: b4194ef5ab1c2c09206ea0acf78cb539bc2fc0b7
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836526"
---
# <a name="feature-engineering-in-data-science"></a>Veri bilimi özellik Mühendisliği
Bu makalede özellik Mühendisliği amaçları açıklar ve makine öğrenme veri geliştirme işleminde rolü örnekleri sağlar. Bu işlem göstermek için kullanılan örnekler, Azure Machine Learning Studio'dan çizilir. 

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Bu **menü** özellikleri veriler için çeşitli ortamlar oluşturmak nasıl açıklar makalelerinin bağlantıları. Bu görev bir adımdır [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Öğrenme işlemini kolaylaştırmak ham verilerden özellikler oluşturarak algoritmaları öğrenme Tahmine dayalı güç artırmak için mühendislik denemeleri özelliği. Mühendislik ve çeşitli özellikler bölümünde özetlenen TDSP bir parçası olan [takım veri bilimi işlemi yaşam döngüsü nedir?](overview.md) Özellik Mühendisliği ve seçim olan bölümleri **geliştirmek özellikleri** TDSP adımında. 

* **özellik Mühendisliği**: Bu işlem veri varolan ham özelliklerinden ek ilgili özellikler oluşturmak ve öğrenme algoritmasını Tahmine dayalı gücünü artırmak için çalışır.
* **Özellik Seçimi**: Bu işlem özgün veri özelliklerinin anahtar kısmı eğitim sorun boyut azaltmak girişimi seçer.

Normalde **özellik Mühendisliği** ek özellikler oluşturmak için önce uygulanır ve daha sonra **özellik seçimi** adım ilgisiz, artık ya da son derece bağıntılı özellikleri ortadan kaldırmak için gerçekleştirilir.

Machine learning'de kullanılan eğitim verileri genellikle özellikleri ayıklama toplanan ham verilerden tarafından geliştirilebilir. El yazısı karakter görüntülerini sınıflandırmak öğrenme bağlamında tasarlanan bir özelliği bir bit ham bit dağıtım verilerinden oluşturulan yoğunluğu harita oluşturulmasını örnektir. Bu haritada karakterleri kenarlarına yalnızca ham dağıtım doğrudan kullanımına kıyasla daha verimli bir şekilde belirlemeye yardımcı olabilir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="create-features-from-your-data---feature-engineering"></a>Verilerinizden - özellikler oluşturmak özellik Mühendisliği
Eğitim verileri her biri bir dizi özellik (değişkenleri veya sütunlarında depolanan alanları) sahip örnekleri (kayıtları veya satır depolanan gözlemleri) oluşan bir matris oluşur. Deneysel tasarımında belirtilen özellikleri verilerdeki düzenleri niteleyen beklenir. Ham veri alanlarını çoğunu doğrudan bir modeli eğitmek için kullanılan seçili özellik kümesine eklenebilir olsa da, genellikle ek (tasarlanan) Özellikleri Gelişmiş eğitim veri kümesi oluşturmak için ham verilerdeki özelliklerinden oluşturulması gerektiğini durum geçerlidir.

Veri kümesi bir modeli eğitimindeki geliştirmek için ne tür bir özellikler oluşturulsun mu? Eğitim geliştirmek tasarlanan özellikleri daha iyi verilerdeki düzenleri ayıran bilgi sağlar. Yeni özellikler açıkça yakalanan ya da özgün veya var olan özellik kümesi kolayca görünen ek bilgiler sağlayan beklenir. Ancak bu işlem bir resimler bir şeydir. Ses ve üretken kararları genellikle bazı etki alanı uzmanlık gerektirir.

Azure Machine Learning ile başlatırken concretely Studio'da sağlanan örnekleri kullanarak bu işlemi kavrayın en kolayıdır. İki örnek burada sunulur:

* Regresyon örnek [bisiklet kiralamaları sayısını tahmin](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) hedef değerlerini nerede bilinen denetimli bir deney olarak
* Bir metin araştırma sınıflandırma örneği kullanarak [özellik karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

## <a name="example-1-add-temporal-features-for-a-regression-model"></a>Örnek 1: bir regresyon modeli için zamana bağlı özellik ekleme
"İsteğe bağlı tahmin bisiklet" deneme Azure Machine Learning Studio'da bir regresyon görev özellikleri mühendislik nasıl yapılacağını göstermek üzere kullanalım. Bu deneme amacı bisiklet, diğer bir deyişle, belirli bir ay/gün/saati içinde bisiklet kiralamaları sayısı talep tahmin etmektir. Veri kümesi "bisiklet kiralama UCI dataset" ham girdi verisi olarak kullanılır. Bu veri kümesi, Washington DC Amerika Birleşik Devletleri'nde bir bisiklet kiralama ağda tutar sermaye Bikeshare şirketten gerçek veri dayanır. Veri kümesi 2011 ve yıl 2012 yıllarda günün belirli bir saat içinde bisiklet kiralamaları sayısını temsil eder ve 17379 satırları ve 17 sütunları içerir. İşlenmemiş özellik kümesi hava koşulları (nem/sıcaklık/Rüzgar hızı) ve günlük (tatil/hafta içi günü) türünü içerir. Tahmin etmek için belirli bir saat içinde bisiklet kiralamaları temsil eden ve hangi 977 için 1'den aralıkları "cnt" sayısı alanıdır.

Eğitim verileri, modelleri aynı algoritması kullanılarak oluşturulan dört regresyon ancak dört farklı eğitim kümeleriyle etkin özellikleri oluşturma hedefle. Dört veri kümeleri aynı ham giriş verilerini temsil eden, ancak Özellikler artan sayıda ile ayarlayın. Bu özellikler, dört kategorilerde gruplanır:

1. A = hava durumu, tatil + haftanın günü + hafta özellikleri için tahmin edilen gün
2. B = her önceki 12 saat içinde rented bisiklet sayısı
3. C = her birinde aynı saat önceki 12 günde rented bisiklet sayısı
4. D = her birinde aynı saat ve aynı gün önceki 12 Hafta rented bisiklet sayısı

Özgün ham verileri zaten, özellik kümesi A yanı sıra, diğer üç özellik kümesi işlemi mühendislik özelliği kullanılarak oluşturulur. Özellik B yakalamaları bisiklet için çok yeni talep ayarlayın. Özellik C yakalamaları bisiklet talep belirli bir saatte ayarlayın. Özellik D yakalamaları talep bisiklet için belirli bir saat ve haftanın belirli günü ayarlayın. Dört eğitim veri kümelerinin her özellik kümesi A, A + B, A + B + C ve A + B + C + D, sırasıyla içerir.

Azure Machine Learning deneme önceden işlenen giriş kümesinden dört dalları aracılığıyla bu dört eğitim veri kümeleri oluşturulur. Soldaki dal dışında her bu dallar içeren bir [R betiği yürütün](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modülü, (özellik kümesinin B, C ve D) türetilen özellikleri sırasıyla oluşturulan ve içeri aktarılan veri kümesine eklenir. Aşağıdaki şekilde ikinci sol dalda özellik kümesi B oluşturmak için kullanılan R betiği gösterilmektedir.

![özellikleri oluşturma](./media/create-features/addFeature-Rscripts.png)

Performans Sonuçları dört modellerin karşılaştırması aşağıdaki tabloda özetlenmiştir: 

![Sonuç karşılaştırma](./media/create-features/result1.png)

En iyi sonuçlar özellikleri tarafından A + B + C gösterilmektedir. Hata oranı ne zaman azaltır Not ek özellik kümesi eğitim verileri dahil edilir. Özellik kümesi B, C regresyon görev için ek ilgili bilgiler sağlayan presumption doğrular. Ancak herhangi bir ek azaltma hata oranı sağlamak için D özellik ekleme görünmüyor.

## <a name="example2"></a> Örnek 2: özellikleri metin araştırma oluşturma
Özellik Mühendisliği belge sınıflandırma ve düşünceleri analiz gibi metin araştırma ilgili görevler yaygın olarak uygulanır. Çeşitli kategorilerde belgeleri sınıflandırmak istediğiniz zaman, örneğin, bir tipik bir belge kategorisindeki word/tümcecikleri başka bir belge kategorisinde ortaya olasılığı daha düşüktür varsayılır. Diğer bir deyişle, sözcük/tümcecikleri dağıtım sıklığını farklı Belge kategorileri niteleyen yapabiliyor. Metin içeriği tek tek parçaları genellikle girdi verisi olarak gördükleri için metin araştırma uygulamalarda sözcük/tümcecik sıklıklarını içeren özellikler oluşturmak için işlem mühendislik özelliği gereklidir.

Bu görev elde etmek için bir yöntem olarak adlandırılan **özellik karma** verimli bir şekilde dizinlerini rastgele metin özellikleri etkinleştirmek için uygulanır. Bu yöntem işlevleri özellikleri karma işlevi uygulayarak ve karma değerlerini dizinlerini doğrudan kullanarak belirli bir dizin için her metin özelliği (sözcükler/tümceleri) ilişkilendirme yerine.

Azure Machine Learning ile var olan bir [özellik karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) bunlar oluşturur modülü sözcük/tümcecik özelliklerini kolayca. Aşağıdaki şekilde bu modülü kullanmanın bir örneği gösterilmektedir. İki sütun girdi veri kümesi içerir: 5, 1 ile arasında değişen kitap derecelendirme ve gerçek gözden geçirme içeriği. Bu amacı [özellik karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modülüdür özellikleri karşılık gelen sözcükler oluşum sıklığını gösteren bir dizi yeni almak için / phrase(s) belirli kitap içinde gözden geçirin. Bu modülü kullanmak için aşağıdaki adımları tamamlayın:

* İlk olarak, giriş metni içeren bir sütun seçin (Bu örnekte "Col2").
* İkinci olarak, yani 2 "Hashing bitsize" 8'e ayarlayın ^ 8 = 256 özellikleri oluşturulacak. Tüm metni word/aşamasında 256 dizinlerini karma. Parametre "Hashing bitsize" aralığı 1-31. Sözcükler / phrase(s) aynı dizine daha büyük bir sayı olması için ayarlama, karma hale getirilmesi olasılığı.
* Üçüncü parametresi 2 "N-gram" olarak ayarlayın. Bu değer unigrams (özelliği her tek word için) ve bigrams (bitişik sözcüklerin her çifti için bir özellik) oluşumu sıklığını giriş metni alır. Parametresi "N-gram" 0'dan bir özelliği dahil edilecek sıralı sözcükler maksimum sayısını gösterir. 10 aralıkları.  

!["Hashing özelliği" modülü](./media/create-features/feature-Hashing1.png)

Aşağıdaki şekilde hangi bu yeni gösterilmektedir özelliği görünümlü.

!["Hashing özelliği" örnek](./media/create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Sonuç
Tasarlanan ve seçilen özelliklerin verilerinde bulunan anahtar bilgileri ayıklamak için çalışır eğitim işlem verimliliğini artırır. Bunlar ayrıca giriş verileri doğru şekilde sınıflandırmak ve sonuçları ilgi daha yerine tahmin etmek için bu modeller gücü geliştirin. Öğrenme daha pkı'ya tractable yapma özelliği mühendislik ve seçim birleştirebilirsiniz. Bunu arttırma ve ayarlama veya modeli eğitmek için gereken özellikleri sayısının azaltılması yapar. Matematiksel konuşarak, modeli eğitmek için seçilen özellikler en az bağımsız değişkenlerinin veri desenleri açıklar ve sonuçları başarıyla tahmin kümesidir.

Bu her zaman olmak zorunda özellik Mühendisliği veya özellik seçimi gerçekleştirmek için değildir. Olup olmadığını veya gerekli verilere el bağlıdır veya toplanan, seçili algoritması ve deneme amacı.

