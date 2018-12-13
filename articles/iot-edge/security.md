---
title: Güvenlik çerçevesi - Azure IOT Edge | Microsoft Docs
description: Güvenlik, kimlik doğrulaması ve Azure IOT Edge geliştirmek için kullanılan ve çözümünüzü tasarlarken düşünülmesi gereken yetkilendirme standartları hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 1042f53147122a7370b464f6bfc892dcee70fe70
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100149"
---
# <a name="security-standards-for-azure-iot-edge"></a>Azure IOT Edge güvenlik standartlarını

Akıllı uç güvenliğini sağlama işleminin uçtan uca IOT çözümünün güven confer gereklidir. Azure IOT Edge, farklı risk profilleri, dağıtım senaryoları, genişletilebilir ve tüm Azure hizmetlerinden beklediğiniz aynı koruma sunan güvenlik için tasarlanmıştır.

Azure IOT Edge, farklı donanım üzerinde çalışan, hem Linux hem de Windows destekler ve çeşitli dağıtım senaryoları için geçerlidir.  Değerlendirilen riski çözüm sahipliği, dağıtım coğrafyasından, verilerin hassasiyet, gizlilik, uygulama dikey ve kanuni gereksinimleri dahil olmak üzere birçok etmenlere bağlıdır.  Belirli senaryolar için somut çözümler sunan yerine, bir genişletilebilir güvenlik çerçevesi ölçekler için tasarlandı iyi grounded ilkeleri temel tasarım mantıklıdır. 
 
Bu makalede güvenlik Çerçevesi'ne genel bakış sağlar. Daha fazla bilgi için [akıllı uç güvenliğini sağlama](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

## <a name="standards"></a>Standartları

Standartlar güvenlik günlerinden olan scrutiny kolaylığı ve kolay uygulama, tanıtın.  İyi düzenlenmiş bir güvenlik çözümü kendisini scrutiny altında güven oluşturmak için değerlendirme ödünç vermek ve dağıtım için bir hurdle olmamalıdır.  Azure IOT Edge güvenliğini sağlamak için tasarım framework'ün gelen zaman içinde kendini kanıtlamış yayılan ve sektörde kanıtlanmış güvenlik bilgisi ve yeniden yararlanmasını protokoller. 

## <a name="authentication"></a>Kimlik Doğrulaması

Hangi aktör, cihazlar ve bileşenleri ile uçtan uca IOT çözümünü değerli teslim katılan bir şüpheli bilmenin verdiği güven oluşturulmasında üst düzey öneme sahiptir.  Oluşturabildiğinden, giriş temelini etkinleştirmek için katılımcı güvenli sorumluluk sunar.  Azure IOT Edge, bu bilgileri kimlik doğrulaması üzerinden attains.  Birincil kimlik doğrulaması için Azure IOT Edge platformunda sertifika tabanlı kimlik doğrulama mekanizmasıdır.  Bu mekanizma, bir dizi ortak anahtar altyapısı (PKIX) Internet Engineering Task Force (IETF) tarafından yöneten standartları türetir.     

Tüm cihazlar, modülleri (cihaz mantık kapsülleyen kapsayıcılar) ve fiziksel olarak veya bir ağ bağlantısı üzerinden de Azure IOT Edge cihazı ile etkileşim aktörler için benzersiz bir sertifika kimlikleri için Azure IOT Edge güvenlik Çerçevesi'ni çağırır.  Her senaryo veya bileşen kendi sertifika tabanlı kimlik doğrulaması genişletilebilirliği güvenlik çerçevesi, Konaklama için güvenli bir seçenek sunan ödünç vermek. 

## <a name="authorization"></a>Yetkilendirme

Temsilci erişimi yetkilisi ve denetleme olanağı, temel güvenlik ilkesi – en az ayrıcalık ilkesini elde etmeye yönelik çok önemlidir.  Cihazlar, modülleri ve aktör yalnızca ve kaynaklara erişim izni kapsamlarına içinde ve yalnızca izin verilen mimari olduğunda veri elde edebilirsiniz.  Bu, bazı izinler yeterli ayrıcalıkları ile diğer mimari zorlanan yapılandırılabilir anlamına gelir.  Örneğin, bir modül Azure IOT Hub için bir bağlantı başlatmak için ayrıcalıklı konfigürasyonuyla yetkilendirilmesi, ancak bir modülde bir Azure IOT Edge cihazı bir modülün başka bir Azure IOT Edge cihaz ikizi neden erişmeli bir neden yoktur.  Bu nedenle, ikincisi mimari precluded. 

Sertifika imzalama haklar ve rol tabanlı erişim denetimi (RBAC), diğer yetkilendirme düzenleri içerir.  Genişletilebilirlik güvenlik framework'ün diğer olgun yetkilendirme düzenleri benimsenmesini izin verir. 

## <a name="attestation"></a>Kanıtlama

Kanıtlama yazılım BITS bütünlüğü sağlar.  Kötü amaçlı yazılım önleme ve algılama için önemlidir.  Azure IOT Edge güvenlik framework üç ana kategori altında kanıtlama sınıflandırır:

* Statik kanıtlama
* Çalışma zamanı kanıtlama
* Yazılım kanıtlama

### <a name="static-attestation"></a>Statik kanıtlama

İşletim sistemleri, tüm çalışma zamanları dahil olmak üzere tüm yazılım BITS bütünlüğünü doğrulama statik kanıtlama olduğundan ve cihaz yapılandırma bilgilerini güçlenme.  Bu genellikle güvenli önyükleme adlandırılır.  Azure IOT Edge cihazları için güvenlik çerçevesi, Silikon satıcılara genişletir ve statik kanıtlama işlemleri güvence altına almak için þeklimize güvenli donanım özellikleri içerir. Bu işlemleri, Güvenli Önyükleme ve güvenli bir üretici yazılımı dahil yükseltme işlemleri.  Silikon satıcılarla yakın iş çalışıyor, böylece tehdit yüzeyini en aza gereksiz bellenim katmanları ortadan kaldırır. 

### <a name="runtime-attestation"></a>Çalışma zamanı kanıtlama

Bir sistem doğrulanmış önyükleme işlemi tamamlandı ve çalışır durumda ve çalışan, iyi tasarlanmış güvenlik sistemleri algılamak sonra kötü amaçlı yazılım sistemleri bağlantı noktaları ve arabirimler üzerinden ekleme ve uygun saptayabilmeli çalışır.  Akıllı uç cihazlarında, kötü amaçlı aktörler fiziksel gözetim içinde bulunamazsınız oynama gibi cihaz arabirimler dışında malcontent eklemesine mümkündür ve yan kanal saldırıları.   Sonra önyükleme işlemi meydana çünkü kötü amaçlı yazılım ya da yetkisiz yapılandırma değişikliklerini biçiminde olabilir, bu tür malcontent Güvenli Önyükleme işlemi tarafından algılanır normalde değil.  Sunulan ya da büyük ölçüde cihazın donanım tarafından zorlanan karşı önlemler, bu tür tehditleri warding doğrultusunda katkıda bulunur.  Azure IOT Edge için güvenlik çerçevesi açıkça çalışma zamanı tehditleri combatting için uzantıları için çağırır.     

### <a name="software-attestation"></a>Yazılım kanıtlama

Akıllı uç sistemleri dahil olmak üzere tüm iyi durumdaki sistemler için Yama ve yükseltmelerinin tfs'deki olması gerekir.  Güvenlik Aksi takdirde, olası bir tehdit vektörlerini olabilirler güncelleştirme işlemleri için önemlidir.  Azure IOT Edge çağırır aracılığıyla güncelleştirmeleri için güvenlik çerçevesi ölçülür ve imzalı paketlerin bütünlüğünü sağlamak ve paket kaynak kimliğini doğrulamak için.  Bu, tüm işletim sistemleri ve uygulama yazılımı BITS geçerlidir. 

## <a name="hardware-root-of-trust"></a>Donanım hiyerarşik güven kökü

Akıllı uç cihazlarının, özellikle olası kötü amaçlı aktörler cihaza fiziksel erişimi olduğu yerde dağıtılmış pek çok dağıtımı cihaz donanım tarafından sunulan güvenlik son derinlemesine koruma için kullanılır.  Bu nedenle, kurcalamaya dirençli donanım anchoring güvende tür dağıtımlar için en büyük/küçük harf önemlidir.  Azure IOT Edge için güvenlik çerçevesi, farklı türdeki donanım çeşitli risk profilleri ve dağıtım senaryoları için güven kökü desteği sunmak için güvenli Silikon donanım satıcıları işbirliği kapsar. Bunlar, Güvenilir Platform Modülü (ISO/IEC 11889) ve Trusted Computing Group'ın cihaz tanımlayıcısı bileşim motoru (DICE) gibi sık kullanılan güvenlik protokolü standartlarına uymak güvenli Silikon kullanımını içerir.  Bu, TrustZones ve yazılım koruma Uzantıları (SGX) gibi güvenli kuşatma teknolojileri de içerir. 

## <a name="certification"></a>Sertifika

Azure IOT Edge cihazları, dağıtım için temin etme, bilinçli kararlar yapmalarına yardımcı olmak için Azure IOT Edge framework sertifika gereksinimlerini içerir.  Güvenlik talepleri ilişkin sertifikaları ve ilgili güvenlik uygulamasının doğrulama sertifikaları için bu gereksinimleri derinlerine dalın.  Örneğin, bir güvenlik talebi sertifika IOT Edge cihazı önyükleme saldırılarına karşı dayanıklılık bilinen güvenli donanım kullandığını bilgilendirmek. Doğrulama sertifika güvenli donanım cihazı bu değer sunmak için düzgün şekilde uygulanmıştır bilgilendirmek.  Basitlik ilkesini mantığıyla framework sertifika yükünü en düşük tutma Vizyonun sunar.   

## <a name="extensibility"></a>Genişletilebilirlik

Birinci sınıf vatandaşı Azure IOT Edge güvenlik Framework'te genişletilebilirlik olur.  Farklı türde iş dönüşümlerini sürüş IOT teknolojisini ile bu senaryoları Gelişmekte olan adrese lockstep içinde güvenlik sorunsuz bir şekilde gelişmek nedeni anlamına gelir.  Azure IOT Edge güvenlik çerçevesi, sağlam bir temel üzerinde genişletilebilirlik dahil etmek için farklı boyut içinde derlemeler ile başlar: 

* Birinci taraf güvenlik hizmetleri için Azure IOT Hub cihaz sağlama hizmeti ister.
* (İzleme güvenlik ağları veya Silikon donanım kanıtlama Hizmetleri mesh gibi) üçüncü taraf hizmetleri yönetilen güvenlik hizmetleri farklı uygulama sıralamasına koydunuz (endüstriyel veya sağlık gibi) veya teknoloji odağı oluşan zengin bir ağ ister. iş ortağı.
* Eski sistemleri ile diğer güvenlik stratejileri, retrofitting eklemek gibi kimlik doğrulaması ve kimlik yönetimi için sertifikalar dışında güvenli teknolojisini kullanarak.
* Donanım güvenli donanım yeni çıkan teknolojiler ve Silikon sorunsuz benimseme için ortak Katkıları güvenli hale getirin.

Bu boyutlar genişletilebilirliği için yalnızca birkaç örnektir ve Azure IOT Edge güvenlik çerçevesi bu genişletilebilirlik desteklemek için baştan güvenli olacak şekilde tasarlanmıştır. 

Akıllı uç güvenli hale getirmenin en yüksek başarı sonunda işbirliğine dayalı katkı sağladığı bir açık ve topluluk tarafından IOT güvenli hale getirmenin genel ilgi odaklı oluşur.  Bu Katkıları güvenli teknolojiler ve hizmetler biçiminde olabilir.  Azure IOT Edge güvenlik çerçevesi, güven ve Azure bulut ile akıllı uç bütünlüğü aynı düzeyde sunmak en fazla kapsamı için genişletilebilir güvenlik için sağlam bir temel sunar.  

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Edge nasıl olduğu hakkında daha fazla okuma [akıllı uç güvenliğini sağlama](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).
