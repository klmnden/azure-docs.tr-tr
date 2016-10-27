<properties
 pageTitle="IoT Hub cihaz yönetimine genel bakış | Microsoft Azure"
 description="Bu makalede Azure IoT Hub’daki cihaz yönetimine genel bakış sunulmaktadır: kurumsal cihaz yaşam döngüsü, yeniden başlatma, fabrika sıfırlaması, üretici yazılımı güncelleştirmesi, yapılandırma, cihaz çiftleri, sorgular, işler"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>


# <a name="overview-of-azure-iot-hub-device-management-(preview)"></a>Azure IoT Hub cihaz yönetimine genel bakış (önizleme)

## <a name="introduction"></a>Giriş

Azure IoT Hub, cihaz ve arka uç geliştiricilerinin sağlam IoT cihaz yönetimi çözümleri oluşturmasını sağlayan özellikler ve bir genişletilebilirlik modeli sunar. IoT cihazları kısıtlı algılayıcılardan tek amaçlı mikro denetleyicilere ve cihaz grupları için iletişimi yönlendiren güçlü ağ geçitlerine varan çeşitler barındırır.  Ayrıca, kullanım örnekleri ve IoT operatörlerinin gereksinimleri sektörler arasında farklılık gösterir.  Bu farklılığa rağmen, Azure IoT Hub cihaz yönetimi çok çeşitli cihaz ve son kullanıcılara uygun özellikler, desenler ve kod kitaplıkları sağlar.

Başarılı bir kurumsal IoT çözümü oluşturmanın önemli bir kısmı, operatörlerin cihaz koleksiyonu için devam eden yönetimi nasıl gerçekleştirdiğine ilişkin bir strateji sağlanmasıdır. IoT operatörleri, işlerinin daha stratejik yönlerine odaklanmalarını sağlayan basit ve güvenilir araç ve uygulamalar gerektirir. Bu makalede aşağıdakiler sunulmaktadır:

- Azure IoT Hub cihaz yönetimi yaklaşımına kısa bir genel bakış.
- Genel cihaz yönetimi ilkelerinin açıklaması.
- Cihaz yaşam döngüsü açıklaması.
- Genel cihaz yönetimi desenlerine genel bakış.

## <a name="iot-device-management-principles"></a>IoT cihaz yönetimi ilkeleri

IoT bir dizi benzersiz cihaz yönetimi zorluğunu beraberinde getirir ve kurumsal sınıftaki her çözüm aşağıdaki ilkeleri hesaba katmalıdır:

![Azure IoT Hub cihaz yönetimi ilkeleri grafiği][img-dm_principles]

- **Ölçek ve otomasyon**: IoT çözümleri, rutin görevleri otomatik hale getirebilen ve oldukça küçük bir operasyon ekibinin milyonlarca cihazı yönetmesine olanak tanıyabilen basit araçlar gerektirir. Operatörler toplu cihaz işlemlerini günlük olarak uzaktan gerçekleştirmeyi ve yalnızca doğrudan dikkat gerektiren sorunlar oluştuğunda uyarılmayı beklemektedir.

- **Açıklık ve uyumluluk**: IoT cihaz ekosistemi olağanüstü çeşitliliğe sahiptir. Yönetim araçları çok sayıda cihaz sınıfına, platforma ve protokole uyum sağlayacak şekilde uyarlanmalıdır. Operatörler en kısıtlı katıştırılmış tek işlemli yongalardan güçlü ve tam işlevsel bilgisayarlara kadar çok sayıda cihaz türünü destekleyebilmelidir.

- **Bağlam tanıma**: IoT ortamları dinamik ve sürekli değişen yapıdadır. Hizmet güvenilirliği üst düzey öneme sahiptir. Cihaz yönetimi işlemleri; bakım kaynaklı kapalı kalma süresinin kritik iş işlemlerini etkilemediğinden veya tehlikeli koşullar oluşturmadığından emin olmak üzere SLA bakım pencerelerini, ağ ve güç durumlarını, kullanım sırasındaki koşulları ve cihazın coğrafi konumunu hesaba katmalıdır.

- **Çok sayıda servis rolü**: Benzersiz iş akışları ve IoT işlemleri için destek çok önemlidir. Operasyon personeli, şirket içi BT bölümlerinin belirtilen kısıtlamalarıyla uyumlu bir şekilde çalışmalıdır.  Ayrıca, denetçilere ve diğer iş yönetimi rollerine gerçek zamanlı cihaz çalışma bilgilerini sunmanın uygun yollarını bulmalıdır.

## <a name="iot-device-lifecycle"></a>IoT cihazı yaşam döngüsü

Tüm kurumsal IoT projelerinde ortak olan genel cihaz yönetimi aşamaları vardır. Azure IoT içinde IoT cihaz yaşam döngüsünün beş aşaması vardır:

![Beş Azure IoT cihaz yaşam döngüsü aşaması şunlardır: planlama, sağlama, yapılandırma, izleme, devre dışı bırakma][img-device_lifecycle]

Bu beş aşamanın her birinde, tam bir çözüm sağlamak için yerine getirilmesi gereken birkaç cihaz operatörü gereksinimi vardır:

- **Plan**: Operatörlerin toplu yönetim işlemleri için bir cihaz grubunu kolayca ve doğru bir şekilde sorgulamasına ve hedeflemesine olanak tanıyan cihaz meta verileri düzeni oluşturmasını sağlar. Bu cihaz meta verilerini etiketler ve özellikler halinde depolamak için cihaz ikizlerini kullanabilirsiniz.

    *Daha fazla okuma*: [Cihaz ikizlerini kullanmaya başlama][lnk-twins-getstarted], [Cihaz ikizlerini anlama][lnk-twins-devguide], [İkiz özelliklerini kullanma][lnk-twin-properties]

- **Sağlama**: IoT Hub’ına yeni cihazları güvenli bir şekilde sağlar ve operatörlerin cihaz özelliklerini hemen bulmasına olanak tanır.  Esnek cihaz kimlikleri ve kimlik bilgileri oluşturmanın yanı sıra bu işlemi bir iş kullanarak toplu halde gerçekleştirmek için IoT Hub cihazı kayıt defterini kullanın. Cihaz ikizindeki cihaz özellikleri aracılığıyla kapasite ve koşullarını raporlamak için cihazlar oluşturun.

    *Daha fazla okuma*: [Cihaz ikizlerini yönetme][lnk-identity-registry], [Cihaz kimliklerinin toplu yönetimi][lnk-bulk-identity], [İkiz özelliklerini kullanma][lnk-twin-properties]

- **Yapılandırma**: Cihazların hem sistem durumunu hem de güvenliğini korurken toplu yapılandırma değişikliklerini ve üretici yazılımı güncelleştirmelerini kolaylaştırır. İstediğiniz özellikleri kullanarak ve doğrudan yöntemler ve yayın işleri ile bu cihaz yönetimi işlemlerini toplu olarak gerçekleştirin.

    *Daha fazla okuma*:  [Doğrudan yöntemler kullanma][lnk-c2d-methods], [Bir cihazda doğrudan yöntem çağırma][lnk-methods-devguide], [İkiz özelliklerini kullanma][lnk-twin-properties], [İşleri zamanlama ve yayınlama][lnk-jobs], [İşleri birden fazla cihazda zamanlama][lnk-jobs-devguide]

- **İzleme**: Operatörleri dikkat gerektirebilecek sorunlar konusunda uyarmak için genel cihaz koleksiyonu durumunu ve devam eden işlemlerin durumunu izler.  Cihazların gerçek zamanlı çalışma koşullarını ve güncelleştirme işlemlerinin durumunu raporlamasına olanak tanımak üzere cihaz ikisi uygulayın. Cihaz ikizi sorgularını kullanarak en acil sorunları ortaya çıkaran güçlü pano raporları oluşturun.

    *Daha fazla okuma*: [İkiz özelliklerini kullanma][lnk-twin-properties], [İkizler ve işler için sorgu dili][lnk-query-language]

- **Devre dışı bırakma**: Bir hata ya da yükseltme döngüsü sonrasında veya hizmet ömrünün sonunda cihazları değiştirin ya da kullanımdan kaldırın.  Fiziksel cihaz değiştiriliyorsa cihaz bilgilerini korumak veya kullanım dışı bırakılıyorsa cihaz bilgilerini arşivlemek için cihaz ikizini kullanın. Cihaz kimliklerini ve kimlik bilgilerini güvenli bir şekilde iptal etmek için IoT Hub cihaz kayıt defterini kullanın.

    *Daha fazla okuma*: [İkiz özelliklerini kullanma][lnk-twin-properties], [Cihaz kimliklerini yönetme][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT Hub cihaz yönetimi modelleri

IoT Hub aşağıdaki cihaz yönetim modellerini sağlar.  [Cihaz yönetimi öğreticileri][lnk-get-started], bu desenleri gerçek senaryonuza uygun hale getirme ve bu çekirdek şablonları temel alan yeni desenler tasarlama konularını daha ayrıntılı olarak gösterir.

- **Yeniden başlatma** - Arka uç uygulaması, yeniden başlatma işleminin başlatıldığını bir doğrudan yöntem aracılığıyla cihaza bildirir.  Cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın yeniden başlatma durumunu güncelleştirir.

    ![Azure IoT Hub cihaz yönetimi yeniden başlatma deseninin grafiği][img-reboot_pattern]

- **Fabrika Sıfırlaması** - Arka uç uygulaması, fabrika sıfırlamasının başlatıldığını bir doğrudan yöntem aracılığıyla cihaza bildirir.  Cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın fabrika sıfırlama durumunu güncelleştirir.

    ![Azure IoT Hub cihaz yönetimi fabrika sıfırlaması deseninin grafiği][img-facreset_pattern]

- **Yapılandırma** - Arka uç uygulaması, cihaz çiftinin istenen özelliklerini kullanarak cihaz üzerinde çalışan yazılımı yapılandırır.  Cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın yapılandırma durumunu güncelleştirir.

    ![Azure IoT Hub cihaz yönetimi yapılandırma deseninin grafiği][img-config_pattern]

- **Üretici Yazılımı Güncelleştirmesi** - Arka uç uygulaması, üretici yazılımı güncelleştirme işleminin başlatıldığını bir doğrudan yöntem aracılığıyla cihaza bildirir.  Cihaz; üretici yazılımı paketini indirmek, üretici yazılımı paketini uygulamak ve son olarak IoT Hub hizmetine yeniden bağlanmak için çok adımlı bir işlem başlatır.  Çok adımlı işlem boyunca cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın ilerleme ve durumunu güncelleştirir.

    ![Azure IoT Hub cihaz yönetimi üretici yazılımı güncelleştirme deseninin grafiği][img-fwupdate_pattern]

- **İlerleme ve durumu raporlama** - Uygulama arka ucu, cihaz üzerinde çalışan eylemlerin durum ve ilerlemesini bildirmek üzere bir dizi cihazda cihaz çifti sorguları çalıştırır.

    ![Azure IoT Hub cihaz yönetimi raporlama ilerleme ve durum deseninin grafiği][img-report_progress_pattern]

## <a name="next-steps"></a>Sonraki Adımlar

Her cihaz yaşam döngüsü aşamasında kurumsal IoT operatörünün gereksinimlerini yerine getiren IoT uygulamaları oluşturmak için Azure IoT Hub cihaz yönetimi tarafından sağlanan özellik, desen ve kod kitaplıklarını kullanabilirsiniz.

Azure IoT Hub cihaz yönetimi özellikleri hakkında daha fazla bilgi almak için [Azure IoT Hub cihaz yönetimini kullanmaya başlama][lnk-get-started] öğreticisine bakın.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md


<!--HONumber=Oct16_HO3-->


