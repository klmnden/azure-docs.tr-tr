---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b640a3cb9382ad72bb48e06c6a7074c96409e2e4
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188992"
---
<!-- description of message routing used in the Azure CLI, PowerShell, and RM routing articles.-->

Simülasyon cihazı tarafından iletiye eklenen özellikler temelinde iletileri farklı kaynaklara yönlendireceksiniz. Özel yönlendirmesi olmayan iletiler varsayılan uç noktaya gönderilir (iletiler/olaylar). Sonraki öğreticide, IOT Hub'ına ileti göndermek ve bunları farklı hedeflere bakın.

|value |Sonuç|
|------|------|
|level="storage" |Azure Depolama'ya yazın.|
|level="critical" |Service Bus kuyruğuna yazın. Mantıksal Uygulama iletiyi kuyruktan alır ve Office 365 kullanarak iletiyi e-postayla gönderir.|
|default |Power BI'ı kullanarak bu verileri görüntüleyin.|

İlk adım, verileri yönlendirilir uç noktayı kurmak sağlamaktır. İkinci adım, bu uç noktasını kullanan ileti rotayı ayarlamaktır. Yönlendirme yedekleme ayarladıktan sonra portalda uç noktaları ve ileti yollarını görüntüleyebilirsiniz.