---
title: Azure IOT OPC UA cihaz yönetim mimarisi | Microsoft Docs
description: OPC İkizi mimarisi
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 904c889866d3aa24e1da387af44b3f589b7769aa
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759330"
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

1. İkiz hizmet REST arabirimi, OPC yayımcı olarak izlenen öğeleri ve abonelikler oluşturmak için de kullanılabilir. OPC Publisher, IOT Hub'ına OPC UA server sistemlerden gönderilecek telemetri sağlar. OPC Publisher hakkında daha fazla bilgi için bkz: [OPC yayımcı](https://github.com/Azure/iot-edge-opc-publisher) github deposu.

   ![OPC İkizi nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin4.png)