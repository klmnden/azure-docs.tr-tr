---
title: Aygıt yapılandırma en iyi Azure IOT hub'ına yönelik | Microsoft Docs
description: IOT cihazları ölçekte yapılandırmak için en iyi uygulamalar hakkında bilgi edinin
author: chrisgre
manager: briz
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 7314c5ec05bec61e5bbbc406b6a39a7c5a8f011f
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036266"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Bir IOT çözüm içinde cihaz yapılandırması için en iyi yöntemler

Azure IOT hub'ı otomatik cihaz yönetiminde büyük cihaz fleets kendi ömürleri tamamen yönetme pek çok yinelenen ve karmaşık görevleri otomatik hale getirir. Bu makalede birçok dahil çeşitli rolleri için en iyi uygulamaları geliştirmek ve IOT çözümünü işletim tanımlar.

* **IOT donanım üreticisinin/Tümleştirici:** üreticileri, IOT donanım, çeşitli üreticileri ya da bir IOT dağıtım için donanım sağlama Üreticiler donanım atayarak tümleştiricileri üretilen veya başka üreticiler tarafından tümleşik. Geliştirme ve üretici yazılımı, katıştırılmış işletim sistemleri ve katıştırılmış yazılım tümleştirme söz konusu.
* **IOT çözüm geliştirici:** bir IOT çözüm geliştirme genellikle bir çözüm geliştirici tarafından yapılır. Bu Geliştirici şirket içi bir ekip veya bu etkinlikte uzmanlaşmış bir sistem Tümleştirici parçası olabilir. IOT çözüm geliştirici sıfırdan IOT çözüm çeşitli bileşenleri geliştirmek, çeşitli standart veya açık kaynak bileşenlerini tümleştirmek veya özelleştirin bir [IOT Çözüm Hızlandırıcısı][lnk-solution].
* **IOT çözümü işleci:** sonra IOT çözüm dağıtılırsa, uzun vadeli işlemleri, izleme, yükseltme ve bakım gerektirir. Bu görevler, bilgi teknolojisi uzmanları, donanım işlemler ve Bakım ekipleri ve genel IOT altyapı doğru davranışını izlemek etki alanı uzmanlarıyla oluşan bir şirket içi ekibi tarafından yapılabilir.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>IOT cihazları ölçekte yapılandırmak için otomatik cihaz yönetimini anlama

Otomatik cihaz Yönetimi içerir birçok yararından biri [cihaz çiftlerini] [ lnk-device-twins] ve [modülü çiftlerini] [ lnk-module-twins] eşitlemek için istenen ve bildirilen durumlar cihazlar ve bulut arasında.  [Otomatik cihaz yapılandırmalarını] [ lnk-auto-device-config] otomatik olarak güncelleştirmek çiftlerini büyük kümelerini ve ilerleme durumu ve uyumluluk summerize. Otomatik cihaz Yönetimi aşağıdaki üst düzey adımları açıklayan geliştirilen ve kullanılır:

* **IOT donanım üreticisinin/Tümleştirici** kullanarak katıştırılmış uygulama içindeki cihaz yönetimi özellikleri uygulayan [cihaz çiftlerini][lnk-device-twins]. Bu özellikler, bellenim güncelleştirmeleri, yazılım yükleme ve güncelleştirme ve ayar yönetimi dahil olabilir.
* **IOT çözüm geliştirici** yönetim katmanı kullanarak aygıt yönetimi işlemlerinin uygulayan [cihaz çiftlerini] [ lnk-device-twins] ve [otomatik cihaz yapılandırmaları][lnk-auto-device-config]. Çözüm, cihaz yönetim görevlerini gerçekleştirmek için bir işleç arabirim tanımlama içermesi gerekir.
* **IOT çözüm işleci** kullandığı birlikte özellikle cihazları için cihaz yönetim görevlerini gerçekleştirmek, bellenim güncelleştirmeleri gibi yapılandırma değişikliklerini başlatmak, ilerlemeyi izlemek ve sorunlarını gidermenizi IOT çözüm sorunları kaynaklanır.

## <a name="iot-hardware-manufacturerintegrator"></a>IOT donanım üreticisinin/Tümleştirici

Donanım üreticileri ve katıştırılmış yazılım geliştirme ile ilgili tümleştiricileri için en iyi yöntemler şunlardır:

* **Uygulama [cihaz çiftlerini][lnk-device-twins]:** cihaz çiftlerini etkinleştirmek istediğiniz yapılandırmayı bulutta ve geçerli yapılandırma ve cihaz özelliklerini raporlamak eşitleme.  Cihaz çiftlerini katıştırılmış uygulamalar içinde uygulamak için en iyi yolu aracılığıyladır [Azure IOT SDK'ları][lnk-azure-sdk].  Çünkü cihaz çiftlerini yapılandırması için en uygun bunlar: bir. çift yönlü iletişim b destekler. Her iki bağlı ve bağlantısı kesilmiş cihaz durumları c izin verir. Nihai tutarlılık d ilkesini izleyin. bulutta tamamen sorgulanabilir olan
* **Aygıt Yönetimi için cihaz çifti yapısı:** cihaz çifti cihaz yönetimi özellikleri mantıksal bölümlere gruplanmış şekilde yapılandırılmalıdır. Bunun yapılması twin diğer bölümleri etkilemeden yalıtılması yapılandırma değişikliklerini olanak sağlar. Örneğin, üretici yazılımı, yazılım, başka bir bölüme istenen özelliklerini içinde bir bölüm oluşturun ve ağ ayarlarını üçüncü bir bölüm. 
* **Rapor cihaz yönetimi için yararlı olan cihaz öznitelikleri:** fiziksel cihazı gibi özniteliklere marka ve model, bellenim, işletim sistemi, seri numarası ve diğer tanımlayıcıları raporlama ve hedefleme için parametre olarak yararlı yapılandırma değişiklikleri.
* **Durumunu ve ilerlemesini raporlama için ana durumları tanımlayan:** en üst düzey durumlar numaralandırılan böylece işleci için bildirilebilir. Örneğin, bellenim güncelleştirme geçerli, indirme, uygulama, devam eden ve hata durumunu raporlar.  Daha fazla bilgi için ek alanlar her durumda tanımlayın.  

## <a name="iot-solution-developer"></a>IOT Çözüm geliştiricisi

Azure'da tabanlı sistemler oluşturmakta olduğunuz IOT çözüm geliştiriciler için en iyi yöntemler şunlardır:

* **Uygulama [cihaz çiftlerini][lnk-device-twins]:** cihaz çiftlerini etkinleştirmek istediğiniz yapılandırmayı bulutta ve geçerli yapılandırma ve cihaz özelliklerini raporlamak eşitleme.  Bulut çözümleri uygulamalar içindeki cihaz çiftlerini uygulamak için en iyi yolu aracılığıyladır [Azure IOT SDK'ları][lnk-azure-sdk].  Çünkü cihaz çiftlerini yapılandırması için en uygun bunlar: bir. çift yönlü iletişim b destekler. Her iki bağlı ve bağlantısı kesilmiş cihaz durumları c izin verir. Nihai tutarlılık d ilkesini izleyin. bulutta tamamen sorgulanabilir olan
* **Cihaz çifti etiketleri kullanarak cihaz düzenlemek:** çözüm kalite çalma veya diğer ayarlar canary gibi çeşitli dağıtım stratejilerini göre cihazların tanımlamak işleci sağlamalıdır. Cihaz kuruluş cihaz çifti etiketleri kullanarak, çözümünüz içinde uygulanabilir ve [sorguları][lnk-queries].  Cihaz kuruluş için yapılandırma toplama aşımı ayarlarına güvenle ve doğru bir şekilde izin vermek gereklidir.
* **Uygulama [otomatik cihaz yapılandırmalarını][lnk-auto-device-config]:** otomatik cihaz yapılandırmalarını dağıtma ve izleme yapılandırma değişikliklerini IOT cihazları cihaz çiftlerini aracılığıyla büyük kümeleri için.  Otomatik cihaz yapılandırmalarını hedef cihaz çiftlerini kümesi **hedef koşulu** twin etiketleri sorgudur aygıttaki veya özellikler bildirdi. **Hedef içerik** içinde hedeflenen cihaz çiftlerini ayarlamak istediğiniz özellikler kümesidir. İçerik hedef IOT donanım üreticisinin/Tümleştirici tarafından tanımlanan cihaz çifti yapısı ile uyumlu olmalıdır.  **Ölçümleri** cihaz çifti sorgulamaları özellikleri bildirilen ve aynı zamanda IOT donanım üreticisinin/Tümleştirici tarafından tanımlanan cihaz çifti yapısı ile uyumlu olmalıdır. Otomatik cihaz yapılandırmalarını avantajı IOT hub'ın hiçbir zaman aşacak bir hızda cihaz çifti işlemlerini gerçekleştirme de [azaltma sınırları] [ lnk-throttling] cihaz çifti okur ve güncelleştirmeler için.
* **Kullanmak [aygıt hizmeti sağlama][lnk-dps]:** çözüm geliştiriciler kullanması gereken cihaz hizmeti sağlama yeni cihazları için cihaz çifti etiketleri atamak sağlayacak şekilde otomatik olarak olacaktır tarafından yapılandırılan **otomatik cihaz yapılandırmalarını** bu etikete sahip çiftlerini adresindeki hedeflenen. 

## <a name="iot-solution-operator"></a>IOT çözüm işleci

Azure üzerinde bir IOT çözümü kullanılarak oluşturulan IOT çözüm İşletmenleri için en iyi yöntemler şunlardır:

* **Cihazları Yönetim için düzenleme:** IOT çözüm tanımlayın veya kalite çalma oluşturulmasını veya diğer kümeleri canary gibi çeşitli dağıtım stratejilerini bağlı cihazları için izin gerekir. Aygıtları kümesi yapılandırma değişiklikleri alma ve diğer ölçekli aygıt yönetim işlemlerini gerçekleştirmek için kullanılır.
* **Aşamalı bir dağıtımı kullanarak yapılandırma değişikliklerini gerçekleştirmek:** çıkışı aşamalı bir toplama alınabildiği bir işleç dağıtır değişiklikleri insanın ufkunu genişleten bir IOT cihazları kümesi için bir genel bir işlemdir. Kademeli olarak geniş ölçek önemli değişiklikler yapma riskini azaltmak için değişiklik yapmak için belirtilir.  İşleç oluşturmak için çözümün arabirimi kullanmalısınız bir [otomatik aygıt yapılandırması][lnk-auto-device-config], ve hedefleme koşul cihazları (örneğin, bir yalancı grup) başlangıç kümesi hedeflemelidir.  İşleci, ardından cihaz ilk kümesini yapılandırma değişikliği doğrulamalıdır.  Doğrulama tamamlandıktan sonra işleci daha büyük bir aygıtları dahil etmek için otomatik cihaz yapılandırmasını güncelleştirin. İşleç önceliği yapılandırmasının şu anda bu aygıtlar için hedeflenen diğer yapılandırmalar daha yüksek olması de ayarlamanız gerekir.  Kullanıma alma otomatik cihaz yapılandırması tarafından bildirilen ölçümleri kullanarak izlenebilir. 
* **Geri alma hataları veya yapılandırma hataları durumunda gerçekleştirmek:** hataları veya yapılandırma hataları neden olan bir otomatik cihaz yapılandırması değiştirerek geri alınabilir **koşul hedefleme** böylece aygıtları yok artık hedefleme koşula.  Daha düşük bir öncelik başka bir otomatik cihaz yapılandırması bu cihazlar için hala hedeflenen emin olun.  Ölçümleri görüntüleyerek geri alma işleminin başarılı olduğunu doğrulayın: alındı geri yapılandırma artık untargeted aygıtı durumunu göstermesi gerekir ve ikinci yapılandırmasının ölçümleri şimdi hala hedeflenen cihazlar sayılarını içermelidir.


## <a name="next-steps"></a>Sonraki adımlar

* Cihaz çiftlerini uygulama hakkında bilgi edinmek [IOT hub cihaz çiftlerini anlama ve kullanma][lnk-device-twins]
* Oluştur, Güncelleştir veya bir otomatik cihaz yapılandırmasında silmek için izleyeceğiniz adımlarda size yol [Yapılandır ve IOT cihazları ölçekte izleme][lnk-auto-device-config].
* Cihaz çiftlerini ve otomatik cihaz yapılandırmalarını kullanarak bir üretici yazılımı güncelleştirmesi desen uygulamak [öğretici: cihaz bellenim güncelleştirme işlemini uygulamak][lnk-firmware-update].

<!-- Links -->
[lnk-device-twins]: iot-hub-devguide-device-twins.md
[lnk-module-twins]: iot-hub-devguide-module-twins.md
[lnk-auto-device-config]: iot-hub-auto-device-config.md
[lnk-firmware-update]: tutorial-firmware-update.md
[lnk-throttling]: iot-hub-devguide-quotas-throttling.md
[lnk-queries]: iot-hub-devguide-query-language.md
[lnk-dps]: ../iot-dps/how-to-manage-enrollments.md
[lnk-azure-sdk]: https://github.com/Azure/azure-iot-sdks
[lnk-solution]: https://azure.microsoft.com/features/iot-accelerators/