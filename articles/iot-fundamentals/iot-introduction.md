---
title: Azure ve Nesnelerin İnterneti (IoT) Tanıtımı
description: Azure IoT'ye ve içindeki hizmetlere genel bakış
author: robinsh
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 10/09/2018
ms.author: robinsh
ms.openlocfilehash: d7e5949be9ea09f94ff6ee86c9f996f2e203eac7
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069446"
---
# <a name="introduction-to-azure-and-the-internet-of-things"></a>Azure ve Nesnelerin İnterneti Tanıtımı

Azure IoT, üç teknoloji ve çözüm alanından oluşur. Çözümler, platform hizmetleri ve edge’in tamamı, IoT uygulamanızın uçtan uca gelişimini kolaylaştırmak için tasarlanmıştır. Bu makale, buluttaki bir IoT çözümünün genel özelliklerinin açıklanmasıyla başlar, ardından Azure IoT’un, IoT projelerindeki zorluklara nasıl yanıt verdiğine ve neden Azure IoT’u benimsemeniz gerektiğine dair bir genel bakışla devam eder.

## <a name="iot-solution-architecture"></a>IOT çözüm mimarisi

IoT çözümleri, sayıları milyonları bulabilecek cihazlar arasında güvenli, çift yönlü iletişim ve çözüm arka ucu gerektirir. Örneğin, bir çözüm cihazdan buluta olay akışınızdan bilgileri açığa çıkarmak için otomatik, tahmine dayalı analizleri kullanabilir. 

Aşağıdaki diyagram tipik bir IoT çözüm mimarisinin temel öğelerini göstermektedir. Diyagram, kullanılan Azure hizmetleri ve cihaz işletim sistemleri gibi belirli uygulama ayrıntılarından bağımsızdır. Bu mimaride, IoT cihazları bulut ağ geçidine gönderdikleri verileri toplar. Bulut ağ geçidi, verileri diğer arka uç hizmetleri tarafından işlenebilir hale getirir. Bu arka uç hizmetleri şuralara veri iletebilir:

* Diğer iş kolu uygulamaları.
* Pano veya başka bir sunu cihazı üzerinden insan kullanıcılar.

![IOT çözüm mimarisi](./media/iot-introduction/iot-reference-architecture.png)

> [!NOTE]
> IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi](https://aka.ms/iotrefarchitecture).

### <a name="device-connectivity"></a>Cihaz bağlantısı

Bir IoT çözüm mimarisinde cihazlar genellikle depolama ve işleme amacıyla buluta telemetri gönderir. Örneğin, Tahmine dayalı bakım senaryosunda çözüm arka ucu, belirli bir pompanın ne zaman bakıma gerek duyacağını saptamak için sensör verilerinin akışını kullanabilir. Cihazlar, bulut uç noktasına ait iletileri okuyarak buluttan cihaza iletileri de alıp yanıtlayabilir. Aynı örnekte çözüm arka ucu, bakımın başlamasından hemen önce akışların yeniden yönlendirilmesini başlatmak amacıyla pompa istasyonundaki diğer pompalara da ileti gönderebilir. Bu yordam, bakım mühendisinin ulaşır ulaşmaz işe başlamasını sağlar.

IoT çözümlerinde görülen en büyük zorluk genellikle cihazları güvenli ve güvenilir bir şekilde bağlamaktır. Bunun nedeni, tarayıcılar ve mobil uygulamalar gibi diğer istemcilerle karşılaştırıldığında IoT cihazlarında farklı özelliklerin bulunmasıdır. IoT cihazları şu özelliklere sahiptir:

* Genellikle insan kullanıcısı bulunmayan katıştırılmış sistemlerdir (bir telefonun aksine).
* Fiziksel erişimin pahalı olduğu uzak konumlarda dağıtılabilir.
* Yalnızca çözüm arka ucu aracılığıyla erişilebilir. Cihazla etkileşime geçmek için başka bir yol yoktur.
* Sınırlı güç ve işleme kaynaklarına sahip olabilir.
* Aralıklı, yavaş veya pahalı bir ağ bağlantısına sahip olabilir.
* Mülkiyete ait, özel veya sektöre özgü uygulama protokolleri kullanması gerekebilir.
* Büyük bir popüler donanım ve yazılım platformu kümesi kullanılarak oluşturulabilir.

Önceki kısıtlamalara ek olarak, tüm IoT çözümlerinin aynı zamanda ölçeklenebilir, güvenli ve güvenilir olması gerekir.

İletişim protokolüne ve ağ kullanılabilirliğine bağlı olarak, bir cihaz doğrudan veya bir ara ağ geçidi üzerinden bulutla iletişim kurabilir. IoT mimarileri genellikle bu iki iletişim deseninin bir karışımını kullanır.

### <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

Modern IoT çözümlerinde veri işleme, bulutta veya cihaz tarafında gerçekleşebilir. Cihaz tarafında işleme, *Uç bilgi işlem* olarak adlandırılır. Verilerin işleneceği yerle ilgili seçim aşağıdaki gibi etkenlere bağlıdır:

* Ağ kısıtlamaları. Cihazlar ile bulut arasındaki bant genişliği sınırlı ise, daha fazla uç işleme yapmak için teşvik vardır.
* Yanıt süresi. Neredeyse gerçek zamanlı olarak cihaz üzerinde işlem yapma gereksinimi varsa, yanıtı cihazın içinde işlemek daha iyi olabilir. Örneğin, acil durumda durdurulması gereken bir robot kolu.
* Yasal ortam. Bazı veriler buluta gönderilemez.

Genel olarak, hem uç hem de bulutta veri işleme aşağıdaki özelliklerin bir birleşimidir:

* Telemetriyi cihazlarınızdan ölçekli alma ve bu verilerin nasıl işleneceğini ve depolanacağını saptama.
* Gerçek zamanlı veya olaydan sonra olmasına bakılmaksızın öngörüler sağlamak için telemetriyi analiz etme.
* Buluttan veya bir ağ geçidi cihazından belirli bir cihaza komut gönderme.

Ayrıca, bir IoT bulut arka ucu şunları sağlamalıdır:

* Aşağıdakileri yapmanıza olanak tanıyan cihaz kayıt özellikleri:
    * Cihaz sağlama.
    * Altyapınıza bağlanmasına izin verilen cihazları denetleme.
* Cihazlarınızın durumunu denetlemeye ve etkinliklerini izlemenize yönelik cihaz yönetimi.

Örneğin, tahmine dayalı bakım senaryosunda bulut arka ucu geçmiş telemetri verilerini depolar. Çözüm, bu verileri kullanarak belirli pompalardaki anormal davranışları gerçek bir soruna neden olmadan önce tanımlar. Veri analizi kullanarak, önleyici çözümün düzeltici bir eylem gerçekleştirmek için cihaza yeniden komut göndermek olduğunu belirleyebilir. Bu işlem, cihaz ile bulut arasında çözüm verimliliğini önemli ölçüde artıran otomatik bir geri bildirim döngüsü oluşturur.

### <a name="presentation-and-business-connectivity"></a>Sunu ve iş bağlantısı

Sunu ve iş bağlantı katmanı son kullanıcıların IoT çözümü ve cihazlarla etkileşime geçmesini sağlar. Kullanıcıların kendi cihazlarından toplanan verileri görüntülemelerini ve çözümlemelerini sağlar. Bu görünümler panolar veya hem geçmiş verileri, hem de yakın gerçek zamanlı verileri görüntüleyebilen BI raporu biçiminde olabilir. Örneğin, bir kullanıcı belirli bir pompa istasyonunun durumunu denetleyebilir ve sistem tarafından gerçekleştirilen tüm uyarıları görebilir. Bu katman, kurumsal iş süreçlerine veya iş akışlarına bağlanmak üzere var olan iş kolu uygulamalarına sahip IoT çözüm arka ucunun tümleştirilmesini de sağlar. Örneğin, tahmine dayalı bakım bir çözümü bir pompaya bakım gerektiğini tanımladığında mühendisin pompayı ziyaretini ayarlayan zamanlama sistemiyle tümleştirilebilir.

## <a name="why-azure-iot"></a>Neden Azure IoT?

Azure IoT, IoT projelerinin karmaşıklığını kolaylaştırır ve güvenlik, altyapı uyumsuzluğu ve IoT çözümünüzü ölçeklendirme gibi zorluklara yanıt verir. Nasıl olacağı aşağıda verilmiştir:

### <a name="agile"></a>Çevik

IoT yolculuğunuza hız kazandırın.

* Ölçek: Küçük bir ölçekle başlayın, dünyanın birçok bölgesinde her zaman her yerde milyonlarca cihaz ve terabaytlarca veriyle istediğiniz boyuta yükseltin.

* Açık: Sahip olduklarınızı kullanın veya istediğiniz cihaza, yazılıma ya da hizmete bağlanarak, gelecek için modernleşme gerçekleştirin.

* Hibrit: Edge üzerinde, bulutta veya bunların arasında bir yerde IoT çözümünüzü dağıtarak ihtiyaçlarınıza göre derleme yapın.

* Hız: Çözüm hızlandırıcıları ve IoT alanındaki yenilik hızı lideri sayesinde daha hızlı dağıtım gerçekleştirin, pazara giriş süresini hızlandırın ve rekabette öne çıkın.

### <a name="comprehensive"></a>Kapsamlı

İşletmeniz için etki yaratın.

* Tam: Microsoft, cihazdan buluta ve büyük verilerden, gelişmiş analize ve yönetilen hizmetlere kadar yayılan eksiksiz bir platform sunan tek IoT çözüm sağlayıcısıdır.

* Başarıya götüren iş ortağı: Dünyanın en büyük iş ortağı ekosisteminin gücünden yararlanın ve sektörde ve dünyada iş kolunu ve teknolojiyi hayata geçirin.

* Veri odaklı: IoT, verilerle ilgilidir ve en iyi IoT çözümleri, verileri depolamak, yorumlamak, dönüştürmek, analiz etmek ve doğru zamanda, doğru yerde, doğru kullanıcıya sunmak için ihtiyaç duyduğunuz tüm araçları bir arada sunar.

* Cihaz odaklı: Microsoft IoT, eski ekipmandan çok geniş bir sertifikalı donanım ekosistemine kadar her şeyin bağlantısını oluşturmanıza olanak sağlar ve edge, mobil ve tümleşik sistemlerde kendi cihazlarınızı derleme yeteneği sunar.

### <a name="secure"></a>Güvenlik

IoT’un en zorlu kısmını oluşturan güvenlik konusunu çözün.

* Güç kazandırma: Microsoft IoT sayesinde, teknoloji, en iyi uygulamalar ve IoT’un en zorlu kısmını oluşturan güvenlik konusunu çözme yetenekleriyle vizyonunuzu bir araya getirebilirsiniz.

* Harekete geçme: Kimlik ve erişim yönetimi, tehdit ve bilgi koruması ve güvenlik yönetimi ile IoT verilerinizin güvenliğini sağlayın ve riski yönetin.

* İç rahatlığı: Cihazlar, yazılımlar, uygulamalar, bulut hizmetleri ve şirket içi ortamlar arasında hassas bilgilerin güvenliğini sağlayın.

* Uyumluluk: Microsoft, IoT cihazları, verileri ve hizmetleri için geniş çapta uluslararası ve sektöre özgü standartları karşılayan güvenlik gereksinimleri oluşturma konusunda sektör lideridir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki teknoloji ve çözüm alanlarını keşfedin.

**Çözümler**

* [IoT çözüm hızlandırıcıları](/azure/iot-suite)
* [IoT Central](/azure/iot-central)

**Platform hizmetleri**

* [IoT Hub’ı](/azure/iot-hub)
* [IoT Hub Cihazı Sağlama Hizmeti](/azure/iot-dps)
* [Azure Haritalar](/azure/azure-maps/)
* [Time Series Insights](/azure/time-series-insights)

**Edge**

* [IoT Edge'e Genel Bakış](/azure/iot-edge)
* [IoT Edge Nedir?](/azure/how-iot-edge-works)
