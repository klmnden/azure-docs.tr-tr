---
title: OPC İkizi - Azure nedir | Microsoft Docs
description: OPC İkizi'ne genel bakış
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 9daf1a7e58af23cb78705691217bf9709359c4d5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60889871"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-device-management"></a>Azure IOT açık Platform iletişim (OPC) cihaz yönetimi nedir?

Bulut ve Fabrika ağ bağlanmak için Azure IOT Edge ve IOT Hub'ı kullanan mikro OPC İkizi oluşur. OPC İkizi bulma, kayıt ve REST API'ler aracılığıyla endüstriyel cihazlara uzaktan denetimi sağlar. OPC İkizi bir OPC birleşik mimari (OPC UA) SDK'sı dilden programlama ve sunucusuz bir iş akışında bulunan gerektirmez. Bu makalede, çeşitli OPC İkizi kullanım örnekleri açıklanmaktadır.

## <a name="discovery-and-control"></a>Bulma ve Denetim
Bulma ve kayıt için basit için OPC İkizi'ni kullanabilirsiniz.

### <a name="simple-discovery-and-registration"></a>Basit bulma ve kayıt
OPC İkizi OPC UA sunucuları bulundu ve kayıtlı Fabrika ağ taramak Fabrika işleçleri sağlar. Alternatif olarak, Fabrika işleçleri de el ile bilinen bulma URL'si kullanarak OPC UA cihazları kaydedebilir. Örneğin, IOT Edge ağ geçidi OPC İkizi modülü ile fabrikadaki yüklendikten sonra tüm OPC UA cihazlara bağlanmak için Fabrika operatör uzaktan bir ağ tarama tetikleyin ve tüm OPC UA sunucuları görsel olarak bakın. 

### <a name="simple-control"></a>Basit Denetim
OPC İkizi olaylarına tepki verme ve Fabrika katı makinelerinde buluttan otomatik olarak veya el ile yeniden çalışma sırasında Fabrika işleçleri sağlar. OPC İkizi OPC UA server hizmetlerini çağırmak için değişkenleri okuma/yazma yöntemleri ve yürütme için adres alanı de göz atın, REST API'ler sağlar. Örneğin, bir ortak, üretim hattı denetlemek için sıcaklık KPI'yı kullanır. Sıcaklık algılayıcısı değişiklik OPC Publisher'ı kullanarak verileri yayımlar. Fabrika işleci sıcaklık eşiğine ulaştığında bir uyarı alır. Üretim hattı otomatik olarak OPC İkizi soğutma imkanı sunar. Fabrika işleci, aşağı seyrek erişimli bildirilir.

## <a name="authentication"></a>Kimlik Doğrulaması
Basit kimlik doğrulaması ve basit bir geliştirici deneyimi için OPC İkizi'ni kullanabilirsiniz.

### <a name="simple-authentication"></a>Basit kimlik doğrulaması 
Azure Active Directory AAD tabanlı kimlik doğrulaması ve uçtan uca denetim OPC İkizi kullanır. Örneğin, OPC İkizi OPC İkizi'ne bir makine üzerinde operatör yürüttü belirlemek için en üstünde oluşturulacak uygulamanın sağlar. Makine tarafında, bu denetim, OPC UA olur. Bulut tarafındaki bir sabit istemci denetim günlüğü ve AAD kimlik doğrulaması REST API ile ilgili depolama aracılığıyla öyledir.

### <a name="simple-developer-experience"></a>Basit bir geliştirici deneyimi 
OPC İkizi REST API'leri aracılığıyla tüm programlama dillerinde yazılan uygulamalar ile kullanılabilir. Geliştiriciler bir çözüme bir OPC UA istemcisi tümleştirme gibi OPC UA SDK'sının bilgi gerekli değildir. OPC İkizi durum bilgisi olmayan, sunucusuz mimarileri ile sorunsuz bir şekilde tümleştirebilirsiniz. Bir uyarı ve olayı Panosu, JavaScript veya TypeScript OPC İkizi C bilgisi kullanarak olayları yanıtlamak için mantığı yazabilirsiniz için geliştirdiği bir uygulama gibi bir tam yığın web Geliştirici C#, ya da tam OPC UA yığını uygulaması. 

## <a name="next-steps"></a>Sonraki adımlar

OPC İkizi ve kullanımları hakkında öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC kasası nedir](overview-opc-twin-architecture.md)