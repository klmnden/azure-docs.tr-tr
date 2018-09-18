---
title: Azure IOT Hub için cihaz yapılandırma en iyi yöntemler | Microsoft Docs
description: IOT cihazlarını uygun ölçekte yapılandırmak için en iyi uygulamalar hakkında bilgi edinin
author: chrisgre
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 5eb0ba659961d809d0ae471034b03263f87e3894
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985507"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Bir IOT çözüm içinde cihaz yapılandırması için en iyi uygulamalar

Azure IOT hub otomatik cihaz Yönetimi döngülerini tamamına büyük cihaz filolarına yönetme çok sayıda yinelenen ve karmaşık görevleri otomatik hale getirir. Bu makalede, geliştirme ve IOT çözümünü işletim birçok dahil çeşitli rolleri için en iyi tanımlar.

* **IOT donanım üreticisi/entegratörü:** üreticiler, IOT donanım, çeşitli üreticileri ya da tedarikçileri bir IOT dağıtım için donanım sağlayan donanım derleyerek tümleştiricileri üretilen veya başka üreticiler tarafından tümleşik. Geliştirme ve üretici yazılımı, katıştırılmış işletim sistemleri ve ekli yazılımdan tümleştirme söz konusu.

* **IOT çözüm geliştirici:** IOT çözümünün geliştirilmesini genellikle bir çözüm geliştirici tarafından yapılır. Bu geliştirici, şirket içi bir takım veya bu etkinliğe uzmanlaşan bir sistem tümleştiricisi parçası olabilir. IOT çözümü geliştirme çeşitli bileşenlerinin sıfırdan IOT çözümü geliştirmek, çeşitli standart veya açık kaynak bileşenlerini tümleştirmek veya özelleştirme bir [IOT Çözüm Hızlandırıcısı](/azure/iot-accelerators/).

* **IOT çözümü işleci:** sonra IOT çözüm dağıtılırsa, uzun süreli işlem, izleme, yükseltme ve bakım gerektirir. Bu görevler, bilgi teknolojisi uzmanları, donanım işlemler ve Bakım takımlar ve kim genel IOT altyapının doğru davranışları etki alanı uzmanlarıyla oluşan bir şirket içi ekibi tarafından yapılabilir.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>IOT cihazlarını uygun ölçekte yapılandırmak için otomatik cihaz yönetimini anlama

Otomatik cihaz Yönetimi'ni içeren birçok yararından biri [cihaz ikizlerini](iot-hub-devguide-device-twins.md) ve [modül ikizlerini](iot-hub-devguide-module-twins.md) istenen ve bildirilen durumlar cihazlar ve bulut arasında eşitlenecek. [Otomatik cihaz yapılandırmaları] [lnk-otomatik-cihaz-config] otomatik olarak büyük çiftleri kümesini güncelleştirmek ve ilerleme durumu ve uyumluluk summerize. Otomatik cihaz Yönetimi aşağıdaki üst düzey adımları açıklamak geliştirilen ve kullanılır:

* **IOT donanım üreticisi/entegratörü** kullanarak katıştırılmış uygulama içinde cihaz yönetimi özellikleri uygulayan [cihaz ikizlerini](iot-hub-devguide-device-twins.md). Bu özellikler, üretici yazılımı güncelleştirmelerini, yazılım yükleme ve güncelleştirme ve ayar yönetimi dahil olabilir.

* **IOT çözüm geliştirici** cihaz yönetimi işlemleri kullanarak yönetim katmanı uygulayan [cihaz ikizlerini](iot-hub-devguide-device-twins.md) ve [otomatik cihaz yapılandırmaları](iot-hub-auto-device-config.md). Çözüm, cihaz yönetim görevlerini gerçekleştirmek için bir işleç arabirimi tanımlama içermelidir.

* **IOT çözüm işleci** kullanan IOT çözümü birlikte özellikle cihazları için cihaz yönetim görevlerini gerçekleştirmek için yapılandırma değişiklikleri gibi üretici yazılımı güncelleştirmelerini başlatma, ilerlemeyi izlemek ve sorunlarını giderme sorunları ortaya çıkar.

## <a name="iot-hardware-manufacturerintegrator"></a>IOT donanım üreticisi/entegratörü

Donanım üreticileri ve katıştırılmış yazılım geliştirme ile ilgili entegratörleri için en iyi uygulamalar şunlardır:

* **Uygulama [cihaz ikizlerini](iot-hub-devguide-device-twins.md):** cihaz ikizlerini etkinleştirme buluttan ve geçerli yapılandırma ve cihaz özelliklerini raporlamak istediğiniz yapılandırma eşitleniyor. Cihaz ikizlerini katıştırılmış uygulamalar içinde uygulamak için en iyi yollarından biri sayesinde [Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks). İçin cihaz ikizlerini yapılandırması için en uygun bunlar:

    * Çift yönlü iletişimi destekler.
    * Her iki bağlı ve bağlantısı kesilmiş cihaz durumları için izin verin.
    * Son tutarlılık ilkesini izleyin.
    * Bulutta tam olarak sorgulanabilir ' dir.

* **Cihaz yönetimi için cihaz ikizi yapısı:** cihaz ikizinde cihaz yönetim özelliklerini mantıksal olarak birlikte bölümler halinde gruplandırılmıştır şekilde yapılandırılmalıdır. Bunun yapılması, yapılandırma değişiklikleri ikizinin diğer bölümleri etkilemeden yalıtılmış olması olanak sağlar. Örneğin, üretici yazılımı, yazılım, başka bir bölüme için istenen özellikler bir bölümündeki oluşturun ve ağ ayarlarını bir üçüncü bölümü. 

* **Rapor, cihaz yönetimi için yararlı olan cihaz öznitelikleri:** fiziksel cihazı gibi öznitelikleri marka ve model, üretici yazılımı, işletim sistemi, seri numarası ve diğer tanımlayıcıları hedeflemek için parametre olarak ve raporlaması için kullanışlıdır yapılandırma değişiklikleri.

* **Durumunu ve ilerlemesini raporlama için ana durumlarını tanımlar:** en üst düzey durumlar numaralandırılan böylece işleci için bildirilebilir. Örneğin, üretici yazılımı güncelleştirme durumunu geçerli, indirme, uygulama, devam eden ve hata olarak rapor. Daha fazla bilgi için ek alanlar üzerinde her bir durum tanımlayın.

## <a name="iot-solution-developer"></a>IOT çözümü geliştirme

Azure'da tabanlı sistemler oluşturmaya IOT çözüm geliştiriciler için en iyi uygulamalar şunlardır:

* **Uygulama [cihaz ikizlerini](iot-hub-devguide-device-twins.md):** cihaz ikizlerini etkinleştirme buluttan ve geçerli yapılandırma ve cihaz özelliklerini raporlamak istediğiniz yapılandırma eşitleniyor. Cihaz ikizlerini bulut çözümleri uygulamaları içinde uygulamak için en iyi yollarından biri sayesinde [Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks). İçin cihaz ikizlerini yapılandırması için en uygun bunlar:

    * Çift yönlü iletişimi destekler.
    * Her iki bağlı ve bağlantısı kesilmiş cihaz durumları için izin verin. 
    * Son tutarlılık ilkesini izleyin.
    * Bulutta tam olarak sorgulanabilir ' dir.

* **Cihaz ikizi etiketleri kullanarak cihazları Düzenleyen:** çözüm işleci kalite halkaları ya da diğer cihazları kanarya gibi çeşitli dağıtım stratejilerini göre kümesini tanımlamak izin vermelidir. Çözümünüz içinde cihaz ikizi etiketleri kullanarak cihaz kuruluş uygulanabilir ve [sorguları](iot-hub-devguide-query-language.md). Cihaz kuruluş için yapılandırma sunulmasını güvenli bir şekilde ve doğru bir şekilde izin vermek gereklidir.

* **Uygulama [otomatik cihaz yapılandırmaları](iot-hub-auto-device-config.md):** otomatik cihaz yapılandırmaları dağıtma ve izleme yapılandırma değişikliklerini için cihaz ikizlerini aracılığıyla IOT cihazlarının daha büyük kümeleri. Otomatik cihaz yapılandırmaları cihaz ikizlerini kümesi hedef **hedef koşulu** bir sorgudur ikizi etiketleri cihazda veya bildirilen özellikler. **Hedef içerik** içinde hedeflenen cihaz ikizlerini ayarlanan istenen özellikleri kümesidir. İçerik hedef, IOT donanım üreticisi/entegratörü tarafından tanımlanan cihaz ikizi yapısı ile hizalamanız gerekir. 

   **Ölçümleri** cihaz çifti sorguları bildirilen özellikler ve aynı zamanda IOT donanım üreticisi/entegratörü tarafından tanımlanan cihaz ikizi yapısı ile hizalamanız gerekir. Otomatik cihaz yapılandırmaları yararı, IOT hub'ı hiçbir zaman aşan bir hızda cihaz ikizi işlemleri de [sınırları](iot-hub-devguide-quotas-throttling.md) cihaz ikizi okumaları ve güncelleştirmeler için.

* **Kullanma [cihaz sağlama hizmeti](../iot-dps/how-to-manage-enrollments.md):** çözüm geliştiricilerin kullanması gereken cihaz sağlama hizmeti yeni cihazları için cihaz ikizi etiketler atama tarafından otomatik olarak yapılandırılacak şekilde  **Otomatik cihaz yapılandırmaları** ikizlerini etiketi içeren, hedeflenen. 

## <a name="iot-solution-operator"></a>IOT çözümü işleci

Azure üzerinde derlenen bir IOT çözümünü kullanarak IOT çözümü işleçleri için en iyi uygulamalar şunlardır:

* **Yönetim için cihazları Düzenleyen:** IOT çözüm tanımlayın veya kalite halkaları oluşturma veya diğer kümeleri kanarya gibi çeşitli dağıtım stratejilerini bağlı cihazlar için izin verin. Cihazları kümesi yapılandırma değişiklikleri alma ve diğer ölçekli cihaz yönetimi işlemleri gerçekleştirmek için kullanılır.

* **Yapılandırma değişiklikleri aşamalı kullanarak yapabilirsiniz:** aşamalı yapabildiği operatörün dağıtır değişiklikleri IOT cihazları insanın ufkunu genişleten bir kümesi için bir genel bir işlemdir. Aşamalı olarak geniş ölçek bozucu değişiklikler yapma riskini azaltmak için değişiklik olmaktır.  İşleci, çözümün arabirimi oluşturmak için kullanmalısınız bir [otomatik cihaz yapılandırma](iot-hub-auto-device-config.md) ve hedefleme koşul cihazları (örneğin, kanarya bir grubu) başlangıç kümesi hedeflemelidir. İşleci, ardından cihaz ilk kümesini yapılandırma değişikliği doğrulamalıdır. 

   Doğrulama tamamlandıktan sonra işleci daha büyük bir cihaz eklemek için otomatik cihaz yapılandırmasını güncelleştirir. İşleç önceliği yapılandırmasının şu anda bu cihazları hedefleyen diğer yapılandırmalar daha yüksek olması de ayarlamanız gerekir. Otomatik cihaz yapılandırması tarafından bildirilen ölçümleri kullanarak dağıtım izlenebilir. 

* **Hataları veya yanlış yapılandırmalarını durumunda geri alma işlemleri gerçekleştirmek:** hataları veya yanlış yapılandırmalarını neden olan otomatik cihaz yapılandırmasını değiştirerek geri alınabilir **koşul hedefleyen** böylece cihaz yok artık hedefleme koşula uyan. Düşük öncelikli başka bir otomatik cihaz yapılandırmasını, bu cihazlar için hala hedeflendiğinden emin olun. Geri alma ölçümleri görüntüleyerek başarılı olduğunu doğrulayın: Toplu geri yapılandırma artık untargeted cihazlar için durumunu göstermesi gerekir ve ikinci yapılandırma ölçümleri artık hala hedeflenen cihazlar için sayımları içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Cihaz ikizlerini uygulama hakkında bilgi edinmek [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](iot-hub-devguide-device-twins.md).

* Oluşturmak, güncelleştirmek veya silmek otomatik cihaz yapılandırması için adımlarında yol [yapılandırma ve izleme IOT cihazlarını uygun ölçekte](iot-hub-auto-device-config.md).

* Cihaz ikizleri ve otomatik cihaz yapılandırmalarında kullanarak bir üretici yazılımı güncelleştirme desenini uygulama [öğretici: bir cihazın üretici yazılımı güncelleştirme işlemini uygulamak](tutorial-firmware-update.md). 
