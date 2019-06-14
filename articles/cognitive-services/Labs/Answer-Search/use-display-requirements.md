---
title: Gereksinimleri - proje yanıt arama görüntülemek ve kullanın
titlesuffix: Azure Cognitive Services
description: Proje yanıt arama uç noktası için gereksinimlerini görüntülemek ve kullanın.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 085cb20e4dad92ed55b5ba0914c677aa50f3ac97
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60345762"
---
# <a name="project-answer-search-use-and-display-requirements"></a>Proje yanıt arama kullanın ve gereksinimleri görüntüleme

Kullanım ve görüntü gereksinimleri için ilgili bilgiler ve içerik herhangi bir uygulama gibi ilişkileri, meta verileri ve diğer sinyaller Bing Bilgi Bankası araması, Bing özel arama, varlık araması, resim arama yapılan çağrılar aracılığıyla kullanılabilir uygulamak, Haber araması, Video arama, görsel arama, Web araması, yazım denetimi ve otomatik öneri API'leri. Uygulama Ayrıntıları bu gereksinimleri için ilgili belirli özellikleri ve sonuçlar için belgelerinde bulunabilir.

## <a name="1-bing-spell-check-and-bing-autosuggest-api"></a>1. Bing yazım denetimi ve Bing otomatik öneri API'sini.

Yapma:

- kopyalama, saklamak veya Bing yazım denetimi veya Bing otomatik öneri API'leri aldığınız herhangi bir veri önbelleği
- Bing yazım denetimi veya Bing otomatik öneri API'leri herhangi bir makine öğrenimi veya eğitmek, değerlendirmek ya da siz veya üçüncü taraflara sunabilir yeni veya var olan hizmetleri geliştirmek için benzer algoritmik etkinliği bir parçası olarak almaya verileri kullanın.

## <a name="2-definitions"></a>2. Tanımlar

- "yanıt" bir kategoriye ait bir yanıtta döndürülen sonuçların ifade eder. Örneğin, Bing Web araması API'si yanıtı Web sonuçları, resim, video ve haber kategorileri içinde yanıtlar içerebilir.
- "yanıt" tüm yanıtlar anlamına gelir ve ilişkili verileri tek bir arama API'sine çağrıda yanıt alınıp;
- "sonuç" yanıt bilgilerinin bir öğeyi gösterir. Örneğin, bir tek haber makaleyle bağlantılı veri kümesini haber yanıtında bir sonucudur.
- "Arama API'leri", toplu olarak, Bing özel arama, varlık araması, resim arama, haber arama, Video arama, görsel arama ve Web arama API'leri anlamına gelir. 


## <a name="3-search-apis"></a>3. Arama API'leri

Bu bölüm 3 gereksinimleri arama API'leri için geçerlidir.

**BİR. Internet arama deneyimi.** Yanıtları döndürülen tüm veriler, yalnızca internet arama deneyimleri kullanılabilir. Bir internet arama deneyimi görüntülenen, uygun içerik anlamına gelir: 
- Son kullanıcının doğrudan sorgu ya da kullanıcının arama faiz ve hedefi (örneğin, kullanıcı belirtilen arama sorgusu); diğer bir gösterge hızlı yanıt ve ilgili 
- Kullanıcıların bulmasına ve veri kaynağına gidin yardımcı olur (örneğin, içeriği veya attribution conspicuously verilerle görüntülenen tıklatılabilir bir bağlantı, bu nedenle sağlanan URL'leri köprü olarak uygulanır); veya, Bing varlık arama API'si varsa, bing.com adresindeki dizinlerde ilgili sorgu için arama sonuçlarını gitmek kullanıcının sağlayan yanıttaki sağlanan bing.com URL'sine görünüşte bağlantısını;
- birden çok sonuç seçmek son kullanıcı içerir (örneğin, birkaç sonuçlardan haber yanıt görüntülenir veya birkaç'den az ise tüm sonuçlar döndürülür); 
- Arama (küçük resimleri küçük resim-kullanıcının ekran derlemekten boyutlandırılır Örneğin, görüntü); amaca uygun bir miktarı ile sınırlıdır 
- içeriğin Internet arama sonuçları (içeriğin "web" olduğu gibi bir deyim); olduğunu son kullanıcıya görünür bir göstergesi içerir ve
- herhangi bir arama API'lerinden alınan veriler kullanımınız uygulanabilir yasa ve (örneğin geçerli lisans ile uyumlu bir Creative Commons lisansı bağlı değilse, üçüncü taraf haklarını ihlal etmemesini sağlamak uygun önlemleri birleşimini içerir Koşulları). Ölçüler ne olabilir, yasal danışmanlar uygun belgelere bakın.
Yalnızca internet arama Deneyimi gereksinimi için URL'yi bulma bölümünde 3E (olmayan görüntüleme URL'si bulma) aşağıda açıklandığı gibi istisnadır. 

**B. Kısıtlamaları.** Yapma:

- kopyalama, saklamak veya tüm veriler (hariç tutma "Hizmet sürekliliği" bölümünde aşağıdaki izin verilen azami ölçüde); yanıtları önbelleğe alma 
- eğitmek, değerlendirmek ya da siz veya üçüncü taraflara sunabilir yeni veya var olan hizmetleri geliştirmek için herhangi bir makine öğrenimi veya benzer algoritmik etkinlik kapsamında arama API'lerinden alınan veriler kullanın.
- sonuçları (diğer bir gereksinimi ihlal etmemesini şekilde yeniden biçimlendirmek üzere daha diğer) içeriğini değiştirme, yasaların gerektirdiği durumlar ya da Microsoft tarafından; kabul ediyorum 
- Attribution ve sonuç içerikle ilişkili URL'leri atlayın;
- atlama, sipariş veya derecelendirme sağlandığında bir yanıtında gösterilen sonuçları dahil olmak üzere yeniden sıralama, (Bing özel arama API'si için bu kural customsearch.ai Portalı aracılığıyla uygulanan yeniden sıralama için geçerli değildir), yasaların gerektirdiği durumlar ya da Microsoft tarafından anlaşılan sürece ;
- içinde bir yanıt herhangi bir parçasını diğer içerik diğer içerik yanıtın bir parçası olduğunu düşünüyorsanız son kullanıcının sonuçlanabilecek bir biçimde görüntüler; 
- bir yanıt herhangi bir bölümünü görüntüleyen herhangi bir sayfa üzerinde Microsoft tarafından sağlanmayan reklam görüntüleme; -Bing görüntü, haber veya videoyu arama API'leri; gelen yanıtları (i) ile herhangi bir reklam görüntüleme veya (ii), filtre veya birincil (veya yalnızca) sınırlı görüntü, haber ve/veya videoyu sonuçlar.

**C. Bildirimler ve markalama.** 

- Göze çarpacak şekilde işlevsel köprü adresinde Microsoft gizlilik bildirimi dahil https://go.microsoft.com/fwlink/?LinkId=521839 , neredeyse her noktasında bir kullanıcı bir arama sorgusu giriş imkanı kullanıcı deneyimi (UX). Köprü "Microsoft Privacy Statement" etiket.
- Bing, adreste yönergeleri ile tutarlı marka öne https://go.microsoft.com/fwlink/?linkid=833278 , neredeyse her noktasında kullanıcı Deneyimini bir kullanıcı bir arama sorgusu giriş olanağı sunar.  Böyle bir marka açıkça kullanıcıya Microsoft internet arama deneyimini güçlendiren olduğunu belirtmek gerekir.
- Her yanıt (veya bir yanıt bölümü) açıklandığı gibi Microsoft Bing Web, resim, Haberler ve Video API'lerini görüntülenen özniteliği https://go.microsoft.com/fwlink/?linkid=833278 sürece Microsoft, aksi halde, kullanımınız için yazma belirtir. 
- Microsoft aksini belirtmedikçe yanıtları (özniteliği ya da yanıtları bölümlerini), Microsoft Bing özel arama API'si görüntülenen belirli kullanımınız için yazma değil.


**D. Yanıtları aktarılıyor.** Bir Mesajlaşma uygulaması veya sosyal medya posta, aşağıdaki durumlardan gibi başka bir kullanıcı, arama API'si yanıt aktarmak için bir kullanıcı devre dışı bırakırsanız: 
- Aktarılan yanıtları gerekir:
  - Aktarma kullanıcıya görüntülenen yanıtların içeriğinden değiştirilmemiş olan içerik oluşur (biçimlendirme değişikliklerini izin verilen);
  - Meta veri biçiminde eklemediğinizden;
  - Bing Web, resim, Haberler ve Video API'lerini yanıtlar için Bing tarafından desteklenen bir internet arama deneyimini yanıt belirten görüntüleme dilini edindiğiniz (örneğin, "Desteklenen tarafından Bing," "Bu görüntüye Bing hakkında daha fazla bilgi edinin" veya Bing logosu kullanarak);
  - Bing özel arama API'si yanıtlar almak için bir internet arama deneyimi (örneğin, "hakkında daha fazla bilgi bu arama sonucunda"); görüntüleme dilini gösteren yanıt alındı
  - Yanıtı oluşturmak için kullanılan tam sorgu öne; ve
  - Tanınmış bir bağlantı veya benzer attribution yanıtının temel alınan kaynağa doğrudan veya bir arama motoru (bing.com, m.bing.com veya özel arama hizmetinizi uygunsa) üzerinden içerir.
- Yanıtları aktarımını otomatik değildir. Aktarım NET bir şekilde yanıt aktarmak için bir hedefi evidencing bir kullanıcı eylemi tarafından başlatılmalıdır.
- Aktarma kullanıcının sorgusuna yanıt olarak görüntülenen yanıtları aktarmak için bir kullanıcı yalnızca sağlayabilir.

**E. Kesintisiz hizmet devamlılığı.** Kopyalamayın, depolamak veya tüm veriler arama API yanıtları önbelleğe alma. Ancak, sürekliliği ve veri işleme hizmeti erişimi etkinleştirmek için sonuçları yalnızca aşağıdaki koşullarda Koru:

**Cihaz.** Tutulan sonuçlar yalnızca kullanılabilir olması koşuluyla, sonuçları bir aygıtta saati (i) 24 saatten daha az için sorgu veya (ii) son kullanıcı için güncelleştirilmiş sonuçları başka bir sorgu gönderdiğinde kadar korumak bir son kullanıcı sağlayabilir:

- daha önce bu cihazda (örneğin, durumunda hizmet kesintisini); bu son kullanıcıya döndürülen sonuç erişmek son kullanıcı etkinleştirmek için veya
- Son kullanıcının gereksinimlerini (örneğin, durumunda beklenen hizmet kesintisi), son kullanıcının sinyallere dayalı olasılığına kişiselleştirilmiş, proaktif sorgu için döndürülen sonuçlarını depolamak için.

**Sunucu.** Sonuçları belirli denetim ve tutulan sonuçları görüntülemek güvenli bir şekilde bir sunucuda tek bir son kullanıcı Koru:

- daha önce bu kullanıcıya, çözümünüzdeki sonuçları (i) korunur 21 günden fazla bir süre zamanından sonra son kullanıcının ilk sorgu koşuluyla değil döndürülen ve (ii) yanıt u sona olarak görüntülenen sonuçların geçmiş bir rapora erişmek son kullanıcı etkinleştirmek için Yeni veya yinelenen sorgu Kullanı; veya
- Proaktif sorgunuz için döndürülen sonuçlarını depolamak için bir son kullanıcının gereksinimlerini, son kullanıcının sinyalleri için sorgu veya (ii) son kullanıcı için güncelleştirilmiş sonuçları başka bir sorgu gönderdiğinde kadar zaman (i) 24 saatten daha az göre olasılığına kişiselleştirilmiş.

Korunan her sonuçları belirli bir kullanıcı için sonuçları başka bir kullanıcının, yani commingled olamaz, her kullanıcı sonuçlarını korunur ve ayrı olarak teslim.

**Genel.** Tutulan sonuçları tüm sunumu için:

- Sorgunun gönderildiği zaman açık, görünür bir bildirim, kapsar.
- Kullanıcı bir düğme veya yeniden sorgular ve güncelleştirilmiş sonuçları elde etmek için benzer yol varsa 
- sonuçları sunuda marka Bing korumak ve
- silin (ve gerekirse yeni bir sorgu ile yenileme) belirtilen zaman dilimlerine içindeki depolanmış sonuç.

**F. Olmayan görüntü URL'si bulma.** Yalnızca, kullanıcı ya da müşteri için bir sorgu duyarlı bilgi kaynakları URL'lerini keşfetme uygulamalarınızdaki için bir internet olmayan arama deneyimini araması yanıtlarında kullanabilirsiniz. Bir raporda veya (ii) yalnızca bu kullanıcı için sağladığınız benzer yanıt böyle URL'leri kopyalayabilir veya müşteri, yanıt olarak, sorgu ve (ii), önemli ek değerli sorgu ile ilgili içeriği. Bu kullanım ve görüntü gereksinimleri bölümleri 3a 3E aracılığıyla gereksinimlerini bu görüntü olmayan kullanımı dışında geçerli değildir: 

- Değil önbellek, kopyalama veya herhangi bir veriyi depolamak veya içeriği veya türetilmiş yanıtından, URL sınırlı kopyalama dışındaki arama, daha önce açıklanan;
- Arama API'lerinden alınan (URL'leri dahil) veri kullanımını herhangi bir uygulanabilir yasalar veya üçüncü taraf haklarını ihlal etmemesini sağlamak; ve
- Train oluşturma, değerlendirmek veya siz veya üçüncü taraflara sunabilir hizmetlerini geliştirmek için herhangi bir arama dizini veya makine öğrenimi veya benzer algoritmik etkinlik kapsamında arama API'lerinden alınan (URL'leri dahil) veri kullanmamanız.

## <a name="next-steps"></a>Sonraki adımlar
[Yanıt arama genel bakış](overview.md)
