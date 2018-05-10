---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2c6f5cf2d89da0c2418ac58ca5d47a8aa05e732f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="internet-of-things-security-best-practices"></a>En iyi güvenlik uygulamalarını, nesnelerin interneti

Bir nesnelerin interneti (IOT) altyapısını koruma sıkı güvenlik derinlemesine stratejisi gerektirir. Bu strateji buluttaki verilerin güvenli, ortak internet üzerinden aktarım sırasında veri bütünlüğü korumak ve güvenli bir şekilde cihazlara sağlamak gerektirir. Her katman genel altyapısında daha yüksek güvenlik güvencesi oluşturur.

## <a name="secure-an-iot-infrastructure"></a>Güvenli bir IOT altyapı

Bu güvenlik derinlemesine strateji geliştirilmiş ve üretim, geliştirme ve IOT cihazları ve altyapı dağıtımı ile ilgili çeşitli Yürütücü etkin bir katılım ile yürütüldü. Bu oynatıcıları üst düzey bir açıklaması verilmiştir.

* **IOT donanım üreticisinin/Tümleştirici**: genellikle bu oynatıcıları çeşitli üreticileri ya da bir IOT dağıtım için donanım sağlama Üreticiler donanım atayarak tümleştiricileri dağıtılan IOT donanım üreticileri yayımlanır üretilen veya başka üreticiler tarafından tümleşik.
* **IOT çözüm geliştirici**: bir IOT çözüm geliştirme genellikle bir çözüm geliştirici tarafından yapılır. Bu Geliştirici parçası bir şirket içi team ya da bu etkinlikte uzmanlaşmış bir sistem Tümleştirici (SI). IOT çözüm geliştirici sıfırdan IOT çözüm çeşitli bileşenleri geliştirmek, çeşitli piyasada veya açık kaynak bileşenlerini tümleştirmek veya ikincil uyarlama ile Çözüm Hızlandırıcıları benimsemeyi.
* **IOT çözüm dağıtıcı**: sonra bir IOT çözüm geliştirilmiş, alanında dağıtılması gerekir. Bu işlem donanım, cihaz bağlantısı ve donanım aygıtları çözümlerinde veya Bulut dağıtımını dağıtımını içerir.
* **IOT çözüm işleci**: sonra IOT çözüm dağıtılırsa, uzun vadeli işlemleri, izleme, yükseltme ve bakım gerektirir. Bu görevler, bilgi teknolojisi uzmanları, donanım işlemler ve Bakım ekipleri ve genel IOT altyapı doğru davranışını izlemek etki alanı uzmanlarıyla içeren bir şirket içi ekibi tarafından yapılabilir.

Aşağıdaki bölümlerde her geliştirmek, dağıtmak ve güvenli bir IOT altyapısı işletmek yardımcı olması için bu oynatıcılar için en iyi yöntemleri sağlamak.

## <a name="iot-hardware-manufacturerintegrator"></a>IOT donanım üreticisinin/Tümleştirici

IOT donanım üreticileri ve donanım tümleştiricileri için en iyi yöntemler verilmiştir.

* **Kapsam en düşük gereksinimler için donanım**: donanım tasarımı donanım ve hiçbir şey daha fazla işlem için gereken en düşük özellikleri içermelidir. Yalnızca gerekirse işlemi cihazın USB bağlantı noktası eklemek için kullanılan bir örnektir. Bu ek özellikler cihaz kaçınılmalıdır istenmeyen saldırı vektörlerinin için açın.
* **Kanıt değiştirmesine donanım olun**: cihaz yüzünde açma veya aygıt parçası kaldırma gibi fiziksel oynama algılamak için mekanizmaları oluşturun. Bunlar sinyalleri değiştirmesine işleçleri bu olayların uyarı buluta karşıya veri akışının bir parçası olabilir.
* **Güvenli donanım yapı**: varsa SMM izin verir, güvenli ve şifrelenmiş depolama alanı gibi güvenlik özellikleri oluşturmak ya da önyükleme Güvenilir Platform Modülü (TPM) üzerinde temel işlevselliği. Bu özellikleri daha güvenli ve genel IOT altyapısını korumaya yardımcı aygıtları olun.
* **Yükseltmeler güvenli hale**: cihaz kullanım ömrü süresince bellenim yükseltmeleri kaçınılmaz. Güvenli yolları aygıtlarla yükseltmeler ve şifreleme güvence bellenim sürümleri oluşturma sırasında ve yükseltildikten sonra güvenli olması cihaz izin verir.

## <a name="iot-solution-developer"></a>IOT Çözüm geliştiricisi

IOT çözüm geliştiriciler için en iyi uygulamalar şunlardır:

* **Güvenli yazılım geliştirme metodolojisini izleyin**: Güvenli yazılım geliştirme güvenlik, kendi uygulama, test ve dağıtım için tüm proje başlangıcından başından başlayarak yukarı düşünmek gerektirir. Platform, dil ve araçları seçimleri tüm bu yöntemleri ile etkilenir. Microsoft güvenlik geliştirme yaşam döngüsü güvenli yazılım oluşturmak için adım adım bir yaklaşım sağlar.
* **Açık kaynaklı yazılım dikkatle seçin**: açık kaynaklı yazılım hızla çözümleri geliştirmek için bir fırsat sağlar. Açık kaynaklı yazılım seçerken her bir açık kaynak bileşenin topluluk etkinlik düzeyini göz önünde bulundurun. Etkin bir topluluk yazılım desteklenir ve sorunları bulunan ve ele sağlar. Alternatif olarak, belirsiz ve etkin olmayan açık kaynaklı yazılım projesinde desteklenmiyor ve sorunları olan değil büyük olasılıkla saptanmalıdır.
* **Dikkatli tümleştirmek**: kitaplıklarını ve API'lerini sınırında birçok yazılım güvenlik açıkları vardır. Geçerli dağıtım için gerekli olmayabilir işlevselliği hala bir API katmanı kullanılabilir. Genel güvenlik sağlamak için tüm arabirimler için güvenlik açıkları tümleştirilmekte bileşenlerinin denetlemek emin olun.

## <a name="iot-solution-deployer"></a>IOT çözüm dağıtıcı

IOT çözüm dağıtımcılar için en iyi yöntemler şunlardır:

* **Donanım güvenli bir şekilde dağıtmak**: IOT dağıtımları ortak alanları ya da Denetimsiz yerel ayarlara olduğu gibi güvenli olmayan konumlarda dağıtılacak donanım gerektirebilir. Böyle durumlarda, donanım dağıtım yetkisiz değiştirmeye karşı kanıt olduğundan emin olun azami ölçüde. Donanımda USB veya diğer bağlantı noktaları varsa, bunlar güvenli bir şekilde ele emin olun. Birçok saldırı vektörüne giriş noktaları olarak kullanabilirsiniz.
* **Kimlik doğrulama anahtarlarını güvenli kalmasına**: dağıtım sırasında her cihaz cihaz kimlikleri gerektirir ve kimlik doğrulaması anahtarları bulut hizmeti tarafından oluşturulan ilişkili. Bu anahtarları daha sonra dağıtım fiziksel olarak güvenli tutun. Güvenliği aşılmış bir tuşa kötü amaçlı bir aygıt tarafından geçici var olan bir cihaz için kullanılabilir.

## <a name="iot-solution-operator"></a>IOT çözüm işleci

IOT çözüm işleçleri için en iyi yöntemler şunlardır:

* **Sistem güncelliğini**: cihaz işletim sistemleri ve tüm aygıt sürücülerini en son sürümlerine yükseltildiğinden emin olun. Windows 10 (IOT veya diğer SKU'ları) otomatik güncelleştirmeler kapatırsanız, Microsoft güncel, IOT cihazları için güvenli bir işletim sistemi sağlama tutar. Diğer işletim sistemleri (örneğin, Linux) tutma güncel sağlamaya yardımcı olur bunlar aynı zamanda kötü amaçlı saldırılara karşı korunur.
* **Kötü amaçlı etkinliği karşı koruma**: işletim sistemi izin veriliyorsa, en son virüsten koruma ve kötü amaçlı yazılımdan koruma özellikleri her cihaz işletim sistemine yükleyin. Bu yöntem en dış tehditlerin azaltılmasına yardımcı olabilir. Uygun adımları uygulayarak Çoğu modern işletim sistemi tehditlere karşı koruyabilirsiniz.
* **Sık denetim**: denetim IOT altyapı güvenlik ilgili sorunlar olduğunda anahtar güvenlik olaylarını yanıtlama. Çoğu işletim sistemleri, hiçbir güvenlik ihlali oluştu emin olmak için sık gözden geçirilmesi gereken yerleşik olay günlüğü sağlar. Denetim bilgilerini ayrı telemetri akışı olarak burada çözümlenmesi bulut hizmetine gönderilebilir.
* **Fiziksel olarak IOT altyapısını koruma**: cihazlara fiziksel erişim kullanarak IOT altyapı kötü güvenlik saldırıları başlatılabilir. Bir önemli güvenlik USB bağlantı noktalarına kötü niyetli kullanımına ve diğer fiziksel erişim karşı korumak için bir uygulamadır. USB bağlantı noktası kullanımı gibi fiziksel erişim oluşmuş olabilir uncovering ihlallerinden için bir anahtar günlüğü. Yeniden, Windows 10 (IOT ve diğer SKU'ları) bu olayların ayrıntılı günlük kaydını etkinleştirir.
* **Bulut kimlik bilgileri korumaya**: Bulut kimlik doğrulaması kimlik bilgilerini yapılandırmak ve bir IOT dağıtım işletim için kullanılan olan büyük olasılıkla erişebilir ve bir IOT sistemden yapmanın en kolay yolu. Parola sık değiştirerek kimlik bilgileri korumak ve ortak makinelerde bu kimlik bilgileri kullanılarak kullanmamalıdır.

Farklı IOT cihaz özelliklerini farklılık gösterir. Bazı aygıtlar ortak masaüstü işletim sistemlerini çalıştıran bilgisayarlar olabilir ve bazı cihazların çok hafif işletim sistemlerini çalıştıran. Daha önce açıklanan en iyi güvenlik uygulamaları değişen derece cinsinden bu aygıtlar için geçerli olabilir. Sağlanırsa, ek güvenlik ve dağıtım en iyi uygulamalar, bu cihazların üreticilerin gelmelidir.

Bazı eski ve kısıtlı cihazlar IOT dağıtımı için özellikle tasarlanmış olabilir değil. Bu cihazların verileri şifrelemek, Internet ile bağlanmak veya gelişmiş denetim sağlamak yeteneği olmaması. Bu durumlarda, modern ve güvenli alan ağ geçidi eski cihazlardan veri toplama ve bu cihazların Internet üzerinden bağlanmak için gereken güvenlik sağlar. Alan ağ geçitleri, güvenli kimlik doğrulaması, şifrelenmiş oturumlarının anlaşma, Bulut ve birçok güvenlik özelliği komutları alınmasını sağlayabilir.