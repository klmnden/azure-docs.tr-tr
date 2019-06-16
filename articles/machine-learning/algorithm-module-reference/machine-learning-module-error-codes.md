---
title: Modül hatalarını giderme
titleSuffix: Azure Machine Learning service
description: Hata kodlarını kullanarak Azure Machine Learning Studio'da modülü özel durum sorunlarını giderme
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 09a2b616e2bba93be86241c64d37daec7d6dea3b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029768"
---
# <a name="exceptions-and-error-codes-for-algorithm--module-reference"></a>Özel durumlar ve algoritma ve modül başvurusu için hata kodları

Azure Machine Learning Studio'da modüllerini kullanarak karşılaşabileceğiniz özel durum kodları ve hata iletileri hakkında bilgi edinin. 

Sorunu çözmek için yaygın nedenler hakkında okumak için bu makaledeki hata arayın. Bir hata iletisinin tam metin Studio'da almanın iki yolu vardır:  
 
- Bağlantıyı **çıkış Günlüğü Görüntüle**, sağ bölmede, En Alta kadar kaydır. Pencerenin en son iki satır, ayrıntılı hata iletisi görüntülenir.  
  
- Hata içeren modülü seçin ve kırmızı X simgesini tıklatın. Yalnızca ilgili hata metni görüntülenir.  
  
Hata iletisi metni yararlı değilse, bize bağlamı ve istenen eklemeler veya değişiklikler hakkında bilgi gönderin. Hata konu hakkında geri bildirim göndermek veya ziyaret [Azure Machine Learning STUDIO Forumu](https://aka.ms/aml-forum-studio) ve bir soru gönderin.  


## <a name="error-0001"></a>Hata 0001  
 Bir veya daha fazla veri kümesinin belirtilen sütunlar bulunamadı, özel durum meydana gelir.  
  
 Sütun seçimini bir modül için yapılır, ancak seçilen sütunların giriş veri kümesinde yok, bu hatayı alırsınız. Bir sütun adı el ile yazdıysanız veya sütun seçiciyi denemeyi çalıştırdığınızda kümenizde yok önerilen bir sütun sağlanırsa, bu hata oluşabilir.  
  
**Çözüm:** Bu özel durum modülü yeniden ziyaret ve başvurulan tüm sütunların var ve sütun adı veya adları doğru olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Bir veya daha fazla belirtilen sütunlar bulunamadı|  
|Sütun adı veya dizin ile "{0}" bulunamadı|  
|Sütun adı veya dizin ile "{0}"yok"{1}"|  
 

## <a name="error-0002"></a>Hata 0002  
 Bir veya daha fazla parametre başlatılamadı, ayrıştırılmış veya dönüştürülmüş öğesinden belirtilen türü hedef yöntem türü tarafından gereken özel durum oluşur.  
  
 Örtük dönüştürme gerçekleştirilemiyor giriş olarak bir parametre belirtin ve bir değer türü beklenen türünden farklı olan Azure Machine Learning'de bu hata oluşur.  
  
**Çözüm:** Modül gereksinimlerini denetleyin ve hangi değer türü gerekip gerekmediğini (tamsayı, dize, çift, vb..)  
  
|Özel durum iletileri|  
|------------------------|  
|Parametre ayrıştırılamadı|  
|Ayrıştırılamadı "{0}" parametresi|  
|(Dönüştürme) ayrıştırılamadı "{0}"parametre"{1}"|  
|Dönüştürme başarısız oldu "{0}"parametresi"{1}"hedef"{2}"|  
|Dönüştürme başarısız oldu "{0}"parametre değeri"{1}"Kimden"{2}"hedef"{3}"|  
|Değer dönüştürme başarısız oldu "{0}"sütununda"{1}"Kimden"{2}"hedef"{3}"ile kullanım biçimi"{4}" sağlanan|  
  

## <a name="error-0003"></a>0003 hata  
 Özel durum oluşur veya daha fazla girişleri null veya boş.  
  
 Tüm girişleri veya bir modüle parametreleri null veya boş ise, Azure Machine Learning'de bu hatayı alırsınız.  Örneğin, bir parametre için herhangi bir değer yazmadınız olduğunda bu hata ortaya çıkabilir. Eksik değerleri olan bir veri kümesi ya da boş bir veri kümesi seçtiğiniz zaman da gerçekleşebilir.  
  
**Çözüm:**
 
+ Özel durum üreten modülün açın ve tüm girişleri belirttiğinizden emin olun. Gerekli tüm girişleri belirtilmiş olduğundan emin olun. 
+ Azure depolama biriminden yüklenen verilerin erişilebilir olduğunu ve belirttiğiniz hesap adı ve anahtarı değiştirilmedi emin olun.  
+ Değerleri eksik veya null değerler için giriş verileri kontrol edin.
+ Bir veri kaynağında bir sorgu kullanarak beklediğiniz biçiminde veri döndürülmüyor doğrulayın. 
+ Yazım hatası veya veri belirtiminde başka değişiklikler olup olmadığını denetleyin.
  
|Özel durum iletileri|  
|------------------------|  
|Bir veya daha fazla girişleri null veya boş.|  
|Giriş "{0}" null veya boş|  
  

## <a name="error-0004"></a>Hata 0004  
 Parametre belirli değerine eşit veya daha az ise özel durum oluşur.  
  
 Modül verileri işlemek gerekli bir sınır değeri iletinin parametreyi altındaysa Azure Machine Learning'de bu hatayı alırsınız.  
  
**Çözüm:** Özel durum modülü yeniden ziyaret ve parametre belirtilen değerden büyük olacak şekilde değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Parametresi, sınır değerinden büyük olmalıdır.|  
|Parametre "{0}" değeri büyük {1}.|  
|Parametre "{0}"değerine sahip"{1}" olacağı büyüktür {2}|  
  


## <a name="error-0005"></a>Hata 0005  
 Parametre belirli bir değerden az ise özel durum oluşur.  
  
 İleti parametreyi altındaysa, Azure Machine Learning'de bu hatayı alırsınız veya modül verileri işlemek gerekli bir sınır değeri eşit.  
  
**Çözüm:** Özel durum modülü yeniden ziyaret ve belirtilen değere eşit veya sıfırdan büyük parametresini değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Parametresi, sınır değerine eşit veya daha büyük olmalıdır.|  
|Parametre "{0}" değeri olmalıdır büyük veya eşittir {1}.|  
|Parametre "{0}"değerine sahip"{1}" olacağı büyüktür veya eşittir {2}.|  
  

## <a name="error-0006"></a>Hata 0006  
 Parametre belirtilen değere eşit veya daha büyük ise özel durum oluşur.  
  
 İleti parametreyi modül verileri işlemek gerekli bir sınır değerine eşit veya daha büyük ise, Azure Machine Learning'de bu hatayı alırsınız.  
  
**Çözüm:** Özel durum modülü yeniden ziyaret ve parametre, belirtilen değerden daha az olacak şekilde değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Parametre uyuşmazlığı. Parametrelerden biri diğerinden daha küçük olmalıdır.|  
|Parametre "{0}" değeri değerinden küçük olmalıdır parametre"{1}" değeri.|  
|Parametre "{0}"değerine sahip"{1}" olmalıdır kısa {2}.|  
  

## <a name="error-0007"></a>Hata 0007  
 Parametre belirli bir değerinden daha büyük ise özel durum oluşur.  
  
 Modülü için Özellikler'de izin verilenden daha büyük bir değer belirtilirse, Azure Machine Learning'de bu hatayı alırsınız. Örneğin, desteklenen bir tarih aralığı dışında bir veri belirtebilirsiniz veya yalnızca üç sütun kullanılabilir olduğunda beş sütun kullanılması gerektiğini gösteriyor olabilir. 
 
 İki şekilde eşleştirmek için gereken veri kümesini belirtiyorsanız, bu hatayı da görebilirsiniz. Örneğin, sütunları yeniden adlandırma ve sütunları dizine göre belirtin, sağladığınız ad sayısını sütun sayısından eşleşmelidir. Başka bir örnek, iki sütun, sütunlar aynı sayıda satıra sahip olduğu gerekir kullanan bir matematik işlemi olabilir. 
  
**Çözüm:**
 
 + Söz konusu modülün açın ve tüm sayısal özellik ayarları gözden geçirin.
 + Tüm parametre değerlerini bu özellik için desteklenen değerler aralığında kalan emin olun.
 + Modülün birden fazla giriş aldığı durumlarda girişleri aynı boyutta olduğundan emin olun.
<!-- + If the module has multiple properties that can be set, ensure that related properties have appropriate values. For example, when using [Group Data into Bins](group-data-into-bins.md), if you use the option to specify custom bin edges, the number of bins must match the number of values you provide as bin boundaries.-->
 + Veri kümesi veya veri kaynağı değişip değişmediğini denetleyin. Sütun sayısı, sütun veri türlerini veya veri boyutu değiştirildikten sonra verileri önceki bir sürümü ile çalışan bir değer bazen başarısız olur.  
  
|Özel durum iletileri|  
|------------------------|  
|Parametre uyuşmazlığı. Parametrelerden biri veya daha az diğerine olmalıdır.|  
|Parametre "{0}"değeri olmalıdır parametre küçüktür veya eşittir"{1}" değeri.|  
|Parametre "{0}"değerine sahip"{1}" olacağı daha az veya buna eşit {2}.|  
  

## <a name="error-0008"></a>Hata 0008  
 Parametre aralık içinde değil, özel durum meydana gelir.  
  
 İletinin parametre modül verileri işlemek gerekli sınırları dışında ise Azure Machine Learning'de bu hatayı alırsınız.  
  
 Örneğin, kullanmayı denerseniz bu hata görüntülenir [Add Rows](add-rows.md) farklı sayıda sütuna sahip iki veri kümeleri birleştirmek için.  
  
**Çözüm:** Özel durum modülü yeniden ziyaret ve belirlenen aralık dahilinde parametre değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Parametre değeri, belirtilen aralıkta değil.|  
|Parametre "{0}" değer aralığında değil.|  
|Parametre "{0}" değer aralığında olmalıdır [{1}, {2}].|  
  

## <a name="error-0009"></a>Hata 0009  
 Azure depolama hesabı adı veya kapsayıcı adı hatalı belirtildiğinde, özel durum oluşur.  
  
Bu hata Azure Machine Learning Studio'da bir Azure depolama hesabı için parametreleri belirtin, ancak adı ve parola çözümlenemiyor oluşur. Parolaları veya hesap adlarını hatalar çeşitli nedenlerle oluşabilir:
 
 + Yanlış türde hesaptır. Machine Learning Studio ile kullanmak için bazı yeni hesap türleri desteklenmez. Bkz: [verileri içeri aktarma](import-data.md) Ayrıntılar için.
 + Girdiğiniz yanlış hesap adı
 + Hesabı artık
 + Depolama hesabı için parola yanlış veya değişti
 + Kapsayıcı adı belirtmediğiniz veya kapsayıcı yok
 + Tam dosya yolunu (blob yolu) belirtmediğiniz
   
**Çözüm:**

Bu tür sorunları, genellikle hesap adı, parola veya kapsayıcı yolu el ile girmeyi deneyin oluşur. Yeni sihirbaz için kullanmanızı öneririz [verileri içeri aktarma](import-data.md) modülü, aramak ve adları denetle yardımcı olur.

Ayrıca hesap, kapsayıcı veya blob silinip silinmediğini denetleyin. Hesap adı ve parola doğru girildiğini ve kapsayıcı bulunduğunu doğrulamak için başka bir Azure depolama yardımcı programını kullanın. 

Bazı yeni hesap türleri için Azure Machine Learning tarafından desteklenmez. Örneğin, yeni "Sık erişimli" veya "soğuk" Depolama türleri machine learning için kullanılamaz. Klasik depolama hesapları ve "Genel amaç" işe olarak oluşturulan depolama hesapları için.

Bir blob tam yolunu belirtilmişse yolu olarak belirtildiğinden emin olun **kapsayıcı/blobname**, ve hem blob, hem de kapsayıcı hesabında mevcut.  
  
 Yolun başında bir eğik çizgi içermemelidir. Örneğin **blob/kapsayıcı/** yanlış ve olarak girilmelidir **kapsayıcı/blob**.  

  
|Özel durum iletileri|  
|------------------------|  
|Azure depolama hesabı adı veya kapsayıcı adı doğru değil.|  
|Azure depolama hesabı adı "{0}"veya kapsayıcı adı"{1}" yanlış; bir biçim kapsayıcı/blob kapsayıcı adı bekleniyordu.|  
  

## <a name="error-0010"></a>0010 hata  
 Giriş veri kümeleri eşleşmesi, ancak olmayan sütun adları kullandıysanız, özel durum oluşur.  
  
 Sütun dizini iletinin iki giriş veri kümesi farklı sütun adları varsa, Azure Machine Learning'de bu hatayı alırsınız.  
  
**Çözüm:** Kullanım [meta verileri Düzenle](edit-metadata.md) veya belirtilen sütun dizini için aynı sütun adı için özgün veri kümesinden değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Giriş veri kümelerinde karşılık gelen dizin sütunlarla farklı adlara sahip.|  
|Sütun adları aynı sütun için değildir {0} (sıfır tabanlı) giriş veri kümeleri ({1} ve {2} sırasıyla).|  
  

## <a name="error-0011"></a>Hata 0011  
 Sütun kümesi bağımsız herhangi bir dataset sütunları için geçerli değildir geçirilen özel durum oluşur.  
  
 Belirtilen sütun seçimi sağlanan veri kümesinde sütun eşleşmezse, Azure Machine Learning'de bu hatayı alırsınız.  
  
 Ayrıca, bir sütun seçmediniz ve en az bir sütun modülünün çalışması gerekiyorsa bu hatayı alabilirsiniz.  
  
**Çözüm:** Sütun Seçimi modülünde dataset sütunları için geçerli olacak şekilde değiştirin.  
  
 Belirli bir sütuna seçtiğiniz modül gerektiriyorsa, bir etiket sütun gibi sağ sütunda seçildiğini doğrulayın.  
  
 Uygun olmayan bir sütun seçiliyse, bunları kaldırmanız ve denemeyi yeniden çalıştırın.  
  
|Özel durum iletileri|  
|------------------------|  
|Belirtilen sütun kümesi herhangi bir dataset sütunları için geçerli değildir.|  
|Belirtilen sütun kümesi "{0}" herhangi bir dataset sütunları için geçerli değildir.|  
  

## <a name="error-0012"></a>Hata 0012  
 Geçirilen bağımsız değişkenler kümesiyle sınıfının örneği oluşturulamadı, özel durum meydana gelir.  
  
**Çözüm:** Bu hata, kullanıcı tarafından eyleme dönüştürülebilir değildir ve gelecek sürümde kaldırılacak.  
  
|Özel durum iletileri|  
|------------------------|  
|Deneyimsiz model, ilk modeli eğitin.|  
|Deneyimsiz modeli ({0}), eğitilen modeli kullanır.|  
  

## <a name="error-0013"></a>Hata 0013  
 Modülü geçirilen learner geçersiz bir tür ise özel durum oluşur.  
  
 Eğitilen bir modelin bağlı Puanlama modülü ile uyumsuz olduğunda bu hata oluşur. <!--For example, connecting the output of [Train Matchbox Recommender](train-matchbox-recommender.md) to [Score Model](score-model.md) (instead of [Score Matchbox Recommender](score-matchbox-recommender.md)) will generate this error when the experiment is run.  -->
  
**Çözüm:**

Learner için uygun olan Puanlama modülü belirlemek ve eğitim modülü tarafından üretilen learner türünü belirler. 

Özelleştirilmiş eğitim modüllerinden birini kullanarak model eğitim, eğitim modeli yalnızca ilgili özel Puanlama modülüne bağlayın. 


|Model türü|Eğitim modülü| Puanlama Modülü|
|----|----|----|
|herhangi bir sınıflandırıcı|[Modeli eğitme](train-model.md) |[Model Puanlama](score-model.md)|
|herhangi bir regresyon modeli|[Modeli eğitme](train-model.md) |[Model Puanlama](score-model.md)|
<!--| Kümeleme modeli| [Kümeleme modeli eğitme](train-clustering-model.md) veya [kümeleme tarama](sweep-clustering.md)| [Veri kümelerine atama](assign-data-to-clusters.md)|
| anomali algılama - sınıfı bir SVM | [Anomali algılama modeli eğitme](train-anomaly-detection-model.md) |[Model Puanlama](score-model.md)|
| anomali algılama - PCA |[Modeli eğitme](train-model.md) |[Model Puanlama](score-model.md) </br> Modeli değerlendirme için bazı ek adımlar gereklidir. |
| anomali algılama - zaman serisi|  [Zaman serisi Anomali algılama](time-series-anomaly-detection.md) |Model verileri eğitir ve puanları oluşturur. Modül eğitilen learner oluşturmaz ve hiçbir ek Puanlama gereklidir. |
| öneri modeli| [Matchbox öneren eğitin](train-matchbox-recommender.md) | [Puan Matchbox öneren](score-matchbox-recommender.md) |
| Görüntü sınıflandırması | [Art arda kullanan görüntü sınıflandırması](pretrained-cascade-image-classification.md) | [Model Puanlama](score-model.md) |
|Vowpal Wabbit modelleri| [Vowpal Wabbit sürüm 7-4 modeli eğitme](train-vowpal-wabbit-version-7-4-model.md) | [Vowpal Wabbit sürüm 7-4 modeli Puanlama](score-vowpal-wabbit-version-7-4-model.md) |   
|Vowpal Wabbit modelleri| [Vowpal Wabbit sürüm 7-10 modeli eğitme](train-vowpal-wabbit-version-7-10-model.md) | [Vowpal Wabbit sürüm 7-10 modeli Puanlama](score-vowpal-wabbit-version-7-10-model.md) |
|Vowpal Wabbit modelleri| [Vowpal Wabbit sürüm 8 modeli eğitme](score-vowpal-wabbit-version-8-model.md) | [Vowpal Wabbit sürüm 8 Model Puanlama](score-vowpal-wabbit-version-8-model.md) |-->
  
|Özel durum iletileri|  
|------------------------|  
|Geçersiz türde learner geçirilir.|  
|Learner "{0}" türü geçersiz.|  


## <a name="error-0014"></a>Hata 0014  
 Sütunu benzersiz değerlerin sayısını izin verilenden daha büyük ise özel durum oluşur.  
  
 Çok fazla benzersiz değerler sütunu içerdiğinde, bu hata oluşur.  Örneğin, bir sütun kategorik veriler işlenmesi gerektiğini belirtirseniz, bu hatayı görebilirsiniz, ancak işlemenin izin vermek için sütunda çok fazla benzersiz değer vardır. Benzersiz değerleri iki giriş sayısı arasında bir uyuşmazlık varsa bu hatayı da görebilirsiniz.   
  
**Çözüm:**

Hata oluşturan modülü açın ve girdi olarak kullanılan sütunları tanımlar. Bazı modüller için seçin ve giriş veri kümesi sağ **Görselleştir** benzersiz değerler ve bunların dağıtım sayısı dahil olmak üzere ayrı ayrı sütunlarda İstatistikler almak için.

Gruplandırma ve kategori için kullanmayı planladığınız sütunları için sütunlardaki benzersiz değerlerin sayısını azaltmak için adımları uygulayın. Sütunun veri türüne bağlı olarak, farklı şekillerde azaltabilir. 
<!--
+ For text data, you might be able to use [Preprocess Text](preprocess-text.md) to collapse similar entries. 
+ For numeric data, you can create a smaller number of bins using [Group Data into Bins](group-data-into-bins.md), remove or truncate values using [Clip Values](clip-values.md), or use machine learning methods such as [Principal Component Analysis](principal-component-analysis.md) or [Learning with Counts](data-transformation-learning-with-counts.md) to reduce the dimensionality of the data.  
-->
> [!TIP]
> Aşağıdan senaryonuzla eşleşen bir çözüm bulunamadı? Hata ve veri türü oluşturulan modülünün adı ve sütunun içeren bu konu hakkında geri bildirim sağlayabilirsiniz. Bilgi sağlamak için daha yaygın senaryoları için sorun giderme adımları hedeflenen kullanacağız.   
  
|Özel durum iletileri|  
|------------------------|  
|Sütunu benzersiz değer sayısı izin verilenden daha büyüktür.|  
|Sütunda benzersiz değerler sayısı: "{0}" demet sayısı daha fazla {1}.|  
  

## <a name="error-0015"></a>Hata 0015  
 Veritabanı bağlantısı başarısız oldu, özel durum meydana gelir.  
  
 Bir hatalı SQL hesap adı, parola, veritabanı sunucusu veya veritabanı adı girin veya veritabanı veya sunucu sorunlar nedeniyle veritabanı ile bağlantı kurulamıyor. Bu hatayı alırsınız.  
  
**Çözüm:** Hesap adı, parola, veritabanı sunucusu ve veritabanı doğru girildiğini ve belirtilen hesabın doğru izin düzeyini olduğunu doğrulayın. Veritabanı şu anda erişilebilir olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Veritabanı bağlantısı yapılırken bir hata oluştu.|  
|Veritabanı bağlantısı kurulurken hata oluştu: {0}.|  
  


## <a name="error-0016"></a>Hata 0016  
 Giriş veri kümeleri modülü geçirilen uyumlu sütun türleri sahip ancak olmayan özel durum ortaya çıkar.  
  
 İki veya daha fazla veri kümelerinde geçirilen sütunların türlerini birbiriyle uyumlu değilse, Azure Machine Learning'de bu hatayı alırsınız.  
  
**Çözüm:** Kullanım [meta verileri Düzenle](edit-metadata.md) veya özgün girdi veri kümesini değiştirme<!--, or use [Convert to Dataset](convert-to-dataset.md)--> Sütun türlerini uyumlu olmasını sağlamak için.  
  
|Özel durum iletileri|  
|------------------------|  
|Giriş veri kümelerinde karşılık gelen dizin sütunları uyumsuz türlere sahip.|  
|Sütunları {0} ve {1} uyumsuzdur.|  
|Sütun öğe türleri sütunu için uyumlu olmayan {0} (sıfır tabanlı) giriş veri kümeleri ({1} ve {2} sırasıyla).|  
  

## <a name="error-0017"></a>Hata 0017  
 Seçili sütun geçerli modülü tarafından desteklenmeyen bir veri türü kullanıyorsa, özel durum oluşur.  
  
 Sütun seçimini bir matematik işlemi için bir dize sütunu ya da bir puan sütun gibi modülü tarafından bir kategorik özelliği sütunun bulunduğu işlenemez bir veri türüne sahip bir sütun içeriyorsa, örneğin, bu hata Azure Machine Learning'de alabilirsiniz Gerekli.  
  
**Çözüm:**
 1. Sorun sütun tanımlayın.
 2. Modül gereksinimlerini gözden geçirin.
 3. Sütun gereksinimlerine uygun şekilde değiştirin. Aşağıdaki modüller birkaç sütunu ve çalıştığınız dönüştürme bağlı olarak değişiklik yapmak için kullanmanız gerekebilir:
    + Kullanım [meta verileri Düzenle](edit-metadata.md) sütunların veri türünü değiştirmek veya sayısal, olmayan-kategorik için kategorik ve benzeri sütun kullanımı özelliği değiştirmek için.
<!--    + Use [Convert to Dataset](convert-to-dataset.md) to ensure that all included columns use data types that are supported by Azure Machine Learning.  If you cannot convert the columns, consider removing them from the input dataset.
    + Use the [Apply SQL Transformation](apply-sql-transformation.md) or [Execute R Script](execute-r-script.md) modules to cast or convert any columns that cannot be modified using [Edit Metadata](edit-metadata.md). These modules provide more flexibility for working with datetime data types.
    + For numeric data types, you can use the [Apply Math Operation](apply-math-operation.md) module to round or truncate values, or use the [Clip Values](clip-values.md) module to remove out of range values.  -->
 4. Son çare olarak, özgün giriş veri kümesi değiştirmeniz gerekebilir.

> [!TIP]
> Aşağıdan senaryonuzla eşleşen bir çözüm bulunamadı? Hata ve veri türü oluşturulan modülünün adı ve sütunun içeren bu konu hakkında geri bildirim sağlayabilirsiniz. Bilgi sağlamak için daha yaygın senaryoları için sorun giderme adımları hedeflenen kullanacağız. 
  
|Özel durum iletileri|  
|------------------------|  
|Sütun geçerli türü işlenemiyor. Türü bir modül tarafından desteklenmiyor.|  
|Sütun türü işlenemiyor {0}. Türü bir modül tarafından desteklenmiyor.|  
|Sütun işleyemiyor "{1}" türündeki {0}. Türü bir modül tarafından desteklenmiyor.|  
|Sütun işleyemiyor "{1}" türündeki {0}. Türü bir modül tarafından desteklenmiyor. Parametre adı: {2}|  
  

## <a name="error-0018"></a>Hata 0018  
 Giriş veri kümesi, geçerli değil. özel durum ortaya çıkar.  
  
**Çözüm:** Bu yüzden bu hata Azure Machine learning'de tek bir çözüm birçok bağlamda görünemez. Genel olarak, hata, bir modül giriş olarak sağlanan veri yanlış sayıda sütun olduğundan veya veri türü gereksinimleri modülünün eşleşmiyor gösterir. Örneğin:  
  
-   Modülü, bir etiket sütun gerektirir. ancak hiçbir sütun etiket olarak işaretlenmiş veya bir etiket sütun henüz seçmediniz.  
  
-   Modülü, veri kategorik ancak verilerinizi sayısal gerektirir.  
  
<!---   The module requires a specific data type. For example, ratings provided to [Train Matchbox Recommender](train-matchbox-recommender.md) can be either numeric or categorical, but cannot be floating point numbers.  -->
  
-   Yanlış biçimde verilerdir.  
  
-   İçeri aktarılan veriler hatalı değerler, geçersiz karakterler içeriyor veya değerler aralığının dışında.  
-   Sütun boş veya çok fazla eksik değerleri içerir.  
  
 Gereksinimler ve verilerinizi nasıl olabilir belirlemek için veri kümesinin girdi olarak kullanan bir modül için Yardım konusunu gözden geçirin.  
  
 <!--We also recommend that you use [Summarize Data](summarize-data.md) or [Compute Elementary Statistics](compute-elementary-statistics.md) to profile your data, and use these modules to fix metadata and clean values: [Edit Metadata](edit-metadata.md) and [Clean Missing Data](clean-missing-data.md), [Clip Values](clip-values.md)-->.  
  
|Özel durum iletileri|  
|------------------------|  
|Veri kümesi, geçerli değil.|  
|{0} Geçersiz veri içeriyor.|  
|{0} ve {1} tutarlı sütun akıllıca olmalıdır.|  
  

## <a name="error-0019"></a>Hata 0019  
 Sütunu sıralanmış değerleri içermesi beklenir, ancak yok, özel durum ortaya çıkar.  
  
 Belirtilen sütun değerleri sırası yoksa Azure Machine Learning'de bu hatayı alırsınız.  
  
**Çözüm:** Girdi veri kümesini el ile değiştirerek sütun değerlerini sıralamak ve modül yeniden çalıştırın.  
  
|Özel durum iletileri|  
|------------------------|  
|Sütundaki değerleri sıralı değildir.|  
|Sütundaki değerleri "{0}" sıralanmaz.|  
|Sütundaki değerleri "{0}"veri kümesinin"{1}" sıralanmaz.|  
  

## <a name="error-0020"></a>Hata 0020  
 Bazı modülü aktarılan veri kümeleri sütun sayısı çok küçükse, özel durum oluşur.  
  
 Azure Machine Learning'de bu hatayı alırsınız yoksa bir modül için yeterli sayıda sütun seçilmedi.  
  
**Çözüm:** Modülün yeniden ziyaret ve bu sütun seçiciyi seçili sütun sayısını doğru olduğundan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Giriş veri kümesi sütunlardaki minimum izin verilenden daha az sayısıdır.|  
|Giriş veri kümesinde sütun sayısı en az izin verilenden daha az olan {0} sütunları.|  
|Giriş veri kümesinde sütun sayısı "{0}" en az izin verilenden daha küçük {1} sütunları.|

## <a name="error-0021"></a>Hata 0021  
 Bazı modülü aktarılan veri kümeleri satır sayısı çok küçükse, özel durum oluşur.  
  
 Olduğunda bu hata Azure Machine Learning'de veri kümesinde belirtilen işlemi gerçekleştirmek için yeterli satır görülür. Örneğin, giriş veri kümesi boşsa veya minimum bazı geçerli olması için satır sayısı gerektiren bir işlem gerçekleştirmek çalışıyorsanız bu hatayı görebilirsiniz. Bu işlemler içerebilir (ancak bunlarla sınırlı değildir) gruplandırma veya sınıflandırma temel istatistiksel yöntemler, belirli türde gruplama ve sayımlarla öğrenme.  
  
**Çözüm:**
 
 + Döndürülen hata modülün açın ve giriş veri kümesi ve modül özelliklerini denetleyin. 
 + Giriş veri kümesi boş olmadığından ve veri modülü Yardım'da açıklanan gereksinimlerini karşılamak için yeterli satır olduğundan emin olun.  
 + Verilerinizi bir dış kaynaktan yüklenirse, veri kaynağının kullanılabilir olduğundan ve hiçbir hata olduğundan emin olun veya daha az sayıda satır almak içeri aktarma işlemi neden olan veri tanımında değiştirin.
 + Bölme, veri türü veya değer, temizleme gibi etkileyen veya birleştirme işlemleri modülünün Yukarı Akış verileri üzerinde bir işlem gerçekleştiriyorsanız, döndürülen satır sayısını belirlemek için bu işlemleri çıkışları denetleyin.  



## <a name="error-0022"></a>Hata 0022  
 Giriş veri kümesi seçili olan sütunlardaki sayısı beklenen sayıya eşit değildir, özel durum meydana gelir.  
  
 Azure Machine learning'de bu hata, aşağı akış modüle ya da işlemi belirli sayıda sütun veya girişleri gerektirir ve çok az veya çok fazla sütun veya girişleri sağlanan oluşabilir. Örneğin:  
  
-   Bir tek etiketli bir sütun veya anahtar sütunu belirtin ve yanlışlıkla birden çok sütun seçilmedi.  
  
-   Sütunları yeniden adlandırma, ancak sütun sayısından daha fazla veya daha az adları sağlanan.  
  
-   Kaynak veya hedef sütun sayısı değiştirildi veya modülü tarafından kullanılan sütun sayısı eşleşmiyor.  
  
-   Değerleri virgülle ayrılmış bir listesi için girişler, sağladığınız ancak değer sayısı eşleşmiyor veya birden çok giriş desteklenmez.  
  
**Çözüm:** Modülün yeniden ziyaret ve sütun sayısını doğru seçili olduğundan emin olmak için sütun seçimi denetleyin. Yukarı Akış modül çıkışları ve aşağı akış işlem gereksinimlerini doğrulayın.  
  
 Birden çok sütun (sütun dizinleri, tüm özellikler, tüm sayısal, vb.) seçebileceğiniz sütun seçimi seçeneklerden birini kullandıysanız, tam sayısı yoluyla seçimin döndürülecek olan sütunları doğrulayın.  
  
 <!--If you are trying to specify a comma-separated list of datasets as inputs to [Unpack Zipped Datasets](unpack-zipped-datasets.md), unpack only one dataset at a time. Multiple inputs are not supported.  -->
  
 Yukarı Akış sütun türü ve numarası değişmediğini doğrulayın.  
  
 Bir modeli eğitmek için bir öneri veri kümesi kullanıyorsanız, kullanıcı öğesi çiftleri veya kullanıcı öğe sıralamasına karşılık gelen öneren sütunları, sınırlı sayıda beklediğini unutmayın. Modeli eğitmek veya öneri veri kümeleri bölme önce ek sütunları kaldırın. Daha fazla bilgi için [verileri bölme](split-data.md).  
  
|Özel durum iletileri|  
|------------------------|  
|Giriş veri kümesi seçili olan sütunlardaki sayısı beklenen sayıya eşit değildir.|  
|Giriş veri kümesi seçili olan sütunlardaki sayısı için eşit değil {0}.|  
|Sütun Seçimi deseni "{0}" girdi veri kümesi eşit değil seçili olan sütunlardaki sayısını sağlar {1}.|  
|Sütun Seçimi deseni "{0}" sağlaması beklenir {1} giriş veri kümesinde, seçili sütunları ancak {2} sütunları veya sağlanır.|  



## <a name="error-0023"></a>Hata 0023  
 Hedef sütunu giriş veri kümesi, geçerli trainer modülü için geçerli değil. özel durum ortaya çıkar.  
  
 Hedef sütun (olarak modül parametrelerini seçili) geçerli veri türü değil, tüm eksik değerleri yer alan veya beklendiği gibi kategorik değildi, Azure Machine learning'de bu hata meydana gelir.  
  
**Çözüm:** Etiketi/hedef sütunu içeriğini incelemek için giriş modülü yeniden ziyaret edin. Tüm eksik değerlerin yok emin olun. Modül hedef sütun kategorik olmasını bekliyor, birden fazla ayrı değer hedef sütunu emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Hedef sütunda giriş veri kümesi desteklenmiyor.|  
|Giriş veri kümesi hedef sütunu desteklenmeyen "{0}".|  
|Giriş veri kümesi hedef sütunu desteklenmeyen "{0}" learner türü için {1}.|  
 

## <a name="error-0024"></a>Hata 0024  
Veri kümesi, bir etiket sütun içermiyor özel durum ortaya çıkar.  

 Etiket sütun modül gerektirir ve veri kümesi bir etiket sütun yok. Azure Machine learning'de bu hata oluşur. Örneğin, puanlanmış veri kümesinin değerlendirme genellikle bir etiket sütun doğruluğu ölçümleri işlem için mevcut olmasını gerektirir.  
 
Bir etiket sütun kümesinde var, ancak doğru Azure Machine Learning tarafından algılanmayan de oluşabilir.
  
**Çözüm:**

+ Hata oluşturan modülü açın ve bir etiket sütun mevcut olup olmadığını belirler. Tek bir sonuç (veya bağımlı değişken) tahmin etmek çalıştığınız sütun içerdiği sürece sütun adı veya veri türü önemli değildir. Etiket sütununun emin değilseniz, bir genel adı gibi arayın *sınıfı* veya *hedef*. 
+  Veri kümesi bir etiket sütun yoksa, etiket sütununda açıkça ya da yanlışlıkla Yukarı Akış kaldırıldığını mümkündür. Veri kümesi bir Yukarı Akış Puanlama modülün çıkışına değil de olabilir.
+ Açıkça sütunun etiket sütun olarak işaretlemek için ekleme [meta verileri Düzenle](edit-metadata.md) modülü ve veri kümesine bağlanın. Yalnızca etiket sütunu seçip **etiket** gelen **alanları** açılır liste. 
+ Etiket olarak yanlış sütun seçilirse, seçebileceğiniz **Temizle etiket** gelen **alanları** meta veri sütunu düzeltmek için. 
  
|Özel durum iletileri|  
|------------------------|  
|Veri kümesi etiketi sütun yok.|  
|Hiçbir etiket sütunu yok "{0}".|  
  

## <a name="error-0025"></a>Hata 0025  
 Veri kümesi bir puan sütun içermiyor özel durum ortaya çıkar.  
  
 Evaluate model girişi geçerli puan içermiyor, Azure Machine learning'de bu hata meydana gelir sütunları. Örneğin, kullanıcı bir veri kümesi ile doğru eğitilen bir modelin puanlanmış veya Puan sütunu açıkça Yukarı Akış atıldı önce değerlendirmeye çalışır. Bu özel durum Ayrıca iki veri kümesi puanı sütunlarda uyumsuz olması durumunda gerçekleşir. Örneğin, bir doğrusal regresörü doğruluğunu bir ikili dosya sınıflandırıcı değeriyle karşılaştırılacak çalışıyor olabilir.  
  
**Çözüm:** Evaluate model girişi yeniden ziyaret ve bir veya daha fazla puan sütunları içeriyorsa inceleyin. Değilse, veri kümesi olmayan puanlanmış veya puan sütunları bir Yukarı Akış modülünde atıldı.  
  
|Özel durum iletileri|  
|------------------------|  
|Veri kümesinde puanı sütun yok.|  
|İçinde puanı sütun yok "{0}".|  
|İçinde puanı sütun yok "{0}" tarafından üretilen bir "{1}". Learner doğru türünü kullanarak bir veri kümesini puan.|  
  

## <a name="error-0026"></a>Hata 0026  
 Aynı ada sahip sütun izin verilmeyen, özel durum meydana gelir.  
  
 Birden çok sütun aynı ada sahip, Azure Machine learning'de bu hata meydana gelir. Bu hatayı alabileceğiniz bir veri kümesi bir başlık satırı yok ve sütun adları otomatik olarak atanır yoludur: Col0, Col1, vb.  
  
**Çözüm:** Sütunları aynı ada sahipse, INSERT bir [meta verileri Düzenle](edit-metadata.md) modülünün giriş veri kümesi ve modül arasında. Sütun seçicide kullanın [meta verileri Düzenle](edit-metadata.md) yeniden adlandırmak için sütun seçmek için içine yeni adlarını yazarak **yeni sütun adları** metin.  
  
|Özel durum iletileri|  
|------------------------|  
|Eşit sütun adları, bağımsız değişkenlerine belirtilir. Modülü tarafından eşit sütun adlarına izin verilmemektedir.|  
|Eşit bağımsız değişkenler sütun adları "{0}"ve"{1}" izin verilmez. Farklı bir ad belirtin.|  
  

## <a name="error-0027"></a>Hata 0027  
 İki nesnenin aynı boyutta olması gerekir ancak olmayan durumlarda durumunda özel durum oluşur.  
  
 Bu, Azure Machine learning'de yaygın bir hatadır ve birçok koşullarına göre neden olabilir.  
  
**Çözüm:** Belirli bir çözümleme yoktur. Ancak, koşullar aşağıdaki gibi denetleyebilirsiniz:  
  
-   Sütunları yeniden adlandırma, her bir liste (ilişkin giriş sütunlarını ve yeni adları listesi) aynı sayıda öğe olduğundan emin olun.  
  
-   Katılma veya iki veri kümesi bitiştirme, aynı şemaya sahip olduklarından emin olun.  
  
-   Birden fazla sütuna sahip iki veri kümesi birleştirilecekse, anahtar sütunları yazın ve seçeneğini aynı verilere sahip olduğunuzdan emin olun **izin yinelemeleri ve sütun sırasını seçimdeki korumak**.  
  
|Özel durum iletileri|  
|------------------------|  
|Geçirilen nesnelerin boyutunu tutarsız.|  
|Boyutu "{0}"boyutuyla tutarsız."{1}".|  
  

## <a name="error-0028"></a>Hata 0028  
 Sütun kümesi yinelenen sütun adları içeriyor ve izin durumda özel durum oluşur.  
  
 Bu hata Azure Machine learning'de sütun adları yinelenmiş oluşur; diğer bir deyişle, benzersiz değil.  
  
**Çözüm:** Tüm sütunları aynı adı, bir örneğini ekleme varsa [meta verileri Düzenle](edit-metadata.md) giriş veri kümesi ve hata oluşturma modülü arasında. Sütun seçicide kullanın [meta verileri Düzenle](edit-metadata.md) yeniden adlandırın ve içine yeni sütun adlarını yazın sütunları seçmek için **yeni sütun adları** metin. Birden çok sütun yeniden adlandırma, yazdığınız değerlerin emin **yeni sütun adları** benzersizdir.  
  
|Özel durum iletileri|  
|------------------------|  
|Yinelenen sütun adları sütun kümesi içerir.|  
|Adı "{0}" yineleniyor.|  
|Adı "{0}"içinde yinelenen"{1}".|  
  

## <a name="error-0029"></a>Hata 0029  
 Geçersiz URI zaman geçirilir durumunda özel durum oluşur.  
  
 Geçersiz URI zaman geçirilir durumunda Azure Machine learning'de bu hata oluşur.  Aşağıdaki koşullardan herhangi biri doğru olduğunda bu hatayı alırsınız:, veya.  
  
-   Genel veya SAS URI'si, Azure Blob Depolama için okuma için sağlanan veya yazma bir hata içeriyor.  
  
-   Zaman penceresi için SAS süresi doldu.  
  
-   Web URL HTTP kaynağı ile bir dosya ya da geri döngü URI'si temsil eder.  
  
-   HTTP üzerinden Web URL'si yanlış biçimlendirilmiş bir URL içerir.  
  
-   Uzak kaynak tarafından URL çözümlenemiyor.  
  
**Çözüm:** Modülün yeniden ziyaret ve URI'nin biçimini doğrulayın. Veri kaynağı HTTP üzerinden bir Web URL'si ise, hedeflenen kaynak bir dosya ya da geri döngü URI'si (localhost) olmadığını doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Geçersiz URI geçirilir.|  
  

## <a name="error-0030"></a>Hata 0030  
 Bir dosyayı indirmek mümkün olmadığında durumda özel durum oluşur.  
  
 Bir dosyayı indirmek mümkün değilse, Azure Machine learning'de bu özel durum oluşur. Bir HTTP kaynaktan denenen bir okuma üç (3) yeniden denedikten sonra başarısız olduğunda, bu özel durum alırsınız çalışır.  
  
**Çözüm:** URI HTTP kaynağı doğru olduğunu ve site Internet üzerinden erişilebilir olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Bir dosyayı karşıdan yükeleyemedi.|  
|Dosya indirilirken hata oluştu: {0}.|  
  

## <a name="error-0031"></a>Hata 0031  
 Sütun kümesindeki sütunların sayısı gerekenden daha az ise özel durum oluşur.  
  
 Seçili sütun sayısı gerekenden daha az ise, Azure Machine learning'de bu hata oluşur.  Gereken en düşük sayıda sütun seçilmedi, bu hatayı alırsınız.  
  
**Çözüm:** Kullanarak ek sütunlar sütun seçime Ekle **Sütun seçiciyi**.  
  
|Özel durum iletileri|  
|------------------------|  
|Sütun kümesindeki sütunların sayısı az sayısıdır.|  
|{0} sütunları belirtilmelidir. Belirtilen sütunları gerçek sayısı {1}.|  

## <a name="error-0032"></a>Hata 0032  
 Bağımsız değişken bir sayı değilse, özel durum oluşur.  
  
 Bağımsız değişken bir double veya NaN ise Azure Machine Learning'de bu hatayı alırsınız.  
  
**Çözüm:** Belirtilen bağımsız değişken geçerli bir değer kullanmak için değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Bağımsız değişken bir sayı değil.|  
|"{0}" bir sayı değil.|  
  

## <a name="error-0033"></a>Hata 0033  
 Bağımsız değişkeni Infinity ise özel durum oluşur.  
  
 Bağımsız değişken sonsuz ise, Azure Machine learning'de bu hata oluşur. Bağımsız değişken ya da ise bu hatayı alırsınız `double.NegativeInfinity` veya `double.PositiveInfinity`.  
  
**Çözüm:** Belirtilen bağımsız değişken geçerli bir değer olacak şekilde değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Bağımsız değişken sınırlı olmalıdır.|  
|"{0}" sınırlı değildir.|  
  

## <a name="error-0034"></a>0034 hata  
 Belirli kullanıcı öğe çifti için birden fazla derecelendirme varsa, özel durum oluşur.  
  
 Bir kullanıcı öğesini çift birden fazla derecelendirme değeri varsa bu hata Azure Machine Learning öneri de gerçekleşir.  
  
**Çözüm:** Kullanıcı öğesi çifti bir derecelendirme değeri yalnızca sahip olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Veri değerleri için birden fazla derecelendirme var.|  
|Kullanıcı için birden fazla derecelendirme {0} ve öğe {1} derecelendirme tahmin veri tablosu.|  
  

## <a name="error-0035"></a>Hata 0035  
 Belirli bir kullanıcı veya öğe için hiçbir özellik sağlandı, özel durum meydana gelir.  
  
 Azure Machine learning'de bu hata, Puanlama için bir öneri modeli kullanmaya çalıştığınız ancak özellik vektör bulunamıyor oluşur.  
  
**Çözüm:**

Matchbox öneren öğesi özellikleri veya kullanıcı özelliklerini kullanırken karşılanması gereken bazı gereksinimler vardır.  Bu hata, bir özellik vektör bir kullanıcı veya giriş olarak sağlanan öğesi eksik olduğunu gösterir.  Her bir kullanıcı veya öğe için veri özelliklerini oluşan bir vektörü kullanılabilir olmasını sağlamak gerekir.  
  
 Örneğin, eğitilmiş bir öneri kullanıcının yaş, konum veya gelir gibi özellikleri kullanarak model, ancak artık puanları eğitim sırasında bazı eşdeğer bir özellik kümesini sağlamalısınız görülmeyen yeni kullanıcılar için oluşturmak istediğiniz (yani, yaş, konumunu ve gelir değerleri) ilgili Öngörüler için bunları yapmak için yeni kullanıcılar için. 
 
 Bu kullanıcılar için herhangi bir özellik yoksa, özellik Mühendisliği uygun özellikler oluşturmak için göz önünde bulundurun.  Örneğin, yaş veya gelir değerleri tek tek kullanıcı yoksa, bir grup kullanıcı için kullanılacak yaklaşık değerlerini üretebilir. 
 
<!--When you are scoring from a recommendation mode, you can use item or user features only if you previously used item or user features during training. For more information, see [Score Matchbox Recommender](score-matchbox-recommender.md).
 
For general information about how the Matchbox recommendation algorithm works, and how to prepare a dataset of item features or user features, see [Train Matchbox Recommender](train-matchbox-recommender.md).  -->
  
 > [!TIP]
 > Çözüm durumunuz için geçerli değil mi? Bu makalede geri bildirim gönderin ve modül ve satır sayısı sütunu dahil olmak üzere bu senaryo hakkında bilgi sağlamak Hoş Geldiniz. Bu bilgileri sağlamak için daha ayrıntılı sorun giderme adımları gelecekte kullanacağız.
   
|Özel durum iletileri|  
|------------------------|  
|Herhangi bir özellik için gerekli kullanıcı veya öğe sağlandı.|  
|Özellikleri {0} gerekli ancak sağlanmadı.|  
  

## <a name="error-0036"></a>Hata 0036  
 Belirli bir kullanıcı veya öğe için birden çok özellik vektör sağlandı, özel durum meydana gelir.  
  
 Bu hata Azure Machine learning'de özellik vektör birden çok kez tanımlanmış olması durumunda gerçekleşir.  
  
**Çözüm:** Özellik vektör birden çok kez tanımlanmamış emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Özellik tanımı bir kullanıcı veya öğe için yinelenen.|  
|Özellik tanımı yinelenen {0}.|  
  

## <a name="error-0037"></a>Hata 0037  
 Birden çok etiket sütunu belirtilir ve yalnızca bir izin, özel durum ortaya çıkar.  
  
 Yeni etiket sütunu için birden fazla sütun seçiliyse, Azure Machine learning'de bu hata oluşur. Hedef ya da etiketi işaretlenmiş için tek bir sütunda en denetimli öğrenme algoritmalarını gerektirir.  
  
**Çözüm:** Yeni bir etiket sütun tek bir sütun seçtiğinizden emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Birden çok etiket sütun belirtilmiş.|  
  

## <a name="error-0038"></a>Hata 0038  
 Özel durum sayısı beklenen öğeler tam bir değer gerekiyorsa oluşur, ancak değil.  
  
 Azure Machine learning'de bu hata beklenen öğe sayısı tam bir değer gerekiyorsa oluşur, ancak değil.  Öğe sayısı için geçerli beklenen değer eşit değilse, bu hatayı alırsınız.  
  
**Çözüm:** Öğeleri doğru sayıda giriş değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Öğe sayısı geçerli değil.|  
|İçindeki öğelerin sayısını "{0}" geçerli değil.|  
|İçindeki öğelerin sayısını "{0}" geçerli sayıya eşit değil {1} öğeyi/öğeleri.|  
  

## <a name="error-0039"></a>0039 hata  
 Bir işlem başarısız oldu, özel durum meydana gelir.  
  
 Bir iç işlem tamamlandığında Azure Machine learning'de bu hata oluşur.  
  
**Çözüm:** Birçok koşullarına göre bu hataya neden olur ve belirli hiçbir remedy yoktur.  
 Aşağıdaki tablo, belirli bir koşul açıklaması tarafından izlenen genel bu hata iletileri içerir. 
 
 Ayrıntı yok mevcutsa [geri bildirim gönder](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) ve oluşturulan hata ve ilgili koşulları modülleri hakkında bilgi sağlar.
  
|Özel durum iletileri|  
|------------------------|  
|İşlem başarısız oldu.|  
|İşlem tamamlanırken hata oluştu: {0}.|  
  

## <a name="error-0040"></a>Hata 0040  
 Kullanım dışı bir modül çağrılırken özel durum oluşur.  
  
 Azure Machine learning'de bu hata, kullanım dışı bir modül çağırırken oluşturulur.  
  
**Çözüm:** Kullanım dışı modülü, desteklenen bir adla değiştirin. Bunun yerine kullanmak için hangi modülü hakkında bilgi için modül çıktı günlüğüne bakın.  
  
|Özel durum iletileri|  
|------------------------|  
|Kullanım dışı modülü erişme.|  
|Modül "{0}" kullanım dışı bırakılmıştır. Kullanım modül "{1}" Bunun yerine.|  
 

## <a name="error-0041"></a>Hata 0041  
 Kullanım dışı bir modül çağrılırken özel durum oluşur.  
  
 Azure Machine learning'de bu hata, kullanım dışı bir modül çağırırken oluşturulur.  
  
**Çözüm:** Kullanım dışı modülü desteklenen olanları kümesiyle değiştirin. Bu bilgiler modülün çıkış günlüğünde görünür olmalıdır.  
  
|Özel durum iletileri|  
|------------------------|  
|Kullanım dışı modülü erişme.|  
|Modül "{0}" kullanım dışı bırakılmıştır. Modüller kullan "{1}" istenen işlevselliği.|  
 

## <a name="error-0042"></a>Hata 0042  
 Sütun başka bir türe dönüştürmek mümkün değilse, özel durum oluşur.  
  
 Azure Machine learning'de bu hata, sütunun belirtilen türe dönüştürülmesi mümkün değil oluşur.  Bir modülü tarih saat, metin, kayan nokta sayısı veya bir tamsayı gibi bir özel veri türü gerektirir, ancak varolan bir sütunla gereken türe dönüştürmek mümkün değildir, bu hatayı alırsınız.  
 
Örneğin, bir sütun seçin ve kullanım matematik işlemi için sayısal veri türüne dönüştürün ve sütunu geçersiz veri içeriyorsa, bu hatayı alırsınız. 

Kategorik bir sütun olarak kayan noktalı sayıları veya birçok benzersiz değerler içeren bir sütun kullanmayı denerseniz, başka bir nedenle bu hatayı alabilirsiniz. 
  
**Çözüm:**

+ Hata oluşturan modülü için Yardım sayfasını açın ve veri türü gereksinimleri doğrulayın.
+ Giriş veri kümesi sütunların veri türlerini gözden geçirin.
+ Sözde şemasız veri kaynaklarındaki veriler inceleyin.
+ Veri kümesi eksik değerleri veya istenen veri türüne dönüştürme engelleyebilecek özel karakterler için denetleyin. 
    + Sayısal veri türleri tutarlı: Örneğin, bir tamsayı sütununda kayan nokta numarası için denetleyin.
    + Metin dizesi ya da sayı sütundaki değerleri NA bakın. 
    + Boole değerleri gerekli veri türüne bağlı olarak uygun bir gösterimi dönüştürülebilir.
    + Unicode olmayan karakterler, sekme karakterlerini veya denetim karakterleri için metin sütunu inceleyin
    + TarihSaat veri modelleme hataları önlemek için tutarlı olmalıdır, ancak temizleme owing to pek çok biçimde karmaşık olabilir. Kullanmayı düşünün <!--the [Execute R Script](execute-r-script.md) or -->[Python betiği yürütme](execute-python-script.md) temizleme işlemini gerçekleştirmek için modüller.  
+ Gerekirse, sütun başarıyla dönüştürülüp şekilde giriş veri kümesi değerleri değiştirin. Değişiklik, gruplama, kesme veya yuvarlama işlem ve aykırı değerleri ortadan kaldırılması veya eksik değerlerin imputation içerebilir. Machine learning'de bazı genel veri dönüştürme senaryolar için aşağıdaki makalelere bakın:
    + [Eksik verileri temizleme](clean-missing-data.md)
    + [Veri normalleştirin](normalize-data.md)
<!--+ [Clip Values](clip-values.md) 
    + [Group Data Into Bins](group-data-into-bins.md)
  -->
 
> [!TIP]
> Çözümlemesi belirsiz veya durumunuz için geçerli değildir? Bu makalede geri bildirim gönderin ve işlem modülü ve sütunun veri türünü de dahil olmak üzere bu senaryo hakkında bilgi sağlamak Hoş Geldiniz. Bu bilgileri sağlamak için daha ayrıntılı sorun giderme adımları gelecekte kullanacağız.  
  
|Özel durum iletileri|  
|------------------------|  
|Dönüştürme izin verilmiyor.|  
|Sütun türü dönüştürülemiyor {0} sütun türü için {1}.|  
|Sütun dönüştürülemedi "{2}" türündeki {0} sütun türü için {1}.|  
|Sütun dönüştürülemedi "{2}" türündeki {0} sütununa "{3}" türündeki {1}.|  
  

## <a name="error-0043"></a>Hata 0043  
 Öğe türü açıkça eşittir uygulamıyor özel durum oluşur.  
  
 Bu hata Azure Machine learning'de kullanılmayan ve kaldırılacak.  
  
**Çözüm:** Yok.  
  
|Özel durum iletileri|  
|------------------------|  
|Erişilebilir bir açık yöntemi bulunan eşittir.|  
|Sütun değerleri karşılaştırılamaz \\"{0}\\" türündeki {1}. Erişilebilir bir açık yöntemi bulunan eşittir.|  


## <a name="error-0044"></a>Hata 0044  
 Var olan değerleri öğe türü sütun türetmek mümkün değilse, özel durum oluşur.  
  
 Bu hata Azure Machine learning'de bir sütun veya sütun bir veri türünün çıkarsanması mümkün olmadığında gerçekleşir. Bu genellikle iki veya daha fazla veri kümeleri farklı öğe türleri ile birleştirerek olduğunda gerçekleşir. Azure Machine Learning bir sütun veya sütunlar bilgi kaybı olmadan tüm değerleri temsil etmesi mümkün olan bir genel türü belirlenemiyor ise, bu hata oluşturur.  
  
**Çözüm:** Birleştirilen her iki veri kümesi içinde belirli bir sütundaki tüm değerleri ya da aynı olduğundan emin olun (sayısal, Boole, kategorik, dize, tarih, vb.) yazın veya aynı türüne dönüştürülebilen.  
  
|Özel durum iletileri|  
|------------------------|  
|Öğe türü sütununun türetilemez.|  
|Sütun için öğe türü türetilemez "{0}"--tüm öğeleri null başvurulardır.|  
|Sütun için öğe türü türetilemez "{0}"veri kümesinin"{1}"--tüm öğeleri null başvurulardır.|  
  

## <a name="error-0045"></a>Hata 0045  
 Özel durum nedeniyle kaynak karma öğe türleri bir sütun oluşturmak mümkün değildir oluşur.  
  
 İki veri kümesi birleştirilmeye öğe türleri farklı olduğunda bu hata Azure Machine learning'de oluşturulur.  
  
**Çözüm:** Birleştirilen her iki veri kümesi içinde belirli bir sütundaki tüm değerler aynı türde (sayısal, Boole, kategorik, dize, tarih, vb.) olduğundan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Sütun karma öğe türleri ile oluşturulamıyor.|  
|Sütun kimliği oluşturulamıyor "{0}" karma öğe türleri: \n\tType veri, [{1}, {0}] olan {2}\n\tType veri [{3}, {0}] olan {4}.|  
  

## <a name="error-0046"></a>Hata 0046  
 Belirtilen yolda dizin oluşturmak mümkün değilse, özel durum oluşur.  
  
 Belirtilen yolda bir dizin oluşturmak mümkün değilse, Azure Machine learning'de bu hata oluşur. Herhangi bir Hive sorgusu için çıktı dizini yolu yanlış veya erişilemeyen parçasıysa, bu hatayı alırsınız.  
  
**Çözüm:** Modülün yeniden ziyaret ve dizin yolu düzgün biçimlendirildiğinden ve geçerli kimlik bilgileriyle erişilebilir olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Geçerli çıkış dizinini belirtin.|  
|Dizin: {0} oluşturulamaz. Geçerli bir yol belirtin.|  
  

## <a name="error-0047"></a>Hata 0047  
 Bazı modülü aktarılan veri kümeleri özelliği sütun sayısı çok küçükse, özel durum oluşur.  
  
 Eğitim için giriş veri kümesi en az sayıda algoritma tarafından gerekli sütunları içermiyor, Azure Machine learning'de bu hata meydana gelir. Genellikle bir veri kümesi ya da boş veya yalnızca eğitim sütunları içerir.  
  
**Çözüm:** Emin var. bir veya daha fazla ek sütunları etiket sütun dışında yapmak için girdi veri kümesini yeniden ziyaret edin.  
  
|Özel durum iletileri|  
|------------------------|  
|Giriş veri kümesi özelliği sütunlardaki minimum izin verilenden daha az sayısıdır.|  
|Giriş veri kümesi özelliği sütun sayısı en az izin verilenden daha az olan {0} sütunları.|  
|Giriş veri kümesi özelliği sütun sayısı "{0}" en az izin verilenden daha küçük {1} sütunları.|  
  

## <a name="error-0048"></a>Hata 0048  
 Bir dosyayı açmaya mümkün olmadığında durumda özel durum oluşur.  
  
 Bu hata Azure Machine learning'de bir dosyayı açmak için okuma veya yazma mümkün olmadığında gerçekleşir. Bu nedenlerden dolayı bu hatayı alabilirsiniz:  
  
-   Kapsayıcı ya da dosya (blob) mevcut değil  
  
-   Dosya veya kapsayıcı erişim düzeyi, dosyaya erişmek izin vermez  
  
-   Dosya okuma veya biçimi yanlış için çok büyük.  
  
**Çözüm:** Modül ve okumak için çalıştığınız dosya yeniden ziyaret edin.  
  
 Kapsayıcı ve dosya adlarını doğru olduğundan emin olun.  
  
 Dosyaya erişim izniniz olduğunu doğrulamak için Klasik Azure portalı ya da bir Azure depolama aracını kullanın.  
  
  <!--If you are trying to read an image file, make sure that it meets the requirements for image files in terms of size, number of pixels, and so forth. For more information, see [Import Images](import-images.md).  -->
  
|Özel durum iletileri|  
|------------------------|  
|Dosya açılamıyor.|  
|Dosya açılırken hata oluştu: {0}.|  


## <a name="error-0049"></a>Hata 0049  
 Dosya ayrıştırma mümkün olmadığında durumda özel durum oluşur.  
  
 Azure Machine learning'de bu hata, bir dosyayı ayrıştırmak mümkün değildir oluşur. Dosya biçimi'ı seçtiyseniz bu hatayı alırsınız [verileri içeri aktarma](import-data.md) modül dosyasının gerçek biçimi eşleşmiyor veya dosya tanınmayan bir karakter içeriyorsa.  
  
**Çözüm:** Modülün yeniden ziyaret ve dosya biçimi seçimi dosyasının biçimi eşleşmiyorsa düzeltin. Mümkünse, herhangi bir geçersiz karakter içermediğinden emin onaylamak için dosyasını inceleyin.  
  
|Özel durum iletileri|  
|------------------------|  
|Bir dosya ayrıştırılamıyor.|  
|Dosya ayrıştırılırken hata oluştu: {0}.|  
  

## <a name="error-0050"></a>0050 hata  
 Ne zaman giriş durumda özel durum oluşur ve çıkış dosyalarının aynıdır.  
  
**Çözüm:** Bu hata Azure Machine learning'de kullanılmayan ve kaldırılacak.  
  
|Özel durum iletileri|  
|------------------------|  
|Belirtilen giriş ve Çıkış dosyalarını aynı olamaz.|


## <a name="error-0051"></a>Hata 0051  
 Birden çok çıktı dosyalarını da aynı olduğunda durumda özel durum oluşur.  
  
**Çözüm:** Bu hata Azure Machine learning'de kullanılmayan ve kaldırılacak.  
  
|Özel durum iletileri|  
|------------------------|  
|Belirtilen çıkış dosyalarını aynı olamaz.|


## <a name="error-0052"></a>Hata 0052  
 Azure depolama hesabı anahtarı hatalı şekilde belirtildi, özel durum meydana gelir.  
  
 Azure depolama hesabına erişmek için kullanılan anahtarı yanlışsa, Azure Machine learning'de bu hata oluşur. Örneğin, Azure depolama anahtarını kopyalanır ve yapıştırılan kesildi veya yanlış anahtar kullandıysanız şu hatayla karşılaşabilirsiniz.  
  
 Bir Azure depolama hesabı anahtarı alma hakkında daha fazla bilgi için bkz. [görüntüleme, kopyalama ve yeniden oluşturma depolama erişim anahtarlarını](https://azure.microsoft.com/documentation/articles/storage-create-storage-account-classic-portal/).  
  
**Çözüm:** Modülün yeniden ziyaret ve Azure depolama anahtarını hesap için doğru olduğundan emin olun. anahtarı yeniden gerekiyorsa, Klasik Azure portalından kopyalayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Azure depolama hesabı anahtarı doğru değil.|  
  

## <a name="error-0053"></a>Hata 0053  
 Hiçbir kullanıcı özellik veya matchbox önerileri için öğeleri olduğunda durumda özel durum oluşur.  
  
 Bu hata Azure Machine learning'de özellik vektör bulunamadığında oluşturulur.  
  
**Çözüm:** Özellik vektör giriş veri kümesinde mevcut olduğundan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Kullanıcı özellikleri veya / ve öğeleri gerekli ancak sağlanmadı.|  

## <a name="error-0054"></a>Hata 0054  
 İşlemi tamamlamak için bir sütunda birbirinden çok az sayıda değer ise özel durum oluşur.  
  
**Çözüm:** Bu hata Azure Machine learning'de kullanılmayan ve kaldırılacak.  
  
|Özel durum iletileri|  
|------------------------|  
|Veri, işlemi tamamlamak için belirtilen sütunda çok az sayıda farklı değer bulunur.|  
|Veri, işlemi tamamlamak için belirtilen sütunda çok az sayıda farklı değer bulunur. Gerekli en düşük gerekliliktir {0} öğeleri.|  
|Veri sütunu birbirinden çok az sayıda değer yok "{1}" işlemi tamamlamak için. Gerekli en düşük gerekliliktir {0} öğeleri.|  
  

## <a name="error-0055"></a>Hata 0055  
 Kullanım dışı bir modül çağrılırken özel durum oluşur.  Kullanım dışı bir modül çağrılacak denerseniz bu hata Azure Machine learning'de görünür.
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Kullanım dışı modülü erişme.|  
|Modül "{0}" kullanım dışı bırakılmıştır.|  

## <a name="error-0056"></a>Hata 0056  
 Bir işlem için Seçili sütunları gereksinimleri bozup özel durum oluşur.  
  
 Belirli veri türünde sütun gerektiren bir işlem için sütunları seçerken Azure Machine learning'de bu hata oluşur. 
 
 Bu hata ayrıca sütunu doğru veri türünü, ancak kullandığınız modülü, sütun da bir özellik, etiket veya Kategorik bir sütun işaretlenmesini gerektirir. oluşabilir.  
  
  <!--For example, the [Convert to Indicator Values](convert-to-indicator-values.md) module requires that columns be categorical, and will raise this error if you select a feature column or label column.  -->
  
**Çözüm:**
  
1.  Şu anda seçili olan sütunların veri türünü gözden geçirin. 

2. Seçili sütunları kategorik, olup olmadığını belirlemek etiket veya özellik sütunları.  
  
3.  Veri türü veya sütun kullanımı için belirli gereksinimler olup olmadığını belirlemek için sütun seçimini olmuş modül için Yardım konusuna bakın.  
  
3.  Kullanım [meta verileri Düzenle](edit-metadata.md) sütun türü bu işlem süresince değiştirmek için. Başka bir örneğini kullanarak özgün değerine için sütun türü değiştirdiğinizden emin olun [meta verileri Düzenle](edit-metadata.md), aşağı akış işlemleri için gerekiyorsa.  
  
|Özel durum iletileri|  
|------------------------|  
|Bir veya daha fazla seçili sütunları izin verilen bir kategoride değildi.|  
|Sütun adı "{0}" içinde izin verilen bir kategori değil.|  
  

## <a name="error-0057"></a>Hata 0057  
 Bir dosya veya zaten mevcut blob oluşturulmaya çalışılırken özel durum oluşur.  
  
 Kullanmakta olduğunuz bu özel durumun meydana [verileri dışarı aktarma](export-data.md) modül veya Azure blob depolama, ancak, Azure Machine Learning'de bir deneme sonuçlarını kaydetmek için başka bir modül girişiminde bir dosya veya zaten mevcut blob oluşturmak.   
  
**Çözüm:**
 
 Yalnızca daha önce özelliğini ayarlarsanız bu hatayı alırsınız **Azure blob depolama alanına yazma modu** için **hata**. Bir veri kümesi zaten var olan bir bloba yazma denerseniz, tasarım gereği, bu modül bir hata oluşturur.
 
 - Modül özelliklerini açın ve özelliğini değiştirin **Azure blob depolama alanına yazma modu** için **üzerine yaz**.
 - Alternatif olarak, farklı bir hedef blob veya dosya adını yazın ve henüz yoksa bir blob belirttiğinizden emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Dosya veya Blob zaten var.|  
|Dosya veya Blob "{0}" zaten mevcut.|  
  

## <a name="error-0058"></a>Hata 0058  
 Veri kümesi beklenen etiket sütunu içermiyor, Azure Machine learning'de bu hata meydana gelir.  
  
 Bu özel etiket sütunu veri veya learner tarafından beklenen veri türü eşleşmiyor sağlanan zaman da meydana gelebilir veya hatalı değerler vardır. Örneğin, bir etiket gerçek değerli sütun bir ikili dosya sınıflandırıcı eğitimindeki kullanırken bu özel durum oluşturulur.  
  
**Çözüm:** Learner veya kullanmakta olduğunuz trainer ve veri kümenizde sütunları veri türlerini çözünürlüğüne bağlıdır. İlk olarak, makine öğrenimi algoritmasının veya eğitim modülü gereksinimlerini doğrulayın.  
  
 Girdi veri kümesini yeniden ziyaret edin. Etiket doğru veri modelini oluşturmakta olduğunuz türüne sahip olarak kabul edilir beklenir sütunu doğrulayın.  
  
 Eksik değerler için girişlerini denetleyin ve kaldırın veya gerekirse değiştirin.  
  
 Gerekirse, ekleme [meta verileri Düzenle](edit-metadata.md) modülü ve etiket sütununda bir etiket olarak işaretlendiğinden emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Etiket sütunu beklendiği gibi değil|  
|Etiket sütunu, beklendiği gibi olduğu "{0}".|  
|Etiket sütununda "{0}"beklenmiyor"{1}".|  
  

## <a name="error-0059"></a>Hata 0059  
 Sütun seçicide belirtilen bir sütun dizini ayrıştırılamıyor. özel durum ortaya çıkar.  
  
 Sütun seçiciyi kullanarak, belirtilen bir sütun dizini nelze analyzovat, Azure Machine learning'de bu hata meydana gelir.  Sütun dizini nelze analyzovat geçersiz bir biçimde olduğunda bu hatayı alırsınız.  
  
**Çözüm:** Geçerli dizin değerini kullanmak için sütun dizini değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Bir veya daha fazla belirtilen sütun dizinleri ya da dizin aralık ayrıştırılamadı.|  
|Sütun dizini veya aralığı "{0}" ayrıştırılamadı.|  
  

## <a name="error-0060"></a>Hata 0060  
 Aralık sütunu aralık dışı bir sütun seçicide belirtildiğinde özel durum oluşur.  
  
 Sütun seçicide belirtilen bir aralık dışı sütun aralığı Azure Machine learning'de bu hata oluşur. Sütun seçiciyi sütunu aralığında kümesindeki sütunlara karşılık gelmiyorsa bu hatayı alırsınız.  
  
**Çözüm:** Dataset sütunları değerine karşılık gelen sütun Seçici sütunu aralığında değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Geçersiz veya belirtilen aralık sütun dizini aralık dışında.|  
|Sütun aralığı "{0}" geçersiz veya aralık dışında.|  
  

## <a name="error-0061"></a>Hata 0061  
 Tablonun sütun sayıları farklı olan bir DataTable tablosuna satır eklemek çalışırken özel durum oluşur.  
  
 Azure Machine learning'de bu hata, farklı sayıda veri kümesini daha sütunları olan bir veri kümesi için bir satır ekleme girişimi oluşur.  Giriş veri kümesindeki sütunları farklı sayıda veri kümesine eklenen satır varsa, bu hatayı alırsınız.  Sütun sayısı farklıysa satır kümesine eklenemiyor.  
  
**Çözüm:** Eklenen satır aynı sayıda sütuna sahip için girdi veri kümesini değiştirmek veya veri kümesi aynı sayıda sütuna sahip için eklenen satır değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Tüm tablolar aynı sayıda sütuna sahip olmalıdır.|  
  

## <a name="error-0062"></a>Hata 0062  
 Farklı learner türleriyle iki modeli karşılaştırmak çalışılırken özel durum oluşur.  
  
 Azure Machine learning'de bu hata, iki farklı puanlanmış veri kümeleri için değerlendirme ölçümleri karşılaştırıldığında oluşturulur. Bu durumda, iki puanlanmış veri kümesi oluşturmak için kullanılan modelleri verimliliğini karşılaştırmak mümkün değildir.  
  
**Çözüm:** Puanlanmış sonuçların aynı türde bir makine öğrenme modelinin (ikili Sınıflandırma, regresyon, çok sınıflı Sınıflandırma, öneri, kümeleme, anomali algılama, vb.) tarafından üretilir doğrulayın Kullanılabilirliğiyle karşılaştırmanızı tüm modelleri, aynı learner türüne sahip olmalıdır.  
  
|Özel durum iletileri|  
|------------------------|  
|Tüm modelleri, aynı learner türüne sahip olmalıdır.|  
  

 <!--## Error 0063  
 This exception is raised when R script evaluation fails with an error.  
  
 This error occurs when you have provided an R script in one of the [R language modules](r-language-modules.md) in Azure Machine Learning, and the R code contains internal syntax errors. The exception can also occur if you provide the wrong inputs to the R script. 
 
 The error can also occur if the script is too large to execute in the workspace. The maximum script size for the **Execute R Script** module is 1,000 lines or 32 KB of work space, whichever is lesser.
  
**Resolution:**

1. In Azure Machine Learning Studio, right-click the module that has the error, and select **View Log**.
2. Examine the standard error log of the module, which contains the stack trace.
    + Lines beginning with [ModuleOutput] indicate output from R.
    + Messages from R marked as **warnings** typically do not cause the experiment to fail.
3. Resolve script issues.  
    + Check for R syntax errors. Check for variables that are defined but never populated.
    + Review the input data and the script to determine if either the data or variables in the script use characters not supported by Azure Machine Learning.
    + Check whether all package dependencies are installed.
    + Check whether your code loads required libraries that are not loaded by default.
    + Check whether the required packages are the correct version.
    + Make sure that any dataset that you want to output is converted to a data frame.  
4.  Resubmit the experiment.

 <!--
> [!NOTE]
> These topics contains examples of R code that you can use, as well as links to experiments in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com) that use R script.
> + [Execute R Script](execute-r-script.md)
> + [Create R Model](create-r-model.md)
-->  
|Özel durum iletileri|  
|------------------------|  
|R betiği değerlendirme sırasında hata oluştu.|  
|R betiği değerlendirmesi sırasında şu hata oluştu:---R hata iletisinden başlangıcı--- {0} ---R hata iletisinden sonu---|  
|R betiği değerlendirmesi sırasında "{1}" şu hata oluştu:---R hata iletisinden başlangıcı--- {0} ---R hata iletisinden sonu---|  
  


## <a name="error-0064"></a>Hata 0064  
 Azure depolama hesabı adı veya depolama anahtarı hatalı şekilde belirtildi, özel durum meydana gelir.  
  
 Azure depolama hesabı adı veya depolama anahtarı hatalı şekilde belirtildi, Azure Machine learning'de bu hata meydana gelir. Depolama hesabı için hatalı bir hesap adı veya parola girerseniz, bu hatayı alırsınız. Hesap adı veya parola el ile girdiğinizde, bu durum oluşabilir. Hesap silindiyse da oluşabilir.  
  
**Çözüm:** Hesap adı ve parola doğru girildiğini ve hesabı bulunduğundan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Azure depolama hesabı adı veya depolama anahtarı doğru değil.|  
|Azure depolama hesabı adı "{0}" veya hesap adı için depolama anahtarı hatalı.|  
  

## <a name="error-0065"></a>Hata 0065  
 Azure blob adı yanlış belirtildiği, özel durum meydana gelir.  
  
 Azure blob adı yanlış belirtildiği, Azure Machine learning'de bu hata meydana gelir.  Varsa hatayı alırsınız:  
  
-   Belirtilen kapsayıcı içinde blob bulunamıyor.  
  
 <!---   The fully qualified name of the blob specified for output in one of the [Learning with Counts](data-transformation-learning-with-counts.md) modules is greater than 512 characters.  -->
  
-   Kapsayıcı yalnızca kaynağı olarak belirtilen bir [verileri içeri aktar](import-data.md) istek kodlaması ile Excel veya CSV biçiminde olduğu zaman; bir kapsayıcıdaki tüm blobları içeriğini birleşimini ile Bu biçimler izin verilmiyor.  
  
-   SAS URI'si geçerli bir blob adını içermiyor.  
  
**Çözüm:** Özel durum modülü yeniden ziyaret edin. Belirtilen blob kapsayıcısında depolama hesabındaki mevcut olduğunu ve izinler blob görmenize izin doğrulayın. Girdi biçiminde olduğundan emin olun **containername/filename** biçimlerini kodlama ile Excel veya CSV varsa. SAS URI'si geçerli bir blob adı içerdiğini doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Azure depolama blobu doğru değil.|  
|Azure depolama blob adı "{0}" yanlış|  
  

## <a name="error-0066"></a>Hata 0066  
 Bir kaynak için bir Azure Blob karşıya yüklenemedi, özel durum meydana gelir.  
  
 Bir kaynak için bir Azure Blob karşıya yüklenemedi, Azure Machine learning'de bu hata meydana gelir.  <!--You will receive this message if [Train Vowpal Wabbit 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) encounters an error attempting to save either the model or the hash created when training the model.--> Her ikisi de giriş dosyası içeren hesabıyla aynı Azure depolama hesabına kaydedilir.  
  
**Çözüm:** Modülün yeniden ziyaret edin. Azure hesap adı, depolama anahtarı ve kapsayıcının doğru olduğunu ve hesabın kapsayıcıya yazma izni olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Azure depolama için kaynak yüklenemedi.|  
|Dosya "{0}" Azure depolama birimine yüklenemedi {1}.|  
  

## <a name="error-0067"></a>Hata 0067  
 Dataset sütunları beklenenden farklı sayıda varsa, özel durum oluşur.  
  
 Dataset sütunları beklenenden farklı sayıda varsa, Azure Machine learning'de bu hata oluşur.  Veri kümesinde sütun sayısı modülü yürütme sırasında bekliyor sütunların sayısından farklı olduğunda bu hatayı alırsınız.  
  
**Çözüm:** Giriş veri kümesi veya parametrelerini değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Beklenmeyen bir datatable tablosundaki sütun sayısı.|  
|Beklenen "{0}"sütunları ancak bulunamadı"{1}" sütunlar yerine.|  
  

## <a name="error-0068"></a>Hata 0068  
 Belirtilen Hive betiğini doğru değilse, özel durum oluşur.  
  
 Bu hata Azure Machine learning'de QL Hive komut dosyasında sözdizimi hataları varsa ya da Hive yorumlayıcı sorgu veya betik yürütülürken bir hatayla karşılaştığında gerçekleşir.  
  
**Çözüm:**

Belirli hataya göre eylem yararlanabilmeniz Hive hata iletisinden normalde geri hata günlüğünde raporlanır. 

+ Modülün açın ve sorgu hatalar için inceleyin.  
+ Sorgu dışında Azure Machine Learning Hadoop kümenizin Hive Konsolu'nda oturum açtıktan ve sorguyu çalıştıran doğru şekilde çalıştığını doğrulayın.  
+ Hive komut yürütülebilir deyimleri ile tek bir satırda açıklama karıştırma aksine ayrı bir satırda açıklama yerleştirmeyi deneyin.  

### <a name="resources"></a>Kaynaklar

Hive sorguları için machine learning ile ilgili Yardım için aşağıdaki makalelere bakın:

+ [Hive tabloları oluşturma ve Azure Blob depolamadan veri yükleme](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-move-hive-tables)
+ [Hive sorguları ile tablolardaki verileri keşfedin](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-explore-data-hive-tables)
+ [Hive sorgularını kullanarak bir Hadoop kümesindeki verilerin özelliklerini oluşturma](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-create-features-hive)
+ [Hive için SQL kullanıcı bilgi sayfası (PDF)](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf)

  
|Özel durum iletileri|  
|------------------------|  
|Hive betiğinin doğru değil.|  
|Hive betiği {0} doğru değil.|  
  

## <a name="error-0069"></a>Hata 0069  
 Belirtilen SQL betiğini doğru değilse, özel durum oluşur.  
  
 Belirtilen SQL betik söz dizimi sorunlarını sahip veya bu tablo ve sütunları belirtilen betik geçerli değilse, Azure Machine learning'de bu hata oluşur. 
 
 SQL altyapısı sorgu veya betik yürütülürken bir hatayla karşılaştığında bu hatayı alırsınız. Belirli hataya göre eylem yararlanabilmeniz SQL hata iletisi normalde geri hata günlüğünde raporlanır.  
  
**Çözüm:** Modülün yeniden ziyaret ve SQL sorgusu hatalar için inceleyin.  
  
 Sorgu dışında Azure ML veritabanı sunucusuna doğrudan oturum açma ve sorguyu çalıştıran doğru şekilde çalıştığını doğrulayın.  
  
 Bildirilen özel durum modülü tarafından oluşturulan SQL ileti varsa, bildirilen hataya göre eylemde. Örneğin, hata iletileri, büyük olasılıkla hata ilişkin yönergeler şunlardır:
+ *Böyle bir sütun veya eksik veritabanı*, gösteren bir sütun adı yanlış yazmış olabilirsiniz. Sütun adının doğru olduğundan emin olup, köşeli ayraç veya tırnak işaretleri içine sütun kimliği kullanmayı deneyin.
+ *SQL mantık hatası yakın \<SQL anahtar sözcüğü\>* , belirtilen anahtar sözcüğü önce bir sözdizimi hatası olabilir belirten

  
|Özel durum iletileri|  
|------------------------|  
|SQL komut dosyası geçersiz.|  
|SQL sorgusu "{0}" doğru değil.|  
|SQL sorgusu "{0}" doğru değildir: {1}|  
  

## <a name="error-0070"></a>Hata 0070  
 Azure var olmayan tablo erişmeye çalışılırken özel durum oluşur.  
  
 Mevcut olmayan bir Azure tablosu erişmeye çalıştığında Azure Machine learning'de bu hata oluşur. Okuma veya Azure tablo depolama alanına yazılmasını yok. Azure depolamada bir tablo belirtirseniz bu hatayı alırsınız. İstediğiniz tabloyu adı yanlış veya hedef adı ve depolama türü arasında bir uyuşmazlık varsa bu durum ortaya çıkabilir. Örneğin, bir tablodan okumak hedeflenen ancak bunun yerine bir blob adı girdiniz.  
  
**Çözüm:** Tablo adının doğru olduğunu doğrulamak için modülü yeniden ziyaret edin.  
  
|Özel durum iletileri|  
|------------------------|  
|Azure tablosu yok.|  
|Azure tablo "{0}" yok.|  
  
## <a name="error-0071"></a>Hata 0071  
 Sağlanan kimlik bilgileri hatalıdır özel durum oluşur.  
  
 Sağlanan kimlik bilgileri yanlış olduğunda, Azure Machine learning'de bu hata oluşur.  
  
 Modül bir HDInsight kümesine bağlanamıyorsanız, bu hatayı alabilirsiniz.  
  
**Çözüm:** Modül yönelik girişleri inceleyin ve hesap adını ve parolayı doğrulayın.  
  
 Bir hataya neden olabilir aşağıdaki sorunlar için denetleyin:  
  
-   Veri kümesi şemasını hedef datatable şeması eşleşmiyor.  
  
-   Sütun adı eksik ya da yanlış yazılmış.  
  
-   Geçersiz karakterler içeren sütun adları içeren bir tabloya yazıyorsunuz. Normalde, bu sütun adları köşeli parantez içine, ancak sütun adları yalnızca harf ve alt çizgi (_) kullanmak için bu işe yaramazsa, Düzenle  
  
-   Yazma çalıştığınız dizeleri tek tırnak işaretleri  
  
 Bir HDInsight kümesine bağlanmaya çalışıyorsanız, hedef kümenin sağlanan kimlik bilgileri ile erişilebilir olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Yanlış kimlik bilgileri geçirilir.|  
|Hatalı kullanıcı adı "{0}" ya da parola geçirilir|  
  

## <a name="error-0072"></a>Hata 0072  
 Özel durum söz konusu olduğunda bağlantı zaman aşımı oluşur.  
  
 Azure Machine learning'de bu hata, bir bağlantı zaman aşımına oluşur. Şu anda veri kaynağı veya yavaş internet bağlantısı gibi bir hedef bağlantı sorunları varsa ya da büyük veri kümesi ve/veya karmaşık bir işlem verileri okumak için SQL sorgusunu gerçekleştirir, bu hatayı alırsınız.  
  
**Çözüm:** Azure depolama veya internet yavaş bağlantı sorunları şu anda olup olmadığını belirler.  
  
|Özel durum iletileri|  
|------------------------|  
|Bağlantı zaman aşımı oluştu.|  
  

## <a name="error-0073"></a>Hata 0073  
 Bir sütunu başka bir türe dönüştürülürken bir hata oluşursa özel durum oluşur.  
  
 Bu hata Azure Machine learning'de sütunu başka bir türe dönüştürmek mümkün değildir oluşur.  Belirli bir tür bir modül gerektirir ve sütun yeni türe dönüştürmek mümkün değildir, bu hatayı alırsınız.  
  
**Çözüm:** Giriş veri kümesi sütunu, iç özel duruma göre dönüştürülebilir şekilde değiştirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Sütun dönüştürülemedi.|  
|Sütun dönüştürülemedi {0}.|  
  

## <a name="error-0074"></a>Hata 0074  
 Özel durum oluşursa, [meta verileri Düzenle](edit-metadata.md) bir seyrek sütun için kategorik dönüştürmeye çalışıyor.  
  
 Azure Machine learning'de bu hata oluşur, [meta verileri Düzenle](edit-metadata.md) bir seyrek sütun için kategorik dönüştürmeye çalışıyor.  İle Kategorik bir seyrek sütun dönüştürmek çalışırken bu hatayı alırsınız **kategorik olun** seçeneği.  Azure machine Learning modülü başarısız olacak şekilde seyrek kategorik diziler desteklemez.  
  
 <!--**Resolution:**
 Make the column dense by using [Convert to Dataset](convert-to-dataset.md) first or do not convert the column to categorical.  -->
  
|Özel durum iletileri|  
|------------------------|  
|Seyrek sütun için kategorik dönüştürülemez.|  
  

## <a name="error-0075"></a>Hata 0075  
Geçersiz bir gruplama işlevi bir veri kümesi quantizing olduğunda kullanılır. özel durum oluşur.  
  
Azure Machine learning'de bu hata, desteklenmeyen bir yöntem kullanarak verileri depoya çalışırken veya parametre birleşimlerini geçersiz olduğunda oluşur.  
  
**Çözüm:**

Hata için bu olay işleme yöntemleri gruplama, daha fazla özelleştirme izin verilen bir Azure Machine Learning daha önceki bir sürümünde kullanıma sunulmuştur. Şu anda tüm gruplama yöntemleri seçimi aşağı açılan listeden, bu nedenle teknik artık bu hatayı almak olası olmalıdır temel alır.

 <!--If you get this error when using the [Group Data into Bins](group-data-into-bins.md) module, consider reporting the issue in the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MachineLearning), providing the data types, parameter settings, and the exact error message.  -->
  
|Özel durum iletileri|  
|------------------------|  
|Geçersiz gruplama işlevi kullanılır.|  
  

## <a name="error-0077"></a>Hata 0077  
 Geçirilen modu bilinmeyen blob dosyası Yazar özel durum oluşur.  
  
 Geçersiz bağımsız değişken belirtimleri blob dosya hedef veya kaynak için geçirilen, Azure Machine learning'de bu hata meydana gelir.  
  
**Çözüm:** Ve Azure blob depolama alanından verileri dışarı aktarma veya içeri aktarın, neredeyse tüm modüllerdeki açılan listesini kullanarak yazma modunu denetleme parametre değerlerini atanır; Bu nedenle, geçersiz bir değer geçirmek mümkün değildir ve bu hatayı yok görünüyor. Bu hata bir sonraki sürümde kaldırılacak.  
  
|Özel durum iletileri|  
|------------------------|  
|Desteklenmeyen bir blob modu yazar.|  
|Desteklenmeyen bir blob yazma modu: {0}.|  
  

## <a name="error-0078"></a>Hata 0078  
 Özel durum oluştuğunda zaman HTTP seçeneği için [verileri içeri aktarma](import-data.md) 3xx, yeniden yönlendirme belirten bir durum kodu alır.  
  
 Bu hata Azure Machine learning'de zaman HTTP seçeneği için [verileri içeri aktar](import-data.md) bir 3xx alır (301, 302, 304, vb.) yeniden yönlendirme gösteren durum kodu. Tarayıcıda başka bir sayfaya yönlendiren HTTP kaynağına bağlanmaya çalışırsanız, bu hatayı alırsınız. Güvenlik için Web sitelerini yeniden yönlendirme nedeniyle, veri kaynağı olarak Azure Machine Learning için izin verilmez.  
  
**Çözüm:** Web sitesi güvenilen bir Web sitesi ise, yeniden yönlendirilen URL'sini doğrudan girin.  
  
|Özel durum iletileri|  
|------------------------|  
|Http yeniden yönlendirmesi izin verilmiyor|  
  

## <a name="error-0079"></a>Hata 0079  
 Azure depolama kapsayıcısı adı yanlış belirtildiği, özel durum meydana gelir.  
  
 Azure depolama kapsayıcısı adı yanlış belirtildiği, Azure Machine learning'de bu hata meydana gelir. Kapsayıcı ve blob (dosya) adını kullanarak belirtmediyseniz bu hatayı alırsınız **blob kapsayıcısı ile başlayan yol** Azure Blob depolama alanına yazarken seçeneği.  
  
**Çözüm:** Yeniden ziyaret [verileri dışarı aktarma](export-data.md) modülü ve belirtilen yola blob kapsayıcı hem dosya adı biçiminde içerdiğini doğrulayın **kapsayıcı/filename**.  
  
|Özel durum iletileri|  
|------------------------|  
|Azure depolama kapsayıcısı adı doğru değil.|  
|Azure depolama kapsayıcısı adı "{0}" yanlış; bir biçim kapsayıcı/blob kapsayıcı adı bekleniyordu.|  
  

## <a name="error-0080"></a>0080 hata  
 Eksik olan tüm değerleri içeren sütun modülü tarafından izin verilmiyor özel durum oluşur.  
  
 Bir veya daha fazla modülü tarafından kullanılan sütunları tüm eksik değerler içerdiğinde, bu hata Azure Machine learning'de oluşturulur. Örneğin, bir modül istatistiklerin her sütun için bilgi işlem, veri içeren bir sütun üzerinde çalışamaz. Böyle durumlarda, bu özel durumla modülü yürütmesi durdurulur.  
  
**Çözüm:** Girdi veri kümesini yeniden ziyaret ve tüm eksik değerler içeren tüm sütunları kaldırın.  
  
|Özel durum iletileri|  
|------------------------|  
|Eksik olan tüm değerleri sütunlarla kullanılamaz.|  
|Sütun {0} eksik olan tüm değerlere sahip.|  
  

## <a name="error-0081"></a>Hata 0081  
 İndirmek için boyut sayısı en az bir özellik seyrek sütunu içeren giriş veri kümesinde özelliği sütun sayısına eşit ise özel durum PCA modülünde gerçekleşir.  
  
 Bu hata Azure Machine learning'de üretilen aşağıdaki koşullar karşılanmalıdır: (a) giriş veri kümesi en az bir seyrek sütun vardır ve (b) istenen boyut son sayısını giriş boyutların sayısı ile aynıdır.  
  
**Çözüm:** Boyutta girişte sayısından az olacak şekilde çıktıda boyut sayısını azaltmayı deneyin. PCA uygulamalarda tipik budur.   <!--For more information, see [Principal Component Analysis](principal-component-analysis.md).  -->
  
|Özel durum iletileri|  
|------------------------|  
|Veri kümesi için seyrek özellik sütunlarını içeren azaltmak için boyut sayısı özelliği sütun sayısından daha az olmalıdır.|  
 

## <a name="error-0082"></a>Hata 0082  
 Bir model başarıyla seri durumdan çıkarılmış olamaz. özel durum oluşur.  
  
 Bu hata Azure Machine Learning, kaydedilmiş bir machine learning modeli oluşur veya dönüştürme tarafından yeni Azure Machine Learning çalışma zamanının bir sürümünü bozucu bir değişiklik nedeniyle yüklenemiyor.  
  
**Çözüm:** Model veya dönüştürme üretilen eğitim denemenizi yeniden çalıştırın ve modeli olmalıdır veya dönüştürme sıfırlanması gerekir.  
  
|Özel durum iletileri|  
|------------------------|  
|Büyük olasılıkla eski bir serileştirme biçimiyle seri için model seri olmayan hale getirilemedi. Yeniden eğitme ve modeli kaydedin.|  
  

## <a name="error-0083"></a>Hata 0083  
 Eğitim veri kümesi için kullandıysanız somut tür learner için kullanılamaz, özel durum oluşur.  
  
 Veri kümesi eğitimli learner ile uyumsuz olduğunda bu hata Azure Machine learning'de oluşturulur. Örneğin, veri kümesini her satırdaki en az bir eksik değer içerebilir ve sonuç olarak, veri kümesinin tamamının eğitim sırasında atlandı. Diğer durumlarda, anomali algılama gibi bazı makine öğrenimi algoritmaları bulunması için etiketleri beklemiyoruz ve etiketleri kümesinde mevcut değilse bu durum oluşturabilir.  
  
**Çözüm:** Giriş veri kümesi gereksinimleri kontrol etmek için kullanılan learner belgelerine başvurun. Tüm gerekli sütun var olduğunu görmek için sütunları inceleyin.  
  
|Özel durum iletileri|  
|------------------------|  
|Eğitim için kullanılan veri kümesi geçersiz.|  
|{0} Eğitim için geçersiz veri içeriyor.|  
|{0} Eğitim için geçersiz veri içeriyor. Learner türü: {1}.|  
  

## <a name="error-0084"></a>Hata 0084  
 Bir R betiğini üretilen puanları değerlendirildiğinde özel durum oluşur. Bu şu anda desteklenmemektedir.  
  
 Azure Machine learning'de bu hata, puanlarını içeren bir R betiği çıktısını modeliyle değerlendirmesi için modüllerinden birini kullanmayı denerseniz oluşur.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|R tarafından üretilen puanları değerlendirme şu anda desteklenmiyor.|  
  

## <a name="error-0085"></a>Hata 0085  
 Özel durum, betik yorumlamalarını bir hata ile başarısız olduğunda gerçekleşir.  
  
 Söz dizimi hataları içeren özel komut dosyası çalıştırılırken, Azure Machine learning'de bu hata oluşur.  
  
**Çözüm:** Kodunuzda bir dış düzenleyici ve hataları gözden geçirin.  
  
|Özel durum iletileri|  
|------------------------|  
|Betik, değerlendirme sırasında hata oluştu.|  
|Betik yorumlama sırasında şu hata oluştu, daha fazla bilgi için çıkış günlüğü görüntüleyin:---hata iletisinden başlangıcı {0} yorumlayıcı--- {1} ---hata iletisinden sonuna {0} yorumlayıcı--- ------|  
  

## <a name="error-0086"></a>Hata 0086  
 Sayım dönüştürme geçersiz özel durum oluşur.  
  
 Bu hata Azure Machine learning'de sayısı tabloyu temel alan bir dönüştürme seçin, ancak seçilen dönüştürme geçerli verileri veya yeni sayısı tablo ile uyumlu değil oluşur.  
  
**Çözüm:** Modül sayısı ve iki farklı biçimlerde dönüşümü oluşturan kurallar kaydetme destekler. Sayısı tabloları birleştiriyorsanız, her iki tabloyu birleştirmek için istediğinize aynı biçimi kullanır doğrulayın.  
  
Genel olarak, sayısı tabanlı dönüşüm dönüştürme ilk olarak oluşturulduğu veri kümesi ile aynı şemaya sahip veri kümeleri için yalnızca uygulanabilir.  
  
 <!-- For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Özel durum iletileri|  
|------------------------|  
|Belirtilen sayım dönüştürme geçersiz.|  
|Sayım dönüştürme Giriş noktasındaki '{0}' geçersiz.|  
|Sayım dönüştürme Giriş noktasındaki '{0}'sayım dönüştürme Giriş noktasındaki birleştirilemez'{1}'. Eşleşme sayım için kullanılan meta verileri doğrulamak için denetleyin.|  
  

## <a name="error-0087"></a>Hata 0087  
 Sayıları modüllerle öğrenme için bir geçersiz sayısı tablo türü belirtildiğinde, özel durum oluşur.  
  
 Azure Machine learning'de bu hata, var olan bir sayı tablosu aktarmayı denerseniz, ancak tablo geçerli veri veya yeni sayısı tablo ile uyumsuz olduğunda oluşur.  
  
**Çözüm:** Dönüşümü oluşturan kuralları ve sayıları kaydetmek için farklı biçimleri vardır. Sayısı tabloları birleştiriyorsanız, her ikisi de aynı biçimi kullanır doğrulayın.  
  
 Genellikle, sayısı tabanlı dönüşüm dönüştürme ilk olarak oluşturulduğu veri kümesi ile aynı şemaya sahip veri kümeleri için yalnızca uygulanabilir.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). -->
  

## <a name="error-0088"></a>Hata 0088  
 Özel durum türü sayıları modüllerle öğrenme için belirtilen geçersiz bir sayım gerçekleşir.  
  
 Çalıştığınızda için özellik kazandırma sayesinde sayısı tabanlı değerinden farklı bir sayım yöntemi kullanmak için desteklenen Azure Machine learning'de bu hata oluşur.  
  
**Çözüm:** Genel olarak, bu hata görülmemelidir için açılan listeden, sayım yöntemleri seçilir.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Özel durum iletileri|  
|------------------------|  
|Geçersiz tür sayım belirtilir.|  
|Belirtilen sayım türü '{0}' geçerli bir sayım türü değil.|  
  

## <a name="error-0089"></a>Hata 0089  
 Özel durum sınıfları belirtilen sayıda sayım için kullanılan bir veri kümesi sınıfları gerçek sayısından daha az olduğunda gerçekleşir.  
  
 Azure Machine learning'de bu hata, sayısı tablo oluşturduğunuz ve sınıfları modülü parametrelerinde daha farklı sayıda etiket sütunu içerdiğinde oluşur.  
  
**Çözüm:** Veri kümeniz denetleyin ve etiket sütunu vardır tam olarak kaç farklı değerleri (olası sınıflar) öğrenin. Sayısı tablo oluşturduğunuzda, bu sınıfların sayısı en az belirtmeniz gerekir.  
  
 Tablo sayısı otomatik olarak kullanılabilen sınıfları sayısı belirlenemiyor.  
  
 Sayısı tablo oluşturduğunuzda, 0 belirtemezsiniz veya herhangi bir sayı, etiket sütununda sınıfları gerçek sayısından küçük.  
  
|Özel durum iletileri|  
|------------------------|  
|Sınıfların sayısı doğru değil. Parametre Bölmesi'nde belirttiğiniz sınıfların sayısı değerinden büyük veya etiket sütununda sınıfları sayısına eşit olduğundan emin olun.|  
|Belirtilen sınıfları sayısı '{0}', bir etiket değerinden büyük değil'{1}' saymak için kullanılan veri kümesindeki. Parametre Bölmesi'nde belirttiğiniz sınıfların sayısı değerinden büyük veya etiket sütununda sınıfları sayısına eşit olduğundan emin olun.|  
  

## <a name="error-0090"></a>Hata 0090  
 Hive tablosu oluşturma başarısız olduğunda özel durum oluşur.  
  
 Kullanmakta olduğunuz Azure Machine learning'de bu hata oluşur [verileri dışarı aktarma](export-data.md) veya bir HDInsight kümesine ve belirtilen bir Hive tablosu için verileri kaydetmek için başka bir seçenek oluşturulamaz.  
  
**Çözüm:** Kümeyle ilişkili Azure depolama hesabı adını denetleyin ve modül özelliklerinde aynı hesabı kullandığınızdan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Hive tablosu oluşturulamadı. Bir HDInsight kümesi için kümeyle ilişkili Azure depolama hesabı adı ne modülü parametresi aracılığıyla geçirilen aynı olduğundan emin olun.|  
|Hive tablosu "{0}" oluşturulamadı. Bir HDInsight kümesi için kümeyle ilişkili Azure depolama hesabı adı ne modülü parametresi aracılığıyla geçirilen aynı olduğundan emin olun.|  
|Hive tablosu "{0}" oluşturulamadı. Bir HDInsight kümesi için kümeyle ilişkili Azure depolama hesabı adı olduğundan emin olun "{1}".|  
 

## <a name="error-0100"></a>0100 hata  
 Desteklenmeyen bir dil için özel bir modülü belirtildiğinde özel durum oluşur.  
  
 Bu hata Azure Machine learning'de özel modül ve ad özelliği oluştururken oluşur **dil** Özel Modül xml tanım dosyasını öğesinde geçersiz bir değere sahip. Şu anda bu özellik için geçerli olan tek değer olduğu `R`. Örneğin:  
  
 `<Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Çözüm:** Doğrulayın, name özelliği **dil** Özel Modül xml tanım dosyasını öğesinde ayarlanır `R`. Dosyayı kaydedin, özel modül zip paketini güncelleştirin ve Özel Modül'ı yeniden eklemeyi deneyin.  
  
|Özel durum iletileri|  
|------------------------|  
|Belirtilen desteklenmeyen özel modül dili|  
  

## <a name="error-0101"></a>Hata 0101  
 Tüm bağlantı noktası ve parametre kimlikleri benzersiz olmalıdır.  
  
 Bir veya daha fazla bağlantı noktaları veya parametreleri aynı ID değeri XML tanım dosyasını bir özel modüldeki atanmış olan Azure Machine learning'de bu hata oluşur.  
  
**Çözüm:** Kimliği değerleri tüm bağlantı noktaları ve parametreleri benzersiz olup olmadığını denetleyin. Xml dosyasını kaydedin, özel modül zip paketini güncelleştirin ve Özel Modül'ı yeniden eklemeyi deneyin.  
  
|Özel durum iletileri|  
|------------------------|  
|Tüm bağlantı noktası ve bir modül için parametre kimlikleri benzersiz olmalıdır|  
|Modül '{0}' yinelenen bağlantı noktası/bağımsız kimlikleri. Tüm bağlantı noktası/bağımsız değişkeni kimlikleri bir modül için benzersiz olmalıdır.|  
  

## <a name="error-0102"></a>Hata 0102  
 Bir ZIP dosyası ayıklandığında oluşturulur.  
  
 Azure Machine learning'de bu hata, .zip uzantılı sıkıştırılmış bir paket alıyorsanız, ancak paket değil oluşur. desteklenen zip biçiminde bir zip dosyası veya dosya kullanmaz.  
  
**Çözüm:** Seçili dosya geçerli bir .zip dosyası ve desteklenen bir sıkıştırma algoritmaları birini kullanarak sıkıştırılmış olan olduğundan emin olun.  
  
 Sıkıştırılmış biçimde veri kümelerini içeri aktarırken bu hatayı alırsanız, dahil edilen tüm dosya desteklenen dosya biçimleri birini kullanın ve Unicode biçiminde olduğunu doğrulayın.  <!--For more information, see [Unpack Zipped Datasets](unpack-zipped-datasets.md).  -->
  
 Yeni bir sıkıştırılmış sıkıştırılmış klasöre istenen dosyaları yeniden eklemeyi deneyin ve Özel Modül'ı yeniden eklemeyi deneyin.  
  
|Özel durum iletileri|  
|------------------------|  
|Belirli bir ZIP dosyası doğru biçimde değil.|  


## <a name="error-0103"></a>Hata 0103  
 Bir ZIP dosyası .xml dosyalarını içermediğinde oluşturulur  
  
 Özel Modül zip paketini hiçbir modül tanım (.xml) dosyalarını içermediğinde bu hata Azure Machine learning'de gerçekleşir. Bu dosyaları zip paketini (örneğin, bir alt klasörü içinde değil) kökünde bulunacak şekilde gerekir  
  
**Çözüm:** Geçici bir klasöre diskinizdeki ayıklayarak bir veya daha fazla xml modül tanımı dosyaları zip paketini kök klasöründe olduğunu doğrulayın. Xml dosyaları doğrudan zip paketini ayıkladığınız klasörde olmalıdır. Bu posta için seçtiğiniz klasör olarak aynı ada sahip bir alt klasör ZIP paketteki oluşturur gibi ZIP xml dosyalarını içeren bir klasörü seçmeyin zip paketini oluşturduğunuzda emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Hiçbir modül tanım dosyalarını (.xml dosyaları) belirli bir ZIP dosyası içermiyor|  


## <a name="error-0104"></a>Hata 0104  
 Modül tanım dosyası bulunamıyor bir betik başvuruda bulunduğunda oluşturulur  
  
 Bu hata Azure Machine learning'de Özel Modül xml tanım dosyasını bir komut dosyası başvuruları çağırınca **dil** ZIP paketteki yok öğesi. Komut dosyası yolunu tanımlanan **Kaynakdosya** özelliği **dil** öğesi. Kaynak dosyası (modül xml tanım dosyalarını aynı konuma) zip paketini köküne göreli yoludur. Betiğin bir alt klasörde, betik dosyasının göreli yolu belirtilmelidir. Örneği için tüm betikler içinde depolanmış, bir **myScripts** zip paketini klasördeki **dil** öğesi bu yoluna eklenecek haritamın **Kaynakdosya** özelliği olarak Aşağıda. Örneğin:  
  
 `<Language name="R" sourceFile="myScripts/CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Çözüm:** Emin olun değerini **Kaynakdosya** özelliğinde **dil** Özel Modül xml tanımını öğesinin doğru olduğundan ve ZIP paketteki doğru göreli yolda kaynak dosyası mevcut.  
  
|Özel durum iletileri|  
|------------------------|  
|Başvurulan R betik dosyası yok.|  
|Başvurulan R betiği '{0}' bulunamıyor. Dosyaya göreli yol tanımları konumdan doğru olduğundan emin olun.|  


## <a name="error-0105"></a>Hata 0105  
 Modül tanım dosyası desteklenmeyen parametre türü içeriyor. Bu hata görüntülenir  
  
 Azure Machine learning'de bu hata, bir özel modül xml tanımı oluşturduğunuzda ve desteklenen bir türde bir parametre veya tanımındaki bağımsız değişken türü eşleşmiyor oluşturulur.  
  
**Çözüm:** Emin olun herhangi öğesinin type özelliği **Arg** Özel Modül xml tanım dosyasını öğesinde desteklenen bir türdür.  
  
|Özel durum iletileri|  
|------------------------|  
|Desteklenmeyen parametre türü.|  
|Desteklenmeyen parametre türü '{0}' belirtilen.|  


## <a name="error-0106"></a>Hata 0106  
 Modül tanım dosyası desteklenmeyen bir giriş türü tanımlayan zaman oluşturulur  
  
 Bu hata Azure Machine learning'de özel modüldeki XML tanımını bir giriş noktası türü, desteklenen bir tür eşleşmediğinde oluşturulur.  
  
**Çözüm:** Özel Modül XML tanımı dosyasındaki bir giriş öğesinin type özelliği'nın desteklenen bir tür olduğundan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Desteklenmeyen giriş türü.|  
|Giriş türü desteklenmeyen '{0}' belirtilen.|  


## <a name="error-0107"></a>Hata 0107  
 Modül tanım dosyası desteklenmeyen bir çıkış türü tanımlayan zaman oluşturulur  
  
 Bu hata Azure Machine learning'de Özel Modül xml tanımı'ndaki bir çıkış bağlantı noktasına türünü desteklenen tür eşleşmediğinde oluşturulur.  
  
**Çözüm:** Bir çıkış öğesi özel modül xml tanımı dosyasındaki öğesinin type özelliği'nın desteklenen bir tür olduğundan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Desteklenmeyen çıkış türü.|  
|Desteklenmeyen çıkış türü '{0}' belirtilen.|  


## <a name="error-0108"></a>Hata 0108  
 Modül tanım dosyası desteklenenden daha fazla giriş veya çıkış bağlantı noktaları tanımladığında oluşturulur  
  
 Çok fazla giriş veya çıkış bağlantı noktaları Özel Modül xml tanımında tanımlandığında, bu hata Azure Machine learning'de oluşturulur.  
  
**Çözüm:** Özel Modül xml tanım içinde tanımlanan giriş ve çıkış bağlantı noktaları sayısının desteklenen bağlantı noktaları en fazla sayısını aşmadığından emin olur.  
  
|Özel durum iletileri|  
|------------------------|  
|Desteklenen giriş veya çıkış bağlantı noktalarının sayısı aşıldı.|  
|Aşıldı sayısı desteklenen '{0}' bağlantı noktaları. İzin verilen en fazla '{0}'bağlantı noktaları olan'{1}'.| 

## <a name="error-0109"></a>Hata 0109  
 Modül tanım dosyası yanlış Sütun Seçici tanımlar zaman oluşturulur  
  
 Sütun Seçici bağımsız değişkeni için söz dizimi bir özel modül xml tanımı bir hata içeriyorsa bu hata Azure Machine learning'de oluşturulur.  
  
**Çözüm:** Sütun Seçici bağımsız değişkeni için söz dizimi bir özel modül xml tanımı bir hata içeriyorsa bu hata oluşturulur.  
  
|Özel durum iletileri|  
|------------------------|  
|Sütun Seçici için desteklenmeyen söz dizimi.|  
  

## <a name="error-0110"></a>Hata 0110  
 Modül tanım dosyası mevcut olmayan giriş bağlantı noktası kimliği başvuran bir sütun Seçici tanımladığında oluşturulur  
  
 Bu hata Azure Machine learning'de üretilen olduğunda *Portıd* özellik ColumnPicker türünde bir değişken, Özellikler öğesi içinde giriş bağlantı kimliği değeri eşleşmiyor.  
  
**Çözüm:** Özel Modül xml tanım içinde tanımlanan bir giriş bağlantı noktası kimliği değerini Portıd özelliği eşleştiğinden emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Sütun Seçici bir mevcut olmayan giriş bağlantı noktasını kimliğe referans verir.|  
|Sütun Seçici başvuran mevcut olmayan giriş bağlantı noktası kimliği '{0}'.|  
  

## <a name="error-0111"></a>Hata 0111  
 Modül tanım dosyası geçersiz bir özellik tanımlar zaman oluşturulur  
  
 Azure Machine learning'de bu hata, bir öğedeki Özel Modül XML tanımı geçersiz bir özellik atandığında oluşturulur.  
  
**Çözüm:** Özelliği özel modül öğesi tarafından desteklenen emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Özellik tanımı geçersiz.|  
|Özellik tanımı '{0}' geçersiz.|  
  

## <a name="error-0112"></a>Hata 0112  
 Modül tanım dosyası ayrıştırılamıyor. zaman oluşturulur  
  
 Bu hata Azure Machine learning'de Özel Modül XML tanımı geçerli bir XML dosyası Ayrıştırılmakta dan engelleyen xml biçiminde bir hata olduğunda oluşturulur.  
  
**Çözüm:** Her öğe açık ve kapalı doğru emin olun. Hiçbir hata XML biçimlendirmesi emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Modül tanım dosyası ayrıştırılamadı.|  
|Modül tanım dosyası ayrıştırılamıyor '{0}'.|  
  

## <a name="error-0113"></a>Hata 0113  
 Modül tanım dosyası hataları içerdiğinde oluşturulur.  
  
 Bu hata Azure Machine learning'de Özel Modül XML tanım dosyasını ayrıştırılabilir ancak özel modüller tarafından desteklenmeyen öğe tanımı gibi hatalarıyla içeren oluşturulur.  
  
**Çözüm:** Özel modül tanım dosyası öğeleri ve özel modüller tarafından desteklenen özellikler tanımlar emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Modül tanım dosyası hataları içeriyor.|  
|Modül tanım dosyası '{0}' hatalar içeriyor.|  
|Modül tanım dosyası '{0}' hatalar içeriyor. {1}|  
  

## <a name="error-0114"></a>Hata 0114  
 Özel Modül başarısız oluşturulurken oluşturulur.  
  
 Bu hata Azure Machine learning'de Özel Modül derleme başarısız olduğunda oluşturulur. Bu bir ortaya çıkar veya modülü ile ilgili daha fazla özel hatalar, özel modül eklenirken bir hata ile karşılaşıldı. Bu hata iletisi içinde ek hataları raporlanır.  
  
**Çözüm:** Bu özel durum iletisi içinde hataları bildirilen çözümleyin.  
  
|Özel durum iletileri|  
|------------------------|  
|Özel modülü başarısız oldu.|  
|Özel Modül ile hatalarla başarısız oluşturur: {0}|  
  

## <a name="error-0115"></a>Hata 0115  
 Bir özel modül varsayılan komut dosyası desteklenmeyen bir uzantısı sahip olduğunda oluşturulur.  
  
 Azure Machine learning'de bu hata, bilinmeyen dosya adı uzantısı kullanan özel bir modül için bir betik sağlayın oluşur.  
  
**Çözüm:** Özel modülüne dahil edilen herhangi bir komut dosyalarını dosya biçimi ve dosya adı uzantısını doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Desteklenmeyen uzantı varsayılan komut dosyası.|  
|Desteklenmeyen dosya uzantısı {0} varsayılan komut dosyası.|  
  

## <a name="error-0121"></a>Hata 0121  
 SQL tablo yazılamaz olduğundan başarısız yazdığında oluşturulur  
  
 Bu hata Azure Machine learning'de kullanırken üretilen [verileri dışarı aktarma](export-data.md) SQL veritabanındaki bir tablo ve tabloya sonuçlarını kaydetmek için modülü için yazılamıyor. Genellikle, bu hatayı görürsünüz [verileri dışarı aktarma](export-data.md) modülü başarıyla SQL Server örneği ile bağlantı kurar, ancak daha sonra Azure ML veri kümesi içeriği tabloya yazamıyor.  
  
**Çözüm:**
 - Özellikler bölmesinde açmak [verileri dışarı aktarma](export-data.md) modülü ve veritabanı ve tablo adlarının doğru girdiğinizden emin olun. 
 - Veriyorsunuz ve verilerin hedef tablo ile uyumlu olduğundan emin olun veri kümesi şemasını gözden geçirin.
 - SQL oturum kullanıcı adıyla ilişkili ve parola tabloya yazma iznine sahip olduğunu doğrulayın. 
 - Özel durum, SQL Server'dan ek hata bilgisi içeriyorsa, düzeltme yapmak için bu bilgileri kullanın.  
  
|Özel durum iletileri|  
|------------------------|  
|Sunucuya, yazamıyor bağlı tabloya.|  
|Sql tablosuna yazılamadı: {0}|  


## <a name="error-0122"></a>Hata 0122  
 Birden çok ağırlık sütun belirtilir ve yalnızca bir izin, özel durum ortaya çıkar.  
  
 Sütun sayısı çok fazla ağırlık sütunlar olarak seçilmiş olan Azure Machine learning'de bu hata oluşur.  
  
**Çözüm:** Giriş veri kümesi ve meta verileri gözden geçirin. Yalnızca bir sütun içeren ağırlıkları emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Birden çok ağırlık sütun belirtilmiş.|  


## <a name="error-0123"></a>Hata 0123  
 Vektör sütunu için etiket sütunu belirtilirse özel durum oluşur.  
  
 Etiket sütunu olarak bir vektör kullanın, Azure Machine learning'de bu hata meydana gelir.  
  
**Çözüm:** Gerekirse, sütunun veri biçimini değiştirmek veya farklı bir sütun seçin.  
  
|Özel durum iletileri|  
|------------------------|  
|Vektör sütununun etiket sütunu olarak belirtilir.|  


## <a name="error-0124"></a>Hata 0124  
 Sayısal olmayan sütunları ağırlık sütunu için belirtilen özel durum ortaya çıkar.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Sayısal olmayan sütun ağırlık sütun olarak belirtilir.|  
  


## <a name="error-0125"></a>Hata 0125  
 Birden fazla veri kümesi şema eşleşmiyor zaman oluşturulur.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Veri kümesi şema eşleşmiyor.|  


## <a name="error-0126"></a>Hata 0126  
 Azure ML desteklenmeyen bir SQL etki alanı kullanıcının belirttiği, özel durum meydana gelir.  
  
 Azure Machine Learning'de desteklenmeyen bir SQL etki alanı kullanıcının belirttiği olduğunda bu hata oluşturulur. İzin verilenler listesinde değil bir etki alanında bir veritabanı sunucusuna bağlanmaya çalışıyorsanız bu hatayı alırsınız. Şu anda izin verilen SQL etki alanları şunlardır: ". database.windows.net",". cloudapp.net", veya ". database.secure.windows.net". Diğer bir deyişle, sunucu, bir Azure SQL server veya bir Azure üzerinde bir sanal makinede olması gerekir.  
  
**Çözüm:** Modülün yeniden ziyaret edin. SQL veritabanı sunucusu kabul edilen etki alanlarından birine ait olduğundan emin olun:  
  
-   .database.windows.net  
  
-   .cloudapp.net  
  
-   . database.secure.windows.net  
  
|Özel durum iletileri|  
|------------------------|  
|Desteklenmeyen SQL etki alanı.|  
|SQL etki alanı {0} Azure ML üzerinde şu anda desteklenmiyor|  
  

## <a name="error-0127"></a>Hata 0127  
 Görüntü piksel boyutu izin verilen limiti aşıyor  
  
 Sınıflandırma için bir görüntü kümesinden görüntüleri okuma ve görüntüleri model işleyebileceğinden daha büyüktür, bu hata oluşur.  
  
 <!--**Resolution:**
 For more information about the image size and other requirements, see these topics:  
  
-   [Import Images](import-images.md)  
  
-   [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)  -->
  
|Özel durum iletileri|  
|------------------------|  
|Görüntü piksel boyutu, izin verilen sınırı aşıyor.|  
|Görüntü dosyası piksel boyutunda '{0}' izin verilen sınırı aşıyor: '{1}'|  


## <a name="error-0128"></a>Hata 0128  
 Koşullu olasılıklar kategorik sütunların sayısı, sınırı aşıyor.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Koşullu olasılıklar kategorik sütunların sayısı, sınırı aşıyor.|  
|Koşullu olasılıklar kategorik sütunların sayısı, sınırı aşıyor. Sütunların{0}'ve'{1}' sorunlu çiftleri.|  


## <a name="error-0129"></a>Hata 0129  
 Veri kümesinde sütun sayısı izin verilen sınırı aşıyor.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Veri kümesinde sütun sayısı izin verilen sınırı aşıyor.|  
|Veri kümesinde sütun sayısı '{0}'izin verilen aşıyor.'|  
|Veri kümesinde sütun sayısı '{0}'izin verilen sınırı aşıyor'{1}'.'|  
|Veri kümesinde sütun sayısı '{0}'izin verilen aşıyor'{1}'sınır'{2}'.'|  
## <a name="error-0130"></a>Hata 0130  
 Tüm satırlarda eğitim veri kümesi eksik değerler içerdiğinde, özel durum oluşur.  
  
 Bazı sütununda bir eğitim veri kümesi boş olduğunda gerçekleşir.  
  
**Çözüm:** Kullanım [eksik verileri temizleme](clean-missing-data.md) modülü eksik olan tüm değerlere sahip bir sütunu kaldırmak için.  
  
|Özel durum iletileri|  
|------------------------|  
|Tüm satırlarda eğitim veri kümesi eksik değerleri içerir.  Eksik değerleri kaldırmak için eksik verileri temizleme modülü kullanmayı düşünün.|  
 

## <a name="error-0131"></a>Hata 0131  
 Bir zip dosyasında bir veya daha fazla veri başarısız olursa farklı geçin ve doğru kaydedilmiş özel durum oluşur.  
  
 Bir zip dosyasında bir veya daha fazla veri kümeleri geçin ve doğru şekilde okuma başarısız olduğunda bu hata oluşturulur. Akışının paketi açılırken zip dosyası veya bir dosya bozuk veya Cihazınızı kutusundan çıkarma ve bir dosyayı genişletme çalışılırken bir sistem hatası olduğu için başarısız olursa bu hatayı alırsınız.  
  
**Çözüm:** Devam etmek nasıl belirlemek için verilen hata iletisinde ayrıntıları kullanın.  
  
|Özel durum iletileri|  
|------------------------|  
|Başarısız sıkıştırılmış veri kümelerini karşıya yükleme|  
|Veri kümesi daraltılmış {0} şu iletiyle başarısız oldu: {1}|  
|Veri kümesi daraltılmış {0} başarısız oldu. bir {1} özel durum iletisi: {2}|  
  

## <a name="error-0132"></a>Hata 0132  
 Açmak için dosya adı belirtildi; birden çok dosyaları zip dosyasında bulunamadı.  
  
 Bu hata, açmak için dosya adı belirtildiğinde oluşturulur; birden çok dosyaları zip dosyasında bulunamadı. Birden fazla sıkıştırılmış dosya .zip dosyasını içerir, ancak bir dosya içinde ayıklama için belirtmedi bu hatayı alırsınız **paket açma veri kümesine** metin kutusundaki **özelliği** modülünün bölmesi. Şu anda yalnızca bir dosya, modülün her çalıştırıldığında ayıklanabilir.  
  
**Çözüm:** Hata iletisi .zip dosyasında bulunan dosyaların bir listesini sağlar. İstenen dosyanın adını kopyalayın ve yapıştırın **paket açma veri kümesine** metin kutusu.  
  
|Özel durum iletileri|  
|------------------------|  
|Zip dosyası birden çok dosya içeriyor; genişletmek için dosyasını belirtmeniz gerekir.|  
|Dosya, birden fazla dosya içeriyor. Genişletmek için dosyayı belirtin. Aşağıdaki dosyalar bulundu: {0}|  
  

## <a name="error-0133"></a>Hata 0133  
 Zip dosyası içinde belirtilen dosya bulunamadı  
  
 Dosya adını girdiğinizde bu hatayı üretilen **paket açma veri kümesine** alanını **özelliği** bölmesinde .zip dosyasında bulunan herhangi bir dosyanın adı ile eşleşmiyor. Bu hatanın en yaygın nedenlerini hata yazarak veya yanlış arşiv dosyası için dosya genişletmek arama var.  
  
**Çözüm:** Modülün yeniden ziyaret edin. Hedeflenen sıkıştırmasını açmak için dosya adı bulunan dosyaları listesinde görünüyorsa, dosya adı kopyalayın ve yapıştırın **paket açma veri kümesine** özellik kutusu. Listeden istediğiniz dosya adını görmüyorsanız doğru .zip dosyasını ve istediğiniz dosya adını doğru olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Belirtilen dosya değildi int zip dosyası bulunamadı.|  
|Belirtilen dosya bulunamadı. Aşağıdaki dosyalar bulundu: {0}|  
  

## <a name="error-0134"></a>Hata 0134
Etiket sütunu eksik veya etiketli satır sayısı yetersiz olduğunda özel durum oluşur.  
  
Modülü, bir etiket sütun gerektirir. ancak bir sütun seçimi içermiyordu veya çok fazla değer etiketi sütun eksik bu hata oluşur.

Bu hata, önceki bir işlemin dataset değiştiğinde bir aşağı akış işlem için yeterli satır kullanılabilir olduğunu da meydana gelebilir. Örneğin, bir ifadede kullandığınızı varsayalım **bölüm ve örnek** değerlerine göre bir veri kümesini ayırmak için modülü. İfadeniz için herhangi bir eşleşme bulunursa bölümünden elde edilen veri kümelerinden birini boş olacaktır.

Çözüm: 

 Sütun seçimini bir etiket sütun içerir, ancak bunu tanınmıyor kullanın [meta verileri Düzenle](edit-metadata.md) etiket sütun olarak işaretlemek için modülü.
  
  <!--Use the [Summarize Data](summarize-data.md) module to generate a report that shows how many values are missing in each column. -->Daha sonra kullanabileceğiniz [eksik verileri temizleme](clean-missing-data.md) modülü etiket sütunu eksik değerler içeren satırları kaldırmak için. 

 Geçerli veri ve işlem gereksinimlerini karşılamak için yeterli satır içerdiğini emin olmak için giriş veri kümeleri denetleyin. Bazı veri sayı satırlarını en düşük gereksinim duydukları, ancak yalnızca birkaç satır ya da yalnızca bir üst bilgi verileri içeren birçok algoritması bir hata iletisi oluşturur.
  
|Özel durum iletileri|
|------------------------|
|Etiket sütunu eksik veya etiketli satır sayısı yetersiz olduğunda özel durum oluşur.|  
|Özel durum oluştuğunda etiket sütunu eksik veya küçüktür {0} satırları etiketli|  
  

## <a name="error-0135"></a>Hata 0135  
 Küme yalnızca kütle merkezi tabanlı desteklenir.  
  
**Çözüm:** Kümeyi başlatmaya centroids kullanmayan özel bir kümeleme algoritmadan yola çıkılarak bir kümeleme modeli değerlendirilecek çalıştınız, bu hatayla karşılaşabilirsiniz.  
  
  <!--You can use [Evaluate Model](evaluate-model.md) to evaluate clustering models that are based on the  [K-Means Clustering](k-means-clustering.md) module. For custom algorithms, use the [Execute R Script](execute-r-script.md) module to create a custom evaluation script.  -->
  
|Özel durum iletileri|  
|------------------------|  
|Küme yalnızca kütle merkezi tabanlı desteklenir.|  
  

## <a name="error-0136"></a>Hata 0136  
 Dosya adı döndürülmedi; Dosya sonucunda işlenecek oluşturulamıyor.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Dosya adı döndürülmedi; Dosya sonucunda işlenecek oluşturulamıyor.|  
  

## <a name="error-0137"></a>Hata 0137  
 Azure depolama SDK'sı, okuma veya yazma sırasında tablo özelliklerini ve dataset sütunları arasında dönüştürme bir hatayla karşılaştı.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Azure tablo depolama özelliği ve veri kümesi sütunu arasında dönüştürme hatası.|  
|Azure tablo depolama özelliği ve veri kümesi sütunu arasında dönüştürme hatası. Ek bilgi: {0}|  

## <a name="error-0138"></a>Hata 0138  
 Bellek, modülün tam çalışan kurulamıyor aşıldı. Aşağı örnekleme veri kümesi, sorun gidermeyi yardımcı olabilir.  
  
 Çalıştıran modülü, Azure kapsayıcısında mevcut olan sayıdan daha fazla bellek gerektirir. Bu hata oluşur. Bu, büyük bir veri kümesi ile çalışıyorsanız ve geçerli işlem belleğe sığamıyorsa oluşabilir.  
  
**Çözüm:** Büyük bir veri kümesini okuma çalıştığınız ve işlem tamamlanamıyor, aşağı örnekleme veri kümesini yardımcı olabilir.  
  
  <!--If you use the visualizations on datasets to check the cardinality of columns, only some rows are sampled. To get a full report, use [Summarize Data](summarize-data.md). You can also use the [Apply SQL Transformation](apply-sql-transformation.md) to check for the number of unique values in each column.  
  
 Sometimes transient loads can lead to this error. Machine support also changes over time. 
  
 Try using [Principal Component Analysis](principal-component-analysis.md) or one of the provided feature selection methods to reduce your dataset to a smaller set of more feature-rich columns: [Feature Selection](feature-selection-modules.md)  -->
  
|Özel durum iletileri|  
|------------------------|  
|Bellek, modülün tam çalışan kurulamıyor aşıldı.|  
  

## <a name="error-0139"></a>Hata 0139  
 Bir sütunu başka bir türe dönüştürmek mümkün değilse, özel durum oluşur.  
  
 Bu hata Azure Machine learning'de bir sütun için farklı bir veri türüne dönüştürmeye çalışır ancak geçerli işlem veya modül türü desteklenmiyor oluşur.  
  
 Hata, bir modül örtük olarak geçerli modül gereksinimlerini karşılamak için verileri dönüştürmeye çalışıyor, ancak dönüştürme mümkün değil de görüntülenebilir.  
  
**Çözüm:**

1. Girişinizi gözden geçirin ve kullanmak istediğiniz sütunun tam veri türü ve hata üreten sütunun veri türünü belirler. Bazen veri türü doğruysa ama bir Yukarı Akış işlem veri türü veya sütun kullanımı değiştirdi Bul düşünebilirsiniz. Kullanım [meta verileri Düzenle](edit-metadata.md) sütun meta verileri özgün durumuna sıfırlamak için modülü. 
2. Belirtilen işlem gereksinimlerini doğrulamak için modül yardım sayfasına bakın. Hangi veri türlerinin bulunduğu geçerli modülü tarafından desteklenir ve ne değer aralığının desteklenen belirler. 
 <!--3. If values need to be truncated, rounded, or outliers removed, use the [Apply Math Operation](apply-math-operation.md) or [Clip Values](clip-values.md) modules to make corrections.-->
4. Dönüştürün veya farklı bir veri türü sütununa dönüştürme mümkün olup olmadığını göz önünde bulundurun. Aşağıdaki modüller tüm verileri değiştirmek için önemli ölçüde esneklik ve güç sağlayın: 
 <!--
   + [Apply SQL Transformation](apply-sql-transformation.md)
   + [Execute R Script](execute-r-script.md)
-->   
   + [Python betiği yürütme](execute-python-script.md).  

> [!NOTE]
> Yine de çalışmıyor mu? Sorunu daha iyi sorun giderme kılavuzu geliştirmenize yardımcı olmak için ek geri bildirim sağlamayı göz önüne alın. Yalnızca bu sayfadaki geri bildirim gönderin ve oluşturulan hata ve başarısız olan veri türünü dönüştürme modül adını sağlayın.
  
|Özel durum iletileri|  
|------------------------|  
|Dönüştürme izin verilmiyor.|  
|Değil dönüştüremedi: {0}.|  
|Değil dönüştüremedi: {0}, satırda {1}.|  
|Sütun türü dönüştürülemiyor {0} sütun türü için {1} satırda {2}.|  
|Sütun dönüştürülemedi "{2}" türündeki {0} sütun türü için {1} satırda {3}.|  
|Sütun dönüştürülemedi "{2}" türündeki {0} sütununa "{3}" türündeki {1} satırda {4}.| 

## <a name="error-0140"></a>Hata 0140  
 Sütun kümesi bağımsız değişkeni, etiket sütun dışındaki diğer sütunları içermiyor geçirilen özel durum oluşur.  
  
 Bir veri kümesi özellikleri dahil olmak üzere birden çok sütun gerektiren bir modülünü için bağlı değilse bu hata oluşur, ancak yalnızca etiket sütunu sağladığınız.  
  
**Çözüm:** Veri kümesini içerecek şekilde en az bir özellik sütununu seçin.  
  
|Özel durum iletileri|  
|------------------------|  
|Belirtilen sütun kümesi etiketi sütun dışındaki diğer sütunları içermiyor.|  
  

## <a name="error-0141"></a>Hata 0141  
 Seçili sayısal sütunlara ve benzersiz değerleri kategorik ve sütun sayısı, dize çok küçükse, özel durum oluşur.  
  
 Azure Machine learning'de bu hata, işlemi gerçekleştirmek için seçilen sütunda benzersiz değerler yeterli olmadığında gerçekleşir.  
  
**Çözüm:** Bazı işlemler özellik ve kategorik sütunlar hakkında istatistiksel işlemler gerçekleştirir ve yeterli değer yoksa, işlem başarısız veya geçersiz bir sonuç döndürür. Veri kümeniz özellik ve etiket sütun kaç değerleri görmek için denetleyin ve gerçekleştirmeye çalıştığınız işlem istatistiksel olarak geçerli olup olmadığını belirler.  
  
 Kaynak veri kümesi geçerliyse, bazı Yukarı Akış veri işleme veya meta veri işlemi varsa ve değiştirilen verileri bazı değerler kaldırıldı denetleyebilir.  
  
 Yukarı Akış işlemleri bölme, örnekleme veya örnekleme eklerseniz, çıkışları beklenen sayıda satır ve değerlerini içeren doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|Seçili sayısal sütunlara ve benzersiz değerleri kategorik ve sütun sayısı, dize çok küçük.|  
|Seçili sayısal sütunlar ve benzersiz değerleri kategorik ve dize sütunlar toplam sayısı (şu anda {0}) en az olmalıdır {1}|  
  

## <a name="error-0142"></a>Hata 0142  
 Sistem kimlik doğrulaması için sertifika yüklenemiyor. özel durum oluşur.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Sertifika yüklenemiyor.|  
|Sertifika {0} yüklenemiyor. Parmak izi olan {1}.|  
  

## <a name="error-0143"></a>Hata 0143  
 Github'dan olması gerekiyorsa kullanıcı tarafından sağlanan URL ayrıştırılamıyor.  
  
 Geçersiz bir URL belirtin ve modülü, geçerli bir GitHub URL'sini gerektirir. Bu hata Azure Machine learning'de oluşur.  
  
**Çözüm:** URL geçerli bir GitHub deposuna başvurduğunu doğrulayın. Diğer site türleri desteklenmez.  
  
|Özel durum iletileri|  
|------------------------|  
|URL, github.com değil.|  
|URL, github.com değil: {0}|  

## <a name="error-0144"></a>Hata 0144  
 Kullanıcı tarafından sağlanan GitHub URL'sini beklenen bölümü eksik.  
  
 Azure Machine learning'de bu hata, geçersiz bir URL biçimi kullanılarak bir GitHub dosya kaynağı belirtin oluşur.  
  
**Çözüm:** GitHub deposunun URL'si geçerli olup olmadığını ve \blob\ veya \tree ile sona erer denetlemek\\.  
  
|Özel durum iletileri|  
|------------------------|  
|GitHub URL'sini ayrıştırılamıyor.|  
|GitHub URL'sini ayrıştırılamıyor (bekleniyor ' \blob\\' veya ' \tree\\' sonra depo adı): {0}|  

## <a name="error-0145"></a>Hata 0145  
 Herhangi bir nedenden dolayı çoğaltma dizini oluşturulamıyor.  
  
 Belirtilen dizin oluşturmak modülü başarısız olduğunda, Azure Machine learning'de bu hata oluşur.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Çoğaltma dizini oluşturulamıyor.|  
  

## <a name="error-0146"></a>Hata 0146  
 Kullanıcı dosyalarını yerel bir dizine sıkıştırması açılmış olduğunda, birleşik yolun çok uzun olabilir.  
  
 Dosyaları ayıklıyorsanız, ancak bazı dosya adları sıkıştırması açılan olduğunda çok uzun olan Azure Machine learning'de bu hata oluşur.  
  
**Çözüm:** Dosya adlarının yolu birleşik şekilde düzenleyin ve dosya adı 248 karakterden uzun.  
  
|Özel durum iletileri|  
|------------------------|  
|Çoğaltma yolu 248 karakterden uzun olduğundan, komut dosyası adını veya yolunu kısaltın.|  

## <a name="error-0147"></a>Hata 0147  
 Öğe Github'dan herhangi bir nedenden dolayı yüklenemedi.  
  
 Bu hata Azure Machine learning'de okunamıyor veya belirtilen dosyalar Github'dan indirin oluşur.  
  
**Çözüm:** Sorun geçici olabilir; başka bir dosyalara erişmeyi deneyebilir. Veya gerekli izinlere sahip olduğunuzu ve kaynak geçerli olduğunu doğrulayın.  
  
|Özel durum iletileri|  
|------------------------|  
|GitHub erişim hatası.|  
|GitHub erişim hatası. {0}|  
  

## <a name="error-0148"></a>Hata 0148  
 Veri ayıklama veya dizin oluşturma sırasında yetkisiz erişim verir.  
  
 Bu hata Azure Machine learning'de bir dizin oluşturun veya depolama alanından verileri okumak çalışıyorsunuz, ancak gerekli izinlere sahip değilsiniz oluşur.  
  
**Çözüm:**
  
|Özel durum iletileri|  
|------------------------|  
|Veri çıkarma sırasında yetkisiz erişim özel durumu.|  
  

## <a name="error-0149"></a>Hata 0149  
 Kullanıcı dosyası içinde GitHub paket yok.  
  
 Belirtilen dosya bulunamıyor Azure Machine learning'de bu hata oluşur.  
  
Çözüm: 
  
|Özel durum iletileri|  
|------------------------|  
|GitHub dosya bulunamadı.|  
|GitHub dosya bulunamadı.: {0}|  
  

## <a name="error-0150"></a>Hata 0150  
 Kullanıcı paketinden gelen komut dosyaları büyük olasılıkla GitHub dosyaları ile bir çakışma nedeniyle sıkıştırması, bulunamadı.  
  
 Azure Machine learning'de bu hata, mevcut bir dosyayı aynı ada sahip olduğunda bir komut dosyası, genellikle ayıklanamıyor oluşur.  
  
Çözüm:
  
|Özel durum iletileri|  
|------------------------|  
|Paketin sıkıştırmasını açın; kurulamıyor GitHub dosyalarla olası ad çakışması.|  
  

## <a name="error-0151"></a>Hata 0151  
 Bulut depolama yazılırken bir hata oluştu. URL'yi kontrol edin.  
  
 Bulut depolama alanına veri yazmak modülü çalışır ancak URL kullanılabilir değil veya geçersiz Azure Machine learning'de bu hata oluşur.  
  
Çözüm: URL'yi kontrol edin ve yazılabilir olduğundan emin olun.  
  
|Özel durum iletileri|  
|------------------------|  
|Bulut depolama (muhtemelen bozuk bir url) dosyasına yazma hatası.|  
|Bulut depolama yazma hatası: {0}. URL'yi kontrol edin.|  
  
## <a name="error-0152"></a>Hata 0152  
 Azure cloud türü modülü bağlamında yanlış belirtildi.  
  
|Özel durum iletileri|  
|------------------------|  
|Hatalı Azure Cloud türü|  
|Hatalı Azure Cloud türü: {0}|  
  
## <a name="error-0153"></a>Hata 0153  
 Belirtilen depolama uç noktası geçersiz.  
  
|Özel durum iletileri|  
|------------------------|  
|Hatalı Azure Cloud türü|  
|Hatalı depolama uç noktası: {0}|  

## <a name="error-0154"></a>Hata 0154  
 Belirtilen sunucu adı çözümlenemedi  
  
|Özel durum iletileri|  
|------------------------|  
|Belirtilen sunucu adı çözümlenemedi|  
|Belirtilen sunucu {0}. documents.azure.com çözümlenemedi|

## <a name="error-0155"></a>Hata 0155  
 DocDb istemcisi, bir özel durum belirtti.  
  
|Özel durum iletileri|  
|------------------------|  
|DocDb istemcisi, bir özel durum belirtti.|  
|DocDb istemcisi: {0}|

## <a name="error-0156"></a>Hata 0156  
 HCatalog sunucu yanıtı hatalı.  
  
|Özel durum iletileri|  
|------------------------|  
|HCatalog sunucu yanıtı hatalı. Tüm hizmetlerin çalıştığından emin olun.|  
|HCatalog sunucu yanıtı hatalı. Tüm hizmetlerin çalıştığından emin olun. Hata ayrıntıları: {0}|

## <a name="error-0157"></a>Hata 0157  
 Azure Cosmos DB'den nedeniyle tutarsız veya farklı belge şemaları okunurken bir hata oluştu. Okuyucu, tüm belgeler aynı şemaya sahip olmasını gerektirir.  
  
|Özel durum iletileri|  
|------------------------|  
|Farklı şemalarla algılanan belgeler. Tüm belgeler aynı şemaya sahip olduğunuzdan emin olun|

## <a name="error-1000"></a>1000 hata  
İç kitaplık özel durum.  
  
Bu hata, aksi takdirde yakalamak üzere iç altyapı hataları işlenmemiş sağlanır. Bu nedenle, bu hatanın nedeni genellikle hata oluşturan modülü bağlı olarak farklı olabilir.  
  
Daha fazla yardım almak için girdi olarak kullanılan veriler de dahil olmak üzere, senaryonun açıklaması ile birlikte Azure Machine Learning Forumu için hata eşlik eden ayrıntılı ileti gönderi öneririz. Bu geri bildirim hataları öncelik sırasına sokmanıza ve daha fazla iş için en önemli sorunları belirlemek için bize yardımcı olur.  
  
|Özel durum iletileri|  
|------------------------|  
|Kitaplık özel durum.|  
|Kitaplık özel durum: {0}|  
|{0} Kitaplık özel durum: {1}|  
