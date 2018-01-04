---
title: "Azure AD Connect eşitleme: mimarisini anlama | Microsoft Docs"
description: "Bu konuda, Azure AD Connect eşitleme mimarisini açıklar ve kullanılan terimler açıklanmaktadır."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: cc6c772f8f5cc86f8b975ac7835ffff85ef3435c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect eşitleme: mimarisini anlama
Bu konu, Azure AD Connect eşitleme için temel mimarisini kapsar. Birçok yönden öncelleri MIIS 2003, ILM 2007 ve FIM 2010 için benzer. Azure AD Connect eşitleme bu teknolojiler evrimi ' dir. Tüm önceki teknolojiler hakkında bilginiz varsa, bu konu içeriği de size tanıdık gelecektir. Eşitleme için yeniyseniz, bu konu, ilgilidir. Ancak değil (Bu konuda eşitleme altyapısı olarak adlandırılır) Azure AD Connect eşitleme özelleştirmeleri yaparken başarılı olması için bu konunun ayrıntılarını bilmek gereksinimi.

## <a name="architecture"></a>Mimari
Eşitleme altyapısı birden çok bağlı veri kaynaklarında saklanan nesneleri tümleşik bir görünümünü oluşturur ve bu veri kaynaklarında kimlik bilgilerini yönetir. Bu tümleşik görünüm bağlı veri kaynakları ve bu bilgileri işlemek nasıl belirleyen kuralları kümesi alınan kimlik bilgileri tarafından belirlenir.

### <a name="connected-data-sources-and-connectors"></a>Bağlı veri kaynakları ve bağlayıcıları
Eşitleme altyapısı Active Directory veya bir SQL Server veritabanı gibi farklı veri depoları kimlik bilgilerini işler. Veritabanı benzer biçimde verileri düzenler ve standart veri erişim yöntemleri sağlayan her veri deposu, eşitleme altyapısı için olası bir veri kaynağı adaydır. Eşitleme altyapısı tarafından eşitlenen veri depoları adlı **bağlı veri kaynakları** veya **bağlı dizinleri** (CD).

Eşitleme altyapısı adlı bir modül içinde bağlı veri kaynağı ile etkileşim yalıtan bir **bağlayıcı**. Her bağlı veri kaynağı belirli bir bağlayıcı türü. Bağlayıcı, bağlı veri kaynağı anladığı biçime gerekli işlemi çevirir.

Bağlayıcılar, bağlı veri kaynağı ile (hem okuma ve yazma) kimlik bilgilerini değiştirmek için API çağrıları yapma. Genişletilebilir bağlantı çerçevesini kullanarak özel bir bağlayıcı eklemek mümkündür. Aşağıda bir bağlayıcı bağlı veri kaynağı için eşitleme altyapısının nasıl bağlanacağını gösterir.

![Arş1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Veri her iki yönde akmasını sağlamak, ancak her iki yönde de aynı anda geçirilemez. Diğer bir deyişle, bir bağlayıcı veri eşitleme altyapısı bağlı veri kaynağına veya bağlı veri kaynağı için eşitleme altyapısı akış olanak tanımak üzere yapılandırılabilir, ancak bu işlemler yalnızca biri bir nesne ve öznitelik için herhangi bir zamanda meydana gelebilir. Yön farklı öznitelikler ve farklı nesneler için farklı olabilir.

Bir bağlayıcı yapılandırmak için eşitlemek istediğiniz nesne türlerini belirtin. Nesne türlerini belirtme eşitleme işlemine dahil nesnelerin kapsamını tanımlar. Sonraki adım eşitlemek için öznitelikleri seçilecek hangi özniteliği ekleme listesi bilinir. Bu ayarlar, iş kurallarını değişikliklere yanıt dilediğiniz zaman değiştirilebilir. Azure AD Connect Yükleme Sihirbazı'nı kullandığınızda, bu ayarları sizin için yapılandırılır.

Bir bağlı veri kaynağı nesneleri dışarı aktarmak için öznitelik ekleme listesi en az bir bağlı veri kaynağı, belirli nesne türünü oluşturmak için gereken en düşük öznitelikler içermelidir. Örneğin, **sAMAccountName** Active Directory içindeki tüm kullanıcı nesneleri olması gerektiğinden bir kullanıcı nesnesi Active Directory için dışarı aktarmak için öznitelik ekleme listesi özniteliği bulunan bir **sAMAccountName** özniteliği tanımlanmamış. Yükleme Sihirbazı'nı yeniden, bu yapılandırmayı sizin için yapar.

Bağlı veri kaynağı bölümleri veya nesneleri, düzenlemek için kapsayıcıları gibi yapısal bileşenleri kullanıyorsa, bağlı veri kaynağı için belirli bir çözüm kullanılan alanlarda sınırlayabilirsiniz.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Eşitleme altyapısı ad alanının iç yapısı
Tüm eşitleme altyapısı ad alanı kimlik bilgilerini saklamak iki ad alanı oluşur. İki ad alanları şunlardır:

* Bağlayıcı alanı (CS)
* Meta veri deposu (MV)

**Bağlayıcı alanı** belirlenmiş nesneleri gösterimlerini bağlı veri kaynağı ve öznitelik ekleme listesinde belirtilen öznitelikleri içeren bir hazırlama alanının değil. Eşitleme altyapısı bağlı veri kaynağında nelerin değiştiğini belirlemek için ve gelen değişiklikler hazırlamak için bağlayıcı alanı kullanır. Eşitleme altyapısı bağlayıcı alanı giden değişiklikleri vermek bağlı veri kaynağı için hazırlamak için de kullanır. Eşitleme altyapısı ayrı bağlayıcı alanı her bağlayıcı için bir hazırlama alanının tutar.

Hazırlama alanı kullanarak, eşitleme altyapısı bağlı veri kaynaklarını bağımsız olarak kalır ve bunların kullanılabilirlik ve erişilebilirlik tarafından etkilenmez. Sonuç olarak, hazırlama alanına veri kullanarak kimlik bilgileri herhangi bir anda işleyebilir. Eşitleme altyapısı yalnızca bağlı veri kaynağı henüz almadı, kimlik bilgilerini eşitleme altyapısı ve bağlı veri kaynağı arasındaki ağ trafiğini azaltır son iletişim oturumu sona erdi veya yalnızca değişiklikleri anında itibaren bağlı veri kaynağı içinde yapılan değişiklikleri isteyebilir.

Ayrıca, eşitleme altyapısı bağlayıcı alanı aşamaları tüm nesneler hakkındaki durum bilgilerini depolar. Yeni veri alındığında eşitleme altyapısı verileri eşitlenmiş olup olmadığını her zaman değerlendirir.

**Meta veri deposu** tüm birleştirilmiş nesneleri, tek bir genel, tümleşik görünümünü sağlayan birden çok bağlı veri kaynaklarından toplanan kimlik bilgilerini içeren bir depolama alanıdır. Meta veri deposu nesneler, bağlı veri kaynakları ve eşitleme işlemi özelleştirmenizi kurallar kümesi alınan kimlik bilgilerini temel alınarak oluşturulur.

Bağlayıcı alanı ad alanı ve meta veri deposu ad eşitleme altyapısı içinde aşağıda gösterilmektedir.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Eşitleme altyapısı kimlik nesneleri
Eşitleme Altyapısı'ndaki nesne ya da bağlı veri kaynağı nesneleri gösterimlerini veya bu nesnelerin altyapısı eşitleme tümleşik görünüme sahiptir. Her eşitleme altyapısı nesnenin genel benzersiz tanıtıcısı (GUID) sahip olması gerekir. Veri bütünlüğü ve nesneleri arasındaki ilişkiler express GUID'ler sağlar.

### <a name="connector-space-objects"></a>Bağlayıcı alanı nesneleri
Eşitleme altyapısı bağlı veri kaynağı ile iletişim kurduğunda, bağlı veri kaynağının kimlik bilgilerini okur ve kimlik nesne gösterimini bağlayıcı alanı oluşturmak için bu bilgileri kullanır. Oluşturun veya bu nesneleri ayrı ayrı silin. Ancak, bir bağlayıcı alanı içindeki tüm nesneler el ile silebilirsiniz.

Bağlayıcı alanı içindeki tüm nesneler iki özniteliklere sahiptir:

* Bir genel benzersiz tanımlayıcı (GUID)
* Ayırt edici ad (DN olarak da bilinir)

Bağlı veri kaynağı benzersiz bir öznitelik nesnesine atarsa, bağlayıcı alanı nesne ayrıca bir bağlayıcı öznitelik olabilir. Bağlayıcı öznitelik bağlı veri kaynağı bir nesneyi benzersiz olarak tanımlar. Eşitleme altyapısı yer işaretine bağlı veri kaynağında bu nesnenin karşılık gelen gösterimini bulmak için kullanır. Eşitleme altyapısı, bir nesne bağlantı nesne ömrü boyunca hiçbir zaman değiştirir varsayar.

Çoğu bağlayıcılar alındığında otomatik olarak her nesne için bir bağlantı oluşturmak için bilinen benzersiz bir tanımlayıcı kullanın. Örneğin, Active Directory Bağlayıcısı kullanır **objectGUID** bir bağlayıcı özniteliği. Açıkça tanımlanmış benzersiz bir tanımlayıcı sağlamaz, bağlı veri kaynakları için bağlantı oluşturma bağlayıcı yapılandırmasını bir parçası olarak belirtebilirsiniz.

Durumda, bir bağlantı oluşturulur veya ne hangi değişikliklerin ve benzersiz olarak tanımlayan bir nesnenin daha fazla benzersiz öznitelikleri yazın (örneğin, çalışan sayısını veya bir kullanıcı kimliği) bağlayıcı alanı nesne tanımlar.

Bağlayıcı alanı nesne aşağıdakilerden biri olabilir:

* Hazırlama nesnesi
* Bir yer tutucu

### <a name="staging-objects"></a>Hazırlama nesneleri
Hazırlama nesne bağlı veri kaynağından belirlenen nesne türleri örneği temsil eder. GUID ve ayırt edici adı ek olarak, bir hazırlama nesnesi nesne türünü belirten bir değer her zaman vardır.

Her zaman içe hazırlama nesneleri bağlayıcı öznitelik için bir değere sahip. Yeni eşitleme altyapısı tarafından sağlanan ve bağlı veri kaynağı tarafından oluşturulan sürecinde olan hazırlama nesneleri bağlantı özniteliği için bir değer yok.

Hazırlama nesnelerini ayrıca iş özniteliklerinin ve eşitleme altyapısı tarafından eşitleme işlemini gerçekleştirmek için gereken işlem bilgilerini geçerli değerleri uygulayın. Hazırlama nesnesinde hazırlanır güncelleştirme türünü belirten bayrakları işlem bilgilerini içerir. Hazırlama nesne yeni kimlik bilgileri henüz işlenmedi bağlı veri kaynağından aldıysa nesne olarak işaretlenir **içeri aktarma bekleyen**. Hazırlama nesne henüz bağlı veri kaynağına verilemez yeni kimlik bilgileri varsa, olarak işaretlenir **dışa aktarma bekleyen**.

Hazırlama bir nesne, bir içeri aktarma veya dışarı aktarma nesne olabilir. Eşitleme altyapısı, bağlı veri kaynağından alınan nesne bilgileri kullanarak alma nesnesi oluşturur. Eşitleme altyapısı bağlayıcıda seçilen nesne türleri biriyle eşleşen yeni bir nesne varlığı hakkında bilgi aldığında, bağlı veri kaynağı nesnesinin bir temsili olarak bağlayıcı alanı alma nesnesi oluşturur.

Aşağıdaki resimde bağlı veri kaynağındaki bir nesneyi temsil eden alma nesnesi gösterir.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

Eşitleme altyapısı, meta veri deposunda nesne bilgileri kullanarak bir dışarı aktarma nesnesi oluşturur. Dışarı aktarma nesneleri bağlı veri kaynağı için bir sonraki iletişim oturumu sırasında dışarı aktarılır. Eşitleme altyapısı perspektifinden verme nesneler bağlı veri kaynağında henüz yok. Bu nedenle, bağlantı özniteliği verme nesne için kullanılabilir değil. Eşitleme altyapısı, nesne aldıktan sonra bağlı veri kaynağı nesnesinin bağlantı özniteliği için benzersiz bir değer oluşturur.

Aşağıdaki çizimde bir dışarı aktarma nesne meta veri deposunda kimlik bilgilerini kullanarak nasıl oluşturulacağını gösterir.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Eşitleme altyapısı, bağlı veri kaynağı nesnesinden yeniden içe aktarılması nesne verme onaylar. Eşitleme altyapısı bunları bu bağlı veri kaynağından İleri alma sırasında aldığında verme nesneleri içeri aktarma nesneler olur.

### <a name="placeholders"></a>Yer tutucuları
Eşitleme altyapısı düz bir ad alanı, nesneleri depolamak için kullanır. Ancak, Active Directory gibi bazı bağlı veri kaynağı hiyerarşik bir ad kullanın. Hiyerarşik ad bilgilerinden düz bir ad alanına dönüştürmek için hiyerarşi korumak için yer tutucuları eşitleme altyapısı kullanır.

Her bir yer tutucu eşitleme motoruna aktarılmadı ancak hiyerarşik adı oluşturmak için gerekli olan bir nesnenin hiyerarşik adı bir bileşeninin (örneğin, bir kuruluş birimi) temsil eder. Bunlar, bağlayıcı alanı nesne hazırlama olmayan nesnelere bağlı veri kaynağındaki başvurular tarafından oluşturulan boşlukları doldurun.

Eşitleme altyapısı yer tutucuları değil henüz içeri aktarıldığını başvurulan nesneleri depolamak için de kullanır. Eşitleme Yöneticisi özniteliğini eklemek için yapılandırılmışsa, örneğin, *Abbie Spencer* nesne ve alınan değeri olan henüz gibi alındı değil bir nesne *CN Lee Sperry, CN = kullanıcılar, DC = fabrikam, DC = com =*, yönetici bilgilerini bağlayıcı alanı yer tutucu olarak depolanır. Yöneticisi nesnesi daha sonra aldıysanız, yer tutucu Nesne Yöneticisi'ni temsil eden hazırlama nesnesi tarafından üzerine yazılır.

### <a name="metaverse-objects"></a>Meta veri deposu nesneleri
Bir meta veri deposu nesnesi toplanmış görünümü içerir bağlayıcı alanı hazırlama nesnelerinin bu eşitleme altyapısı vardır. Eşitleme altyapısı alma nesneleri bilgileri kullanarak meta veri deposu nesnesi oluşturur. Bir tek meta veri deposu nesnesine birkaç bağlayıcı alanı nesne bağlanabilir, ancak bir bağlayıcı alanı nesne için birden fazla meta veri deposu nesnesi bağlanamaz.

Meta veri deposu nesneleri el ile oluşturulan veya silinemez. Eşitleme altyapısı herhangi bir bağlayıcı alanı nesne bağlantısını bağlayıcı alanı yok meta veri deposu nesneleri otomatik olarak siler.

Meta veri deposu içinde karşılık gelen bir nesne türü için bir bağlı veri kaynağı içindeki nesneleri eşleştirmek için nesne türleri ve ilişkili öznitelikleri önceden tanımlanmış bir dizi genişletilebilir bir şemayı eşitleme altyapısı sağlar. Yeni nesne türleri ve meta veri nesnelerinin öznitelikleri oluşturabilirsiniz. Tek değerli veya birden çok değerli öznitelikleri olabilir ve öznitelik türlerini dizeler, başvurular, sayıları ve Boole değerleri olabilir.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Hazırlama ve meta veri deposu nesneleri arasındaki ilişkiler
Eşitleme altyapısı ad alanı içinde veri akışı hazırlama ve meta veri deposu nesneleri arasındaki bağlantı ilişki etkinleştirilir. Bir meta veri deposu nesnesine bağlı hazırlama bir nesne olarak adlandırılan bir **birleştirilmiş nesne** (veya **bağlayıcı nesnesi**). Bir meta veri deposu nesnesine bağlı olmayan bir hazırlama nesnesi olarak adlandırılan bir **nesne üyeliğinden** (veya **ayırıcı nesne**). Birleştirilmiş ve üyeliğinden koşullarını içeri aktarma ve bağlı bir dizinden verileri dışarı aktarma sorumlu Bağlayıcılarla karıştırır değil tercih ettiği.

Yer tutucuları hiçbir zaman bir meta veri deposu nesnesine bağlıdır

Birleştirilmiş bir nesne, bir hazırlama nesnesi ve tek meta veri deposu nesnesine bağlı ilişkisini oluşur. Birleştirilmiş nesnelerin öznitelik değerleri bağlayıcı alanı nesne bir meta veri deposu nesnesi arasındaki eşitlemek için kullanılır.

Hazırlama nesne eşitleme sırasında birleştirilmiş bir nesne olduğunda öznitelikleri hazırlama nesnesi ve meta veri deposu nesnesi arasında akabilir. Öznitelik akışı yönlüdür ve içeri aktarma özniteliği kuralları ve dışarı aktarma özniteliği kuralları kullanılarak yapılandırılır.

Bir tek bağlayıcı alanı nesne yalnızca bir meta veri deposu nesnesine bağlanabilir. Bununla birlikte, her meta veri deposu nesnesi aşağıdaki çizimde gösterildiği gibi birden çok bağlayıcı alanı nesne aynı veya farklı bağlayıcı alanları bağlanabilir.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Hazırlama nesnesi ve meta veri deposu nesnesi arasındaki bağlı ilişki kalıcıdır ve belirlediğiniz kurallara göre kaldırılabilir.

Bağlantısı kesilmiş bir nesne herhangi bir meta veri deposu nesnesine bağlı olmayan bir hazırlama nesnesidir. Bağlantısı kesilmiş bir nesnenin değerlerini olmayan özniteliği herhangi bir ek meta veri içindeki işlendi. Bağlı veri kaynağına karşılık gelen nesne öznitelik değerlerini eşitleme altyapısı tarafından güncelleştirilmez.

Bağlantısı kesilmiş nesneler kullanarak, eşitleme altyapısında kimlik bilgilerini depolamak ve daha sonra işleme. Bağlayıcı alanı bağlantısı kesilmiş bir nesne olarak hazırlama nesnenin koruyarak birçok avantaj sunar. Sistem zaten bu nesnenin hakkında gerekli bilgiyi hazırlanan olduğundan, bağlı veri kaynağı'ndan bu nesne bir gösterimini İleri alma işlemi sırasında yeniden oluşturmak gerekli değildir. Bağlı veri kaynağı geçerli bağlantı olsa bile bu şekilde, her zaman bağlı veri kaynağının tam bir anlık görüntü eşitleme altyapısı vardır. Bağlantısı kesilmiş nesneleri birleştirilmiş nesnelere ve bunun tersi de, belirlediğiniz kurallara bağlı olarak dönüştürülebilir.

Alma nesnesi, bağlantısı kesilmiş bir nesne olarak oluşturulur. Bir verme nesne birleştirilmiş bir nesne olmalıdır. Sistem mantığı kuralın zorunlu kılan ve birleştirilmiş bir nesne değil her dışarı aktarma nesneyi siler.

## <a name="sync-engine-identity-management-process"></a>Eşitleme altyapısı Kimlik Yönetimi işlemi
Kimlik Yönetimi işlem kimlik bilgilerini farklı bağlı veri kaynakları arasında nasıl güncelleştirileceğini denetler. Kimlik Yönetimi üç işlemde oluşur:

* İçeri Aktarma
* Eşitleme
* Dışarı Aktarma

İçeri aktarma işlemi sırasında eşitleme altyapısı bağlı veri kaynağından gelen kimlik bilgileri değerlendirir. Değişiklikler algıladığında, bu yeni hazırlama nesneler oluşturur ya da eşitleme için bağlayıcı alanı hazırlama varolan nesneleri güncelleştirir.

Eşitleme işlemi sırasında eşitleme altyapısı bağlayıcı alanı oluşan değişiklikleri yansıtacak şekilde meta veri güncelleştirmeleri ve meta veri deposunda oluşan değişiklikleri yansıtmak için bağlayıcı alanı güncelleştirir.

Dışa aktarma işlemi sırasında eşitleme altyapısı nesneleri hazırlama hazırlanır ve dışarı aktarma gibi bekleyen işaretlenen değişiklikleri çıkışı iter.

Her işlem kimlik bilgileri akışları bir bağlı veri kaynağından diğerine oluştuğu aşağıdaki şekilde gösterilmiştir.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>İçeri aktarma işlemi
İçeri aktarma işlemi sırasında eşitleme altyapısı kimlik bilgileri güncelleştirmelerini değerlendirir. Eşitleme altyapısı hazırlama nesneyle ilgili kimlik bilgileriyle bağlı veri kaynağından alınan kimlik bilgilerini karşılaştırır ve hazırlama nesne güncelleştirmeleri gerekli olup olmadığını belirler. Yeni verileri hazırlama nesneyi güncelleştirmek gerekliyse, hazırlama nesne bekleyen alma olarak işaretlenir.

Eşitleme altyapısı eşitleme önce bağlayıcı alanı nesne hazırlama göre değişen kimlik bilgileri işleyebilir. Bu işlem, aşağıdaki avantajları sunar:

* **Verimli eşitleme**. Eşitleme sırasında işlenen veri miktarını en aza indirilir.
* **Verimli yeniden eşitleme**. Eşitleme altyapısının eşitleme altyapısı veri kaynağına yeniden bağlanmayı olmadan kimlik bilgilerini nasıl işlediği değiştirebilirsiniz.
* **Eşitleme önizlemek için Fırsat**. Kimlik Yönetimi işlem hakkında varsayımlar doğru olduğunu doğrulamak için eşitleme önizleyebilirsiniz.

Bağlayıcıda belirtilen her bir nesne için eşitleme altyapısı bağlayıcı bağlayıcı alanı nesne gösterimini bulmak önce çalışır. Eşitleme altyapısı bağlayıcı alanı hazırlama tüm nesneleri inceler ve eşleşen bir bağlantı özniteliğine sahip bir karşılık gelen hazırlama nesnesi bulmaya çalışır. Varolan hazırlama nesnesi yok eşleşen bir bağlayıcı öznitelik varsa, eşitleme altyapısı aynı ayırt edici adına sahip bir karşılık gelen hazırlama nesnesi bulmaya çalışır.

Eşitleme altyapısı yer işaretine göre değil ancak ayırt edici adı ile eşleşen hazırlama bir nesne bulduğunda, aşağıdaki özel davranış oluşur:

* Bağlayıcı alanı bulunan nesnesi sahip hiçbir bağlantı sonra eşitleme altyapısı bu nesneyi bağlayıcı alanından kaldırır ve bu bağlantılı olarak meta veri deposu nesne işaretler **sonraki eşitlemeyi Çalıştır sağlama yeniden**. Daha sonra yeni alma nesnesi oluşturur.
* Bağlayıcı alanı bulunan nesnesi bir bağlantı varsa, eşitleme altyapısı bu nesne ya da yeniden adlandırılmış veya silinmiş bağlı dizininde olduğunu varsayar. Böylece gelen nesne yerleştirebilirsiniz geçici, yeni bir ayırt edici adı için bağlayıcı alanı nesne atar. Eski nesnesi daha sonra olur **geçici**, bekleyen yeniden adlandırma veya sorunu çözmek için silme içeri aktarmak bağlayıcı.

Eşitleme altyapısı bağlayıcıda belirtilen nesne karşılık gelen hazırlama bir nesne bulur değişiklikleri uygulamak için ne tür belirler. Örneğin, eşitleme altyapısı yeniden adlandırın veya bağlı veri kaynağı nesnesinde silin veya nesnenin öznitelik değerleri yalnızca güncelleştirebilir.

Güncelleştirilen verileri hazırlama nesneleriyle bekleyen alma olarak işaretlenir. İçeri aktarmalar bekleyen farklı türleri kullanılabilir. İçeri aktarma işleminin sonucunu bağlı olarak, bağlayıcı alanı hazırlama nesnesinde içe aktarma türleri bekleyen aşağıdakilerden birini sahiptir:

* **None**. Hiçbir değişiklik hazırlama nesne özniteliklerinden herhangi biri için kullanılabilir. Eşitleme altyapısı, bu tür olarak içeri aktarma bekleyen işaretlemez.
* **Ekleme**. Bağlayıcı alanı içinde yeni alma nesne hazırlama nesnesidir. Eşitleme altyapısı bu tür alma gibi ek meta veri işleme için bekleyen işaretler.
* **Güncelleştirme**. Eşitleme altyapısı karşılık gelen hazırlama nesnesini bağlayıcı alanı bulur ve böylece güncelleştirmeleri öznitelikleri için meta veri deposunda işlenebilir içeri aktarma bekleyen bu türü olarak işaretler. Nesnesini yeniden adlandırma güncelleştirmeleri içerir.
* **Silme**. Eşitleme altyapısı karşılık gelen hazırlama nesnesini bağlayıcı alanı bulur ve böylece birleştirilmiş nesne silinebilir içeri aktarma bekleyen bu türü olarak işaretler.
* **Silme ve ekleme**. Eşitleme altyapısı karşılık gelen hazırlama nesnesini bağlayıcı alanı bulur, ancak nesne türleri eşleşmiyor. Bu durumda, bir değişiklik hazırlanan delete ekleme. Bir silme-add-değiştirme, nesne türünü değiştiğinde farklı kural kümesini bu nesne için geçerli olduğundan, bu nesnenin tam bir eşitleme gerçekleşmelidir eşitleme altyapısı gösterir.

Hazırlama bir nesnenin bekleyen alma durumu ayarlayarak, önemli ölçüde bunun nedenle veri güncelleyen nesneleri işlemek sistem verdiğinden eşitleme sırasında işlenen veri miktarını azaltmak mümkündür.

### <a name="synchronization-process"></a>Eşitleme işlemi
Eşitleme iki ilgili işlemden oluşur:

* Meta veri içeriğini bağlayıcı alanı verileri kullanarak güncelleştirildiğinde eşitleme, gelen.
* Bağlayıcı alanı içerik meta veri deposunda verileri kullanarak güncelleştirildiğinde giden eşitleme.

Bağlayıcı alanı hazırlanan bilgileri kullanarak, gelen eşitleme işlemi meta veri deposunda bağlı veri kaynaklarında depolanan verilerin tümleşik görünümünü oluşturur. Kuralların nasıl yapılandırıldığına bağlı olarak tüm hazırlama nesneleri veya yalnızca bir bekleyen alma bilgileri toplanır.

Meta veri deposu nesnelerini değiştirdiğinizde giden eşitleme işlemi güncelleştirmeleri nesnelerini dışa aktarın.

Gelen eşitleme, bağlı veri kaynaklarından alınan kimlik bilgilerini meta dizesinde tümleşik görünümü oluşturur. Eşitleme altyapısı, bağlı veri kaynağından olan son kimlik bilgilerini kullanarak kimlik bilgileri herhangi bir anda işleyebilir.

**Gelen eşitleme**

Gelen eşitleme aşağıdaki işlemleri kapsar:

* **Sağlama** (olarak da bilinir **projeksiyon** giden Eşitleme sağlama bu işlem ayırt etmek önemli olup olmadığını). Eşitleme altyapısı hazırlama nesnesini temel alan yeni bir meta veri deposu nesnesi oluşturur ve bunları bağlar. Sağlama nesne düzeyinde bir işlemdir.
* **Katılma**. Eşitleme altyapısı hazırlama nesne var olan bir meta veri deposu nesnesine bağlar. Bir birleştirme nesne düzeyinde bir işlemdir.
* **Öznitelik akışı alma**. Eşitleme altyapısı meta veri deposu nesnesinin öznitelik akışı adlı öznitelik değerlerini güncelleştirir. İçeri aktarma öznitelik akışı hazırlama nesnesi ve bir meta veri deposu nesnesi arasında bir bağlantı gerektiren bir öznitelik düzeyi işlemdir.

Sağlama meta veri deposunda nesneleri oluşturan yalnızca işlemidir. Sağlama, bağlantısı kesilmiş nesneleri içeri aktarma nesneleri etkiler. Sağlama sırasında eşitleme altyapısı alma nesnesi nesne türüne karşılık gelen ve her iki nesneleri, böylece birleştirilmiş bir nesne oluşturma arasında bir bağlantı kurar bir meta veri deposu nesnesi oluşturur.

Katılma işlemi de nesneleri içeri aktar ve meta veri deposu nesnesi arasında bir bağlantı kurar. Katılma ve sağlama arasındaki farkı birleştirme işlemi alma nesnesi sağlama işlemini yeni bir meta veri deposu nesnesi oluşturur bir var olan meta veri deposu nesne için bağlanılır gerektirmesidir.

Eşitleme altyapısı, eşitleme kuralı yapılandırmasında belirtilen ölçütleri kullanarak bir meta veri deposu nesnesine alma nesnesi katmayı dener.

Sağlama ve birleştirme işlemleri sırasında eşitleme altyapısı bağlantısı kesilmiş bir nesne onları birleştirilmiş bir meta veri deposu nesnesine bağlar. Bu nesne düzeyinde işlemler tamamlandıktan sonra eşitleme altyapısı ilişkili meta veri deposu nesnesi öznitelik değerlerini güncelleştirebilirsiniz. Bu işlem, içeri aktarma öznitelik akışı olarak adlandırılır.

Yeni veri taşıyan ve bir meta veri deposu nesnesine bağlı tüm alma nesneler üzerinde içeri aktarma öznitelik akışı gerçekleşir.

**Giden eşitleme**

Bir meta veri deposu nesnesi değiştirmek ancak silinmez giden eşitleme güncelleştirmeleri nesnelerini dışa aktarın. Giden eşitleme hedefi meta veri deposu nesnelerdeki değişiklikleri bağlayıcı alanları hazırlama nesneleri için güncelleştirme gerekip gerekmediğini değerlendirmek için kullanılır. Bazı durumlarda, hazırlama değişiklikleri gerektiren tüm bağlayıcı alanları nesneleri güncelleştirilemiyor. Değiştirilen hazırlama nesneleri, onları nesneleri dışarı verme gibi bekleyen işaretlenir. Nesneleri daha sonra gönderilen bağlı veri kaynağına verme işlemi sırasında bu dışarı aktarma.

Giden eşitleme üç işlem vardır:

* **Sağlama**
* **Sağlama kaldırma**
* **Öznitelik akışı dışarı aktarma**

Sağlama ve sağlamayı kaldırma her iki nesne düzeyinde işlemleridir. Sağlamayı kaldırmayı yalnızca sağlama işlemi başlatabilirsiniz çünkü sağlama üzerinde bağlıdır. Sağlama bir meta veri deposu nesnesi ve bir verme nesnesi arasındaki bağlantıyı kaldırdığında sağlamayı tetiklenir.

Meta veri nesneleri için değişiklik uygulandığında sağlama her zaman tetiklenir. Meta veri deposu nesnelere yapılan değişiklikler, eşitleme altyapısı hazırlama işleminin bir parçası aşağıdaki görevlerden birini gerçekleştirebilirsiniz:

* Bir meta veri deposu nesnesi yeni oluşturulan dışa aktarma nesnesine burada bağlı katılan nesnelerini oluşturun.
* Birleştirilmiş bir nesne yeniden adlandırın.
* Bir meta veri deposu nesnesi ve nesneleri, bağlantısı kesilmiş bir nesne oluşturma hazırlama arasındaki bağlantıları çıkarın.

Sağlama yeni bir bağlayıcı nesnesi oluşturmak için eşitleme altyapısı gerektiriyorsa, nesne bağlı veri kaynağında henüz var olmadığından meta veri deposu nesnesinin bağlı olduğu hazırlama her zaman bir dışarı aktarma nesne nesnesidir.

Sağlama eşitleme altyapısı bağlantısı kesilmiş bir nesne oluşturma birleştirilmiş bir nesne çıkarın gerektiriyorsa sağlamayı tetiklenir. Sağlamanın kaldırılması işlemini nesneyi siler.

Sağlama kaldırma sırasında bir dışa aktarma nesne silme fiziksel nesne silmez. Nesne olarak işaretlenen **silinmiş**, silme işlemi nesnede hazırlanır anlamına gelir.

Dışa aktarma öznitelik akışı da benzer şekilde alma öznitelik akışı gelen eşitleme sırasında oluşur giden eşitleme işlemi sırasında oluşur. Dışarı aktarma öznitelik akışı katılan yalnızca meta veri deposu ve dışarı aktarma nesneler arasında oluşur.

### <a name="export-process"></a>Dışa aktarma işlemi
Dışa aktarma işlemi sırasında eşitleme altyapısı bekleyen bağlayıcı alanı içinde verme olarak işaretlenir ve ardından güncelleştirmeleri bağlı veri kaynağına gönderir. tüm verme nesneleri inceler.

Eşitleme altyapısı verme başarısını belirleyebilir ancak yeteri kadar Kimlik Yönetimi işlemi tamamlanır belirleyemiyor. Bağlı veri kaynağı nesneleri her zaman başka işlemler tarafından değiştirilebilir. Eşitleme altyapısı bağlı veri kaynağına kalıcı bir bağlantı olmadığından, yalnızca başarılı aktarma bildirim dayalı bağlı veri kaynağı içindeki bir nesnenin özelliklerini ilgili haline getirmek yeterli değil.

Örneğin, bağlı veri kaynağı bir işlemde özgün değerlerine nesnenin özniteliklerini değiştirebilirsiniz (hemen veri eşitleme altyapısı tarafından gönderilen ve başarılı bir şekilde bağlı veri kaynağında uyguladıktan sonra başka bir deyişle, bağlı veri kaynağı değerlerin üzerine).

Eşitleme altyapısı depoları verebilir ve hazırlama her nesne hakkındaki durum bilgilerini alabilirsiniz. Öznitelik ekleme listesinde belirtilen özniteliklerinin değerlerini son verme itibaren değişip değişmediğini depolanmasını içeri ve uygun şekilde tepki vermek için durum etkinleştirir eşitleme altyapısı dışarı aktarma. Eşitleme altyapısı içeri aktarma işlemi bağlı veri kaynağına dışarı öznitelik değerleri onaylamak için kullanır. İçeri ve dışarı aktarılan bilgiler arasında bir karşılaştırma eşitleme altyapısı dışarı aktarma başarılı oldu veya yinelenmesi gerekip gerekmediğini belirlemek aşağıdaki çizimde gösterildiği gibi sağlar.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Örneğin, 5, bağlı veri kaynağına değerinde, öznitelik C, eşitleme altyapısı dışarı verirse verme durumu belleğinde C = 5 depoladığı. Bu nesne sonuçları (diğer bir deyişle, farklı bir değer son bağlı veri kaynağından alınan sürece) bu değer kalıcı olarak nesneye uygulandığını değil eşitleme altyapısı varsayar çünkü C = 5 bağlı veri kaynağına yeniden dışarı aktarın girişimi her ek dışarı aktarma. C = 5, nesne üzerinde bir içeri aktarma işlemi sırasında alındığında verme bellek temizlenir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.

