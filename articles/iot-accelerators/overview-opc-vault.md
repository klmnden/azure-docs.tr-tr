---
title: OPC kasası - Azure nedir | Microsoft Docs
description: OPC kasası genel bakış
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 99dfcaeb1ef5b52e6827f1b3ac65d6201557a8fb
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490385"
---
# <a name="what-is-opc-vault"></a>OPC kasası nedir?

Kasa OPC yapılandırma, kaydetme ve sertifika yaşam döngüsü OPC UA sunucusu ve istemci uygulamalarını bulutta yönetme bir mikro hizmetidir. Bu makalede, OPC kasanın basit kullanım durumları açıklanmaktadır.

## <a name="certificate-management"></a>Sertifika yönetimi

Örneğin, kendi OPC UA sunucunuz, yeni oluşturulan istemci uygulamaya bağlanmak bir üretim şirketi gerekir. Üretici ilk erişim sunucu makinesinin yaptığında, bir hata iletisi hemen istemci uygulamaya güvenli olmadığını belirtmek için OPC UA sunucu uygulaması gösterilmektedir. Bu mekanizma vicious atölyede deşifre etme engelleyen herhangi bir uygulamada yetkisiz erişimi önlemek için OPC UA server makinesi içinde yerleşik olarak bulunur.

## <a name="application-security-management"></a>Uygulama güvenlik yönetimi
Profesyonel bir güvenlik OPC kasa mikro hizmet OPC kasa sertifika kayıt defteri, depolama ve yaşam döngüsü yönetimi için tüm işlevlere sahip olduğu herhangi bir istemci uygulama ile iletişim kurmak OPC UA sunucusu kolayca etkinleştirmek için kullanır. Artık bir OPC UA sunucusuna güvenli bir şekilde bağlıdır, bu yeni oluşturulan istemci uygulamasına iletişim kurabilir

## <a name="the-complete-opc-vault-architecture"></a>Tüm OPC kasa mimarisi
Aşağıdaki diyagramda, tüm OPC kasa mimarisi gösterilmektedir.

![OPC kasa mimarisi](media/overview-opc-vault-architecture/opc-vault.png)