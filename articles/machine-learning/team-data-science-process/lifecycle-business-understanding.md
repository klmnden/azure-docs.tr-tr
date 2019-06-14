---
title: Team Data Science Process içinde iş anlama
description: Hedefleri, görevleri ve iş anlama aşamasına veri bilimi projelerinizi Team Data Science Process için teslim edilebilirler.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 35d03a52125bd2646f86b96bcffe123d9fab7f64
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303562"
---
# <a name="the-business-understanding-stage-of-the-team-data-science-process-lifecycle"></a>Team Data Science Process yaşam döngüsü aşaması iş anlama

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirler ile Team Data Science işlem (TDSP) iş anlama aşama ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İşin gereksinimlerini anlama**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Model hedefler olarak görev yapacak olan ve, ilgili ölçümleri kullanılan anahtar değişkenleri projenin başarısını belirlemek belirtin.
* İş erişimi olduğundan veya almak gereken ilgili veri kaynaklarını tanımlar.

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada ele iki ana görevi vardır: 

   * **Hedefleri tanımlamanıza**: Anlama ve iş sorunlarını belirlemek için müşteri ve diğer proje katılımcıları çalışın. Veri bilimi teknikleri hedefleyebilen iş hedeflerinizi sorular düzenleyin.
   * **Veri kaynaklarını tanımlama**: Projenin hedefleri tanımlamanıza soruları yanıtlamanıza yardımcı olan ilgili verileri bulun.

### <a name="define-objectives"></a>Hedeflerini belirleyin
1. Bu adımın merkezi bir hedefi tahmin analizi ihtiyaçlarınızın önemli iş değişkenleri belirlemektir. Bu değişkenler diyoruz *model hedefleri*, ve onlarla ilişkili ölçümleri projenin başarısını belirlemek için kullanıyoruz. İki tür hedeflerle satış tahminlerini veya bir sipariş edilen sahte olasılığını örnekleridir.

2. Proje hedefleri isteyen ve ilgili, belirli ve anlaşılır "sharp" sorularını iyileştirme tanımlayın. Veri bilimi adları ve sayı gibi soruları yanıtlamak için kullandığı bir işlemdir. Genellikle veri bilimi veya beş türde soruları yanıtlamak için makine öğrenimini kullanır:
 
   * Ne kadar nasıl? (gerileme)
   * Kategori? (sınıflandırma)
   * Hangi grubun? (küme)
   * Bu tuhaf mi? (anomali algılama)
   * Hangi seçeneğin alınıp? (öneri)

   Hangi soruları sormaya ve nasıl yanıtlama, iş hedeflerinizi başarır belirler.

3. Proje ekibi, roller ve sorumluluklar üyelerinin belirterek tanımlarsınız. Daha fazla bilgi bulmak gibi üzerinde yineleme bir üst düzey kilometre planı geliştirin. 

4. Başarı ölçütleri tanımlayın. Örneğin, bir müşteri karmaşıklığı tahmini elde etmek isteyebilirsiniz. Bu üç aylık Proje sonunda bir doğruluk oranı "x" yüzde ihtiyacınız var. Bu verilerle azaltmak için promosyon müşteri karmaşıklığı sunabilir. Ölçümler olmalıdır **akıllı**: 

   * **S**b & elirli 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**IME bağlı 

### <a name="identify-data-sources"></a>Veri kaynaklarını tanımlama
Diyez sorularınızın yanıtlarını bilinen örneklerini içeren veri kaynakları belirleyin. Aşağıdaki veriler için bakın:

* Soruyu ilgili veriler. Ölçümleri hedefle ilgili özellikler ve hedef var mı?
* Doğru bir modeli hedef ölçü ve ilgi çekici özellik verileri.

Örneğin, var olan sistemler toplamak ve sorunu çözmek ve proje hedeflere ulaşmak için veri ek türleri oturum gerektiğini fark edebilirsiniz. Bu durumda, dış veri kaynakları için aramak veya yeni veriler toplamak için sistemlerinizi update isteyebilirsiniz.

## <a name="artifacts"></a>Yapıtlar
Bu aşamada teslim edilebilir öğeler şunlardır:

   * [Kurucu belge](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): Standart şablon TDSP projesi yapı tanımında sağlanır. Kurucu, oturma belge belgesidir. Şablon proje boyunca yeni bulmalar yaptığınız ve iş gereksinimleri değiştikçe güncelleştirin. Bulma işlemi ilerlemeyi olarak daha fazla ayrıntı ekleme, bu belge üzerinde yineleme yapmak için kullanılan anahtardır. Bir müşteriye ve diğer proje katılımcıları söz konusu değişiklikler yaparken ve açıkça bunları değişiklikleri nedenlerle iletişim.  
   * [Veri kaynakları](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/Data%20Defintion.md#raw-data-sources): **Ham veri kaynakları** bölümünü **veri tanımlarını** TDSP projesinde bulunan rapor **veri raporu** klasörü, veri kaynaklarını içerir. Bu bölümde, ham veriler için özgün ve hedef konumları belirtir. Daha sonraki aşamalarda, analitik ortamınıza verileri taşımak için komut dosyaları gibi ek ayrıntıları doldurun.  
   * [Veri sözlükleri](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Data_Dictionaries): Bu belge, istemci tarafından sağlanan veri açıklamaları sağlar. Bu açıklamalar varsa şema (veri türleri ve varsa doğrulama kuralları bilgi) ve varlık ilişkisi diyagramları hakkında bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İşin gereksinimlerini anlama](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

İşlemin belirli senaryolar için tüm adımları gösteren uçtan uca tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantıları ve küçük resim açıklamaları senaryolarıyla bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 
