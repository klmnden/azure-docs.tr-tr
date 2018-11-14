---
title: Azure IOT Central cihaz köprüsü oluşturun | Microsoft Docs
description: IOT Central, IOT Central uygulamasına (Sigfox, Parçacık, şeyler ağ vb.) diğer IOT bulutlara bağlanmak için cihaz köprü oluşturun.
services: iot-central
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 11/8/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 83f053a8815f31803f536920497fdc42e72d2a2d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629275"
---
# <a name="build-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>IOT Central, IOT Central için diğer IOT bulutlara bağlanmak için cihaz köprüsü oluşturun

*Bu konu, Yöneticiler için geçerlidir.*

IOT Central cihaz köprüsü, Sigfox, Parçacık, şeyler ağ ve diğer bulutlarda IOT Central uygulamanıza bağlanan bir açık kaynak çözümüdür. Varlık Sigfox'ın düşük güç geniş alan ağına bağlı cihazları izleme kullandığınız ya da parçacık cihaz bulut cihazlarda izleme veya TTN cihazlarda izleme Toprak nem kullanarak uzaktan kalite kullanarak doğrudan yapabilecekleriniz IOT gücünden yararlanın IOT Central cihaz Köprüsü'nü kullanarak orta. Cihaz köprüsü, diğer bulut IOT Central uygulamanıza cihazlarınıza gönderin veri ileterek, IOT Central ile diğer IOT bulut bağlanır. IOT Central uygulamanızda derleme kuralları ve bu veriler üzerinde analiz çalıştırın, Microsoft Flow ve Azure Logic apps'te iş akışları oluşturun, yapabilirsiniz verileri ve daha fazlasını dışarı aktarın. Alma [IOT Central cihaz köprüsü](https://aka.ms/iotcentralgithubdevicebridge) github'dan

## <a name="what-is-it-and-how-does-it-work"></a>Nedir ve nasıl çalışır?
IOT Central cihaz köprüsü, github'da açık kaynaklı bir çözümdür. Çeşitli Azure kaynakları ile özel bir Azure Resource Manager şablonu Azure aboneliğinize dağıtan bir "Azure'a dağıtın" düğmesi ile kullanıma hazır. Kaynakları şunlardır:
-   Azure işlev uygulaması
-   Azure Depolama Hesabı
-   App Service planı (S1 katmanı)
-   Azure Key Vault işlev uygulaması, cihaz köprüsü kritik parçasıdır. Diğer IOT platformları veya basit bir Web kancası tümleştirmesi aracılığıyla özel hiçbir platforma HTTP POST isteği alır. Sigfox ve parçacık TTN bulutlara bağlanma gösteren örnekler sağladık. Platformunuza işlev uygulamanız için HTTP POST istekleri gönderebilir, özel IOT bulutuna bağlanmak için bu çözümü kolayca genişletebilirsiniz.
İşlev uygulaması verileri IOT Central tarafından kabul edilen bir biçime dönüştürür ve boyunca DPS API'leri iletir.

![Azure işlevleri ekran görüntüsü](media/howto-build-iotc-device-bridge/azfunctions.png)

IOT Central uygulamanızı yönlendirilmiş ileti cihaz tarafından cihaz Kimliğini tanıyorsa bu cihaz için yeni bir ölçüm görünür. Cihaz kimliği, IOT Central uygulamanız tarafından hiçbir zaman görüldü, işlev uygulamanızı yeni bir cihaz, cihaz kimliği ile kaydolmayı deneyecek ve IOT Central uygulamanızda "ilişkilendirilmemiş aygıt" olarak görünür. 

## <a name="how-do-i-set-it-up"></a>Nasıl, ayarladığım?
Ayrıntılı Github deposunda ve benioku dosyasındaki yönergeleri listelenir. 

## <a name="pricing"></a>Fiyatlandırma
Bu tüm Azure aboneliğinizde barındırılır. Tahmini maliyet sağlanan kaynakların çoğunu geldiği [fiyatı, standart bir App Service planı]( https://azure.microsoft.com/en-us/pricing/details/app-service/windows/). Bu konu hakkında daha fazla bilgi ve bu README dosyasında azaltmak için olası yolları.

## <a name="next-steps"></a>Sonraki adımlar

IOT Central cihaz köprü oluşturulacağını öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazlarınızı yönetme](howto-manage-devices.md)