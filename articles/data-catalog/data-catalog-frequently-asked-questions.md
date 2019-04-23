---
title: Azure veri Kataloğu hakkında sık sorulan sorular
description: Veri kaynağı bulma, açıklama ve yönetim özelliklerini de dahil olmak üzere, Azure veri Kataloğu hakkında sık sorulan sorular.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 7c5241b9df23bb0334a39f2c684fd1bdff40b4c2
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998470"
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure veri Kataloğu hakkında sık sorulan sorular
Bu makalede, Azure veri Kataloğu hizmeti ile ilgili sık sorulan soruların yanıtlarını sağlar.

## <a name="what-is-azure-data-catalog"></a>Azure Veri Kataloğu nedir?
Veri Kataloğu, kayıt ve kurumsal veri kaynakları için bulma sistemi olarak görev yapan Microsoft azure'da barındırılan tam olarak yönetilen bir hizmettir. Veri Kataloğu ile analistler, veri bilimcilerine ve geliştiricilere, herhangi bir kullanıcı kaydetme, keşfedin, anlamak ve veri kaynaklarını.

## <a name="what-customer-challenges-does-it-solve"></a>Mevcut hangi müşteri sorunları çözüyor?
Veri Kataloğu veri kaynağı bulma ve "veri" sorunlarını ele alır, bu kullanıcıların keşfedin ve kurumsal veri kaynaklarını anlama.

## <a name="what-are-its-target-audiences"></a>Hedef kitlelere nelerdir?
Veri Kataloğu dahil olmak üzere teknik ve teknik olmayan kullanıcılar için tasarlanmıştır:

* Veri geliştiricileri ve BI ve analiz uzmanları için: Başkalarının kullanmak veri ve analiz içeriği oluşturmaktan sorumlu olan kişiler.
* Veri stewards: Veriler, ne anlama geldiğini ve nasıl kullanılması amaçlanmıştır hakkında bilgiye sahip kişiler.
* Veri tüketicileri: Kolayca bulmak için gereken kişilere anlama ve kendi istediğiniz aracı kullanarak, işini yapması için ihtiyaç duydukları verilere bağlanın.
* Merkezi BT: Yüzlerce veri kaynağına İşletme kullanıcıları tarafından bulunabilir hale getirmek gereksinim duyan ve verilerin nasıl kullanıldığı ve kim tarafından gözetim korumak gereksinim duyan kişiler.

## <a name="what-is-its-availability-by-region"></a>Bölgelere göre kullanılabilirliğini nedir?
Veri Kataloğu Hizmetleri şu veri merkezlerinde şu anda kullanılabilir:

* Batı ABD
* Doğu ABD
* Batı Avrupa
* Kuzey Avrupa
* Avustralya Doğu
* Güneydoğu Asya

## <a name="what-are-its-limits-on-the-number-of-data-assets"></a>Veri varlıklarını sayısı kendi sınırlarına nelerdir?
Ücretsiz sürüm, veri kataloğu için 5000 kayıtlı veri varlıklarını sınırlıdır.

, Veri Kataloğu standart sürümü, en çok 100.000 kayıtlı veri varlıklarını destekler.

Herhangi bir nesne, bir veri varlığı olarak görünümler, dosyalar ve raporlar, sayıları tablolar gibi veri Kataloğu'nda kayıtlı.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Desteklenen veri kaynağı ve varlık türleri nelerdir?
Şu anda desteklenen veri kaynakları listesi için bkz. [veri Kataloğu DSR](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Başka bir veri kaynağı için destek nasıl ister?
Özellik istekleri ve diğer geri bildirim göndermek için Git [Azure geri bildirim forumları veri Kataloğu](https://feedback.azure.com/forums/906052-data-catalog/category/320788-data-sources).

## <a name="how-do-i-get-started-with-data-catalog"></a>Veri Kataloğu'nu kullanmaya nasıl başlayabilirim?
Başlamak için en iyi giderek yoludur [veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md). Bu makalede hizmet özellikleri bir uçtan uca genel bakıştır.

## <a name="how-do-i-register-my-data"></a>Verilerimi nasıl kaydedebilirim?
Veri Kataloğu'nda, verilerinizi kaydetmek için:
1. Azure veri Kataloğu portalında içinde **Yayımla** alanında, Azure veri Kataloğu Kayıt Aracı'nı başlatın. 
2. Veri Kataloğu veri kaynağı kayıt aracı, veri Kataloğu portalına erişmek için kullandığınız aynı kimlik bilgileriyle oturum açın.
3. Veri kaynağı ve belirli kaydetmek istediğiniz varlıkları seçin.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Hangi özelliklerin kayıtlı veri varlıklarını ayıklayın?
Belirli özellikleri, veri kaynağından veri kaynağına farklılık gösterir ancak genel olarak, aşağıdaki bilgileri veri Kataloğu Yayımlama hizmeti ayıklar:

* Varlık adı
* Varlık Türü
* Varlık açıklaması
* Öznitelik/sütun adları
* Öznitelik/sütun veri türleri
* Öznitelik/sütun açıklaması

> [!IMPORTANT]
> Veri Kataloğu ile veri varlıklarını kaydetme taşımayın veya verilerinizi buluta kopyalayın. Bir veri kaynağından alınan varlıklar kaydetme varlıklar meta verileri Azure'a kopyalar ancak verileri var olan veri kaynağı konumu olarak kalır. Bu kuralın istisnası olarak, varlıkları kaydettiğinizde Önizleme kayıtları ya da veri profili yüklemek seçin ' dir. 20 kayıt kadar bir önizleme dahil ettiğinizde her varlığından kopyalanır ve veri Kataloğu'nda bir anlık görüntü olarak depolanır. Veri profili eklediğinizde, toplam bilgileri hesaplanır ve katalogdaki depolanan meta veriler dahil. Tablo, sütun başına null değerler yüzdesi veya sütun için en düşük, en yüksek ve ortalama değerler boyutunu toplam bilgileri içerebilir. 
>
>

> [!NOTE]
> Birinci sınıf bir sahip SQL Server Analysis Services gibi veri kaynakları için **açıklama** özelliği, veri Kataloğu veri kaynağı kayıt aracını, özellik değeri ayıklar. SQL Server ilişkisel veritabanları için eksik bir birinci sınıf **açıklama** özelliği, veri Kataloğu veri kaynağı kayıt aracını değerini ayıklar **ms_description** genişletilmiş özelliği için nesneler ve sütun. Daha fazla bilgi için [kullanarak veritabanı nesneleri üzerinde Genişletilmiş Özellikler](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-to-appear-in-the-catalog"></a>Ne kadar bu Kataloğu'nda görünmesi yeni kayıtlı varlıkların sürer?
Veri Kataloğu ile varlıkları kaydettikten sonra bir süre 5-10 veri Kataloğu portalında göründükleri önce beklenecek saniye olabilir.

## <a name="how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Nasıl açıklama ve meta verileri benim için kayıtlı veri varlıklarını zenginleştirin?
İçin kayıtlı varlık meta verilerini sağlamak için en basit yolu, veri Kataloğu Portalı'nda varlık seçin ve ardından değerleri seçili nesne için Özellikler bölmesi veya şema bölmesinde girin sağlamaktır.

Ayrıca, bazı meta veri uzmanları ve etiketler gibi kayıt işlemi sırasında sağlayabilir. O anda kayıtlı tüm varlıklar veri Kataloğu yayımlama hizmetinde sağladığınız değerler geçerlidir. Ek açıklamalar için Portalı'nda kısa süre önce kayıtlı nesneleri görüntülemek için seçin **portalı görüntüle** veri Kataloğu veri kaynağı kayıt aracını son ekranındaki düğmesi.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Kayıtlı veri nesnelerim nasıl silebilirim?
Portalda nesneyi seçip, ardından tıklayarak veri Kataloğu'ndan bir nesne silebilirsiniz **Sil** düğmesi. Nesne kaldırma, veri Kataloğu'ndan meta verilerini kaldırır, ancak temel alınan veri kaynağına etkilemez.

## <a name="what-is-an-expert"></a>Uzman nedir?
Uzman veri nesnesi hakkında bilgiye dayalı bir perspektif olan bir kişi anlamına gelir. Bir nesne birden çok uzmanlar olabilir. Uzman bir nesne için "sahip" olması gerekmez, ancak verilerin nasıl olabilir ve kullanılması gereken bilen birisi teknolojidir.

## <a name="how-do-i-share-information-with-the-data-catalog-team-if-i-encounter-problems"></a>Ben bir sorunla karşılaşırsanız nasıl bilgileri veri Kataloğu ekiple paylaşırım?
Sorunları bildirmek için bilgi paylaşmak ve soru sorun, Git [Azure veri Kataloğu Forumunda](https://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-the-catalog-work-with-another-data-source-that-im-interested-in"></a>Katalog, şunlarla ilgileniyorum başka bir veri kaynağı ile çalışır mı?
Etkin bir şekilde daha fazla veri kaynakları veri Kataloğu'na ekleme üzerinde çalışıyoruz. Desteklenen belirli bir veri kaynağına bakın, bunu önerir (veya zaten önerilen değilse, desteği ses) giderek isterseniz [Azure geri bildirim forumları veri Kataloğu](https://feedback.azure.com/forums/906052-data-catalog).

## <a name="how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Azure veri Kataloğu Power bı'daki veri Kataloğu'na nasıl Office 365 için ilişkili mi?
Azure veri Kataloğu Power bı'daki veri Kataloğu'nun evrim olarak düşünebilirsiniz. Spring 2017 itibarıyla Azure veri kataloğu için Excel paylaşımı ve Excel 2016'daki sorgular ve Power Query bulunmasını etkinleştirmek için kullanılır. Veri Kataloğu özellikleri Excel'de Power BI Pro lisansı olan kullanıcılar tarafından kullanılabilir.

## <a name="what-permissions-do-i-need-to-register-assets-with-data-catalog"></a>Varlıklar, veri Kataloğu'na kaydetmek hangi izinlerin gerekiyor?
Veri Kataloğu kayıt aracı çalıştırmak için meta veriler kaynaktan okumanıza izin verir veri kaynağında izinleri gerekir. Bir önizleme de dahil etmek için Kaydedilmekte nesne verilerden okunacak olanak sağlayan izinleri olmalıdır.

Veri Kataloğu, ayrıca hangi kullanıcıların ve grupların meta veri Kataloğu'na ekleyebilirsiniz kısıtlamak katalog yöneticileri sağlar. Ek bilgi için bkz: [veri kataloğu ve veri varlıklarına erişim güvenliğinin nasıl sağlanacağını](data-catalog-how-to-secure-catalog.md).

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Veri Kataloğu de şirket içi dağıtım için kullanılabilir hale getirilir?
Veri Kataloğu, karma veri kaynağı bulma çözümü sunmak için hem buluttaki hem de şirket içi veri kaynaklarıyla çalışabilirsiniz bir bulut hizmetidir. Şu anda bir sürümü çalıştıran şirket içi veri Kataloğu hizmeti için plan yok.

## <a name="can-i-extract-more-or-richer-metadata-from-the-data-sources-i-register"></a>Daha fazla veya daha zengin meta veriler kaydedebilirim veri kaynaklarından ayıklayabilmeniz için?
Veri Kataloğu yeteneklerini genişletmek etkin olarak çalışıyoruz. Kayıt sırasında veri kaynağından ayıklanan meta verileri ek olmasını istiyorsanız, bunu (veya bu, zaten önerilen değilse oylayın) önerin [Azure geri bildirim forumları veri Kataloğu](https://feedback.azure.com/forums/906052-data-catalog). 

Sütun/şema meta verileri, önizlemeler veya burada bu meta veri kaynağı kayıt aracı tarafından ayıklanan değil veri kaynakları için veri profilleri eklemek istiyorsanız, bu meta verileri eklemek için veri Kataloğu API'sini kullanabilirsiniz. Ek bilgi için bkz: [Azure veri Kataloğu REST API'si](https://docs.microsoft.com/rest/api/datacatalog/).

## <a name="how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Yalnızca belirli kişilerin bunları keşfedebilmesi için kayıtlı veri varlıklarının görünürlüğünü nasıl kısıtlarım?
Veri Kataloğu'nda veri varlıklarını seçin ve ardından **Sahipliği Al** düğmesi. Veri Kataloğu'nda veri varlıklarının sahipleri ya da sahibi varlıklarını bulmak veya görünürlük belirli kullanıcılara sınırlamak tüm kullanıcılara izin vermek için görünürlük ayarlarını değiştirebilirsiniz. Ek bilgi için bkz: [Azure veri Kataloğu'nda veri varlıklarını yönetme](data-catalog-how-to-manage.md).

## <a name="how-do-i-update-the-registration-for-a-data-asset-so-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Veri kaynağındaki değişiklikleri katalogda yansıtılır. böylece bir veri varlığı kaydı nasıl güncelleştirebilirim?
Katalogda zaten kayıtlı veri varlıklarını meta verilerini güncelleştirmek için basitçe varlıkları içeren veri kaynağını yeniden kaydedin. Veri kaynağında herhangi bir değişiklik Kataloğu'nda sütunları eklenmesi veya tabloları veya görünümleri kaldırılması gibi güncelleştirilir ancak kullanıcı tarafından sağlanan ek açıklamaları korunur.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Sorumun cevabı burada bulamadığınız. Yanıtlar için nereye gidebilirim?
Git [Azure veri Kataloğu Forumunda](https://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Burada sorulan sorular aşamalarından burada bulabilirsiniz.
