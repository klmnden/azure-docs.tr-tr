---
title: OPC İkizi mimarisi - Azure | Microsoft Docs
description: OPC İkizi mimarisi
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 6ce9394f3d454bda5ead51f2c77a47db137a5136
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606172"
---
# <a name="opc-twin-architecture"></a>OPC İkizi mimarisi

Aşağıdaki diyagramlarda OPC İkizi mimarisi gösterilmektedir.

## <a name="discover-and-activate"></a>Bulma ve etkinleştirme

1. İşleci, modüldeki tarama ağ etkinleştirir veya bulma URL'si kullanarak tek seferlik bir bulmayı kolaylaştırır. Uygulama bilgilerini ve bulunan uç noktaları telemetri işleme için ekleme aracıya gönderilir.  OPC UA cihaz ekleme Aracısı bulma veya Tarama modunda olduğunda OPC İkizi IOT Edge modülü tarafından gönderilen OPC UA sunucu bulma olayları işler. Uygulama kaydı ve OPC UA cihaz kayıt defterinde güncelleştirmeler bulma olayları sonucu.

   ![OPC İkizi nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin1.png)

1. Operatör bulunan uç noktası'nın sertifikasını inceler ve erişim için kayıtlı bir uç nokta çifti etkinleştirir. 

   ![OPC İkizi nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin2.png)

## <a name="browse-and-monitor"></a>Göz atma ve izleme

1. Sonra işleci göz atın veya sunucu bilgi modeli incelemek, nesne değişkenleri okuma/yazma ve yöntemleri çağırmak için İkizi hizmeti REST API'sini kullanabilirsiniz.  Kullanıcı, OPC UA tam olarak HTTP ve JSON ifade edilen basitleştirilmiş bir API kullanır.

   ![OPC İkizi nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin3.png)

1. İkiz hizmet REST arabirimi, OPC yayımcı olarak izlenen öğeleri ve abonelikler oluşturmak için de kullanılabilir. OPC Publisher, IOT Hub'ına OPC UA server sistemlerden gönderilecek telemetri sağlar. OPC Publisher hakkında daha fazla bilgi için bkz: [OPC yayımcı nedir](overview-opc-publisher.md).

   ![OPC İkizi nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin4.png)
