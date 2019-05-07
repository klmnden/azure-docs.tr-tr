---
title: 'Azure AD Connect eşitleme: Azure Mimarisi - anlama'
description: Bu konuda, Azure AD Connect eşitleme mimarisini açıklar ve kullanılan terimler açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fac0f9143918d3f273812e53abfb88d6a56f7a71
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138594"
---
# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect eşitleme: Mimariyi anlama
Bu konu temel mimarisini Azure AD Connect eşitlemesi ele alınmaktadır. Birçok yönden öncelleri MIIS 2003 ILM 2007 ve FIM 2010 için benzerlik gösterir. Azure AD Connect eşitleme, çünkü bu teknolojiler olur. Tüm önceki teknolojiler hakkında bilginiz varsa, bu konunun içeriği de size tanıdık gelecektir. Eşitleme için yeni başladıysanız, bu konu, hakkındadır. Ancak böyle değildir (Bu konudaki eşitleme altyapısı olarak bilinir) Azure AD Connect eşitlemesini özelleştirme yaparken başarılı olması için bu konunun ayrıntılarını öğrenmek için bir gereksinim.

## <a name="architecture"></a>Mimari
Eşitleme altyapısı, birden fazla bağlı veri kaynakları içinde depolanan nesneleri tümleşik bir görünümünü oluşturur ve bu veri kaynaklarında kimlik bilgilerini yönetir. Tümleşik bu görünüm, bağlı veri kaynakları ve bu bilgileri işlemek nasıl belirleyen kuralları kümesi alınan kimlik bilgileri tarafından belirlenir.

### <a name="connected-data-sources-and-connectors"></a>Bağlı veri kaynakları ve bağlayıcılar
Eşitleme altyapısı, Active Directory veya bir SQL Server veritabanı gibi farklı veri depolarından kimlik bilgilerini işler. Veritabanına benzer biçimde, verileri düzenler ve standart veri erişim yöntemlerine sağlayan her veri deposu, eşitleme altyapısı için olası bir veri kaynağı adaydır. Eşitleme altyapısı tarafından eşitlenen veri depoları olarak adlandırılan **bağlı veri kaynakları** veya **bağlı dizinleri** (CD).

Eşitleme altyapısı adlı bir modül içinde bağlı bir veri kaynağı ile etkileşimi kapsülleyen bir **bağlayıcı**. Her bağlı veri kaynağı türü, belirli bir bağlayıcı vardır. Bağlayıcı, gerekli bir işlemin bağlı veri kaynağı anlayan biçimine çevirir.

Bağlayıcılar, bağlı veri kaynağı ile (hem okuma ve yazma) kimlik bilgilerini değiştirmek için API çağrıları yapabilir. Genişletilebilir bağlantı çerçevesini kullanarak özel bir bağlayıcı eklemek mümkündür. Aşağıdaki çizimde, bir bağlayıcı bağlı bir veri kaynağı için eşitleme altyapısının nasıl bağlandığını gösterir.

![Arş1](./media/concept-azure-ad-connect-sync-architecture/arch1.png)

Herhangi bir yönde veri akışı, ancak her iki yönde de aynı anda geçirilemez. Diğer bir deyişle, veri eşitleme altyapısı bağlı veri kaynağına veya bağlı veri kaynağı eşitleme altyapısında akış olanak tanımak için bir bağlayıcı yapılandırılabilir, ancak bu işlemler yalnızca biri için bir nesne ve öznitelik herhangi bir zamanda meydana gelebilir. Yönü, farklı nesneler için ve farklı öznitelikler için farklı olabilir.

Bir bağlayıcı yapılandırmak için eşitlemek istediğiniz nesne türlerini belirtin. Nesne türlerini belirtme eşitleme işlemine dahil nesnelerin kapsamını tanımlar. Sonraki adım eşitlemek için öznitelikleri, öznitelik ekleme listesi bilinir seçmektir. Bu ayarlar, iş kurallarınızı değişikliklere yanıt vermek istediğiniz zaman değiştirilebilir. Azure AD Connect Yükleme Sihirbazı'nı kullandığınızda, bu ayarlar sizin için yapılandırılır.

Bağlı veri kaynağına nesneleri dışarı aktarmak için öznitelik ekleme listesi en az bir bağlantılı veri kaynağındaki belirli nesne türünü oluşturmak için gereken en düşük öznitelikler içermesi gerekir. Örneğin, **sAMAccountName** özniteliği tüm kullanıcı nesnelerini Active Directory'de olması gerektiğinden, bir kullanıcı nesnesi Active Directory'ye aktarma öznitelik ekleme listesine eklenmesi gereken bir **sAMAccountName** özniteliği tanımlanmamış. Yeniden Yükleme Sihirbazı'nı bu yapılandırmayı sizin için yapar.

Bağlı veri kaynağı bölümler veya kapsayıcılar nesneleri, düzenlemek için gibi yapısal bileşenler kullanıyorsa, bağlı veri kaynağındaki belirli bir çözüm için kullanılan alanları sınırlayabilirsiniz.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Eşitleme altyapısı ad alanı iç yapısı
Kimlik bilgilerini saklamak iki ad alanı tüm eşitleme altyapısı ad alanı oluşur. İki ad alanları şunlardır:

* Bağlayıcı alanı (CS)
* Meta veri deposu (MV)

**Bağlayıcı alanına** belirlenmiş nesneleri temsillerini bağlı bir veri kaynağı ve öznitelik ekleme listesinde belirtilen öznitelikleri içeren bir hazırlama alanı. Eşitleme altyapısı, bağlı veri kaynağında nelerin değiştiğini belirlemek ve gelen değişiklikler aşama için bağlayıcı alanında kullanır. Eşitleme altyapısı bağlayıcı alanında dışarı aktarma bağlı veri kaynağına giden değişiklikleri hazırlamak için de kullanır. Eşitleme altyapısı, her bağlayıcı için bir hazırlama alanı olarak ayrı bağlayıcı alanına tutar.

Hazırlama alanı kullanarak, eşitleme altyapısı bağlı veri kaynakları bağımsız olarak kalır ve, kullanılabilirlik ve erişilebilirlik tarafından etkilenmez. Sonuç olarak, veri hazırlama alanını kullanarak kimlik bilgilerini dilediğiniz zaman işleyebilir. Eşitleme altyapısı, yalnızca son iletişim oturumu sona erdi veya yalnızca değişiklikleri anında iletme ağ azaltan, bağlı veri kaynağı henüz almadı, kimlik bilgileri için içinde bağlı veri kaynağına yapılan değişiklikleri isteyebilir Eşitleme altyapısı ve bağlı veri kaynağı arasındaki trafiği.

Ayrıca, eşitleme altyapısı ve bağlayıcı alanında buna aşamaları tüm nesneler hakkında durum bilgileri depolar. Yeni veri alındığında, eşitleme altyapısı verileri önceden eşitlenmiş olup olmadığını her zaman değerlendirilir.

**Meta veri deposu** tüm birleştirilmiş nesnelerinin tek bir genel, tümleşik görünüm birden fazla bağlı veri kaynaklarından toplanan kimlik bilgilerini içeren bir depolama alanıdır. Meta veri deposu nesne eşitleme işlemi özelleştirmenize izin veren kuralları kümesi ve bağlı veri kaynakları arasında alınan kimlik bilgilerini temel alınarak oluşturulur.

Bağlayıcı alanı ad alanı ve meta veri deposu ad alanı eşitleme altyapısı içinde aşağıda gösterilmiştir.

![Arch2](./media/concept-azure-ad-connect-sync-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Eşitleme altyapısı kimlik nesneleri
Eşitleme altyapısı nesneleri ya da bağlı veri kaynağı nesneleri temsillerini ya da bu nesneler eşitleme altyapısı tümleşik görünümü vardır. Her eşitleme altyapısı nesne bir genel benzersiz tanıtıcısı (GUID) sahip olmalıdır. Veri bütünlüğü ve nesneler arasındaki express ilişkileri Guıd'lerinizi sağlayın.

### <a name="connector-space-objects"></a>Bağlayıcı alanı nesne
Eşitleme altyapısı bağlı veri kaynağı ile iletişim kurduğunda, bağlı veri kaynağı kimlik bilgileri okuyan ve bu bilgileri kimlik nesnesinin bir temsili bağlayıcı alanında oluşturmak için kullanır. Oluşturamaz veya bu nesneleri ayrı ayrı silin. Ancak, el ile bir bağlayıcı alanında tüm nesneleri silebilirsiniz.

Tüm nesneleri bağlayıcı alanında iki özniteliklere sahiptir:

* Bir genel benzersiz tanıtıcısı (GUID)
* Bir ayırt edici ad (DN olarak da bilinir)

Bağlı veri kaynağı benzersiz bir öznitelik nesnesine atarsa, nesneleri bağlayıcı alanında da bir bağlantı özniteliği olabilir. Bağlantı özniteliği, bağlı veri kaynağı bir nesneyi benzersiz olarak tanımlar. Eşitleme altyapısı, bağlı veri kaynağına karşılık gelen bu nesnenin gösterimini bulmak için yer işareti kullanır. Eşitleme altyapısı, bir nesnenin bağlantı nesne yaşam süresi boyunca hiçbir zaman değişiklik varsayar.

Çoğu bağlayıcılar, bilinen bir benzersiz tanımlayıcı alındığında otomatik olarak her nesne için bir bağlantı oluşturmak için kullanın. Örneğin, Active Directory Bağlayıcısı kullanan **objectGUID** bir bağlayıcı özniteliği. Açıkça tanımlanmış bir benzersiz tanımlayıcı sağlamaz, bağlı veri kaynakları için bağlantı oluşturma bağlayıcı yapılandırmasını bir parçası olarak belirtebilirsiniz.

Durumda, bir bağlantı oluşturulmuştur veya ne hangi değişikliklerin ve benzersiz biçimde tanımlayan bir nesnenin daha fazla benzersiz öznitelik yazın (örneğin, çalışan sayısını veya bir kullanıcı kimliği) bağlayıcı alanında nesneyi tanımlar.

Bağlayıcı alanı nesne aşağıdakilerden biri olabilir:

* Bir hazırlama nesnesi
* Bir yer tutucu

### <a name="staging-objects"></a>Hazırlama nesneleri
Bir hazırlama nesnesi, bağlı veri kaynağından bir örneğini belirtilen nesne türlerini temsil eder. GUID ve ayırt edici adı ek olarak, bir hazırlama nesnesi, nesne türünü gösteren bir değer her zaman vardır.

Her zaman içe hazırlama nesneleri çapa özniteliği için bir değere sahip. Yeni eşitleme altyapısı tarafından sağlanan ve bağlı veri kaynağında oluşturulan sürecinde olan hazırlama nesneleri çapa özniteliği için bir değer yok.

Hazırlama nesneleri ayrıca iş özniteliklerinin ve işletimsel bilgi eşitleme altyapısı tarafından eşitleme işlemini gerçekleştirmek için gereken geçerli değerleri uygulayın. Operasyonel bilgi hazırlama nesnesinde aşamalı bir güncelleştirme türünü belirten bir bayrak içerir. Bir hazırlama nesnesi henüz işlenmemiş bağlı veri kaynağından yeni kimlik bilgileri aldıysa nesne olarak işaretlenmiş **içeri aktarma bekleyen**. Bir hazırlama nesnesi henüz bağlı veri kaynağına verilemez yeni kimlik bilgisi yoksa, olarak işaretlenmiş **bekleyen dışa aktarım**.

Bir hazırlama nesnesi, bir içeri aktarma veya dışarı aktarma nesne olabilir. Eşitleme altyapısı, bağlı veri kaynağından alınan nesne bilgileri kullanarak içeri aktarma nesnesi oluşturur. Eşitleme altyapısı varlığı hakkında bilgi bağlayıcısında seçili nesne türlerinin herhangi biriyle eşleşen yeni bir nesnenin aldığında, bağlı veri kaynağı nesnesinin bir temsili olarak bağlayıcı alanında bir içeri aktarma nesnesi oluşturur.

Aşağıdaki resimde, bağlı veri kaynağındaki bir nesneyi temsil eden bir içeri aktarma nesnesi gösterilmektedir.

![Arch3](./media/concept-azure-ad-connect-sync-architecture/arch3.png)

Eşitleme altyapısı, meta veri deposunda nesne bilgileri kullanarak bir dışarı aktarma nesnesini oluşturur. Nesneleri dışa aktar sonraki iletişim oturumu sırasında bağlı veri kaynağına aktarılır. Eşitleme altyapısı açısından bakıldığında, dışarı aktarma nesneler bağlı veri kaynağında henüz yok. Bu nedenle, bağlantı özniteliği bir dışarı aktarma nesne için kullanılabilir değildir. Eşitleme altyapısı nesneyi aldıktan sonra bağlı veri kaynağı nesnesinin çapa özniteliği için benzersiz bir değer oluşturur.

Aşağıdaki çizim, meta veri deposunda kimlik bilgilerini kullanarak dışarı aktarma nesnesini nasıl oluşturulduğunu gösterir.

![Arch4](./media/concept-azure-ad-connect-sync-architecture/arch4.png)

Eşitleme altyapısı dışarı aktarma nesnesinin nesne bağlı veri kaynağından yeniden içe aktarılması onaylar. Bunları, bağlı veri kaynağından İleri alma işlemi sırasında aldığında eşitleme altyapısı, nesne içeri aktarma nesneleri dışarı aktarın.

### <a name="placeholders"></a>Yer tutucuları
Eşitleme altyapısı düz bir ad alanı, nesneleri depolamak için kullanır. Ancak, bazı bağlı veri kaynakları Active Directory gibi bir hiyerarşik ad alanı kullanın. Düz bir ad alanı hiyerarşik ad alanı bilgilerinden dönüştürmek için hiyerarşi korumak için yer tutucular eşitleme altyapısını kullanır.

Her bir yer tutucu eşitleme motoruna içeri aktarılmayan ancak hiyerarşik adı oluşturmak için gerekli olan bir nesnenin hiyerarşik ad bir bileşeninin (örneğin, bir kuruluş birimi) temsil eder. Bunlar, bağlı veri kaynağı içindeki nesneleri bağlayıcı alanında hazırlama değil nesnelere olan başvurular tarafından oluşturulan boşluk doldurun.

Eşitleme altyapısı yer tutucuları değil henüz aktarıldığından başvurulan nesneleri depolamak için de kullanır. Örneğin, eşitleme için yönetici özniteliği içerecek şekilde yapılandırıldıysa *Abbie Spencer* nesne ve alınan değeri olduğundan henüz gibi içeri aktarılan ayarlanmamış bir nesne *CN Lee Sperry, CN = kullanıcılar, DC = fabrikam, DC = com* , yönetici bilgilerini bağlayıcı alanında yer tutucu olarak depolanır. Yöneticisi nesnesi daha sonra içeri aktarılırsa, yer tutucu nesne yöneticisini temsil eder hazırlama nesnesi tarafından üzerine yazılır.

### <a name="metaverse-objects"></a>Meta veri deposu nesneleri
Bir meta veri deposu nesnesi toplu görünüm içeriyor bağlayıcı alanında hazırlama nesnelerin, eşitleme altyapısı vardır. Eşitleme altyapısı, nesneleri içeri aktarma bilgileri kullanarak meta veri deposu nesnesi oluşturur. Tek bir meta veri deposu nesnesine birkaç bağlayıcı alanı nesne bağlanabilir, ancak birden fazla meta veri deposu nesne için bir bağlayıcı alanı nesnesi bağlı olamaz.

Meta veri deposu nesne el ile oluşturulan veya silinemez. Eşitleme altyapısı, herhangi bir bağlayıcı alanı nesne için bir bağlantı ve bağlayıcı alanında buna sahip olmayan meta veri deposu nesne otomatik olarak siler.

Meta veri deposu içinde karşılık gelen bir nesne türüne bağlı bir veri kaynağı içindeki nesneleri eşleştirmek için genişletilebilir bir şema nesne türleri ve ilişkili öznitelikleri önceden tanımlanmış bir dizisiyle eşitleme altyapısı sağlar. Yeni nesne türleri ve meta veri nesnelerinin öznitelikleri oluşturabilirsiniz. Tek değerli ya da birden çok değerli öznitelikleri olabilir ve öznitelik türlerini dizeleri, başvurular, sayılar ve Boole değerleri olabilir.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Hazırlama nesneleri ve meta veri deposu nesneleri arasındaki ilişkiler
Eşitleme altyapısı ad alanı içinde veri akışı hazırlama nesneleri ve meta veri deposu nesne arasındaki bağlantıyı ilişki etkinleştirilir. Bir meta veri deposu nesnesine bağlı bir hazırlama nesnesi olarak adlandırılan bir **birleştirilmiş nesne** (veya **bağlayıcı nesnesini**). Bir meta veri deposu nesnesine bağlı olmayan bir hazırlama nesnesi olarak adlandırılan bir **nesne üyeliğinden** (veya **ayırıcı nesnesi**). Birleştirilmiş ve üyeliğinden koşulları bağlı bir dizinden verileri dışarı aktarma ve içeri aktarma sorumlu Bağlayıcılarla karıştırmamaya tercih edilen.

Yer tutucuları hiçbir zaman bir meta veri deposu nesnesine bağlı

Birleştirilmiş bir nesne, bir hazırlama nesnesi ve tek bir meta veri deposu nesnesine bağlı ilişkisini oluşur. Katılan nesnelerini, öznitelik değerleri arasında bir bağlayıcı alanı nesnesi ve bir meta veri deposu nesnesi eşitlemek için kullanılır.

Bir hazırlama nesnesi birleştirilmiş bir nesne eşitleme sırasında hale geldiğinde, öznitelikleri hazırlama nesnesi ve meta veri deposu nesnesi arasında akabilir. Öznitelik akışı yönlüdür ve içeri aktarma öznitelik kuralları ve dışarı aktarma öznitelik kuralları kullanılarak yapılandırılır.

Tek bağlayıcı alanı nesnesi için yalnızca bir meta veri deposu nesne bağlanabilir. Ancak, her meta veri deposu nesne aşağıdaki çizimde gösterildiği gibi birden çok bağlayıcı alanı nesne aynı veya farklı bir bağlayıcı alanları bağlanabilir.

![Arch5](./media/concept-azure-ad-connect-sync-architecture/arch5.png)

Bağlı ilişki hazırlama nesnesi ve bir meta veri deposu nesnesi arasında kalıcıdır ve belirlediğiniz kurallara göre kaldırılabilir.

Herhangi bir meta veri deposu nesnesine bağlı olmayan bir hazırlama nesnesi bir yarıçap nesnesidir. Bağlantısı kesilmiş bir nesne değerleri değil öznitelik meta veri deposu içinde daha fazla herhangi işlendi. Bağlı veri kaynağına karşılık gelen nesnenin öznitelik değerleri, eşitleme altyapısı tarafından güncelleştirilmez.

Ayrılmış nesneleri kullanarak, eşitleme altyapısında kimlik bilgilerini depolamak ve daha sonra işlem. Bağlayıcı alanı bağlantısı kesilmiş bir nesne olarak bir hazırlama nesnesi tutma birçok avantaj sunar. Sistem, bu nesne hakkında gerekli bilgileri zaten hazırlamıştır olduğundan, bağlı veri kaynağından bu nesne bir temsilini İleri alma işlemi sırasında yeniden oluşturmak gerekli değildir. Bağlı veri kaynağı için geçerli bağlantı olsa bile bu şekilde, her zaman bağlı veri kaynağının tam bir anlık görüntü eşitleme altyapısı sahiptir. Ayrılmış nesnelerin birleştirilmiş nesnelere ve tersi, belirlediğiniz kurallara bağlı olarak dönüştürülebilir.

İçeri aktarma nesnesi, bağlantısı kesilmiş bir nesne olarak oluşturulur. Bir dışarı aktarma nesnesini birleştirilmiş bir nesne olmalıdır. Sistem mantığı, bu kural uygular ve birleştirilmiş bir nesne değil her dışarı aktarma nesnesini siler.

## <a name="sync-engine-identity-management-process"></a>Eşitleme altyapısı Kimlik Yönetimi işlemi
Kimlik Yönetimi işlemi, kimlik bilgilerini farklı bağlı veri kaynakları arasında nasıl güncelleştirileceğini denetler. Kimlik Yönetimi, üç işlemde oluşur:

* İçeri Aktarma
* Eşitleme
* Dışarı Aktarma

İçeri aktarma işlemi sırasında bağlı veri kaynağından gelen kimlik bilgilerini eşitleme altyapısı değerlendirir. Değişiklik algılandığında, hazırlama yeni nesneler oluşturur veya eşitleme için bağlayıcı alanında mevcut hazırlama nesneleri güncelleştirir.

Eşitleme işlemi sırasında eşitleme altyapısı ve bağlayıcı alanında buna gerçekleşen değişiklikleri yansıtacak şekilde meta veri güncelleştirmeleri ve bağlayıcı alanında, meta veri deposunda ortaya çıkan değişiklikleri yansıtacak şekilde güncelleştirir.

Dışarı aktarma işlemi sırasında eşitleme altyapısı, nesneleri hazırlama üzerinde hazırlanır ve bekleyen dışa aktarım olarak işaretlenmiş değişiklikleri iter.

Her işlem kimlik bilgileri akışı bağlı veri kaynağından diğerine oluştuğu aşağıdaki şekilde gösterilmiştir.

![Arch6](./media/concept-azure-ad-connect-sync-architecture/arch6.png)

### <a name="import-process"></a>İçeri aktarma işlemi
İçeri aktarma işlemi sırasında eşitleme Altyapısı güncelleştirmeleri kimlik bilgileri için değerlendirir. Eşitleme altyapısı, bir hazırlama nesnesi hakkında kimlik bilgileri bağlı veri kaynağından alınan kimlik bilgilerini karşılaştırır ve hazırlama nesnesi güncelleştirmeleri gerekli olup olmadığını belirler. Hazırlama nesnesi yeni veriler ile güncelleştirmek gerekliyse, hazırlama nesnesi olarak içeri aktarma bekleyen işaretlenir.

Eşitleme altyapısının eşitleme önce bağlayıcı alanında nesneleri tarafından hazırlama, değişen kimlik bilgileri işleyebilir. Bu işlem, aşağıdaki avantajları sunar:

* **Verimli eşitleme**. Eşitleme sırasında işlenen veri miktarı en aza indirilir.
* **Verimli yeniden eşitleme**. Eşitleme altyapısının eşitleme altyapısı veri kaynağına yeniden bağlanmayı olmadan kimlik bilgileri nasıl işlediğini değiştirebilirsiniz.
* **Eşitleme önizlemek için bir fırsat**. Eşitleme, Kimlik Yönetimi işlemi ile ilgili varsayım doğru olduğunu doğrulamak için önizleyebilirsiniz.

Bağlayıcısında belirtilen her bir nesne için eşitleme altyapısı ilk nesnenin gösterimini Bağlayıcısı'nın bağlayıcı alanında dener. Eşitleme altyapısı, bağlayıcı alanında tüm hazırlama nesneleri inceler ve eşleşen bir bağlantı özniteliği olan bir karşılık gelen hazırlama nesnesi bulmaya çalışır. Mevcut bir hazırlama nesnesi yok eşleşen bir bağlantı özniteliği varsa, eşitleme altyapısı, aynı ayırt edici adını karşılık gelen hazırlama nesne bulmayı dener.

Eşitleme altyapısı tarafından ayırt edici adı ancak bağlantı eşleşen bir hazırlama nesnesi bulduğunda, aşağıdaki özel durum oluşur:

* Bağlayıcı alanında bulunan hiçbir bağlantı nenesindeki sonra eşitleme altyapısı bağlayıcı alanının bu nesneyi kaldırır ve bağlı olduğu için olarak meta veri deposu nesne işaretler **sağlama çalıştırmak, sonraki eşitlemede yeniden**. Ardından yeni içeri aktarma nesnesi oluşturur.
* Nesnesi bağlayıcı alanında bulunan bir bağlantı varsa, eşitleme altyapısı bu nesne ya da yeniden adlandırılmış veya bağlantılı dizinde silinmiş olduğunu varsayar. Gelen nesne hazırlayabilirsiniz, böylece geçici, yeni bir ayırt edici ad için bağlayıcı alanı nesnesi atar. Ardından eski nesne haline gelir **geçici**, bekleyen yeniden adlandırma veya silme sorunu çözmek için içeri aktarmak bağlayıcı.

Eşitleme altyapısı bağlayıcısında belirtilen nesneye karşılık gelen bir hazırlama nesnesi bulur, değişiklikleri uygulamak için ne tür belirler. Örneğin, eşitleme altyapısı yeniden adlandırmak veya bağlantılı veri kaynağındaki nesnesini silme veya nesnenin öznitelik değerleri yalnızca güncelleştirebilir.

Güncelleştirilen verilerle hazırlama nesneleri içeri aktarma gibi bekleyen işaretlenir. İçeri aktarmalar bekleyen farklı türlerde kullanılabilir. İçeri aktarma işleminin sonucunu bağlı olarak, bir hazırlama nesnesi bağlayıcı alanında aşağıdakilerden birini içeri aktarma türü bekleyen sahiptir:

* **None**. Herhangi bir hazırlama nesnesi özniteliklerini değişiklik yapmadan kullanılabilir. Eşitleme altyapısı, bu tür olarak içeri aktarma bekleyen işaretlemez.
* **Ekleme**. Hazırlama nesnesi, bağlayıcı alanında yeni bir içeri aktarma nesnesidir. Eşitleme altyapısı, bu tür alma gibi ek meta veri işleme için bekleyen işaretler.
* **Güncelleştirme**. Eşitleme altyapısı ve bağlayıcı alanında buna karşılık gelen bir hazırlama nesnesi bulur ve böylece güncelleştirmeleri öznitelikleri için meta veri deposunda işlenebilir bu türü olarak içeri aktarma bekleyen işaretler. Nesneyi yeniden adlandırma güncelleştirmeleri içerir.
* **Silme**. Eşitleme altyapısı ve bağlayıcı alanında buna karşılık gelen bir hazırlama nesnesi bulur ve böylece birleştirilmiş nesnesi silinebilir, bu türü olarak içeri aktarma bekleyen işaretler.
* **Silme ve ekleme**. Eşitleme altyapısı ve bağlayıcı alanında buna karşılık gelen bir hazırlama nesnesi bulur, ancak nesne türleri eşleşmiyor. Bu durumda, bir değişiklik aşamalı silme ekleme. Bir silme-ekleme değiştirme, nesne türünü değiştiğinde farklı kural kümesi bu nesne için geçerli olduğundan, bu nesnenin tam bir eşitleme gerçekleşmelidir eşitleme altyapısında gösterir.

Bir hazırlama nesnesi bekleyen içeri aktarma durumu ayarlayarak, eşitleme sırasında bunu yapmanız bu nedenle, güncelleştirilmiş verileri nesneleri işlemek sistem izin verdiğinden işlenen veri miktarı ciddi ölçüde düşürün mümkündür.

### <a name="synchronization-process"></a>Eşitleme işlemi
Eşitleme iki ilgili işlemden oluşur:

* Meta veri içeriğini ve bağlayıcı alanında buna verileri kullanarak güncelleştirildiğinde eşitleme, gelen.
* Meta veri deposunda verileri kullanarak bağlayıcı alanında içeriği güncelleştirildiğinde giden eşitleme.

Gelen eşitleme işlemi bağlayıcı alanında hazırlanır bilgileri kullanarak, meta veri deposunda bağlı veri kaynakları, depolanan verilerin tümleşik görünümünü oluşturur. Kuralların nasıl yapılandırıldığına bağlı olarak tüm hazırlama nesneleri veya yalnızca bir bekleyen alma bilgileri toplanır.

Meta veri deposu nesneleri değiştirdiğinizde güncelleştirmelerini giden eşitleme işlemi, nesneleri dışarı aktarın.

Gelen eşitleme meta veri olan bağlı veri kaynaklarından alınan kimlik bilgileri tümleşik görünümü oluşturur. Eşitleme altyapısı, bağlı veri kaynağından bulunan en son kimlik bilgilerini kullanarak kimlik bilgilerini dilediğiniz zaman işleyebilir.

**Gelen eşitleme**

Gelen eşitleme, aşağıdaki işlemler bulunur:

* **Sağlama** (olarak da adlandırılan **projeksiyon** bu işlem, giden eşitleme sağlamasından ayırt etmek önemli olup olmadığını). Eşitleme altyapısı, bir hazırlama nesnesini temel alan yeni bir meta veri deposu nesnesi oluşturur ve bunları bağlar. Sağlama nesne düzeyinde bir işlemdir.
* **Birleştirme**. Eşitleme altyapısı, bir hazırlama nesnesi mevcut bir meta veri deposu nesnesine bağlar. Bir birleştirme nesne düzeyinde bir işlemdir.
* **İçeri aktarma öznitelik akışı**. Eşitleme altyapısı öznitelik akışı, meta veri deposu nesnesinin adlı öznitelik değerlerini güncelleştirir. İçeri aktarma öznitelik akışı bir hazırlama nesnesi ve bir meta veri deposu nesnesi arasında bir bağlantı gerektiren bir öznitelik düzeyi işlemdir.

Sağlama meta veri deposunda nesneleri oluşturan yalnızca işlemidir. Sağlama, bağlantısı kesilmiş bir nesne olan nesneleri içeri aktar etkiler. Sağlama sırasında eşitleme altyapısı, her iki nesneleri, bu nedenle birleştirilmiş bir nesne oluşturma arasında bir bağlantı kurar ve içeri aktarma nesnesi nesne türüne karşılık gelen bir meta veri deposu nesnesi oluşturur.

Katılma işlemi de nesneleri içeri aktar ve bir meta veri deposu nesnesi arasında bir bağlantı kurar. Katılma ve sağlama arasındaki farkı katılma işlemi içeri aktarma nesnesi bağlı olan, burada sağlama işlemi yeni bir meta veri deposu nesnesi oluşturur bir var olan meta veri deposu nesne için gerektirmesidir.

Eşitleme altyapısı, bir içeri aktarma nesnesi, eşitleme kuralı yapılandırmasında belirtilen ölçütleri kullanarak bir meta veri deposu nesnesine katmayı dener.

Sağlama ve birleştirme işlemleri sırasında eşitleme altyapısı bağlantısı kesilmiş bir nesne katılmış yönetilmelerini bir meta veri deposu nesnesine bağlar. Bu nesne düzeyinde işlemler tamamlandıktan sonra eşitleme altyapısı öznitelik değerlerinin ilişkili meta veri deposu nesnesinin güncelleştirebilirsiniz. Bu işlem, içeri aktarma öznitelik Akış adı verilir.

Yeni veri taşıyan ve herhangi bir meta veri deposu nesnesine bağlı tüm içeri aktarma nesneler üzerinde içeri aktarma öznitelik akışı gerçekleşir.

**Giden eşitleme**

Giden eşitleme güncelleştirmeleri bir meta veri deposu nesnesi değiştirebilirsiniz ancak silinmez, nesneleri dışarı aktarın. Giden eşitleme amacı, meta veri deposu nesnelerde yapılan değişiklikler bağlayıcı alanları hazırlama nesnelerine güncelleştirmeleri gerekip gerekmediğini değerlendirin sağlamaktır. Bazı durumlarda, söz konusu hazırlama değişiklikler gerektirebilir tüm bağlayıcı alanları nesneleri güncelleştirilemiyor. Değiştirilen hazırlama nesneler, nesneleri dışarı aktarın yönetilmelerini dışarı aktarma gibi bekleyen işaretlenir. Nesne daha sonra itilir bağlı veri kaynağına dışarı aktarma işlemi sırasında bu dışarı aktarma.

Giden eşitleme üç işlem sahiptir:

* **Sağlama**
* **Sağlamayı kaldırma**
* **Dışarı aktarma öznitelik akışı**

Sağlama ve sağlamayı kaldırma her iki nesne düzeyinde işlemlerdir. Sağlama kaldırmayı yalnızca sağlamayı başlatamadık çünkü sağlama üzerinde bağlıdır. Meta veri deposu nesnesi ve bir verme nesnesi arasındaki bağlantı sağlama kaldırdığında, sağlamayı kaldırma tetiklenir.

Sağlama, her zaman değişiklikleri meta veri nesnelere uygulandığında tetiklenir. Meta veri deposu nesneleri için değişiklik yapıldığında, eşitleme altyapısı hazırlama işleminin bir parçası olarak aşağıdaki görevlerden herhangi birini gerçekleştirebilirsiniz:

* Burada bir meta veri deposu nesnesi bir yeni oluşturulan dışa aktarma nesnesiyle bağlantılı birleştirilmiş nesneleri oluşturma.
* Birleştirilmiş bir nesneyi yeniden adlandırma.
* Bir meta veri deposu nesnesi ve nesneleri, bağlantısı kesilmiş bir nesne oluşturma hazırlama arasındaki bağlantıları çıkma.

Yeni bir bağlayıcı nesnesi oluşturmak için eşitleme altyapısı sağlama gerektiriyorsa, nesne bağlı veri kaynağında henüz var olmadığından meta veri deposu nesne bağlandığı hazırlama her zaman bir dışarı aktarma nesnesini nesnedir.

Sağlama bağlantısı kesilmiş bir nesne oluşturma, birleştirilmiş bir nesne çıkma için eşitleme altyapısı gerektiriyorsa sağlamayı kaldırma tetiklenir. İşlemini nesneyi siler.

Sağlamayı kaldırma sırasında bir dışarı aktarma nesnesini silme fiziksel olarak nesne silmez. Nesne olarak işaretlenmiş **silinmiş**, silme işlemi nesnede hazırlanılır anlamına gelir.

Dışarı aktarma öznitelik akışında da benzer bir biçimde içeri aktarma öznitelik akışı gelen eşitleme sırasında gerçekleşir giden eşitleme işlemi sırasında oluşur. Dışarı aktarma öznitelik akışında katılan yalnızca meta veri deposu ve dışarı aktarma nesneler arasında gerçekleşir.

### <a name="export-process"></a>Dışa aktarma işlemi
Dışarı aktarma işlemi sırasında eşitleme altyapısı, bekleyen bağlayıcı alanında verme olarak işaretlenir ve ardından güncelleştirmeleri bağlı veri kaynağına gönderir. tüm nesneleri dışa aktar inceler.

Eşitleme altyapısı verme başarısını belirleyebilirsiniz ancak kimlik yönetimi işleminin tamamlandığından emin yeterince belirlenemiyor. Bağlı veri kaynağı nesneleri her zaman diğer işlemler tarafından değiştirilebilir. Eşitleme altyapısı bağlı veri kaynağına kalıcı bir bağlantı olmadığından, bir başarılı dışarı aktarma bildirimi yalnızca temel bağlı veri kaynağındaki bir nesnenin özellikleri hakkında varsayımlar yapmak yeterli değil.

Örneğin, bağlı veri kaynağı bir işlemde özgün değerlerine nesnenin özniteliklerini değiştirebilir (veri eşitleme altyapısı tarafından gönderilen ve diğer başarıyla uygulanan hemen sonra başka bir deyişle, bağlı veri kaynağı değerlerin üzerine bağlı veri kaynağı).

Eşitleme altyapısı depoları, dışarı aktarma ve hazırlama her nesne hakkındaki durum bilgilerini alın. Öznitelik ekleme listesinde belirtilen öznitelik değerleri son aktarma işlemi bu yana değişmişse depolanmasını almak ve uygun şekilde tepki vermek için durum etkinleştirir eşitleme altyapısı dışarı aktarın. Eşitleme altyapısı öznitelik değerleri, bağlı veri kaynağına dışarı onaylamak için içeri aktarma işlemi kullanır. İçeri ve dışarı aktarılan bilgileri arasında bir karşılaştırma, eşitleme altyapısı dışarı aktarma başarılı oldu veya yinelenmesi gerekip gerekmediğini belirlemek aşağıdaki çizimde gösterildiği gibi sağlar.

![Arch7](./media/concept-azure-ad-connect-sync-architecture/arch7.png)

Özniteliği C 5 değerini, bağlı veri kaynağına sahip, eşitleme altyapısı dışarı aktarır, örneğin, C = 5, dışarı aktarma durumu bellekte depolar. Bu nesne üzerindeki her ek verme, eşitleme altyapısı, bu değer sınıflandırmanıza nesneye uygulanmadığını varsayar çünkü C = 5 bağlı veri kaynağına yeniden dışarı aktarma denemesi sonuçlanıyor (diğer bir deyişle, farklı bir değer son içe aktarıldığı sürece bağlı veri kaynağı). Dışarı aktarma bellek, C = 5, nesne üzerinde bir alma işlemi sırasında alındığında temizlenir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

