---
title: Azure IOT kenar güvenlik | Microsoft Docs
description: Güvenlik, kimlik doğrulama ve yetkilendirme IOT kenar aygıtların
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f198efe9ff5e4862a3bbe872ab50e5848c9dbb5c
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030589"
---
# <a name="securing-azure-iot-edge"></a>Azure IOT kenar güvenliğini sağlama

Akıllı kenar güvenli hale getirme işlemi bir uçtan uca IOT çözümünün güveni confer gereklidir. Azure IOT kenar farklı risk profilleri, dağıtım senaryoları, genişletilebilir ve tüm Azure hizmetlerinden beklediğiniz aynı koruma sunar güvenlik için tasarlanmıştır.

Azure IOT kenar farklı donanımda çalışan, Linux ve Windows destekler ve farklı dağıtım senaryoları için geçerlidir.  Biçilen risk çözüm sahipliği, dağıtım Coğrafya, veri duyarlılık, gizlilik, uygulamanızın dikey ve yasal gereksinimleri dahil olmak üzere birçok etmenlere bağlıdır.  Belirli senaryolar için somut çözümleri sunarak yerine, Ölçek için tasarlanmış iyi grounded ilkeleri temel bir genişletilebilir güvenlik framework tasarlamak için mantıklıdır. 
 
Bu makalede güvenlik framework genel bir bakış sağlar. Daha fazla bilgi için bkz: [akıllı kenar güvenli hale getirme][lnk-edge-blog].

## <a name="standards"></a>Standartları

Standartlar güvenlik hallmark olan scrutiny kolaylığı ve uygulama, kolaylığı yükseltin.  İyi tasarlanmış bir güvenlik çözümü kendisini güven oluşturmak için değerlendirme altında scrutiny ödünç vermek ve dağıtım için bir hurdle olmamalıdır.  Azure IOT kenar güvenliğini sağlamak için tasarım framework'ün gelen time-tested yayılan ve benzerlik ve yeniden yararlanmak için kanıtlanmış endüstri güvenlik protokollerini. 

## <a name="authentication"></a>Kimlik Doğrulaması

Hangi aktörler, aygıtları ve bileşenleri bir uçtan uca IOT çözümü ile değerli teslimini katılan şüpheli bilerek güven oluşturulmasında dönüştürmektir.  Bu tür bilgi giriş temelini etkinleştirme için katılımcılarının güvenli sorumluluk sunar.  Azure IOT uç kimlik doğrulaması üzerinden bu bilgi attains.  Birincil kimlik doğrulaması Azure IOT kenar platform için sertifika tabanlı kimlik doğrulama mekanizmasıdır.  Bu mekanizma standartları ortak anahtar altyapısı (PKIX) Internet Engineering Task Force (IETF) tarafından yöneten bir dizi türetir.     

Tüm cihazlar, modüller (aygıt mantık kapsülleyen kapsayıcıları) ve fiziksel olarak veya bir ağ bağlantısı üzerinden olması ile Azure IOT sınır cihazı etkileşim aktörler için benzersiz sertifika kimlikleri için Azure IOT kenar güvenlik framework çağırır.  Her senaryo ya da bileşen kendisi için sertifika tabanlı kimlik doğrulaması için güvenlik framework genişletilebilirlik Konaklama için güvenli seçenek sunar ödünç. 

## <a name="authorization"></a>Yetkilendirme

Temsilci yetkilisi ve Denetim erişimi olanağı, temel güvenlik ilkesi – en az ayrıcalık prensibi elde doğrultusunda önemlidir.  Cihazları, modüller ve aktörler yalnızca kaynakları ve veri izni kapsamlarına içinde ve yalnızca izin verilen mimari olduğunda erişmek.  Bu, bazı izinler yeterli ayrıcalıkları ve diğerleri mimari zorlanan ile yapılandırılabilir anlamına gelir.  Örneğin, bir modül ayrıcalıklı yapılandırma aracılığıyla Azure IOT Hub'ına bir bağlantı başlatmak için yetkili, ancak neden bir Azure IOT sınır cihazı bir modüldeki başka bir Azure IOT sınır cihazı bir modüle twin erişmek için kullanması herhangi bir neden yoktur.  Bu nedenle, ikincisi mimari precluded. 

Sertifika imzalama hakları ve rol tabanlı erişim denetimi (RBAC) diğer yetkilendirme düzenleri içerir.  Güvenlik framework genişletilebilirlik diğer olgun yetkilendirme düzenleri benimsenmesi izin verir. 

## <a name="attestation"></a>Kanıtlama

Kanıtlama yazılım BITS bütünlüğünü sağlar.  Kötü amaçlı yazılım önleme ve algılama için önemlidir.  Azure IOT kenar güvenlik framework üç ana kategoriler altında kanıtlama sınıflandırır:

* Statik kanıtlama
* Çalışma zamanı kanıtlama
* Yazılım kanıtlama

### <a name="static-attestation"></a>Statik kanıtlama

İşletim sistemleri, tüm çalışma zamanları dahil olmak üzere tüm yazılım BITS bütünlüğünü doğrulama statik kanıtlama olduğunu ve cihaz yapılandırma bilgileri gücü.  Bu genellikle güvenli önyükleme adlandırılır.  Azure IOT sınır cihazları için güvenlik çerçevesi Silikon satıcılara genişletir ve statik kanıtlama işlemleri güvence altına almak için þeklimize güvenli donanım özellikleri içerir. Güvenli Önyükleme ve güvenli bellenim bu işlemleri dahil yükseltme işlemleri.  Silicon satıcılarla yakın işbirliği içinde çalışma, böylece tehdit yüzeyini en aza gereksiz bellenim Katmanlar ortadan kaldırır. 

### <a name="runtime-attestation"></a>Çalışma zamanı kanıtlama

Kötü amaçlı yazılım sistemleri bağlantı noktaları ve arabirimler aracılığıyla ekleme ve uygun önlemler almak bir sistem doğrulanmış önyükleme işlemi tamamlandıktan ve çalışır durumda ve çalışan, iyi tasarlanmış güvenli sistemleri algılamak bir kez çalışır.  Kötü amaçlı aktör fiziksel gözetim akıllı kenar cihazları, izinsiz gibi cihaz arabirimleri dışında araçlarla malcontent eklemesine mümkündür ve yan kanal saldırıları.   Bunlar önyükleme işlemi sonrasında olduğundan kötü amaçlı yazılım veya yetkisiz yapılandırma değişikliklerini biçiminde olabilir, bu tür malcontent Güvenli Önyükleme işlemi tarafından algılanmayan normalde.  Sunulan ya da büyük ölçüde cihazın donanım tarafından zorlanan önlemler katkıda bulunan tür tehditleri warding bulunun.  Azure IOT kenar güvenlik framework açıkça çalışma zamanı tehditleri combatting uzantıları için çağırır.     

### <a name="software-attestation"></a>Yazılım kanıtlama

Akıllı kenar sistemleri de dahil olmak üzere tüm sağlıklı sistemleri düzeltme eklerinin ve yükseltme için uygun olması gerekir.  Güvenlik Aksi takdirde, olası tehdit vektörlerini olabilir güncelleştirme işlemleri için önemlidir.  Azure IOT kenar çağrıları aracılığıyla güncelleştirmeleri için güvenlik çerçevesi ölçülen ve bütünlüğünü sağlamak ve paket kaynak kimliğini doğrulamak için paketleri imzalı.  Bu, tüm işletim sistemleri ve uygulama yazılımı BITS geçerlidir. 

## <a name="hardware-root-of-trust"></a>Donanım kök güveni

Akıllı sınır cihazları, özellikle de olası kötü amaçlı aktörler aygıt fiziksel erişim sahip olduğu yerlerde dağıtılan birçok dağıtımları aygıt donanım tarafından sunulan güvenlik son derinlemesine koruma için kullanılır.  Bu nedenle, yetkisiz değiştirmeye karşı dirençli donanım anchoring güvende böyle dağıtımları için en büyük/küçük harf önemlidir.  Azure IOT kenar güvenlik framework donanım kök çeşitli risk profilleri ve dağıtım senaryoları uyum sağlamak için güven farklı türdeki sunmak için güvenli Silikon donanım satıcıları işbirliği kapsar. Bunlar, Güvenilir Platform Modülü (ISO/IEC 11889) ve Trusted Computing Group'ın aygıt tanımlayıcısı birleşim altyapısı (BÖLMEK) gibi ortak güvenlik protokolü standartlarına uymak güvenli Silikon kullanımını içerir.  Bunlar ayrıca TrustZones ve yazılım koruma Uzantıları (SGX) gibi güvenli enclave teknolojileri içerir. 

## <a name="certification"></a>Sertifika

Müşteriler kendi dağıtımı için Azure IOT sınır cihazları temin etme, bilinçli kararlar almanıza yardımcı olmak için Azure IOT kenar framework sertifika gereksinimlerini içerir.  Güvenlik taleplerini ilgili sertifikalar ve güvenlik uygulamanız doğrulama için ilgili sertifikaları bu gereksinimleri temel.  Örneğin, güvenlik talep sertifika IOT sınır cihazı önyükleme saldırıları sağlayacak şekilde bilinen güvenli donanım kullandığını bildirmek. Doğrulama sertifika güvenli donanım aygıtı bu değeri sunmak için düzgün bir şekilde gerçekleştirildi bildirmek.  Basitlik ilkesini mantığıyla framework sertifika yükünü en az tutmayı görme sunar.   

## <a name="extensibility"></a>Genişletilebilirlik

Genişletilebilirlik birinci sınıf vatandaşı Azure IOT kenar güvenlik Framework'te ' dir.  Farklı türlerde iş dönüşümleri yürüten IOT teknolojisiyle bu senaryoları Gelişmekte olan adresine lockstep içindeki güvenlik sorunsuz bir şekilde gelişmesi neden anlamına gelir.  Azure IOT kenar güvenlik framework üzerinde genişletilebilirlik dahil etmek için farklı boyut içinde derlemeler için sağlam bir temel ile başlar: 

* Birinci taraf güvenlik hizmetleri için Azure IOT Hub cihaz hizmeti sağlama ister.
* (İzleme güvenlik ağları Silikon donanım kanıtlama Hizmetleri kafes veya gibi) üçüncü taraf hizmetleri yönetilen Güvenlik Hizmetleri (endüstriyel veya Sağlık Hizmeti gibi) farklı bir uygulama verticals veya teknoloji odağı zengin bir ağ ister. İş ortakları.
* Diğer güvenlik stratejileri ile retrofitting dahil etmek için eski sistemlere gibi kimlik doğrulama ve kimlik yönetimi için sertifikaları dışında güvenli teknolojisini kullanarak.
* Ortaya çıkan güvenli donanım teknolojileri ve silicon sorunsuz benimsenmesi için donanım iş ortağı Katkıları güvenli hale getirin.

Bunlar yalnızca birkaç örnek genişletilebilirliği için boyut ve Azure IOT kenar güvenlik framework bu genişletilebilirlik desteklemek üzere sıfırdan güvenli olacak şekilde tasarlanmıştır. 

Sonunda, akıllı kenar korumak yüksek başarı IOT korumak genel ilgi alanı tarafından yönetilen bir açık topluluktan işbirliği Katkıları oluşur.  Bu Katkıları güvenli teknolojiler ve hizmetler biçiminde olabilir.  Azure IOT kenar güvenlik framework güven ve bütünlüğü akıllı edge'de Azure bulut ile aynı düzeyde sunmak en fazla kapsamı için genişletilebilir güvenlik için sağlam bir temel sunar.  

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT kenar nasıl olduğu hakkında daha fazla okuma [akıllı kenar güvenli hale getirme][lnk-edge-blog].

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/ 