---
title: Güvenlik çerçevesi - Azure IOT Edge | Microsoft Docs
description: Güvenlik, kimlik doğrulaması ve Azure IOT Edge geliştirmek için kullanılan ve çözümünüzü tasarlarken düşünülmesi gereken yetkilendirme standartları hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 8aadddbc9ae13a87f89db4d7e7189ea7aa8aeef5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612040"
---
# <a name="security-standards-for-azure-iot-edge"></a>Azure IOT Edge güvenlik standartlarını

Azure IOT Edge, veri ve analiz için akıllı bir ucu taşırken belirlidir risk senaryoları için tasarlanmıştır. IOT Edge güvenlik standartlarını, yine de beklediğiniz tüm Azure hizmetlerinden koruma sunmaya devam ederken farklı risk profilleri ve dağıtım senaryoları için esneklik sağlar. 

Azure IOT Edge çalıştıran çeşitli donanım yapar ve modelleri, çeşitli işletim sistemlerini destekler ve çeşitli dağıtım senaryoları için geçerlidir. Dağıtım senaryosu risklerini değerlendirmesine çözüm sahipliği, dağıtım coğrafyasından, verilerin hassasiyet, gizlilik, uygulama dikey ve kanuni gereksinimleri dahil olmak üzere birçok etmenlere bağlıdır. Belirli senaryolar için somut çözümler sunan yerine IOT Edge ölçekler için tasarlandı iyi grounded ilkelerine dayalı bir genişletilebilir güvenlik çerçevesi. 
 
Bu makalede, IOT Edge güvenlik Çerçevesi'ne genel bakış sağlar. Daha fazla bilgi için [akıllı uç güvenliğini sağlama](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

## <a name="standards"></a>Standartları

Standartlar scrutiny kolaylığı ve ikisi için de güvenlik hallmarks olan uygulama, kolay tanıtın. Bir güvenlik çözümü kendisini scrutiny altında güven oluşturmak için değerlendirme ödünç vermek ve dağıtım için bir hurdle olmamalıdır. Azure IOT Edge güvenliğini sağlamak için tasarım framework'ün üzerinde zaman içinde kendini kanıtlamış temel alır ve sektörde kanıtlanmış güvenlik bilgisi ve yeniden kullanımı için protokoller. 

## <a name="authentication"></a>Kimlik Doğrulaması

Bir IOT çözümünü dağıttığınızda, yalnızca güvenilir aktör, cihazlar ve modülleri çözümünüzü erişimi olduğunu bilmeniz gerekir. Oluşturabildiğinden, katılımcıların güvenli sorumluluk sunar. Azure IOT Edge, bu bilgileri kimlik doğrulaması üzerinden attains. Sertifika tabanlı kimlik doğrulaması için kimlik doğrulaması için Azure IOT Edge platformunda birincil mekanizmadır. Bu mekanizma, bir dizi ortak anahtar altyapısı (PKIX) Internet Engineering Task Force (IETF) tarafından yöneten standartları türetir.     

Tüm cihazlar, modülleri ve Azure IOT Edge cihaz, fiziksel ya da bir ağ bağlantısı üzerinden etkileşim aktörler benzersiz sertifika kimlikleri olması gerekir. Ancak, her senaryonuz ya da bileşen kendi sertifika tabanlı kimlik doğrulaması için uygun. Bu senaryolarda, güvenlik framework'ün genişletilebilirlik güvenli alternatifler sağlar. 

## <a name="authorization"></a>Yetkilendirme

Kullanıcılar ve sistemin bileşenleri kaynaklarına ve verilerine kendi rolleri gerçekleştirmek için gereken en düşük kümesini yalnızca erişiminiz olması en düşük öncelik ilkesini diyor. Cihazlar, modülleri ve aktör yalnızca kaynakları ve verileri kendi izin kapsamı içinde ve yalnızca izin verilen mimari olduğunda erişmelidir. Bazı izinler yeterli ayrıcalığa sahip yapılandırılabilir ve diğer mimari uygulanır.  Örneğin, bir modül ayrıcalıklı yapılandırma aracılığıyla Azure IOT Hub için bir bağlantı başlatmak için yetkili olması. Ancak, bir modülde bir Azure IOT Edge cihazı bir modülün başka bir Azure IOT Edge cihaz ikizi neden erişmeli bir neden yoktur.

Diğer yetkilendirme düzenleri, sertifika imzalama hakları ve rol tabanlı erişim denetimi (RBAC) içerir. 

## <a name="attestation"></a>Kanıtlama

Kanıtlama yazılım BITS bütünlüğü sağlar.  Kötü amaçlı yazılım önleme ve algılama için önemlidir.  Azure IOT Edge güvenlik framework üç ana kategori altında kanıtlama sınıflandırır:

* Statik kanıtlama
* Çalışma zamanı kanıtlama
* Yazılım kanıtlama

### <a name="static-attestation"></a>Statik kanıtlama

Statik kanıtlama bir cihazdaki işletim sistemi, tüm çalışma zamanları ve cihaz eklentisini yapılandırma bilgilerini de dahil olmak üzere tüm yazılım bütünlüğünü doğrular. Bu genellikle güvenli önyükleme adlandırılır. Azure IOT Edge cihazları için güvenlik çerçevesi üreticilerine genişletir ve statik kanıtlama işlemleri sağlamak güvenli donanım özellikleri içerir. Bu işlemleri, Güvenli Önyükleme ve güvenli bir üretici yazılımı dahil yükseltin.  Silikon satıcılarla Kapat işbirliği içinde çalışan gereksiz bellenim katmanları ortadan kaldırır, böylece tehdit yüzeyini en aza indirir. 

### <a name="runtime-attestation"></a>Çalışma zamanı kanıtlama

İyi tasarlanmış sistemler, Güvenli Önyükleme işlemi tamamlandı ve çalışır duruma geldikten sonra kötü amaçlı yazılım ekleme ve uygun saptayabilmeli girişimlerinin algılanmasına. Kötü amaçlı yazılım saldırılarını sistemin bağlantı noktaları ve arabirimler sisteme erişmek için hedefleyebilir. Veya kötü amaçlı aktörler cihazla değiştirmesine veya yan kanal saldırıları erişmek için kullandığı bir cihaza fiziksel erişiminiz varsa. Önyükleme işlemi sonrasında eklenmiş durumda olduğundan, kötü amaçlı yazılım ya da yetkisiz yapılandırma değişikliklerini biçiminde olabilir, bu tür malcontent tarafından statik kanıtlama algılanamıyor. Önlemler sunulan ya da bu tür tehditleri yüzden için cihazın donanım Yardım tarafından zorunlu tutulur.  Azure IOT Edge için güvenlik çerçevesi çalışma zamanı tehditleri mücadele etmek için uzantıları açıkça çağırır.  

### <a name="software-attestation"></a>Yazılım kanıtlama

Akıllı uç sistemleri dahil olmak üzere tüm sağlıklı sistemlerine Yama ve yükseltmelerinin kabul etmeniz gerekir.  Güvenlik Aksi takdirde, olası bir tehdit vektörlerini olabilirler güncelleştirme işlemleri için önemlidir.  Azure IOT Edge çağırır aracılığıyla güncelleştirmeleri için güvenlik çerçevesi ölçülür ve imzalı paketlerin bütünlüğünü sağlamak ve paket kaynak kimliğini doğrulamak için.  Bu standart tüm işletim sistemleri ve uygulama yazılımı BITS geçerlidir. 

## <a name="hardware-root-of-trust"></a>Donanım hiyerarşik güven kökü

Birçok akıllı uç cihazları, özellikle olası kötü amaçlı aktörler cihaza fiziksel erişimi olduğu yerde cihazlar için son derinlemesine koruma için cihaz donanım güvenlik sağlıyor. Bu tür dağıtımlar için kurcalamaya dirençli donanım önemlidir. Azure IOT Edge, farklı türdeki donanım çeşitli risk profilleri ve dağıtım senaryoları için güven kökü desteği sunmak için güvenli Silikon donanım satıcıları teşvik etmektedir. Donanım güven Güvenilir Platform Modülü (ISO/IEC 11889) ve Trusted Computing Group'ın cihaz tanımlayıcısı bileşim motoru (DICE) gibi sık kullanılan güvenlik protokolü standartları gelir. TrustZones ve yazılım koruma Uzantıları (SGX) ayrıca donanım güven sağlamak gibi kuşatma teknolojileri güvenli hale getirin. 

## <a name="certification"></a>Sertifika

Azure IOT Edge cihazları, dağıtım için temin etme, bilinçli kararlar yapmalarına yardımcı olmak için Azure IOT Edge framework sertifika gereksinimlerini içerir.  Güvenlik talepleri ilişkin sertifikaları ve ilgili güvenlik uygulamasının doğrulama sertifikaları için bu gereksinimleri derinlerine dalın.  Örneğin, bir güvenlik talebi sertifika IOT Edge cihazı önyükleme saldırılarına karşı dayanıklılık bilinen güvenli donanım kullandığını bilgilendirmek. Doğrulama sertifika güvenli donanım cihazı bu değer sunmak için düzgün şekilde uygulanmıştır bilgilendirmek.  Basitlik ilkesini mantığıyla framework sertifika yükünü en düşük düzeyde tutmak çalışır.   

## <a name="extensibility"></a>Genişletilebilirlik

Farklı türde iş dönüşümlerini sürüş IOT teknolojisini ile bu güvenlik senaryoları Gelişmekte olan adrese paralel gelişmek nedeni anlamına gelir.  Azure IOT Edge güvenlik çerçevesi, sağlam bir temel üzerinde genişletilebilirlik dahil etmek için farklı boyut içinde derlemeler ile başlar: 

* Birinci taraf güvenlik hizmetleri için Azure IOT Hub cihaz sağlama hizmeti ister.
* (İzleme güvenlik ağları veya Silikon donanım kanıtlama Hizmetleri mesh gibi) üçüncü taraf hizmetleri yönetilen güvenlik hizmetleri farklı uygulama sıralamasına koydunuz (endüstriyel veya sağlık gibi) veya teknoloji odağı oluşan zengin bir ağ ister. iş ortağı.
* Eski sistemleri ile diğer güvenlik stratejileri, retrofitting eklemek gibi kimlik doğrulaması ve kimlik yönetimi için sertifikalar dışında güvenli teknolojisini kullanarak.
* Donanım güvenli donanım yeni çıkan teknolojiler ve Silikon benimseme için ortak Katkıları güvenli hale getirin.

Akıllı uç güvenli hale getirmenin en yüksek başarı sonunda işbirliğine dayalı katkı sağladığı bir açık ve topluluk tarafından IOT güvenli hale getirmenin genel ilgi odaklı oluşur.  Bu Katkıları güvenli teknolojiler ve hizmetler biçiminde olabilir.  Azure IOT Edge güvenlik çerçevesi, güven ve Azure bulut ile akıllı uç bütünlüğü aynı düzeyde sunmak en fazla kapsamı için genişletilebilir güvenlik için sağlam bir temel sunar.  

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Edge nasıl olduğu hakkında daha fazla okuma [akıllı uç güvenliğini sağlama](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).
