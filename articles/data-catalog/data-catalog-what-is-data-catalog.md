---
title: "Azure Veri Kataloğu&quot;na giriş | Microsoft Docs"
description: "Bu makalede, özelliklerinin ve tasarımında giderilmesi hedeflenen sorunların yanı sıra Microsoft Azure Veri Kataloğu&quot;na genel bakış sunulmaktadır. Veri Kataloğu, analistlerden veri bilimcilerine ve geliştiricilere kadar herhangi bir kullanıcının veri kaynaklarını kaydetmesine, bulmasına, anlamasına ve kullanmasına olanak tanıyan özellikler sağlar."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: cc733907-17ec-4153-9f0c-5b3754b2db19
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 05/15/2017
ms.author: maroche
ms.translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: fb8f43f5bb5725da30e67cdf5d7b066fe40ed003
ms.contentlocale: tr-tr
ms.lasthandoff: 04/07/2017


---
# <a name="what-is-azure-data-catalog"></a>Azure Veri Kataloğu nedir?
Azure Veri Kataloğu, kullanıcıların ihtiyaç duyduğu veri kaynaklarını bulmasına ve bulduğu veri kaynaklarını anlamasına olanak tanıyan ve aynı zamanda kuruluşların var olan yatırımlarından daha fazla değer elde etmesine yardımcı olan, tamamen yönetilen bir bulut hizmetidir. Veri Kataloğu, analistlerden veri bilimcilerine ve geliştiricilere kadar herhangi bir kullanıcının veri kaynaklarını bulmasına, anlamasına ve kullanmasına olanak tanıyan özellikler sunar. Veri Kataloğu, meta verilere ve ek açıklamalara yönelik bir kitle kaynak modeli içerir ve tüm kullanıcıların bir veri topluluğu ve kültürü oluşturmak üzere bilgileriyle katkıda bulunmasına olanak tanır.

## <a name="discovery-challenges-for-data-consumers"></a>Veri tüketicileri için bulma zorlukları
Geleneksel olarak, kurumsal veri kaynaklarının bulunması grupsal bilgilere dayanan organik bir süreç olmuştur. Bu, bilgi varlıklarından en yüksek değeri elde etmek isteyen şirketler için çeşitli zorluklar teşkil etmektedir.

* Kullanıcılar, başka bir işlemin parçası olarak karşılaşmadıkça veri kaynaklarının varlığından haberdar değildir ve veri kaynaklarının kayıtlı olduğu merkezi bir konum bulunmamaktadır.
* Bir kullanıcı bir veri kaynağının konumunu bilmediği sürece, bir istemci uygulaması kullanarak verilere bağlanamamaktadır; veri kullanımı deneyimleri, kullanıcıların bağlantı dizesini veya yolunu bilmesini gerektirmektedir.
* Bir kullanıcı bir veri kaynağının belgelerinin konumunu bilmediği sürece, verilerin hedeflenen kullanımlarını anlayamamaktadır; veri kaynakları ve belgeler farklı konumlarda bulunmakta ve farklı deneyimlerle kullanılmaktadır.
* Kullanıcının bir bilgi varlığı ile ilgili soruları olması durumunda, verilerden sorumlu uzmanı veya ekibi bulması ve bu uzmanlarla çevrimdışı olarak iletişime geçmesi gerekir; veriler ile verilerin kullanımına yönelik uzman perspektiflerine sahip kişiler arasında açık bir bağlantı mevcut değildir.
* Bir kullanıcı, veri kaynağına erişim isteme işlemini anlamadığı sürece, veri kaynağının ve belgelerinin bulunması kullanıcıya ihtiyaç duyduğu veriler için yine de erişim sağlamaz.

## <a name="discovery-challenges-for-data-producers"></a>Veri üreticileri için bulma zorlukları
Veri tüketicileri bu sorunlarla karşılaşırken, bilgi varlıkları oluşturmaktan ve bunların bakımını yapmaktan sorumlu kullanıcılar kendilerine özgü zorluklarla yüz yüze gelmektedir.

* Veri kaynaklarına tanımlayıcı meta verilerle açıklama eklemek genellikle sonuç vermeyen bir işlemdir; istemci uygulamaları genellikle veri kaynağında depolanan açıklamaları yok sayar.
* Veri kaynakları için belge oluşturmak genellikle sonuç vermeyen bir işlemdir; belgelerin veri kaynağı ile eşitlenmiş şekilde kalmasının sağlanması devam eden bir sorumluluktur ve belgelerin genellikle güncel olmadığı düşünüldüğünden, kullanıcılar belgelere güvenmez.
* Veri kaynağına erişimin kısıtlanması ve veri tüketicilerinin nasıl erişim isteneceğini bilmesinin sağlanması sürekli karşılaşılan bir zorluktur.

Veri kaynağına yönelik belgelerin oluşturulması ve bakımının yapılması karmaşık ve zaman alan işlemlerdir. Bu belgelerin, veri kaynağını kullanan herkesin erişimine hazır hale getirilmesi ise genellikle daha da karmaşık ve zaman alan bir işlemdir.

Bu zorluklar bir araya geldiğinde, kurumsal verilerin kullanımını ve anlaşılmasını teşvik etmek ve desteklemek isteyen şirketler için ciddi bir engel oluşturmaktadır.

## <a name="azure-data-catalog-can-help"></a>Azure Veri Kataloğu bu konuda yardımcı olabilir
Veri Kataloğu, bu sorunları gidermek ve kuruluşların var olan bilgi varlıklarından en yüksek değeri elde edebilmesini sağlamak üzere tasarlanmıştır. Veri Kataloğu, veri kaynaklarını yönettikleri verilere ihtiyaç duyan kullanıcılar tarafından kolayca bulunabilir ve anlaşılabilir kılarak bu konuda destek sağlar.

Veri Kataloğu, veri kaynağının kaydedilebileceği bulut tabanlı bir hizmet sağlar. Veriler var olan konumunda kalırken, meta verilerin kopyası ve veri kaynağı konumuna yönelik bir başvuru Veri Kataloğu'na eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

Bir veri kaynağı kaydedildikten sonra veri kaynağının meta verileri, kaydı gerçekleştiren kullanıcı veya kuruluştaki diğer kullanıcılar tarafından zenginleştirilebilir. Herhangi bir kullanıcı, açıklama, etiket veya veri kaynağı erişimi istemeye yönelik belge ve işlemler gibi diğer meta verileri sağlayarak bir veri kaynağına açıklama ekleyebilir. Bu tanımlayıcı meta veriler, veri kaynağından kaydedilen yapısal meta verilere (sütun adları ve veri türleri gibi) ek niteliğindedir.

Veri kaynaklarını ve bunların kullanımını bulup anlamak, kaynakların kaydedilmesindeki birincil amaçtır. Kurumsal kullanıcılar uğraşları (iş zekası, uygulama geliştirme, veri bilimi veya doğru verilerin gerekli olduğu başka herhangi bir görev olabilir) için veriye ihtiyaç duyduğunda, ihtiyaçlarını karşılayan verileri hızlı bir şekilde bulmak, verilerin amacına uygunluğunu değerlendirmek için verileri anlamak ve veri kaynağını tercih ettikleri araçta açarak bu verileri kullanmak üzere Veri Kataloğu bulma deneyimini kullanabilir. Veri Kataloğu ayrıca, kullanıcıların önceden kaydedilmiş olan veri kaynaklarını etiketleyerek, belgeleyerek ve bunlara açıklama ekleyerek ve katalog kullanıcıları topluluğu tarafından sonrasında bulunabilmesini, anlaşılabilmesini ve kullanılabilmesini sağlamak üzere yeni veri kaynaklarını kaydederek kataloğa katılımda bulunmasına olanak tanır.

![Veri Kataloğu Özellikleri](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="get-started-with-data-catalog"></a>Veri Kataloğu ile çalışmaya başlama
Veri Kataloğu ile çalışmaya başlamak için [www.azuredatacatalog.com](https://www.azuredatacatalog.com) adresini ziyaret edin.

Başlangıç kılavuzuna [buradan](data-catalog-get-started.md) erişebilirsiniz.

## <a name="learn-more-about-data-catalog"></a>Veri Kataloğu hakkında daha fazla bilgi edinin
Veri Kataloğu'nun özellikleri hakkında daha fazla bilgi edinmek için bkz.

* [Veri kaynaklarını kaydetme](data-catalog-how-to-register.md)
* [Veri kaynaklarını bulma](data-catalog-how-to-discover.md)
* [Veri kaynaklarına açıklama ekleme](data-catalog-how-to-annotate.md)
* [Veri kaynaklarını belgeleme](data-catalog-how-to-documentation.md)
* [Veri kaynaklarına bağlanma](data-catalog-how-to-connect.md)
* [Büyük verilerle çalışma](data-catalog-how-to-big-data.md)
* [Veri varlıklarını yönetme](data-catalog-how-to-manage.md)
* [İş Sözlüğünü ayarlama](data-catalog-how-to-business-glossary.md)
* [Sık Sorulan Sorular](data-catalog-frequently-asked-questions.md)

