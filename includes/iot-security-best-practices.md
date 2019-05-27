---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 2138eed9975abe804442c476d19b5b7229685362
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146632"
---
# <a name="security-best-practices-for-internet-of-things-iot"></a>Nesnelerin interneti (IOT) için en iyi güvenlik uygulamaları

Bir nesnelerin interneti (IOT) altyapısını koruma sıkı bir güvenlik savunma stratejisi gerektirir. Bu strateji, verileri bulutta güvenli, genel internet üzerinden aktarım sırasında veri bütünlüğünü korumak ve güvenli bir şekilde cihaz sağlamayı gerektirir. Her katman, daha yüksek güvenlik güvencesi genel altyapı oluşturur.

## <a name="secure-an-iot-infrastructure"></a>Güvenli bir IOT altyapısı

Bu güvenlik savunma stratejisi geliştirilen ve üretim, geliştirme ve IOT cihazları ve altyapı dağıtımı ile ilgili çeşitli Yürütücü etkin katılım ile yürütülür. Bu oyuncuların üst düzey bir açıklaması verilmiştir.

* **IOT donanım üreticisi/entegratörü**: Genellikle, bu oyuncuların çeşitli üretici veya tedarikçileri üretilen veya başka üreticiler tarafından tümleşik bir IOT dağıtımı için donanım sağlayarak donanım derleyerek tümleştiricileri dağıtılan IOT donanım üreticilerine cihazlardır.

* **IOT çözüm geliştirici**: Bir IOT çözümünün geliştirilmesini, genellikle bir çözüm geliştirici tarafından yapılır. Bu Geliştirici bölüm şirket içi bir takım veya bu etkinliğe uzmanlaşan bir sistem entegratörü (sı). IOT çözüm geliştirici çeşitli bileşenlerinin sıfırdan IOT çözümü geliştirmek, çeşitli kullanıma hazır veya açık kaynak bileşenlerini tümleştirmek veya Çözüm Hızlandırıcıları ile küçük uyarlama benimseyin.

* **IOT çözüm dağıtıcı**: Bir IOT çözümü geliştirdiğini sonra alanına dağıtılması gerekiyor. Bu işlem donanım, cihaz bağlantısı ve donanım cihazlarının çözümlerinde veya Bulut dağıtımını dağıtımını içerir.

* **IOT çözüm işleci**: IOT çözümü dağıtıldıktan sonra uzun süreli işlem, izleme, yükseltme ve bakım gerektirir. Bu görevler, bilgi teknolojisi uzmanları, donanım işlemler ve Bakım takımlar ve genel IOT altyapının doğru davranışları olan etki alanı uzmanlarıyla kapsar şirket içi bir ekip tarafından yapılabilir.

Aşağıdaki bölümlerde her geliştirme, dağıtma ve güvenli bir IOT altyapısı işletin yardımcı olmak için bu Yürütücü için en iyi yöntemler sağlar.

## <a name="iot-hardware-manufacturerintegrator"></a>IOT donanım üreticisi/entegratörü

IOT donanım üreticilerinin ve donanım entegratörleri için en iyi uygulamalar aşağıda verilmiştir.

* **Kapsam en düşük gereksinimlere donanım**: Donanım tasarımı, donanım ve hiçbir şey daha fazla işlem için gereken en düşük özellikleri eklemeniz gerekir. Yalnızca gereken durumlarda işlemi cihazın USB bağlantı noktası eklemek için kullanılan bir örnektir. Bu ek özellikler cihaz kaçınılmalıdır istenmeyen saldırı vektörlerinin için açın.

* **Kavram değiştirmesine donanım olun**: Cihaz yüzünde açarak veya bir bölümü cihazın kaldırılması gibi fiziksel oynama algılamak için mekanizmaları oluşturun. Bu sinyalleri değiştirmesine bu olayların işleçleri uyarı cloud'a yüklenebilir veri akışının parçası olabilir.

* **Güvenli donanım derleme**: COGS izin veriyorsa, güvenli ve şifrelenmiş depolama ya da Güvenilir Platform Modülü (TPM) üzerinde tabanlı önyükleme işlevsellik gibi güvenlik özelliklerini oluşturun. Bu özellikler cihazların daha güvenli ve genel IOT altyapısı korunmasına yardımcı olun.

* **Yükseltmeler güvenli hale getirmek**: Cihaz kullanım ömrü süresince üretici yazılımını yükseltme kaçınılmazdır. Güvenli yolları cihazlarla yükseltmeler ve şifreleme güvencesi bellenim sürümleri oluşturma, cihazın sırasında ve sonrasında yükseltmeleri güvenli olmasını sağlar.

## <a name="iot-solution-developer"></a>IOT çözümü geliştirme

IOT çözüm geliştiriciler için en iyi uygulamalar şunlardır:

* **Güvenli bir yazılım geliştirme metodolojisinin izleyin**: Güvenli yazılım geliştirmenin en başından başlayarak, uygulama, test ve dağıtımı için tüm proje yeni bir güvenlik düşünmek gerekir. Platform, diller ve Araçlar Seçenekler tüm bu yöntemiyle etkilenmiştir. Microsoft güvenlik geliştirme yaşam döngüsü güvenli yazılımlar oluşturmaya yönelik adım adım bir yaklaşım sağlar.

* **Açık kaynak yazılım dikkatle seçin**: Açık kaynak yazılım çözümlerinizi hızla geliştirin fırsatı sunar. Açık kaynak yazılım seçerken, her bir açık kaynak bileşen için topluluk etkinliği düzeyini göz önünde bulundurun. Yazılım desteklenir ve sorunları bulunan ve yönelik etkin bir topluluk sağlar. Alternatif olarak, belirsiz ve etkin olmayan açık kaynak yazılım projesinde desteklenmiyor ve sorunları olan büyük olasılıkla olması bulunamaz.

* **Dikkatli tümleştirme**: Birçok yazılım güvenlik açıkları, kitaplıkları ve API'ler sınırında mevcut. Geçerli dağıtım için gerekli olmayabilir işlevselliği yine de bir API katmanı kullanılabilir. Genel güvenliğini sağlamak için güvenlik açıkları tümleştirilmektedir bileşenlerinin tüm arabirimleri denetimi emin olun.

## <a name="iot-solution-deployer"></a>IOT çözüm dağıtıcı

IOT çözüm planlayıcılar için en iyi uygulamalar şunlardır:

* **Donanım güvenli bir şekilde dağıtmak**: IOT dağıtımları gibi ortak alanları ya da Denetimsiz yerel ayarlara güvenli olmayan konumlardaki dağıtılacak donanım gerektirebilir. Bu gibi durumlarda, donanım dağıtım artıklığının olmasını sağlamak en yüksek ölçüde. Donanımda USB veya diğer bağlantı noktaları varsa, bunların güvenli bir şekilde kapsama alındığından emin olun. Çok sayıda saldırı vektörlerinin giriş noktaları olarak kullanabilirsiniz.

* **Kimlik doğrulama anahtarlarını güvende tutun**: Dağıtım sırasında her cihaz, cihaz kimliklerini ve bulut hizmeti tarafından oluşturulan ilişkili kimlik doğrulaması anahtarlarını gerektirir. Bu anahtarları dağıtımdan sonra bile, fiziksel olarak güvenli tutun. Güvenliği aşılmış bir tuşa kötü amaçlı bir cihaz tarafından geçici var olan bir cihaz için kullanılabilir.

## <a name="iot-solution-operator"></a>IOT çözümü işleci

IOT çözüm işleçleri için en iyi uygulamalar şunlardır:

* **Sistemin güncel**: Cihaz işletim sistemleri ve tüm cihaz sürücülerini en son sürümüne yükseltildiğinden emin olun. Otomatik güncelleştirmeler Windows 10 (IOT veya diğer SKU'lar) açarsanız, Microsoft güncel, güvenli bir işletim sistemi için IOT cihaz sağlama tutar. Tutma (Linux gibi) diğer işletim sistemlerinin güncel sağlamaya yardımcı olur bunlar aynı zamanda kötü amaçlı saldırılara karşı korunur.

* **Kötü amaçlı etkinliklere karşı korumaya**: İşletim sistemi izin veriyorsa, en son virüs ve kötü amaçlı yazılımdan koruma özelliklerinden her cihaz işletim sistemine yükleyin. Bu yöntem en dış tehditlerin azaltılmasına yardımcı olabilir. Uygun adımları izleyerek, çoğu modern işletim sistemi tehditlere karşı koruyabilirsiniz.

* **Sık denetim**: Güvenlikle ilgili sorunlar için IOT altyapısı denetimi güvenlik olaylarına yanıt vermek anahtardır. Çoğu işletim sistemleri, hiçbir güvenlik ihlali oluştu emin olmak için sık incelenmelidir yerleşik olay günlüğü sağlar. Denetim bilgilerini ayrı telemetri akışı olarak bunu edilebilecekleri bulut hizmetine gönderilebilir.

* **Fiziksel olarak IOT altyapısını koruma**: Cihazlara fiziksel erişim kullanarak IOT altyapısı kötü güvenlik saldırıları başlatılabilir. Bir önemli güvenlik kötü niyetli kullanımına USB bağlantı noktaları ve diğer fiziksel erişime karşı korumaya yönelik uygulamadır. USB bağlantı noktası kullanımı gibi fiziksel erişim oluşmuş olabilir analiziyle ihlallerini anahtardan günlüğe kaydetme. Yeniden, Windows 10 (IOT ve diğer SKU'lar), bu olayların ayrıntılı günlük kaydını etkinleştirir.

* **Bulut kimlik bilgilerini koruyun**: Bulut kimlik doğrulaması kimlik bilgilerini yapılandırmak ve çalışan IOT dağıtım için kullanılan büyük olasılıkla erişmek ve tehlikeye bir IOT sisteminin en kolay yoludur. Parolayı sık sık değiştirerek kimlik bilgilerini koruma ve genel makinelerde bu kimlik bilgilerini kullanmadığınızdan.

Farklı IOT cihaz özellikleri değişir. Bazı cihazlar ortak masaüstü işletim sistemlerini çalıştıran bilgisayarlar olabilir ve bazı cihazların çok hafif işletim sistemleri çalışıyor olabilir. Daha önce açıklanan güvenlik en iyi uygulamaları bu cihazlara değişen derece cinsinden geçerli olabilir. Sağlanırsa, ek güvenlik ve dağıtım en iyi Üreticiler bu cihazların gelmelidir.

Bazı eski ve kısıtlı cihazları IOT dağıtım için özel olarak tasarlanmış olabilir değil. Bu cihazlar, verileri şifrelemek için Internet ile bağlanmak veya gelişmiş denetim sağlamak özelliğini olmayabilir. Bu gibi durumlarda, modern ve güvenli bir alan ağ geçidi eski cihazlardan veri toplama ve bu cihazların Internet üzerinden bağlanması için gereken güvenlik sağlar. Alan ağ geçitleri, güvenli kimlik doğrulaması, şifrelenmiş oturumlarının anlaşma, Bulut ve diğer birçok güvenlik özelliği komutları alınmasını sağlayabilirsiniz.