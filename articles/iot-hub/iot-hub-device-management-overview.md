<properties
 pageTitle="Cihaz yönetimine genel bakış | Microsoft Azure"
 description="Azure IoT Hub cihaz yönetimine genel bakış"
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
 ms.date="09/16/2016"
 ms.author="bzurcher"/>




# Azure IoT Hub cihaz yönetimine genel bakış (önizleme)

## Azure IoT cihaz yönetimi yaklaşımı

Azure IoT Hub cihaz yönetimi, IoT içindeki çeşitli cihaz ve protokoller için IoT cihaz yönetiminden yararlanan cihazlara ve arka uçlara yönelik özellikler ve genişletilebilirlik modeli sağlar.  IoT içindeki cihazlar çok kısıtlı sensörler, tek amaçlı mikro denetleyiciler ve diğer cihazlar ile protokolleri etkinleştiren daha güçlü ağ geçitlerine varan çeşitlilikte cihazları içerir.  IoT çözümleri ayrıca dikey etki alanlarında ve her bir etki alanındaki operatörler için benzersiz kullanım örnekleri içeren uygulamalarda büyük ölçüde farklılık gösterir.  IoT çözümleri çeşitli cihazlar ve operatörler için cihaz yönetimi sağlamak üzere IoT Hub cihaz yönetimi özellikleri, modelleri ve kod kitaplıklarından yararlanabilir.  

## Giriş

Başarılı bir IoT çözümü oluşturmanın önemli bir kısmı, operatörlerin cihaz grubu için devam eden yönetimi nasıl gerçekleştirdiğine ilişkin bir strateji sağlanmasıdır. IoT operatörleri hem basit hem de güvenilir olup, işlerinin daha stratejik yönlerine odaklanmalarını sağlayan araç ve uygulamalar gerektirir. Azure IoT Hub, en önemli cihaz yönetimi modellerini kolaylaştıran IoT uygulamaları oluşturmaya yönelik yapı taşları sağlar.

Cihazlar, cihazı buluta güvenli bir şekilde bağlayan cihaz yönetimi aracısı adlı basit bir uygulama çalıştırdığında IoT Hub tarafından yönetildiği kabul edilir. Aracı kodu, uygulama tarafından bir operatörün cihaz durumunu uzaktan onaylamasını ve ağ yapılandırma değişikliklerini uygulama veya üretici yazılımı güncelleştirmelerini dağıtma gibi yönetim işlemlerini gerçekleştirmeyi sağlar.

## IoT cihaz yönetimi ilkeleri

IoT bir dizi yönetim zorlukları getirir ve çözümü aşağıdaki IoT cihaz yönetimi özelliklerini hesaba katmalıdır:

![][img-dm_principles]

- **Ölçek ve otomasyon**: IoT rutin görevleri otomatik hale getirebilen ve oldukça küçük bir operasyon ekibinin milyonlarca cihazı yönetmesine olanak tanıyabilen basit araçlar gerektirir. Operatörler toplu cihaz işlemlerini günlük olarak uzaktan gerçekleştirmeyi ve yalnızca doğrudan dikkat gerektiren sorunlar oluştuğunda uyarılmayı beklemektedir.

- **Açıklık ve uyumluluk**: IoT cihaz ekosistemi olağanüstü çeşitliliğe sahiptir. Yönetim araçları çok sayıda cihaz sınıfına, platforma ve protokole uyum sağlayacak şekilde uyarlanmalıdır. Operatörler en kısıtlı katıştırılmış tek işlemli yongalardan güçlü ve tam işlevsel bilgisayarlara kadar tüm cihazları destekleyebilmelidir.

- **Bağlam tanıma**: IoT ortamları dinamik ve sürekli değişen yapıdadır. Hizmet güvenilirliği üst düzey öneme sahiptir. Cihaz yönetimi işlemleri; bakım kaynaklı kapalı kalma süresinin kritik iş işlemlerini etkilemediğinden veya tehlikeli koşullar oluşturmadığından emin olmak üzere SLA bakım pencerelerini, ağ ve güç durumlarını, kullanım sırasındaki koşulları ve cihazın coğrafi konumunu hesaba katmalıdır.

- **Çok sayıda servis rolü**: Benzersiz iş akışları ve IoT işlemleri için destek çok önemlidir. Operasyon personeli ayrıca iç BT bölümlerinin belirtilen kısıtlamalarıyla uyumlu bir şekilde çalışmalı ve ilgili cihaz çalışma bilgilerini denetçilere ve diğer yönetim rollerine göstermelidir.

## IoT cihazı yaşam döngüsü 

IoT projeleri büyük ölçüde farklılık gösterse de cihazları yönetmek için bir dizi genel model vardır. Azure IoT içinde bu modeller beş ayrı aşamadan oluşan IoT cihazı yaşam döngüsünde tanımlanır:

![][img-device_lifecycle]

1. **Plan**: Operatörlerin toplu yönetim işlemleri için bir cihaz grubunu kolayca ve doğru bir şekilde sorgulamasına ve hedeflemesine olanak tanıyan cihaz özelliği düzeni oluşturmasını sağlar.

    *İlgili yapı taşları*: [Cihaz çiftleri ile çalışmaya başlama][lnk-twins-getstarted], [Çift özelliklerini kullanma][lnk-twin-properties]

2. **Sağlama**: IoT hub’ında cihazların kimliğini güvenli bir şekilde sağlar ve operatörlerin cihaz özellikleri ile mevcut durumu hemen bulmasına olanak tanır.

    *İlgili yapı taşları*: [IoT Hub ile çalışmaya başlama][lnk-hub-getstarted], [Çift özelliklerini kullanma][lnk-twin-properties]

3. **Yapılandırma**: Cihazların hem sistem durumunu hem de güvenliğini korurken toplu yapılandırma değişikliklerini ve üretici yazılımı güncelleştirmelerini kolaylaştırır.

    *İlgili yapı taşları*: [Çift özelliklerini kullanma][lnk-twin-properties], [C2D Yöntemleri][lnk-c2d-methods], [İşleri Zamanlama/Yayınlama][lnk-jobs]

4. **İzleme**: Operatörleri dikkat gerektirebilecek sorunlar konusunda uyarmak için genel cihaz grubu durumunu ve devam eden toplu güncelleştirmelerin durumunu izler.

    *İlgili yapı taşları*: [Çift özelliklerini kullanma][lnk-twin-properties]

5. **Devre dışı bırakma**: Bir hata ya da yükseltme döngüsü sonrasında veya hizmet ömrünün sonunda cihazları değiştirin ya da kullanımdan kaldırın.

    *İlgili yapı taşları*:
    
## IoT Hub cihaz yönetimi modelleri

IoT Hub aşağıdaki (başlangıç) cihaz yönetim modellerini sağlar.  [Öğreticilerde][lnk-get-started] gösterildiği gibi bu modelleri senaryonuza uyacak şekilde genişletebilir ve bu temel modellere göre diğer senaryolar için yeni modeller tasarlayabilirsiniz.

1. **Yeniden başlatma** - Arka uç uygulaması, yeniden başlatma işleminin başlatıldığını bir D2C yöntemi aracılığıyla cihaza bildirir.  Cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın yeniden başlatma durumunu güncelleştirir. 

    ![][img-reboot_pattern]

2. **Fabrika Sıfırlaması** - Arka uç uygulaması, fabrika sıfırlamasının başlatıldığını bir D2C yönetimi aracılığıyla cihaza bildirir.  Cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın fabrika sıfırlama durumunu güncelleştirir.

    ![][img-facreset_pattern]

3. **Yapılandırma** - Arka uç uygulaması, cihaz çiftinin istenen özelliklerini kullanarak cihaz üzerinde çalışan yazılımı yapılandırır.  Cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın yapılandırma durumunu güncelleştirir. 

    ![][img-config_pattern]

4. **Üretici Yazılımı Güncelleştirmesi** - Arka uç uygulaması, üretici yazılımı güncelleştirme işleminin başlatıldığını bir D2C yöntemi aracılığıyla cihaza bildirir.  Cihaz; üretici yazılımı paketini indirmek, üretici yazılımı paketini uygulamak ve son olarak IoT Hub hizmetine yeniden bağlanmak için çok adımlı bir işlem başlatır.  Çok adımlı işlem boyunca cihaz, cihaz çiftinin bildirilen özelliklerini kullanarak cihazın ilerleme ve durumunu güncelleştirir. 

    ![][img-fwupdate_pattern]

5. **İlerleme ve durumu raporlama** - Uygulama arka ucu, cihaz üzerinde çalışan eylemlerin durum ve ilerlemesini bildirmek üzere bir dizi cihazda cihaz çifti sorguları çalıştırır.

    ![][img-report_progress_pattern]

## Sonraki Adımlar

Geliştiriciler Azure IoT Hub tarafından sağlanan yapı taşlarını kullanarak, her bir cihaz yaşam döngüsü aşamasında benzersiz IoT operatörü gereksinimlerini karşılayan IoT uygulamaları oluşturabilir.

Azure IoT Hub cihaz yönetimi özellikleri hakkında daha fazla bilgi almak için [Azure IoT Hub cihaz yönetimini kullanmaya başlama][lnk-get-started] öğreticisine bakın.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md


<!--HONumber=Oct16_HO1-->


