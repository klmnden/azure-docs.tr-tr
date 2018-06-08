---
title: İş anlama takım veri bilimi işlemi yaşam döngüsü - Azure aşaması | Microsoft Docs
description: Hedefler, görevler ve veri bilimi projelerinizi iş anlama aşaması için teslim edilebilir
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 6d8eedbbf4a682443e73ecb9cf9496f3cdd1cd9d
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837210"
---
# <a name="business-understanding"></a>Kurumsal yaklaşım

Bu makalede hedefleri, görevleri ve iş anlama aşama takım veri bilimi işlem (TDSP) ile ilgili sonuçlara özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

   1. **İş anlama**
   2. **Veri alımı ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

Görsel bir TDSP yaşam döngüsü şöyledir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Model hedefleri olarak hizmet verecek olan ve, ilgili ölçümleri kullanılan anahtar değişkenleri proje başarısını belirlemek belirtin.
* İş erişimi veya almak gereken ilgili veri kaynaklarını tanımlayın.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele iki ana görevleri şunlardır: 

   * **Hedefleri tanımlayın**: anlamak ve iş sorunlarını belirlemek için müşteri ve diğer Paydaşlar ile çalışır. Veri bilimi teknikleri hedefleyebilirsiniz iş hedeflerini tanımlayın soruları oluşturmak.
   * **Veri kaynaklarını tanımlama**: Bul yardımcı olan ilgili verilerin proje amaçlarını tanımlama soruları yanıtlayın.

### <a name="define-objectives"></a>Hedefleri tanımlayın
1. Bu adımın merkezi bir hedefi tahmin etmek için analiz gereken önemli iş değişkenleri belirlemektir. Biz bu değişkenleri olarak başvurmak *model hedefleri*, ve bunlarla ilişkilendirilmiş ölçümleri proje başarısını belirlemek için kullanırız. İki tür hedefleri satış tahminleri veya sahte olan bir sırayı olasılığını gösterilebilir.

2. Proje hedefleri soran ve ilgili, belirli ve anlaşılır "diyez" soruları daraltmayı tanımlayın. Veri bilimi adları ve numaraları gibi soruları yanıtlamak için kullandığı bir işlemdir. Sharp sorular sormak daha fazla bilgi için bkz: [veri bilimi nasıl](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blogu. Genellikle veri bilimi veya machine learning beş tür soruları yanıtlamak kullanılır:
 
   * Ne kadar veya kaç tane? (regresyon)
   * Hangi kategori? (sınıflandırma)
   * Hangi Grup? (kümeleme)
   * Bu tuhaf mi? (anomali algılama)
   * Hangi seçeneği yapılması gerekir? (öneri)

   Hangi soruları soran ve nasıl yanıtlama, iş hedeflerinize başarır belirler.

3. Proje ekibi, roller ve sorumluluklar üyeleri belirterek tanımlayın. Daha fazla bilgi bulmak gibi üzerinde yinelemek bir üst düzey kilometre taşı planı geliştirin. 

4. Başarı ölçütleri tanımlayın. Örneğin, bir müşteri karmaşası öngörü elde etmek isteyebilirsiniz. Bu üç aylık proje sonuna "x" yüzde bir doğruluk oranını gerekir. Bu verilerle azaltmak için müşteri promosyonlar karmaşıklığı sunabilir. Ölçümleri olmalıdır **akıllı**: 

   * **S**elirli 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**IME bağlama 

### <a name="identify-data-sources"></a>Veri kaynaklarını tanımlama
Sharp sorularınıza bilinen örnekleri içeren veri kaynakları belirleyin. Aşağıdaki veriler için bakın:

* Sorunun ilgili veriler. Hedef ve hedef ilgili özelliklerin ölçüleri var mı?
* Model hedef doğru ölçü ve ilgi özelliklerini verileri.

Örneğin, varolan sistemler toplamak ve sorun gidermek ve proje hedeflerinize ulaşmak için veri ek türlerini oturum gerektiğini fark edebilirsiniz. Bu durumda, dış veri kaynakları için konum veya yeni verileri toplamak için sistemlerinizi güncelleştirme isteyebilirsiniz.

## <a name="artifacts"></a>Yapıtlar
Bu aşamada teslim edilebilir öğeler şunlardır:

   * [Kurucu belge](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): standart şablon TDSP Proje yapısı tanımında sağlanır. Kurucu belge yaşam belgesidir. Şablonu proje boyunca yeni bulmaları yaptığınız ve iş gereksinimleri değiştikçe güncelleştirin. Daha fazla ayrıntı, bulma işlemi ilerlemeyi olarak ekleme, bu belgedeki bağlı yinelemek için kullanılan anahtardır. Müşteri tutmak ve diğer Paydaşlar değişiklikler yaparken söz konusu ve bunları değişiklikler nedeniyle açık bir şekilde iletir.  
   * [Veri kaynakları](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/Data%20Defintion.md#raw-data-sources): **ham veri kaynakları** bölümünü **veri tanımlarını** TDSP projesinde bulunan rapor **veri raporu** klasörü verileri içerir kaynakları. Bu bölümde ham verileri için özgün ve hedef konumları belirtir. Sonraki aşamalarda, analitik ortamınız için verileri taşımak için komut dosyaları gibi ek ayrıntıları doldurun.  
   * [Veri sözlükler](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Data_Dictionaries): Bu belgede istemci tarafından sağlanan veri açıklamalarını sağlar. Bu açıklamalar varsa şemanın (veri türleri ve varsa doğrulama kuralları hakkında bilgi) ve varlık ilişkisi diyagramları hakkındaki bilgileri içerir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her adımda TDSP yaşam döngüsü bağlantıları verilmiştir:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alımı ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımlar gösteren baştan sona tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantılar ve küçük resim açıklamaları senaryolarla listesini sağlar. İzlenecek yollar bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl gösterilmektedir. 

Azure Machine Learning Studio kullanan TDSPs adımları yürütmek nasıl örnekleri için bkz: [TDSP Azure Machine Learning ile kullanmak](http://aka.ms/datascienceprocess).
