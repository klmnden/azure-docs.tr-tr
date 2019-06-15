---
title: Azure IOT Hub ile cihaz yönetimine genel bakış | Microsoft Docs
description: Kurumsal cihaz yaşam döngüsü ve cihaz yönetim modellerini Azure IOT Hu--cihaz yönetimine genel bakış gibi yeniden başlatma, Fabrika sıfırlaması, üretici yazılımı güncelleştirmesi, yapılandırma, cihaz çiftleri, sorgular, işler.
author: bzurcher
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: briz
ms.openlocfilehash: bdc55af23568b5785a831e81f352400c728c902e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400985"
---
# <a name="overview-of-device-management-with-iot-hub"></a>IoT Hub ile cihaz yönetimine genel bakış

Azure IoT Hub, cihaz ve arka uç geliştiricilerinin güçlü cihaz yönetimi çözümleri oluşturmasını sağlayan özellikler ve bir genişletilebilirlik modeli sunar. Cihazlar, kısıtlı algılayıcılardan tek amaçlı mikro denetleyicilere ve cihaz grupları için iletişimi yönlendiren güçlü ağ geçitlerine varan çeşitler barındırır.  Ayrıca, kullanım örnekleri ve IoT operatörlerinin gereksinimleri sektörler arasında farklılık gösterir.  Bu farklılığa rağmen, IoT Hub ile cihaz yönetimi çok çeşitli cihaz ve son kullanıcılara uygun özellikler, desenler ve kod kitaplıkları sağlar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Başarılı bir kurumsal IoT çözümü oluşturmanın önemli bir kısmı, operatörlerin cihaz koleksiyonu için devam eden yönetimi nasıl gerçekleştirdiğine ilişkin bir strateji sağlanmasıdır. IoT operatörleri, işlerinin daha stratejik yönlerine odaklanmalarını sağlayan basit ve güvenilir araç ve uygulamalar gerektirir. Bu makalede aşağıdakiler sunulmaktadır:

* Azure IoT Hub cihaz yönetimi yaklaşımına kısa bir genel bakış.
* Genel cihaz yönetimi ilkelerinin açıklaması.
* Cihaz yaşam döngüsü açıklaması.
* Genel cihaz yönetimi desenlerine genel bakış.

## <a name="device-management-principles"></a>Cihaz yönetimi ilkeleri

IoT bir dizi benzersiz cihaz yönetimi zorluğunu beraberinde getirir ve kurumsal sınıftaki her çözüm aşağıdaki ilkeleri hesaba katmalıdır:

![Cihaz yönetimi ilkeleri grafiği](media/iot-hub-device-management-overview/image4.png)

* **Ölçek ve Otomasyon**: IOT çözümleri, rutin görevleri otomatik hale getirmek ve milyonlarca cihazı yönetmesine olanak oldukça küçük bir operasyon tanıyabilen basit Araçlar gerektirir. Operatörler toplu cihaz işlemlerini günlük olarak uzaktan gerçekleştirmeyi ve yalnızca doğrudan dikkat gerektiren sorunlar oluştuğunda uyarılmayı beklemektedir.

* **Açıklık ve Uyumluluk**: Cihaz ekosistemi olağanüstü çeşitliliğe. Yönetim araçları çok sayıda cihaz sınıfına, platforma ve protokole uyum sağlayacak şekilde uyarlanmalıdır. Operatörler en kısıtlı katıştırılmış tek işlemli yongalardan güçlü ve tam işlevsel bilgisayarlara kadar çok sayıda cihaz türünü destekleyebilmelidir.

* **Bağlam tanıma**: IOT ortamları dinamik ve sürekli değişen. Hizmet güvenilirliği üst düzey öneme sahiptir. Cihaz yönetimi işlemleri bakım kapalı kalma süresinin kritik iş işlemlerini etkilemediğinden veya tehlikeli koşullar oluşturmadığından emin olmak için aşağıdaki faktörleri göz önünde bulundurmalıdır:

    * SLA bakım pencereleri
    * Ağ ve güç durumları
    * Kullanım koşulları
    * Cihaz coğrafi konumu

* **Çok sayıda servis rolü**: Benzersiz iş akışları ve IOT işlemleri rolleri için destek çok önemlidir. Operasyon personeli, şirket içi BT bölümlerinin belirtilen kısıtlamalarıyla uyumlu bir şekilde çalışmalıdır.  Ayrıca, denetçilere ve diğer iş yönetimi rollerine gerçek zamanlı cihaz çalışma bilgilerini sunmanın uygun yollarını bulmalıdır.

## <a name="device-lifecycle"></a>Cihaz yaşam döngüsü
Tüm kurumsal IoT projelerinde ortak olan genel cihaz yönetimi aşamaları vardır. Azure IoT içinde, cihaz yaşam döngüsünün beş aşaması vardır:

![Beş Azure IoT cihaz yaşam döngüsü aşaması şunlardır: planlama, sağlama, yapılandırma, izleme, devre dışı bırakma](./media/iot-hub-device-management-overview/image5.png)

Bu beş aşamanın her birinde, tam bir çözüm sağlamak için yerine getirilmesi gereken birkaç cihaz operatörü gereksinimi vardır:

* **Plan**: Bunları kolayca tanıyan cihaz meta verileri düzeni oluşturmasını olanak tanır ve doğru bir şekilde sorgulamasına ve cihazların toplu yönetim işlemleri için bir grubu hedeflemek. Bu cihaz meta verilerini etiketler ve özellikler halinde depolamak için cihaz ikizlerini kullanabilirsiniz.
  
    *Daha fazla okuma*: 
    * [Cihaz ikizlerini kullanmaya başlama](iot-hub-node-node-twin-getstarted.md)
    * [Cihaz ikizlerini anlama](iot-hub-devguide-device-twins.md)
    * [Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)
    * [Bir IOT çözüm içinde cihaz yapılandırması için en iyi uygulamalar](iot-hub-configuration-best-practices.md)

* **Sağlama**: Güvenli bir IOT hub'ına yeni cihazları hazırlama ve cihaz özelliklerini hemen bulmasına olanak tanır.  Esnek cihaz kimlikleri ve kimlik bilgileri oluşturmanın yanı sıra bu işlemi bir iş kullanarak toplu halde gerçekleştirmek için IoT Hub kimlik kayıt defterini kullanın. Cihaz ikizindeki cihaz özellikleri aracılığıyla kapasite ve koşullarını raporlamak için cihazlar oluşturun.
  
    *Daha fazla okuma*: 
    * [Cihaz kimliklerini yönetme](iot-hub-devguide-identity-registry.md)
    * [Cihaz kimliklerinin toplu Yönetimi](iot-hub-bulk-identity-mgmt.md)
    * [Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)
    * [Bir IOT çözüm içinde cihaz yapılandırması için en iyi uygulamalar](iot-hub-configuration-best-practices.md)
    * [Azure IoT Hub Cihazı Sağlama Hizmeti](https://azure.microsoft.com/documentation/services/iot-dps)

* **Yapılandırma**: Hem durumu hem de güvenliğini korurken toplu yapılandırma değişikliklerini ve üretici yazılımı güncelleştirmelerini cihazlara kolaylaştırır. İstediğiniz özellikleri kullanarak ve doğrudan yöntemler ve yayın işleri ile bu cihaz yönetimi işlemlerini toplu olarak gerçekleştirin.
  
    *Daha fazla okuma*:
    * [Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)
    * [Yapılandırma ve IOT cihazlarını uygun ölçekte izleme](iot-hub-auto-device-config.md)
    * [Bir IOT çözüm içinde cihaz yapılandırması için en iyi uygulamalar](iot-hub-configuration-best-practices.md)

* **İzleyici**: Genel cihaz koleksiyonu sistem durumu, devam eden işlemler ve operatörleri dikkat gerektirebilecek sorunlar durumunu izleyin.  Cihazların gerçek zamanlı çalışma koşullarını ve güncelleştirme işlemlerinin durumunu raporlamasına olanak tanımak üzere cihaz ikisi uygulayın. Cihaz ikizi sorgularını kullanarak en acil sorunları ortaya çıkaran güçlü pano raporları oluşturun.
  
    *Daha fazla okuma*: 
    * [Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)
    * [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md)
    * [Yapılandırma ve IOT cihazlarını uygun ölçekte izleme](iot-hub-auto-device-config.md)
    * [Bir IOT çözüm içinde cihaz yapılandırması için en iyi uygulamalar](iot-hub-configuration-best-practices.md)

* **Devre dışı bırakma**: Değiştirin veya cihazları bir hatadan sonra yetkisini da yükseltme döngüsü veya hizmet ömrünün sonunda.  Fiziksel cihaz değiştiriliyorsa cihaz bilgilerini korumak veya kullanım dışı bırakılıyorsa cihaz bilgilerini arşivlemek için cihaz ikizini kullanın. Cihaz kimliklerini ve kimlik bilgilerini güvenli bir şekilde iptal etmek için IoT Hub kimlik kayıt defterini kullanın.
  
    *Daha fazla okuma*: 
    * [Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)
    * [Cihaz kimliklerini yönetme](iot-hub-devguide-identity-registry.md)

## <a name="device-management-patterns"></a>Cihaz yönetimi modelleri

IoT Hub aşağıdaki cihaz yönetim modellerini sağlar. [Cihaz Yönetimi öğreticileri](iot-hub-node-node-device-management-get-started.md) bu desenleri gerçek senaryonuza uygun hale getirme ve bu çekirdek şablonları temel alan yeni desenler tasarlama konularını daha ayrıntılı olarak gösterir.

* **Yeniden başlatma**: Arka uç uygulaması, bu bir yeniden başlatma işleminin başlatıldığını bir doğrudan yöntem aracılığıyla cihaza bildirir.  Cihaz, bildirilen özellikleri kullanarak cihazın yeniden başlatma durumunu güncelleştirir.
  
    ![Cihaz yönetimi yeniden başlatma deseninin grafiği](./media/iot-hub-device-management-overview/reboot-pattern.png)

* **Fabrika sıfırlaması**: Arka uç uygulaması, Fabrika sıfırlamasının başlatıldığını bir doğrudan yöntem aracılığıyla cihaza bildirir. Cihaz, bildirilen özellikleri kullanarak cihazın fabrika sıfırlaması durumunu güncelleştirir.
  
    ![Cihaz yönetimi fabrika sıfırlama deseninin grafiği](./media/iot-hub-device-management-overview/facreset-pattern.png)

* **Yapılandırma**: Arka uç uygulaması, istenen özellikleri kullanarak cihaz üzerinde çalışan yazılımı yapılandırır kullanır. Cihaz, bildirilen özellikleri kullanarak cihazın yapılandırma durumunu güncelleştirir.
  
    ![Cihaz yönetimi yapılandırma deseninin grafiği](./media/iot-hub-device-management-overview/configuration-pattern.png)

* **Üretici yazılımı güncelleştirmesi**: Arka uç uygulaması, nerede güncelleştirmeyi bulun ve güncelleştirme işlemini izlemek için cihazları bildirmek için güncelleştirmeyi almak için cihazları seçmek için bir otomatik cihaz Yönetim yapılandırması kullanır. Cihaz indirip, doğrulama, uygulamak ve cihazın IOT Hub hizmetine yeniden bağlanmadan önce yeniden için çok adımlı bir işlem başlatır. Çok adımlı işlem boyunca cihaz, bildirilen özellikleri kullanarak cihazın ilerlemesini ve durumunu güncelleştirir.
  
    ![Cihaz yönetimi üretici yazılımı güncelleştirme deseninin grafiği](media/iot-hub-device-management-overview/fwupdate-pattern.png)

* **İlerleme ve durumu raporlama**: Çözüm, bir dizi cihaz üzerinde çalışan eylemlerin ilerlemesini ve durumunu raporlamak için cihazlar, cihaz ikizi sorgularını son çalıştırır, yedekleyin.
  
    ![Cihaz yönetimi raporlama ilerleme ve durum deseninin grafiği](./media/iot-hub-device-management-overview/report-progress-pattern.png)

## <a name="next-steps"></a>Sonraki Adımlar

IoT Hub’ın cihaz yönetimi için sağladığı özellik, desen ve kod kitaplıkları, her cihaz yaşam döngüsü aşamasında kurumsal IoT operatörünün gereksinimlerini yerine getiren IoT uygulamaları oluşturmanıza olanak sağlar.

IOT Hub'ında cihaz yönetimi özellikleri hakkında bilgi almaya devam etmek için bkz. [cihaz yönetimini kullanmaya başlama](iot-hub-node-node-device-management-get-started.md) öğretici.