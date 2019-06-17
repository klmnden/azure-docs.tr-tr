---
title: Bing arama API'leri için gereksinimleri görüntülemek ve kullanın
titleSuffix: Azure Cognitive Services
description: Görüntüleme gereksinimleri uygulamalarınıza Bing arama API'leri sonuçlardan arayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: 5575668f164b97142e7c4b2ddb2608c3173426a6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60499894"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Bing Arama API’si kullanım ve görüntüleme gereksinimleri

Bu kullanım ve görüntü gereksinimleri aşağıdaki Bing arama ilişkileri meta verileri ve diğer sinyaller dahil olmak üzere API'lerinden, ilgili bilgiler ve içerik herhangi bir uygulama için geçerlidir.

- Bing Özel Arama
- Bing Varlık Arama
- Bing Resim Arama
- Bing Haber Arama
- Bing Video Arama
- Bing Görsel Arama
- Bing Web Araması
- Bing Yazım Denetimi
- Bing Otomatik Öneri

## <a name="definitions"></a>Tanımlar


|Terim  |Açıklama  |
|---------|---------|
|Yanıt     | Bir kategori sonuçlarının bir yanıt döndürdü. Örneğin, Bing Web araması API'si yanıtı yanıtlar Web sonuçları, resim, video, visual ve haber kategorileri içerebilir. |
|Yanıt     | Tüm yanıtlar ve yanıt olarak tek bir arama API'sine çağrıda alınan ilişkili veriler. |
|Sonuç    | Bir öğe bir yanıt bilgileri. Örneğin, bir tek haber makaleyle bağlantılı veri kümesini haber yanıtında bir sonucudur. |
|Arama API'leri    | toplu olarak, Bing özel arama, varlık araması, resim arama, haber arama, Video arama, görsel arama, yerel iş arama ve Web arama API'leri. |

## <a name="bing-spell-check-and-bing-autosuggest-api-restrictions"></a>Bing yazım denetimi ve Bing otomatik öneri API'si kısıtlamaları

Yapma:

- Kopyalama, saklamak veya Bing yazım denetimi veya Bing otomatik öneri API'leri almaya herhangi bir veri önbelleği.
- Bing yazım denetimi veya Bing otomatik öneri API'leri machine learning'e ya da benzer algoritmik etkinliği bir parçası olarak almaya verileri kullanın. Bu veriler eğitmek, değerlendirmek ya da siz veya üçüncü taraflara sunabilir yeni veya var olan hizmetleri geliştirmek için kullanmayın.

## <a name="bing-search-apis"></a>Bing Arama API’leri

> [!NOTE]
> Bu bölümdeki gereksinimleri yalnızca arama, Bing yazım denetimi veya Bing otomatik öneri içermez API'leri için geçerlidir. 

### <a name="internet-search-experience-requirements"></a>Internet arama deneyimi gereksinimleri

Yanıtları döndürülen tüm veriler, yalnızca internet arama deneyimleri kullanılabilir. Bir internet arama deneyimi görüntülenen içerik anlamına gelir: 

- İlgili olduğundan ve yanıt verdiğinden son kullanıcının doğrudan sorgu ya da diğer göstergesi arama ilgi ve hedefi (örneğin, bir kullanıcı belirtilen arama sorgusu). 

- Kullanıcıların bulmasına ve yanıtın veri kaynağına gidin yardımcı olur. Örneğin, yanıt köprüler tıklanabilir bağlantılar sağlama.

- Aralarından seçim yapabileceğiniz kullanıcı için birden çok sonuç içerir. 

- Kullanıcıların arama olanak sağlayan bir yerleşimden var.

- İçeriğin bir internet arama sonucu olduğunu görünür bir göstergesi içerir. Örneğin, "web" içeriği olan ifade.

- Bing arama API'si verilerinizi uygulanabilir yasa ve üçüncü taraf haklarını ihlal etmemesini sağlamak için tüm diğer uygun ölçüler içerir. Ölçüler ne olabilir, yasal danışmanlar uygun belgelere bakın.

Yalnızca bu internet arama deneyimi gereksinimleri için keşif URL'si, bu makalenin sonraki bölümlerinde açıklandığı istisnadır. 

### <a name="restrictions"></a>Kısıtlamalar

Yapma:

- Kopyalama, saklamak veya tüm veriler yanıtları önbelleğe alma (hariç tutma tarafından izin verilen azami ölçüde [kesintisiz hizmet devamlılığı](#continuity-of-service). 

- Machine learning'e ya da benzer algoritmik etkinlik kapsamında arama API'lerinden alınan veriler kullanın. Bu veriler eğitmek, değerlendirmek ya da siz veya üçüncü taraflara sunabilir yeni veya var olan hizmetleri geliştirmek için kullanmayın.

- (Dışındaki herhangi bir gereksinim ihlal etmemesini şekilde yeniden biçimlendirmek üzere), sonuçları içerik değiştirme yasaların gerektirdiği durumlar ya da Microsoft tarafından kabul sürece. 

- Öznitelik bilgileri ve URL'leri sonucu içerikle ilişkili atlayın.

- Yeniden sıralama, tarafından atlandığını dahil olmak üzere, sonuçları bir sipariş veya derecelendirme sağlandığında, yasaların gerektirdiği durumlar haricinde bir yanıt olarak görüntülenen veya Microsoft tarafından kabul. 

    > [!NOTE]
    > Bu gereksinim Bing özel arama API'si için portal aracılığıyla uygulanan yeniden sıralama için geçerli değildir.

- Diğer içerik yanıt herhangi bir bölümü içinde bir kullanıcının diğer içerik yanıtın bir parçası olduğunu düşünüyorsanız sunulmasını şekilde görüntüleyin. 

- Bir yanıt herhangi bir bölümünü görüntüleyen herhangi bir sayfa üzerinde Microsoft tarafından sağlanmayan reklamları görüntüleyin. 

- Yanıt sayfalarındaki tüm tanıtım görüntüle:
    - Bing görüntü, haber arama, Video arama veya görsel arama API'leri
    - Filtrelenmiş olan veya birincil (veya yalnızca) sınırlı görüntü, haber ve/veya video veya görsel arama sonuçları.

### <a name="notices-and-branding"></a>Bildirimler ve markalama 
Yapın:

- İşlevsel bir köprü göze çarpacak şekilde dahil [Microsoft gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=521839), neredeyse her noktasında bir kullanıcı bir arama sorgusu giriş imkanı kullanıcı deneyimi (UX). Bağ etiketi **Microsoft gizlilik bildirimi**.

- Bing, tutarlı marka öne [Bing olan marka kullanım kılavuzuna](https://go.microsoft.com/fwlink/?linkid=833278), neredeyse her bir kullanıcı bir arama sorgusu giriş imkanı UX noktası. Böyle bir marka gerekir kullanıcıya açıkça durum Microsoft internet arama deneyimini güçlendiren.

- Microsoft, aksi takdirde, kullanımınız için yazma belirtmediği sürece her yanıt (veya bir yanıt bölümü) Microsoft'a, Bing Web araması, resim arama, haber arama, Video araması ve görsel arama API'leri görüntülenen özniteliği. Bu açıklanan [Bing olan marka kullanım kılavuzuna](https://go.microsoft.com/fwlink/?linkid=833278). 

Yapma:

- Belirli kullanımınız için yazılı olarak Aksi halde Microsoft belirtmediği sürece öznitelik yanıtları (veya yanıtlarını bölümlerini) Bing özel arama API'den Microsoft, görüntülenir.

### <a name="transferring-responses"></a>Yanıtları aktarma

Bir Mesajlaşma uygulaması veya sosyal medya posta, aşağıdaki durumlardan gibi başka bir kullanıcı, arama API'si yanıt aktarmak için bir kullanıcı devre dışı bırakırsanız: 

- Aktarılan yanıtları gerekir:
  - Aktarma kullanıcıya görüntülenen yanıtların içeriğinden değiştirilmemiş olan içerik oluşur. Biçimlendirme değişikliklerini verilebilir.
  - Herhangi bir veri meta veri biçiminde dahil.
  - Bing Web, görüntü, Haberler, Video ve görsel API'ler yanıtlar için Bing tarafından desteklenen bir internet arama deneyimini aracılığıyla görüntüleme dilini gösteren yanıt alındı. Örneğin, Bing bu görüntü ile ilgili daha fazla "Bing tarafından desteklenen" veya "Bilgi" gibi dil görüntüleyebilir veya Bing logosu kullanabilirsiniz.
  - Bing özel arama API'si yanıtlar almak için bir internet arama deneyimi görüntüleme dilini gösteren yanıt alındı. Örneğin, "Hakkında daha fazla bilgi bu arama sonucunda." gibi dil görüntüleyebilir
  - Yanıtı oluşturmak için kullanılan tam sorgu çarpacak şekilde görüntüleyin.
  - Tanınmış bir bağlantı veya benzer attribution yanıtının temel alınan kaynağa doğrudan veya bir arama motoru (bing.com, m.bing.com veya özel arama hizmetinizi uygunsa) üzerinden içerir.
- Yanıtları aktarımını otomatik değildir. Aktarım NET bir şekilde yanıt aktarmak için bir hedefi evidencing bir kullanıcı eylemi tarafından başlatılmalıdır.
- Aktarma kullanıcının sorgusuna yanıt olarak görüntülenen yanıtları aktarmak için bir kullanıcı yalnızca sağlayabilir.

### <a name="continuity-of-service"></a>Kesintisiz hizmet devamlılığı 

Kopyalamayın, depolamak veya tüm veriler arama API yanıtları önbelleğe alma. Ancak, sürekliliği ve veri işleme hizmeti erişimi etkinleştirmek için sonuçları yalnızca aşağıdaki koşullarda Koru:

#### <a name="device"></a>Cihaz

Tutulan sonuçlar yalnızca kullanılabilir olması koşuluyla, sonuçları bir aygıtta saati (i) 24 saatten daha az için sorgu veya (ii) şirketin başka bir sorgu için güncelleştirilmiş sonuçları, bir kullanıcının gönderdiğini kadar korumak bir kullanıcı sağlayabilir:

- Kullanıcının erişim sağlamak için daha önce bu cihazda (örneğin, durumunda hizmet kesintisini) söz konusu kullanıcı için döndürülen sonuçlanır.
- Kullanıcının sinyaller (örneğin, durumunda beklenen hizmet kesintisi) göre kullanıcının gereksinimlerinin olasılığına kişiselleştirilmiş, proaktif sorgu için döndürülen sonuçlarını depolamak için.

#### <a name="server"></a>Sunucusu

Güvenli bir şekilde sizin denetlediğiniz bir sunucuda tek bir kullanıcı için belirli sonuçları tutabilir ve yalnızca tutulan sonuçları görüntüleyin:

- Çözümünüzdeki bu kullanıcıya daha önce döndürülen sonuçların geçmiş bir rapora erişmek kullanıcı etkinleştirmek için. Sonuçları değil (i) 21 günden fazla bir süre zamanından sonra son kullanıcının ilk sorgu korunur ve (ii) bir kullanıcının yeni veya yinelenen sorgusuna yanıt olarak görüntülenir.
- Kullanıcı gereksinimlerini, kullanıcının sinyalleri üzerinde temel olasılığına proaktif sorgunuz için döndürülen sonuçlarını depolamak için kişiselleştirilmiş. Bu sonuçları saati (i) 24 saatten daha az için sorgu veya (ii) şirketin başka bir sorgu için güncelleştirilmiş sonuçları bir kullanıcının gönderdiğini kadar depolayabilirsiniz.

Korunan her sonuçları belirli bir kullanıcı için başka bir kullanıcı için sonuçlarla commingled olamaz. Diğer bir deyişle, her kullanıcı sonuçlarını korunur ve gerekir ayrı olarak teslim.

### <a name="general"></a>Genel 

Tutulan sonuçları tüm sunumu için:

- Sorgunun gönderildiği zaman açık, görünür bir bildirim içerir.
- Mevcut bir düğme veya benzer bir kullanıcı için yeniden sorgular ve almak için anlamına gelir, sonuçları güncelleştirildi. 
- Sonuçları sunuda marka Bing korur.
- silin (ve gerekirse yeni bir sorgu ile yenileme) belirtilen zaman dilimlerine içindeki depolanmış sonuç.

### <a name="non-display-url-discovery"></a>Görüntü olmayan URL'yi bulma 

Yalnızca, kullanıcı ya da müşteri için bir sorgu duyarlı bilgi kaynakları URL'lerini keşfetme uygulamalarınızdaki için bir internet olmayan arama deneyimini araması yanıtlarında kullanabilirsiniz. Bir rapor ya da benzer yanıt sağladığınız böyle URL'leri kopyalayabilirsiniz:

- Yalnızca bu kullanıcı veya müşteri, yanıt olarak sorgu.
- Yalnızca bu önemli ek değerli içerik, sorgu ile ilgili içeriyorsa.

Arama API'lerinin önceki bölümlerde kullanın ve görüntü gereksinimleri aşağıdaki istisnalar dışında bu görüntü olmayan kullanım için geçerli değildir: 

- Değil önbellek, kopyalama veya herhangi bir veri veya içerik, saklamak veya daha önce açıklanan kopyalama, sınırlı URL dışındaki arama yanıt öğesinden türetilen.
- Arama API'lerinden alınan (URL'leri dahil) veri kullanımını herhangi bir uygulanabilir yasalar veya üçüncü taraf haklarını ihlal etmemesini sağlamak.
- Herhangi bir arama dizini veya machine learning veya benzer algoritmik etkinlik kapsamında arama API'lerinden alınan (URL'leri dahil) veri kullanmayın. Bu veri train oluşturma, değerlendirmek veya siz veya üçüncü taraflara sunabilir hizmetlerini geliştirmek için kullanmayın.

## <a name="gdpr-compliance"></a>GDPR uyumluluğu  

Genel veri koruma yönetmeliği (GDPR) ve arama API'leri, Bing yazım denetimi API'si ve Bing otomatik öneri API'si çağrılarını bağlantılı olarak işlenir Avrupa Birliği tabi kişisel verileri göre siz ve Microsoft olduğunu biliyoruz. bağımsız veri denetleyicilerine GDPR altında. Bağımsız olarak, GDPR ile uyum için sorumlu olursunuz.  

