---
title: Proje yanıt arama kullanın ve gereksinimlerini - Microsoft Bilişsel hizmetler görüntülemek | Microsoft Docs
description: Proje yanıt arama uç noktası için gereksinimlerini görüntülemek ve kullanın.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 6e8eaaaa2c83a1420f2de86b23e15f4f19f7a565
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354136"
---
# <a name="project-answer-search-use-and-display-requirements"></a>Proje yanıt arama kullanın ve gereksinimleri görüntüleme

Kullanın ve görüntü gereksinimleri içerik ve ilgili bilgileri, herhangi bir uygulaması için örneğin, ilişkileri, meta verileri ve Bing Bilgi Bankası araması, Bing özel arama, varlık arama, görüntü Ara çağrıları aracılığıyla kullanılabilen diğer sinyaller Uygula Haber arama, Video arama, Visual arama arama Web, yazım denetimi ve otomatik öneri API'leri. Uygulama Ayrıntıları bu gereksinimleri ile ilgili belirli özellikleri ve sonuçları için belgelerinde bulunabilir.

## <a name="1-bing-spell-check-and-bing-autosuggest-api"></a>1. Bing yazım denetimi ve Bing otomatik öneri API.

Yapma:

- kopyalama, saklamak veya Bing yazım denetimi veya Bing otomatik öneri API'leri aldığınız herhangi bir veriyi önbelleğe alma
- Bing yazım denetimi veya Bing otomatik öneri API'leri herhangi bir machine learning veya eğitmek, değerlendirmek veya siz veya üçüncü tarafların sunabilir yeni veya var olan hizmetlerini geliştirmek için benzer algoritmik etkinlik bir parçası olarak almaya verileri kullanın.

## <a name="2-definitions"></a>2. Tanımlar

- "yanıt" bir yanıtta döndürülen sonuçları kategorisine ifade eder. Örneğin, Bing Web arama API yanıtından Web sayfası sonuçları, resim, video ve haber kategorilerde yanıtlar içerebilir;
- "yanıt" tüm yanıtlar anlamına gelir ve yanıt bir arama API için tek bir çağrı olarak alınan verileri ilişkili;
- "sonuç" öğeyi yanıt bilgilerini ifade eder. Örneğin, bir tek haber makaleyle bağlantılı veri kümesi bir haber yanıtında bir sonucudur.
- "Arama API'leri", topluca Bing özel arama, varlık arama, görüntü arama, haber arama, Video arama, Visual arama ve Web arama API'leri anlamına gelir. 


## <a name="3-search-apis"></a>3. API arama

Bu bölüm 3'te gereksinimleri arama API'leri için geçerlidir.

**BİR. Internet arama deneyimi.** Yanıtları döndürülen tüm veriler yalnızca Internet arama deneyimleri kullanılabilir. Bir internet arama deneyimi görüntülenen, olarak geçerli içerik anlamına gelir: 
- Son kullanıcının doğrudan sorgu veya diğer göstergesi kullanıcının arama faiz ve hedefi (örneğin, kullanıcı belirtilen arama sorgusu); esnek ve ilgili 
- Kullanıcıların bulmasına ve veri kaynakları için gidin yardımcı olur (örneğin, içeriği veya attribution conspicuously verilerle görüntülenen tıklatılabilir bir bağlantı olması için sağlanan URL'leri köprü olarak uygulanan); veya, Bing varlık arama API varsa, görünür şekilde aratıp ilgili sorguyu için arama sonuçlarını gitmek kullanıcının sağlayan yanıt sağlanan aratıp URL bağlantı;
- arasından seçim yapmak son kullanıcı için birden çok sonuç içerir (örneğin, haber yanıt birkaç sonuçlarından görüntülenir veya daha az birkaç varsa tüm sonuçları döndüren); 
- Arama (küçük resimleri küçük resim-kullanıcının görünen orantılı olarak boyutlandırılır Örneğin görüntü); amaca uygun bir tutar sınırlıdır 
- içeriğin Internet arama sonuçları (içeriğin "Web'den" olduğu gibi bir deyim); olduğundan son kullanıcının görünür bir gösterge içerir ve
- Arama API'lerden alınan verileri kullanımınız herhangi bir yasaların veya (örneğin ile ilgili lisans uymak Creative Commons lisansı, bağlı olan, üçüncü taraf haklarını ihlal emin olmak uygun önlemleri, herhangi bir birleşimini içerir Koşulları). Ne ölçüleri olabilir belirlemek için yasal danışmanlar uygun belgelere bakın.
Yalnızca Internet arama Deneyimi gereksinimi URL bulma için bölüm 3E (olmayan görüntüleme URL'sini bulma) aşağıda açıklandığı gibi istisnadır. 

**B. Kısıtlamaları.** Yapma:

- kopyalama, saklamak veya yanıtlardan (hariç tutma "Hizmet sürekliliği" bölümünde aşağıdaki tarafından izin verilen azami ölçüde); herhangi bir veri önbelleğe alma 
- Arama API'lerden herhangi bir machine learning veya benzer algoritmik etkinlik bir parçası olarak alınan veriler eğitmek, değerlendirmek ya da siz veya üçüncü tarafların sunabilir yeni veya var olan hizmetleri geliştirmek için kullanın.
- sonuçları (diğer herhangi bir gereksinim ihlal olmayan bir şekilde yeniden biçimlendirmek üzere daha diğer) içeriğini değiştirme yasalar tarafından gerekli veya Microsoft tarafından; anlaşılan sürece 
- Attribution ve sonuç içerikle ilişkili URL'lerin atlayın;
- Sipariş, kusurdan, sipariş veya derecelendirme sağlandığında bir yanıt olarak gösterilen sonuçları dahil olmak üzere (Bing özel arama API için bu kural customsearch.ai Portalı aracılığıyla uygulanan yeniden sıralama için geçerli değildir), yasalar tarafından gerekli veya Microsoft tarafından anlaşılan sürece ;
- bir yanıt herhangi bir kısmını içindeki diğer içeriği diğer içerik yanıtın bir parçası olduğunu düşünmek için bir son kullanıcı sunulmasını bir biçimde görüntüler; 
- bir yanıt herhangi bir bölümünü görüntüler herhangi bir sayfasında Microsoft tarafından sağlanmayan reklam görüntüler; -Tüm tanıtım Bing görüntüsünden, haber veya Video arama API'leri; yanıtları (i) ile görüntüleme ya da (II) olan veya filtre öncelikle (veya yalnızca) sınırlı görüntü, haber ve/veya video sonuçları.

**C. Bildirimler ve markalama.** 

- Göze çarpacak şekilde adresinde Microsoft gizlilik bildirimi işlevsel köprü dahil https://go.microsoft.com/fwlink/?LinkId=521839, her bir kullanıcı bir arama sorgusu giriş olanağı sunar kullanıcı deneyimini (UX) noktasında yakınında. Köprü "Microsoft gizlilik bildirimi" etiketi.
- Marka, adresinde yönergeleri ile tutarlı Bing öne https://go.microsoft.com/fwlink/?linkid=833278, her bir kullanıcı bir arama sorgusu giriş olanağı sunar UX noktasında yakınında.  Bu tür markalama açıkça kullanıcıya Microsoft Internet arama deneyimi başlatırken olduğunu belirtmek gerekir.
- Her yanıt (veya bir yanıt kısmı) Bing Web, görüntü, Haberler ve Video API'leri bölümünde açıklandığı gibi Microsoft görüntülenen özniteliği https://go.microsoft.com/fwlink/?linkid=833278sürece Microsoft, aksi halde kullanımınız için yazma belirtir. 
- Microsoft aksini belirtmedikçe yanıtları (özniteliği ya da yanıtları bölümlerini) Microsoft'a, Bing özel arama API'SİNDEN görüntülenen belirli kullanımınız için yazma değil.


**D. Dosya aktarımı yanıtlar.** İleti bir uygulamanın veya sosyal medya nakil, aşağıdaki durumlardan gibi bir arama API yanıtından başka bir kullanıcı, aktarmak bir kullanıcı etkinleştirirseniz: 
- Aktarılan yanıtları gerekir:
  - Dosya aktarımı kullanıcıya görüntülenen yanıtların içerikten değiştirilmemiş olan içerik oluşur (biçimlendirme değişiklikleri izin verilen);
  - Herhangi bir veri meta verileri formunda içermemesi;
  - Bing Web, görüntü, Haberler ve Video API'leri yanıtlar için Bing ile güçlendirilmiştir bir internet arama deneyimi yanıt belirten görüntüleme dilini edindiğiniz (örneğin, "Gücü tarafından Bing," "Bing bu görüntüye hakkında daha fazla bilgi edinin" veya Bing logosu kullanarak);
  - Bing özel arama API'sinden yanıtlar için görüntüleme dilini yanıt belirten bir internet arama deneyimi (örneğin, "hakkında daha fazla bilgi bu arama sonucu"); alındı
  - Yanıtı oluşturmak için kullanılan tam sorgu öne; ve
  - Belirgin bağlantı veya benzer attribution yanıtının temel alınan kaynak için doğrudan veya arama motoru (aratıp, m.bing.com veya geçerli olarak özel arama hizmetinizi) aracılığıyla içerir.
- Yanıtları aktarımını otomatikleştirmek değil. Bir aktarım açıkça bir yanıt aktarmak için bir hedefi evidencing bir kullanıcı eylemi tarafından başlatılması gerekir.
- Dosya aktarımı kullanıcının sorgusuna yanıt olarak görüntülenen yanıtları aktarmak bir kullanıcı yalnızca sağlayabilir.

**E. Kesintisiz hizmet devamlılığı.** Kopyalama, depolamaz veya arama API yanıtlardan herhangi bir veri önbelleğe alma. Ancak, hizmet erişim ve veri işleme sürekliliği etkinleştirmek için aşağıdaki koşullarda yalnızca sonuçları tutabilir:

**Aygıt.** Tutulan sonuçları yalnızca kullanılabilir olması koşuluyla sorgunun ya da son kullanıcının güncelleştirilmiş sonuçları için başka bir sorgu gönderdiğinde (II) kadar sonuçları bir cihazı için küçük (i) 24 saatlik zaman korumak bir son kullanıcı sağlayabilir:

- daha önce (örneğin, durumunda hizmet kesintisi); bu cihazda, son kullanıcıya döndürülen sonuçları erişmek son kullanıcı etkinleştirmek için veya
- Öngörülü sorgunuz için döndürülen Sonuçların depolanacağı, son kullanıcının sinyaller (örneğin, durumunda beklenen hizmet kesintisi) göre son kullanıcının gereksinimlerinin kapatıldığını kişiselleştirilmiş.

**Sunucu.** Sonuçları belirli denetim ve yalnızca tutulan sonuçları görüntülemek güvenli bir şekilde bir sunucuda tek bir son kullanıcı tutabilir:

- daha önce çözümünüzdeki, o kullanıcı için sonuçları (i) korunur 21 günden fazla bir süre son kullanıcının ilk sorgu zaman koşuluyla değil döndürülen ve (II) yanıt olarak bir son u görüntülenen sonuçların bir geçmiş rapor erişmek son kullanıcı etkinleştirmek için Ser'ın yeni veya yinelenen sorgu; veya
- Öngörülü sorgunuz için döndürülen Sonuçların depolanacağı, son kullanıcının sinyaller (i) 24 saat sorgunun ya da son kullanıcının güncelleştirilmiş sonuçları için başka bir sorgu gönderdiğinde (II) kadar zamandan daha az için temel bir son kullanıcının gereksinimlerinin kapatıldığını kişiselleştirilmiş.

Korunan her sonuçları belirli bir kullanıcı için başka bir kullanıcı için sonuçlarla yani commingled olamaz, her kullanıcı sonuçlarını korunur ve ayrı olarak teslim.

**Genel.** Tutulan sonuçları tüm sunumu için:

- bir sorgu gönderildiği saati Temizle, görünen duyuru, kapsar.
- Kullanıcı bir düğme veya yeniden sorgulamak ve güncelleştirilmiş sonuçları elde etmek için benzer bir şekilde sunmak, 
- sonuçları sunuya markalama Bing korumak ve
- silin (ve gerekirse yeni bir sorgu ile yenileme) belirtilen üretildikten içinde depolanan sonuçları.

**F. Olmayan görüntü URL bulma.** Kullanıcı ya da müşteri bir sorguya yanıt veren bilgi kaynakları URL'lerini keşfetme birinde tek amacı için bir internet olmayan arama deneyimi arama yanıtları yalnızca kullanabilirsiniz. Bir rapor ya da benzeri bir yanıt (i) yalnızca o kullanıcıya sağlamak böyle URL'leri kopyalamak veya önemli ek değerli içerik sorgu ile ilgili müşteri, sorgulayan ve (II), yanıt içerir. Bu kullanım ve görüntü gereksinimleri bölümleri 3a 3E aracılığıyla gereksinimlerini dışında bu görüntü olmayan kullanım için uygulanmaz: 

- Değil önbelleğe, kopyalama veya herhangi bir veriyi depolamak veya içeriği veya türetilmiş, sınırlı URL kopyalanması dışında arama yanıt, daha önce açıklanan;
- (URL'leri dahil) veri arama API'lerden alınan kullanımınız herhangi bir yasaların veya üçüncü taraf haklarını ihlal emin olun; ve
- Tren oluşturun, değerlendirmek ya da siz veya üçüncü tarafların sunabilir hizmetleri geliştirmek için arama API'lerden tüm arama dizini ya da makine öğrenme veya benzer algoritmik etkinlik parçası olarak alınan (URL'leri dahil) verileri kullanmamanız.

## <a name="next-steps"></a>Sonraki adımlar
[Yanıt arama genel bakış](overview.md)
