---
title: Azure IOT Hub için cihaz yapılandırma en iyi yöntemler | Microsoft Docs
description: IOT cihazlarını uygun ölçekte yapılandırmak için en iyi uygulamalar hakkında bilgi edinin
author: chrisgre
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: c97395981ea3af90c7b0c590cb049fccc7392304
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797201"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Bir IOT çözüm içinde cihaz yapılandırması için en iyi uygulamalar

Azure IOT hub otomatik cihaz Yönetimi döngülerini tamamına büyük cihaz filolarına yönetme çok sayıda yinelenen ve karmaşık görevleri otomatik hale getirir. Bu makalede, geliştirme ve IOT çözümünü işletim birçok dahil çeşitli rolleri için en iyi tanımlar.

* **IOT donanım üreticisi/entegratörü:** IOT donanım üreticileri çeşitli üretici veya tedarikçileri üretilen veya başka üreticiler tarafından tümleşik bir IOT dağıtımı için donanım sağlayarak donanım derleyerek tümleştiricileri. Geliştirme ve üretici yazılımı, katıştırılmış işletim sistemleri ve ekli yazılımdan tümleştirme söz konusu.

* **IOT çözümü geliştirme:** Bir IOT çözümünün geliştirilmesini, genellikle bir çözüm geliştirici tarafından yapılır. Bu geliştirici, şirket içi bir takım veya bu etkinliğe uzmanlaşan bir sistem tümleştiricisi parçası olabilir. IOT çözümü geliştirme çeşitli bileşenlerinin sıfırdan IOT çözümü geliştirmek, çeşitli standart veya açık kaynak bileşenlerini tümleştirmek veya özelleştirme bir [IOT Çözüm Hızlandırıcısı](/azure/iot-accelerators/).

* **IOT çözümü işleci:** IOT çözümü dağıtıldıktan sonra uzun süreli işlem, izleme, yükseltme ve bakım gerektirir. Bu görevler, bilgi teknolojisi uzmanları, donanım işlemler ve Bakım takımlar ve kim genel IOT altyapının doğru davranışları etki alanı uzmanlarıyla oluşan bir şirket içi ekibi tarafından yapılabilir.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>IOT cihazlarını uygun ölçekte yapılandırmak için otomatik cihaz yönetimini anlama

Otomatik cihaz Yönetimi'ni içeren birçok yararından biri [cihaz ikizlerini](iot-hub-devguide-device-twins.md) ve [modül ikizlerini](iot-hub-devguide-module-twins.md) istenen ve bildirilen durumlar cihazlar ve bulut arasında eşitlenecek. [Otomatik cihaz yapılandırmaları](iot-hub-auto-device-config.md) otomatik olarak büyük çiftleri kümesini güncelleştirmek ve ilerleme durumunu ve uyumluluğunu özetler. Otomatik cihaz Yönetimi aşağıdaki üst düzey adımları açıklamak geliştirilen ve kullanılır:

* **IOT donanım üreticisi/entegratörü** kullanarak katıştırılmış uygulama içinde cihaz yönetimi özellikleri uygulayan [cihaz ikizlerini](iot-hub-devguide-device-twins.md). Bu özellikler, üretici yazılımı güncelleştirmelerini, yazılım yükleme ve güncelleştirme ve ayar yönetimi dahil olabilir.

* **IOT çözüm geliştirici** cihaz yönetimi işlemleri kullanarak yönetim katmanı uygulayan [cihaz ikizlerini](iot-hub-devguide-device-twins.md) ve [otomatik cihaz yapılandırmaları](iot-hub-auto-device-config.md). Çözüm, cihaz yönetim görevlerini gerçekleştirmek için bir işleç arabirimi tanımlama içermelidir.

* **IOT çözüm işleci** kullanan IOT çözümü birlikte özellikle cihazları için cihaz yönetim görevlerini gerçekleştirmek için yapılandırma değişiklikleri gibi üretici yazılımı güncelleştirmelerini başlatma, ilerlemeyi izlemek ve sorunlarını giderme sorunları ortaya çıkar.

## <a name="iot-hardware-manufacturerintegrator"></a>IOT donanım üreticisi/entegratörü

Donanım üreticileri ve katıştırılmış yazılım geliştirme ile ilgili entegratörleri için en iyi uygulamalar şunlardır:

* **Uygulama [cihaz ikizlerini](iot-hub-devguide-device-twins.md):** Cihaz ikizlerini eşitleme istenen yapılandırma buluttan ve geçerli yapılandırmanıza ve cihaz özelliklerini raporlamak etkinleştirin. Cihaz ikizlerini katıştırılmış uygulamalar içinde uygulamak için en iyi yollarından biri sayesinde [Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks). İçin cihaz ikizlerini yapılandırması için en uygun bunlar:

    * Çift yönlü iletişimi destekler.
    * Her iki bağlı ve bağlantısı kesilmiş cihaz durumları için izin verin.
    * Son tutarlılık ilkesini izleyin.
    * Bulutta tam olarak sorgulanabilir ' dir.

* **Cihaz yönetimi için cihaz ikizi yapısı:** Cihaz ikizi, cihaz yönetim özelliklerini mantıksal olarak birlikte bölümler halinde gruplandırılmıştır şekilde yapılandırılmalıdır. Bunun yapılması, yapılandırma değişiklikleri ikizinin diğer bölümleri etkilemeden yalıtılmış olması olanak sağlar. Örneğin, üretici yazılımı, yazılım, başka bir bölüme için istenen özellikler bir bölümündeki oluşturun ve ağ ayarlarını bir üçüncü bölümü. 

* **Cihaz yönetimi için yararlı olan rapor cihaz öznitelikleri:** Fiziksel cihazı gibi öznitelikleri marka ve model, üretici yazılımı, işletim sistemi, seri numarası, ve diğer tanımlayıcıları yapılandırma değişiklikleri hedeflemek için parametre olarak ve raporlama için kullanışlıdır.

* **Durumunu ve ilerlemesini raporlama için ana durumları tanımlayın:** Böylece işleci için bildirilen üst düzey durumlar listelenmiş. Örneğin, üretici yazılımı güncelleştirme durumunu geçerli, indirme, uygulama, devam eden ve hata olarak rapor. Daha fazla bilgi için ek alanlar üzerinde her bir durum tanımlayın.

## <a name="iot-solution-developer"></a>IOT çözümü geliştirme

Azure'da tabanlı sistemler oluşturmaya IOT çözüm geliştiriciler için en iyi uygulamalar şunlardır:

* **Uygulama [cihaz ikizlerini](iot-hub-devguide-device-twins.md):** Cihaz ikizlerini eşitleme istenen yapılandırma buluttan ve geçerli yapılandırmanıza ve cihaz özelliklerini raporlamak etkinleştirin. Cihaz ikizlerini bulut çözümleri uygulamaları içinde uygulamak için en iyi yollarından biri sayesinde [Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks). İçin cihaz ikizlerini yapılandırması için en uygun bunlar:

    * Çift yönlü iletişimi destekler.
    * Her iki bağlı ve bağlantısı kesilmiş cihaz durumları için izin verin.
    * Son tutarlılık ilkesini izleyin.
    * Bulutta tam olarak sorgulanabilir ' dir.

* **Cihaz ikizi etiketleri kullanarak cihazları düzenleyin:** Çözüm, kalite halkaları ya da diğer cihazları kanarya gibi çeşitli dağıtım stratejilerini göre kümesini tanımlamak işleç izin vermelidir. Çözümünüz içinde cihaz ikizi etiketleri kullanarak cihaz kuruluş uygulanabilir ve [sorguları](iot-hub-devguide-query-language.md). Cihaz kuruluş için yapılandırma sunulmasını güvenli bir şekilde ve doğru bir şekilde izin vermek gereklidir.

* **Uygulama [otomatik cihaz yapılandırmaları](iot-hub-auto-device-config.md):** Otomatik cihaz yapılandırmaları dağıtma ve cihaz ikizlerini aracılığıyla IOT cihazlarının daha büyük kümeleri için izleme yapılandırmasını değiştirir. Otomatik cihaz yapılandırmaları cihaz ikizlerini kümesi hedef **hedef koşulu** bir sorgudur ikizi etiketleri cihazda veya bildirilen özellikler. **Hedef içerik** içinde hedeflenen cihaz ikizlerini ayarlanan istenen özellikleri kümesidir. İçerik hedef, IOT donanım üreticisi/entegratörü tarafından tanımlanan cihaz ikizi yapısı ile hizalamanız gerekir.

   **Ölçümleri** cihaz çifti sorguları bildirilen özellikler ve aynı zamanda IOT donanım üreticisi/entegratörü tarafından tanımlanan cihaz ikizi yapısı ile hizalamanız gerekir. Otomatik cihaz yapılandırmaları yararı, IOT hub'ı hiçbir zaman aşan bir hızda cihaz ikizi işlemleri de [sınırları](iot-hub-devguide-quotas-throttling.md) cihaz ikizi okumaları ve güncelleştirmeler için.

* **Kullanım [cihaz sağlama hizmeti](../iot-dps/how-to-manage-enrollments.md):** Çözüm geliştiricilerin kullanması gereken cihaz sağlama hizmeti yeni cihazları için cihaz ikizi etiketler atama tarafından otomatik olarak yapılandırılacak şekilde **otomatik cihaz yapılandırmaları** ikizlerini etiketi içeren, hedeflenen. 

## <a name="iot-solution-operator"></a>IOT çözümü işleci

Azure üzerinde derlenen bir IOT çözümünü kullanarak IOT çözümü işleçleri için en iyi uygulamalar şunlardır:

* **Yönetim için cihazları düzenleyin:** IOT çözümü tanımlayın veya kalite halkaları oluşturma veya diğer kümeleri kanarya gibi çeşitli dağıtım stratejilerini bağlı cihazlar için izin gerekir. Cihazları kümesi yapılandırma değişiklikleri alma ve diğer ölçekli cihaz yönetimi işlemleri gerçekleştirmek için kullanılır.

* **Yapılandırma değişiklikleri aşamalı kullanarak gerçekleştirin:**  Aşamalı yapabildiği değişiklikleri operatörün insanın ufkunu genişleten bir IOT cihazları kümesine dağıtır, genel bir işlemdir. Aşamalı olarak geniş ölçek bozucu değişiklikler yapma riskini azaltmak için değişiklik olmaktır.  İşleci, çözümün arabirimi oluşturmak için kullanmalısınız bir [otomatik cihaz yapılandırma](iot-hub-auto-device-config.md) ve hedefleme koşul cihazları (örneğin, kanarya bir grubu) başlangıç kümesi hedeflemelidir. İşleci, ardından cihaz ilk kümesini yapılandırma değişikliği doğrulamalıdır.

   Doğrulama tamamlandıktan sonra işleci daha büyük bir cihaz eklemek için otomatik cihaz yapılandırmasını güncelleştirir. İşleç önceliği yapılandırmasının şu anda bu cihazları hedefleyen diğer yapılandırmalar daha yüksek olması de ayarlamanız gerekir. Otomatik cihaz yapılandırması tarafından bildirilen ölçümleri kullanarak dağıtım izlenebilir.

* **Hataları veya yanlış yapılandırmalarını durumunda geri alma işlemleri gerçekleştirin:**  Hata veya yanlış yapılandırmalarını neden olan otomatik cihaz yapılandırmasını değiştirerek geri alınabilir **koşul hedefleyen** böylece cihazlar artık hedefleme koşulu karşılamıyor. Düşük öncelikli başka bir otomatik cihaz yapılandırmasını, bu cihazlar için hala hedeflendiğinden emin olun. Geri alma ölçümleri görüntüleyerek başarılı olduğunu doğrulayın: Toplu geri yapılandırma artık untargeted cihazlar için durumunu göstermesi gerekir ve ikinci yapılandırma ölçümleri artık hala hedeflenen cihazlar için sayımları içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Cihaz ikizlerini uygulama hakkında bilgi edinmek [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](iot-hub-devguide-device-twins.md).

* Oluşturmak, güncelleştirmek veya silmek otomatik cihaz yapılandırması için adımlarında yol [yapılandırma ve izleme IOT cihazlarını uygun ölçekte](iot-hub-auto-device-config.md).

* Cihaz ikizleri ve otomatik cihaz yapılandırmalarında kullanarak bir üretici yazılımı güncelleştirme desenini uygulama [Öğreticisi: Cihaz üretici yazılımı güncelleştirme işlemini uygulamak](tutorial-firmware-update.md).
