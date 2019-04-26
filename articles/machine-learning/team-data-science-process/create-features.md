---
title: Veri bilimi - Team Data Science Process içinde özellik Mühendisliği
description: Özellik Mühendisliği amaçları açıklanır ve machine Learning veri geliştirme sürecinde rolü örnekleri sağlar.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/21/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8b0973007a78b492cff1c5ffc2ce1e43116a0847
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60398663"
---
# <a name="feature-engineering-in-data-science"></a>Veri bilimi, özellik Mühendisliği
Bu makalede, özellik Mühendisliği amaçları açıklanır ve machine Learning veri geliştirme sürecinde rolü örnekleri sağlar. Bu işlemi canlandırmak adına kullanılan örnekleri, Azure Machine Learning Studio'dan çizilir. 

Bu görev bir adımdır [Team Data Science işlem (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

Learning işlemini kolaylaştırmaya yardımcı olması ham verilerden özellikler oluşturarak öğrenimi algoritmaları, Tahmine dayalı power artırmak için mühendislik denemeleri özellik. Özellik Seçimi ve mühendislik özetlenen TDSP bir parçası olan [Team Data Science Process yaşam döngüsü nedir?](overview.md) Özellik Mühendisliği ve seçimi olan bölümleri **geliştirme özellikleri** TDSP. adımı. 

* **özellik Mühendisliği**: Bu işlem verilerinde mevcut ham özelliklerinden ilgili ek özellikler oluşturma ve Tahmine dayalı öğrenme algoritmasını gücünü artırmak için çalışır.
* **Özellik Seçimi**: Bu işlem özgün veri özellikleri anahtar kısmını eğitim sorunun işlenemez azaltmak girişimi seçer.

Normalde **özellik Mühendisliği** ek özellikler oluşturmak için öncelikle uygulanır ve ardından **özellik seçimi** adım, ilgisiz, yedekli ya da son derece bağıntılı özellikleri ortadan kaldırmak için gerçekleştirilir.

Machine learning'de kullanılan eğitim verileri, genellikle toplanan ham verileri ayıklama özellikleri tarafından geliştirilebilir. Bir bit ham bit dağıtım verilerden oluşturulmuş yoğunluk Haritası oluşturulmasını görüntüleri el yazısı karakterleri sınıflandırmak öğrenme bağlamında mühendislik uygulanan bir özellik örneğidir. Bu harita, kenarlarını karakterleri yalnızca ham dağıtım doğrudan kullanımına kıyasla daha verimli bir şekilde belirlemeye yardımcı olabilir.

Belirli ortamlarda verilerin özelliklerini oluşturma için aşağıdaki makalelere bakın:

* [SQL Server'daki verilerin özelliklerini oluşturma](create-features-sql-server.md)
* [Bir Hadoop kümesinde Hive sorgularını kullanarak verilerin özelliklerini oluşturma](create-features-hive.md)

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="create-features-from-your-data---feature-engineering"></a>Verilerinizden - özelliklerini oluşturma özellik Mühendisliği
Eğitim verileri, her biri özellikleriyle (değişkenleri veya sütunlarda depolanan alanları) sahip örnekleri (kayıtları veya gözlemleri satırlarda depolanan) oluşan bir matris oluşur. Deneysel tasarımında belirtilen özellikleri, veri desenlerinde niteleyen beklenir. Ham veri alanlarının çoğunu doğrudan bir modeli eğitmek için kullanılan seçili özellik kümesine eklenebilir olsa da, genellikle ek (tasarlanan) özellikleri bir Gelişmiş bir eğitim veri kümesi oluşturmak için ham verilerdeki özelliklerinden oluşturulması gerektiğini durum geçerlidir.

Veri kümesi bir modeli eğitimindeki geliştirmek için ne tür bir özellikler oluşturulsun mu? Eğitim geliştiren mühendislik uygulanan özellikleri, verilerdeki desenleri daha iyi ayırır bilgileri sağlar. Açıkça yakalanan ya da bir kolayca görülebilecek özgün veya varolan bir özellik kümesi, ek bilgi sağlamak için yeni özellikler bekleniyor. Ancak bu işlem, bir resimler şeydir. Ses ve üretken kararları, genellikle bazı etki alanı uzmanlığı gerektirir.

Azure Machine Learning'i kullanmaya Başlarken concretely Studio'da sağlanan örnekleri kullanarak bu işlemi kavramak kolaydır. Burada sunulan iki örnek:

* Regresyon örnek [bisiklet kiralama sayısını tahmin](https://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) hedef değerleri bilinir burada denetimli bir denemede
* Bir metin araştırma sınıflandırma örneği kullanarak [özellik karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

## <a name="example-1-add-temporal-features-for-a-regression-model"></a>Örnek 1: Zamana bağlı bir regresyon modeli özellikleri ekleyin
"Talep tahmini bisiklet" denemeyi Azure Machine Learning Studio'da bir regresyon görev için özellik işlemini göstermek için kullanalım. Bu deneyde amaç, diğer bir deyişle, bir özel ay/gün/saat içinde bisiklet kiralama sayısını bisiklet talebini tahmin etmektir. Veri kümesi "bisiklet kiralama UCI veri kümesi" ham girdi verisi olarak kullanılır. Bu veri kümesi bir Amerika Birleşik Devletleri'nde, Washington DC bisiklet kiralama ağında tutan büyük Bikeshare şirketin gerçek verileri temel alır. Veri kümesi, 2011 ve yılı 2012 yıllarda günün belirli bir saat içinde bisiklet kiralama sayısını temsil eder ve 17379 satırları ve 17 sütunları içerir. Hava koşulları (sıcaklık/nem/Rüzgar hızı) ve günlük (tatil/haftanın günü) türü ham özellik kümesini içerir. Tahmin etmek için "cnt" sayısı belirli bir saat içinde bisiklet kiralama temsil eden ve hangi 977 için 1 ile arasında bir alandır.

Eğitim verileri, dört regresyon modellerini aynı algoritmayı kullanılarak oluşturulan ancak dört farklı bir eğitim veri kümeleri ile etkin özellikler oluşturmak hedefle. Dört veri kümeleri aynı ham girdi verilerini temsil eder, ancak artan sayıda özellikleri ile ayarlayın. Bu özellikler, dört kategorilerde gruplanır:

1. Bir hafta özellikleri için tahmin edilen günlük hava durumu, tatil + hafta içi = +
2. B her önceki 12 saat içinde gibi verilere dayanarak bisiklet sayısını =
3. C gibi aynı saat, 12 önceki günde her verilere dayanarak bisiklet sayısını =
4. D gibi her birinde aynı saat ve aynı gün önceki 12 Hafta verilere dayanarak bisiklet sayısını =

Özgün ham verileri kullanarak zaten özellik kümesi A yanı sıra, işlem mühendislik özelliği aracılığıyla diğer üç özellik kümesi oluşturulur. Özellik B yakalamaları bisiklet çok son talep ayarlayın. Özelliği, belirli bir saatte bisiklet talebini C yakalamaları ayarlayın. Özelliği, belirli bir saat ve haftanın belirli gün bisiklet D yakalamaları talep ayarlayın. Dört eğitim veri kümeleri her özellik kümesi A, A + B, A + B + C ve A + B + C + D, sırasıyla içerir.

Azure Machine Learning denemesi bu dört eğitim veri kümeleri aracılığıyla dört dallardan önceden işlenmiş giriş veri kümesi oluşturulur. En soldaki dal dışında her biri bu dallar içeren bir [R betiği yürütme](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modül içinde (özellik kümesi B ve C D) türetilen özellikleri sırasıyla oluşturulur ve içeri aktarılan veri kümesine eklenir. Aşağıdaki şekilde ikinci sol dalda B özellik kümesi oluşturmak için kullanılan bir R betiği gösterir.

![Özellikleri oluşturma](./media/create-features/addFeature-Rscripts.png)

Performans sonuçlarını olan dört model oluşturduğunuz bir karşılaştırmasını aşağıdaki tabloda özetlenmiştir: 

![Sonuç karşılaştırma](./media/create-features/result1.png)

En iyi sonuçları A + B + C özellikleri tarafından gösterilir. Hata oranının ne zaman azaltır Not ek özellik kümesi, eğitim verileri dahil edilir. Bu özellik kümesi B, C regresyon görev ilgili ek bilgileri sağlayın presumption doğrular. Ancak, ek hata oranının kısalma sağlamak için D özellik ekleme görünmüyor.

## <a name="example2"></a> Örnek 2: İçinde metin araştırma özellikleri oluşturma
Özellik Mühendisliği, metin madenciliği, belge sınıflandırma ve yaklaşım analizi gibi ilgili görevleri yaygın olarak uygulanır. Belgeler, çeşitli kategorilerde sınıflandırmak istediğiniz zaman, örneğin, bir tipik bir belge kategorisindeki word/tümcecikleri başka bir belge kategorisinde oluşma olasılığı daha varsayılır. Diğer bir deyişle, sözcük/tümcecikleri dağıtım sıklığını farklı Belge kategorileri niteleyen kuramıyor. Metin içeriği tek tek parça genellikle girdi verisi olarak gördükleri için metin araştırma uygulamalarda, sözcüğün/tümceciğin frekansları ilgili özellikler oluşturmak için işlem mühendislik özelliği gereklidir.

Bu görevi gerçekleştirmek için bir yöntem olarak adlandırılan **özellik karma** verimli bir şekilde dizinler rastgele metin özellikleri etkinleştirmek için uygulanır. Her metin özelliği (sözcük/tümcecikleri) belirli bir dizinden bir karma işlevi özellikleri için uygulama ve dizinlerin karma değerlerini kullanarak, doğrudan bu yöntemi işlevleri ilişkilendirilmek yerine.

Azure Machine Learning'de var. bir [özellik karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) bu oluşturan modülü sözcük/tümcecik özellikleri uygun şekilde. Aşağıdaki şekilde bu modülü kullanarak bir örnek gösterilmektedir. İki sütun giriş veri kümesi içerir: 1'den 5'e kadar kitap derecelendirme ve gerçek gözden geçirme içeriği. Bu [özellik karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modülüdür özellikleri karşılık gelen sözcükleri yinelenme sıklığını gösteren yeni bir sürü almak için / içinde belirli bir kitap phrase(s) gözden geçirin. Bu modülü kullanmak için aşağıdaki adımları tamamlayın:

* İlk olarak, giriş metni içeren sütunu seçin (Bu örnekte "Sütun2").
* İkinci olarak, 8'e ayarlayın 2 anlamına gelir "Hashing bitsize" ^ 8 = 256 özellikleri oluşturulacak. Tüm metni word/aşamasında 256 İndekslere karma. "Hashing bitsize" parametresini 1 ile 31 arasında. Sözcükler / phrase(s) aynı dizine daha büyük bir sayı olarak ayarlıyorsanız sağlaması olasılığı.
* Üçüncü parametre 2 "N-gram" olarak ayarlayın. Bu değer, yinelenme sıklığı unigrams (her tek sözcük için bir özelliği) ve bigrams (bitişik bir kelimelerin her çifti için bir özellik) giriş metni alır. Parametresi, 0'dan gösteren bir özellik eklenmesini sıralı sözcük sayısı 10 "N-gram" aralıkları.  

!["Hashing özelliği" modülü](./media/create-features/feature-Hashing1.png)

Aşağıdaki şekilde hangi bu yeni gösterilmektedir özellik görünür.

![Örnek "Hashing özelliği"](./media/create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Sonuç
Tasarlanmış ve seçilen özelliklerin verileri yer alan anahtar bilgileri ayıklamak için çalışır eğitim işleminin verimliliğini artırın. Bunlar ayrıca giriş verileri doğru bir şekilde sınıflandırmak için ve ilgilendiğiniz sonuçları yerine daha tahmin etmek üzere bu modellerinin gücünden geliştirin. Öğrenme hesaplama açısından daha tractable yapmak için özellik Mühendisliği ve seçimi birleştirebilirsiniz. Bunu geliştirmek ve ardından ayarlamak veya bir model eğitip için gereken özellikleri sayısını azaltmayı yapar. Matematiksel anlamda, modeli eğitmek için seçilen en az sayıda bağımsız değişkenlere, veri desenlerinde açıklamak ve sonuçlar'ı başarıyla tahmin özellikleridir.

Her zaman mutlaka özellik Mühendisliği veya özellik seçimi gerçekleştirmek için değil. Olup olmadığını veya gerekli verilere el bağlıdır veya toplandı, seçili algoritma ve deneme amacı.

