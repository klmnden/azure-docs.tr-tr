---
title: "Azure veri Kataloğu sık sorulan sorular | Microsoft Docs"
description: "Veri kaynağı bulma, ek açıklama ve yönetim yetenekleri de dahil olmak üzere Azure veri Kataloğu hakkında sık sorulan sorular."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 11e4bc7e71b4a94c3a0eda4275745b1beb44974d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure veri Kataloğu sık sorulan sorular
Bu makale, Azure veri Kataloğu hizmeti ile ilgili sık sorulan soruların yanıtlarını sağlar.

## <a name="what-is-azure-data-catalog"></a>Azure Veri Kataloğu nedir?
Veri Kataloğu kurumsal veri kaynaklarını bulma ve kayıt sistemi olarak hizmet veren Microsoft Azure içinde barındırılan tam olarak yönetilen bir hizmettir. Veri Kataloğu ile analistlerden veri bilimcilerine ve geliştiricilere, herhangi bir kullanıcı kaydetme, bulmak, anlamak ve veri kaynaklarını tüketebilir.

## <a name="what-customer-challenges-does-it-solve"></a>Mevcut hangi müşteri sorunları çözüyor?
Kullanıcıları bulmak ve kurumsal veri kaynaklarını anladığınızdan emin veri Kataloğu, veri kaynağı bulma ve "koyu verilerinin" sorunlarını ele alır.

## <a name="what-are-its-target-audiences"></a>Hedef kitlenizi nelerdir?
Veri Kataloğu dahil olmak üzere teknik ve teknik olmayan kullanıcılar için tasarlanmıştır:

* Veri geliştiricileri ve BI ve analiz uzmanlarının: başkalarının kullanmak verileri ve çözümlemeler içerik oluşturmaktan sorumlu olan kişiler.
* Veri stewards: verileri, ne anlama geldiğini ve nasıl kullanılmak üzere tasarlanmıştır hakkında bilgi olan kişiler.
* Veri tüketicileri: kolayca bulmak için gereken kişilere anlamak ve kendi seçtikleri aracını kullanarak, işlerini gerçekleştirmek için ihtiyaç duydukları verilere bağlanın.
* Merkezi BT: veri kaynakları yüzlerce iş kullanıcılar tarafından bulunabilir yapmak gereksinim duyan ve gözetim üzerinden verileri nasıl kullanıldığını ve kim tarafından korumak gereksinim duyan kişilere.

## <a name="what-is-its-availability-by-region"></a>Bölgeye göre kendi kullanılabilirlik nedir?
Veri Kataloğu Hizmetleri şu veri merkezlerinde şu anda kullanılabilir:

* Batı ABD
* Doğu ABD
* Batı Avrupa
* Kuzey Avrupa
* Avustralya Doğu
* Güneydoğu Asya

## <a name="what-are-its-limits-on-the-number-of-data-assets"></a>Veri varlıklarını sayısı kendi sınırlarına nelerdir?
Boş veri Kataloğu sürümü 5000 kayıtlı veri varlıklarına sınırlıdır.

, Veri Kataloğu standart sürümü en fazla 100.000 kayıtlı veri varlıklarını destekler.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Desteklenen veri kaynağı ve varlık türleri nelerdir?
Şu anda desteklenen veri kaynaklarının listesi için bkz: [veri Kataloğu DSR](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Başka bir veri kaynağı için destek nasıl istek mu?
Özellik istekleri ve diğer geri bildirim göndermek için Git [Azure veri Kataloğu Forumu](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-do-i-get-started-with-data-catalog"></a>Veri Kataloğu ile çalışmaya nasıl?
Başlamak için en iyi giderek yoludur [veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md). Bu makalede hizmet özelliklerine uçtan uca genel bakış ' dir.

## <a name="how-do-i-register-my-data"></a>Verilerim nasıl kaydettirebilirim?
Veri Kataloğu'nda, verilerinizi kaydetmek için:
1. Azure veri Kataloğu portalında içinde **Yayımla** alanında, Azure veri Kataloğu Kayıt Aracı'nı başlatın. 
2. Veri Kataloğu yayınlama uygulamada, veri Kataloğu portalına erişmek için kullandığınız aynı kimlik bilgilerinizle oturum açın.
3. Veri kaynağı ve kaydetmek istediğiniz belirli varlıklar seçin.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Hangi özelliklerin kayıtlı veri varlıklarını ayıklayın?
Belirli özellikler veri kaynağına veri kaynağından farklı ancak genel olarak, veri Kataloğu Yayımlama hizmeti aşağıdaki bilgileri ayıklar:

* Varlık adı
* Varlık türü
* Varlık açıklaması
* Öznitelik/sütun adları
* Öznitelik/sütun veri türleri
* Öznitelik/sütun açıklaması

> [!IMPORTANT]
> Veri Kataloğu ile veri varlıklarını kaydetme taşımayın veya buluta verilerinizi kopyalayın. Veri kaynağındaki varlıklar kaydetme varlıklar meta verileri için Azure kopyalar, ancak veriler var olan veri kaynağı konumda kalır. Bu kural varlıklar kaydederken Önizleme kayıtları ya da veri profili karşıya yüklemek seçerseniz istisnadır. En fazla 20 kayıt önizlemesini dahil ettiğinizde her varlığından kopyalanır ve veri Kataloğu'nda anlık görüntü olarak depolanır. Veri profili eklediğinizde, toplama bilgi hesaplanır ve kataloğunda depolanan meta veriler dahil. Tablo, sütun başına null değerler yüzdesi veya sütunlar için en az, en fazla ve ortalama değerler boyutunu toplam bilgiler içerebilir. 
>
>

> [!NOTE]
> Birinci sınıf bir sahip SQL Server Analysis Services gibi veri kaynakları için **açıklama** özelliği, uygulama yayımlama veri Kataloğu, özellik değeri ayıklar. SQL Server ilişkisel veritabanları için hangi eksikliği birinci sınıf bir **açıklama** özelliği, uygulama yayımlama veri Kataloğu ayıklar değerinden **ms_description** özelliği için genişletilmiş nesneler ve sütun. Daha fazla bilgi için bkz: [veritabanı nesneleri üzerinde genişletilmiş özellikler kullanarak](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-to-appear-in-the-catalog"></a>Nasıl yeni kayıtlı varlık Kataloğu'nda görünmesi için sürmesi gerektiğini?
Veri Kataloğu ile varlıklar kaydettikten sonra bir süre 5-10 veri Kataloğu Portalı'nda görünmesi saniye olabilir.

## <a name="how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Nasıl açıklama ve meta veriler için my kayıtlı veri varlıklarını zenginleştirmek?
Kayıtlı varlık için meta verilerini sağlamak için basit veri Kataloğu Portalı'nda varlığı seçin ve ardından değerleri seçili nesne için Özellikler bölmesinde veya şema bölmesinde girmek için yoldur.

Ayrıca kayıt işlemi sırasında uzmanlar ve etiketler gibi bazı meta verileri sağlayabilir. O anda kayıtlı tüm varlıklar için veri Kataloğu yayınlama hizmetinde sağladığınız değerleri uygulayın. Ek açıklamalar için Portalı'nda son kayıtlı nesneleri görüntülemek için seçin **portalı görüntüle** uygulama yayımlama veri Kataloğu'nun son ekranında düğmesini.

## <a name="how-do-i-delete-my-registered-data-objects"></a>My kayıtlı veri nesneleri nasıl silebilirim?
Bir nesne portalda nesneyi seçip, ardından veri Kataloğu'ndan silebilirsiniz **silmek** düğmesi. Nesne kaldırma meta veri Kataloğu'ndan kaldırır, ancak veri kaynağındaki etkilemez.

## <a name="what-is-an-expert"></a>Uzman nedir?
Uzman bir veri nesnesi hakkında bilinçli bir perspektif sahip bir kullanıcıdır. Bir nesne birden çok uzmanlar olabilir. Uzman bir nesne için "sahip" olması gerekmez, ancak yalnızca nasıl veri ve kullanılması gereken bilir kişidir.

## <a name="how-do-i-share-information-with-the-data-catalog-team-if-i-encounter-problems"></a>Sorunlarla karşılaşırsanız nasıl ı bilgi veri Kataloğu paylaşabilirsiniz?
Sorunları bildirmek için bilgi paylaşmak ve soru sormak, Git [Azure veri Kataloğu Forumu](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-the-catalog-work-with-another-data-source-that-im-interested-in"></a>Katalog ilginizi ben başka bir veri kaynağı ile çalışır mı?
Etkin bir şekilde daha fazla veri kaynakları veri Kataloğu'na eklemek için çalışıyoruz. Desteklenen belirli bir veri kaynağına bakın, bunu önerir (veya zaten önerilen değilse, desteği sesli) giderek isterseniz [Azure veri Kataloğu Forumu](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Nasıl Azure veri Kataloğu Office 365 için Power BI veri Kataloğu'na ilişkili mi?
Azure veri Kataloğu Power BI veri Kataloğu'nun bir evrimi olarak düşünebilirsiniz. İlkbahar 2017 sürümünden itibaren Azure veri Kataloğu Excel için paylaşımı ve Excel 2016 sorgularda ve Power Query bulunmasını etkinleştirmek için kullanılır. Veri Kataloğu özellikleri Excel, Power BI Pro lisansına sahip olan kullanıcılar için kullanılabilir.

## <a name="what-permissions-do-i-need-to-register-assets-with-data-catalog"></a>Veri Kataloğu ile varlıklar kaydetmek hangi izinlerin gerekiyor mu?
Veri Kataloğu kayıt aracını çalıştırmak için meta veriler kaynaktan okumaya izin veren veri kaynağı izinleri gerekir. Ayrıca bir önizleme eklemek için Kaydedilmekte nesneleri verileri okumak izin izinleri olmalıdır.

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Veri Kataloğu de şirket içi dağıtım kullanımına sunulacak?
Veri Kataloğu, karma bir veri kaynağı bulma çözümü sunmak için bulut ve şirket içi veri kaynakları ile çalışabilirsiniz bir bulut hizmetidir. Şu anda çalışan şirket içi veri Kataloğu hizmetini sürümü için plan yok.

## <a name="can-i-extract-more-or-richer-metadata-from-the-data-sources-i-register"></a>Daha fazla ya da daha zengin meta veri ı kaydetmek veri kaynaklarından veri ayıklamak?
Etkin bir şekilde veri Kataloğu yeteneklerini genişletmek için çalışıyoruz. Kayıt sırasında veri kaynağından ayıklanan ek meta veri sahip olmak istiyorsanız, bunu (veya oy için bu, zaten önerilen değilse) önermek [Azure veri Kataloğu Forumu](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Gelecekte, biz genişletilebilirlik API aracılığıyla yeni veri kaynağı türleri eklemek için üçüncü taraf izin verir.

## <a name="how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Böylece yalnızca belirli kişilerin bulabilmesi için nasıl kayıtlı veri varlıklarının görünürlüğünü kısıtlayabilir?
Veri Kataloğu'nda veri varlıklarının seçin ve ardından **Sahipliği Al** düğmesi. Veri Kataloğu veri varlıklarını sahipleri ya da ait varlıklarını bulmak ve görünürlük belirli kullanıcılara kısıtlamak tüm kullanıcıların izin vermek için görünürlük ayarlarını değiştirebilirsiniz.

## <a name="how-do-i-update-the-registration-for-a-data-asset-so-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Böylece veri kaynağındaki değişiklikler katalogda yansıtılır nasıl bir veri varlığına kaydı güncelleştiririm?
Katalogda zaten kayıtlı veri varlıklarını meta verilerini güncelleştirmek için yalnızca varlıkları içeren veri kaynağını yeniden kaydedin. Kataloğu'nda değişiklikleri veri kaynağındaki sütunları eklenmesi veya tablolar veya görünümler kaldırılması gibi güncelleştirildi, ancak kullanıcılar tarafından sağlanan ek açıklamaları korunur.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Sorumun cevabı burada cevaplanıp değil. Yanıtlar için nereye?
Git [Azure veri Kataloğu Forumu](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Var. sorulan sorular yollarını burada bulabilirsiniz.
