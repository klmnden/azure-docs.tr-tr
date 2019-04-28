---
title: Nesnelerin interneti (IOT) Azure İnternet'e giriş
description: Azure IOT ve Yardım örnekler de dahil olmak üzere IOT hizmetleriyle temelleri açıklayan giriş IOT kullanımını göstermektedir.
author: robinsh
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 10/11/2018
ms.author: robinsh
ms.openlocfilehash: e1cb588d68153a88d8b55b2696b376c4eb8704f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61232260"
---
# <a name="what-is-azure-internet-of-things-iot"></a>Azure nesnelerin interneti (IOT) nedir?

Azure Nesnelerin İnterneti (IoT), Microsoft tarafından yönetilen ve milyarlarca IoT varlığını bağlayan, izleyen ve denetleyen bulut hizmetlerinden oluşan bir koleksiyondur. Daha basit bir deyişle, bir IoT çözümü bulutta çalışan ve birbiriyle iletişim halinde olan bir veya daha çok IoT cihazından ve arka uç hizmetinden oluşur. 

Bu makalede, IOT temelleri açıklanır, kullanım örnekleri hakkında konuşuyor ve sekiz farklı hizmetler kullanılabilir kısaca açıklanmaktadır. Nelerin kullanılabildiğini anlama tarafından yakından yardımcı olmak üzere daha senaryonuz aramak istediğiniz anlayabilir.

## <a name="introduction"></a>Giriş

Bir IOT çözümü ana bölümleri şunlardır: cihazları, arka uç Hizmetleri ve ikisi arasındaki iletişim. 

### <a name="iot-devices"></a>IoT cihazları

Cihazlar genellikle İnternet'e bir bağlantı hattı panonun bağlı sensörlerden ile oluşur. Çok sayıda cihaz, bir Wi-Fi yonga iletişim kurar. IOT cihazları bazı örnekleri aşağıda verilmiştir:

* bir uzak Petrol pompası baskısı sensörlerini
* Sıcaklık ve nem sensörlerden air-conditioning birim
* ivmeölçer bir Asansör içinde
* Durum algılayıcılar odada

Prototip oluşturma için sık kullanılan iki temel MX yonga IOT Devkit cihazlardan, Microsoft ile Raspberry PI cihazlardır. MX yonga Devkit sıcaklık, baskısı, nem, hem de bir ilgili jiroskop ve ivme ölçer, bir magnetometer ve Wi-Fi yonga için yerleşik algılayıcılara sahiptir. Raspberry PI senaryonuz için gerekenleri tam olarak seçebilmeniz için sensörlerden, birçok farklı türde ekleyebilirsiniz, IOT bir cihazdır. 

Endüstrinin en büyük kullanılabilir IOT cihazları hakkında daha fazla bilgi için kontrol [IOT için sertifikalı cihaz Kataloğu](https://catalog.azureiotsolutions.com/alldevices).

[IOT cihaz SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) , gereksinim duyduğunuz görevleri yapabilmeniz cihazlarınızda çalışan uygulamalar oluşturmanıza olanak tanır. SDK'ları ile IOT hub'ınıza telemetri göndermek, IOT Hub'ından iletiler ve güncelleştirmeleri almak ve benzeri.

### <a name="communication"></a>İletişim

Cihazınız, her iki yönde de arka uç Hizmetleri ile iletişim kurabilir. Arka uç çözümüyle cihaz kuran gösteren örnekleri aşağıda verilmiştir.

#### <a name="examples"></a>Örnekler 

* Cihazınızı sıcaklık bir mobil soğutma kamyon 5 dakikada bir IOT hub'a gönderebilir. 

* Arka uç hizmeti, bir sorunun tanılanmasına yardımcı olmak için daha sık telemetri göndermek için cihaz sorabilirsiniz. 

* Cihazınız, algılayıcılardan okuma değerlere göre uyarıları gönderebilirsiniz. Örneğin, bir batch reaktör bulunan bir kimyasal izliyorsanız, Sıcaklıkların belirli bir değeri aştığında bir uyarı göndermek isteyebilirsiniz.

* Cihazınızı bilgileri için bir Pano için İnsan operatörler tarafından görüntüleme gönderebilirsiniz. Örneğin, bir refinery denetim odasında sıcaklığı ve Basıncı izlemesini işlece izin vermeme bu kanalı üzerinden akan birim yanı sıra her kanal gösterebilir. 

Bu görevler ve daha fazlasını kullanarak uygulanabilir [IOT cihaz SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).

#### <a name="connection-considerations"></a>Bağlantı konuları

IoT çözümlerinde görülen en büyük zorluk genellikle cihazları güvenli ve güvenilir bir şekilde bağlamaktır. IOT cihazları tarayıcılar ve mobil uygulamalar gibi diğer istemcilerle karşılaştırıldığında farklı özelliklere sahip olmasıdır. IoT cihazları şu özelliklere sahiptir:

* Genellikle insan kullanıcısı bulunmayan katıştırılmış sistemlerdir (bir telefonun aksine).

* Fiziksel erişimin pahalı olduğu uzak konumlarda dağıtılabilir.

* Yalnızca çözüm arka ucu aracılığıyla erişilebilir. Cihazla etkileşime geçmek için başka bir yol yoktur.

* Sınırlı güç ve işleme kaynaklarına sahip olabilir.

* Aralıklı, yavaş veya pahalı bir ağ bağlantısına sahip olabilir.

* Mülkiyete ait, özel veya sektöre özgü uygulama protokolleri kullanması gerekebilir.

### <a name="back-end-services"></a>Arka uç Hizmetleri 

Arka uç hizmetinin sağlayabileceği işlevlerin bazıları aşağıda verilmiştir.

* Cihazlarınızdan ölçekleyerek telemetri alma ve bu veri depolamak ve işlemek nasıl belirleme.

* Gerçek zamanlı veya olaydan sonra Öngörüler sağlamak için telemetriyi analiz etme.

* Komutları buluttan belirli bir cihaza gönderme. 

* Cihazlar ve hangi cihazların altyapıya bağlanabilir denetim sağlama.

* Cihazlarınızın durumunu denetleyebilir ve bunların etkinliklerini izleyin.

Örneğin, bir Tahmine dayalı bakım senaryosunda bulut arka ucu geçmiş telemetri depolar. Çözüm, bu verileri kullanarak belirli pompalardaki anormal davranışları gerçek bir soruna neden olmadan önce tanımlar. Veri analizi kullanarak, önleyici çözümün düzeltici bir eylem gerçekleştirmek için cihaza yeniden komut göndermek olduğunu belirleyebilir. Bu işlem, cihaz ile bulut arasında çözüm verimliliğini önemli ölçüde artıran otomatik bir geri bildirim döngüsü oluşturur.

## <a name="an-iot-example"></a>IOT örneği

İşte bir şirket örneği IOT milyonlarca dolar tasarruf için kullanılır. 

Bir büyük cattle ranch yüz binlerce İnekler yoktur. Bu, diğer birçok İnekler izlemenize ve nasıl bunlar yapılması ve çok sayıda geçici sürüş gerektirir bildiğiniz bir sorun teşkil olur. Bir arka uç hizmeti veritabanına yazılması GPS koordinatlarını ve sıcaklık gibi bilgileri göndermesini her tek inek sensörlerden iliştirilmiş.

Ardından gelen veri tarayan ve aşağıdaki gibi sorulara denetlemek her inek verileri analiz eden analitik bir hizmet vardır:

* Bir sıcaklık inek çalışıyor mu? Ne kadar süreyle inek bir sıcaklık çalıştırıldıktan? Bir günden daha uzun süredir, GPS koordinatlarını edinmek ve inek bulun ve uygunsa antibiyotikleri ile ele almanız gidin. 

* İnek aynı yerde birden fazla bir gün için mi? Bu durumda, GPS koordinatlarını edinmek ve inek Bul gidin. İnek bir Uçurum dışına düştükten? İnek yapılan saldırıda? İnek Yardım gerekiyor mu? 

Bu IOT çözüm uygulama çok fazla para kaydetmeden kendi hayvanlar üzerinde denetimi etrafında sürüş harcayabileceğiniz sahip oldukları süreyi Kes denetleyin ve İnekler hızlı bir şekilde ele almanız şirket için mümkün kıldı. Şirketler IOT kullanma daha fazla gerçek zamanlı konuşmaların örnekler için bkz [Microsoft Teknik örnek olay incelemeleri için IOT](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured). 

## <a name="iot-services"></a>IoT Hizmetleri

Azure IOT ile ilgili birkaç hizmet mevcuttur ve kullanmak istediğiniz hangi birini anlamak kafa karıştırıcı olabilir. IOT Central hem de IOT Çözüm Hızlandırıcıları gibi bazı kendi çözümünüzü oluşturabilir ve hızla işe koyulun yardımcı olması için şablonlar sağlar. Kullanılabilir diğer hizmetleri kullanarak kendi çözümlerinizi da tam olarak geliştirebilirsiniz--bağlıdır ne kadar Yardım istediğiniz ve ne kadar denetim. Kullanılabilir hizmetlerin listesini İşte yanı sıra bunları için kullandığınız.

1. [**IOT Central**](../iot-central/overview-iot-central.md): Bu bağlayın, izleyin ve IOT cihazlarınızı yönetmenize yardımcı olan bir SaaS çözümüdür. Başlamak için oluşturma ve cihazın işleçleri kullanan temel bir IOT Central uygulamasına test cihaz türünüz için bir şablon seçin. IOT Central uygulamasına aygıtlarını izlemek ve yeni cihazları hazırlama de olanak sağlar. Ayrıntılı hizmet özelleştirmesi gerektirmeyen basit çözümleri hizmetidir. 

2. [**IOT Çözüm Hızlandırıcıları**](/azure/iot-suite): Bu, bir IOT çözümü geliştirme sürecinizi hızlandırmak için kullanabileceğiniz PaaS çözümlerini oluşan bir koleksiyondur. Sağlanan bir IOT çözüm ile Başlat ve bu çözüm gereksinimlerinizi tam olarak özelleştirebilirsiniz. Arka uç özelleştirmek için Java veya .NET becerileri ve görselleştirmeyi özelleştirmek için JavaScript becerileri gerekir. 

3. [**IOT hub'ı**](/azure/iot-hub/): Bu hizmet bir IOT hub'ı ve izleme ve denetim milyarlarca IOT cihazlarının cihazlarınızdan bağlanmanıza olanak tanır. IOT cihazlarınızı ve arka ucunuz çift yönlü iletişimi gerekiyorsa bu özellikle yararlıdır. IOT Central hem de IOT Çözüm Hızlandırıcıları için temel alınan hizmete budur. 

4. [**IOT Hub cihazı sağlama hizmeti**](/azure/iot-dps/): IOT Hub, IOT hub'ınıza cihazların güvenli bir şekilde sağlamak için kullanabileceğiniz bir yardımcı hizmettir budur. Bu hizmet ile kolayca milyonlarca cihaza hızla, birer birer sağlama yerine sağlayabilirsiniz. 

5. [**IOT Edge**](/azure/iot-edge/): Bu hizmet, IOT Hub temelinde geliştirilen. IOT cihazları yerine buluttaki verileri çözümlemek için kullanılabilir. İş yükünüz bölümlerini uca taşıyarak, daha az ileti buluta gönderilmesi gerekir. 

6. [**Azure dijital İkizlerini**](../digital-twins/index.yml): Bu hizmet, kapsamlı modelleri, fiziksel ortam oluşturmanıza olanak sağlar. Kişiler, boşluk ve cihazlar arasındaki etkileşimler ve ilişkiler modelleyebilirsiniz. Örneğin, bakım için bir Fabrika, bir Elektrik şebekesi için gerçek zamanlı enerji gereksinimlerini çözümlemek veya bir office için kullanılabilir alanı kullanımını en iyi duruma getirme tahmin edebilirsiniz.

7. [**Zaman serisi öngörüleri**](/azure/time-series-insights): Bu hizmet, zaman serisi verilerini IOT cihazlar tarafından oluşturulan büyük miktarda sorgu depolamak ve görselleştirmenize olanak sağlar. Bu hizmeti ile IOT hub'ı kullanabilirsiniz. 

8. [**Azure haritalar**](/azure/azure-maps): Bu hizmet, web ve mobil uygulamalara coğrafi bilgileri sağlar. Tam bir dizi REST API'leri ve bunun yanı sıra hem Apple hem de Windows cihazlar için masaüstü ve mobil uygulamalar üzerinde çalışan esnek uygulamalar oluşturmak için kullanılan web tabanlı bir JavaScript denetimiyle yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bazı gerçek iş örnekleri ve kullanılan mimarisi için bkz: [Microsoft Azure IOT teknik incelemeleri](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured).

İle bir IOT DevKit deneyebileceğiniz bazı örnek projeler için bkz: [IOT DevKit proje Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/). 

Farklı Hizmetleri ve nasıl kullanıldıkları daha kapsamlı bir açıklama için bkz. [Azure IOT Hizmetleri ve teknolojileri](iot-services-and-technologies.md).

IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi](https://aka.ms/iotrefarchitecture).
