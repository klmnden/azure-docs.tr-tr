---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Soğutmalı otomatları yönetmek için yeni bir Azure IoT Central uygulaması oluşturun. Simülasyon cihazlarınızdan oluşturulan telemetri verilerini görüntüleyin.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/15/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: af06766d89804b2f3d0aaf061494fb836f6ec262
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465614"
---
# <a name="create-an-azure-iot-central-application"></a>Azure IoT Central uygulaması oluşturma

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Microsoft Azure IoT Central uygulamanızı tanımlayabilirsiniz. Bu hızlı başlangıçta şu işlemleri nasıl yapacağınız gösterilir:

- Örnek bir _cihaz şablonu_ ve simülasyon _cihazları_ içeren bir Azure IoT Central uygulaması oluşturun.
- Uygulamanızda **Soğutmalı Otomat** cihaz şablonunun özelliklerini görüntüleyin.
- Simülasyon **Buzdolabı** cihazlarınızın telemetri verilerini ve analizini görüntüleyin.

Bu hızlı başlangıçta, bir cihaz şablonundan simülasyon **Buzdolabı** cihazını görüyorsunuz. Simülasyon cihazı:

* Uygulamanıza sıcaklık ve basınç gibi telemetri verileri gönderir.
* Uygulamanıza hareket uyarısı gibi cihaz özelliği değerlerini bildirir.
* Uygulamada ayarlayabileceğiniz fan hızı gibi cihaz ayarlarını içerir.

Azure IoT Central uygulamasında bir cihaz şablonundan simülasyon cihazı oluşturduğunuzda, simülasyon cihazı gerçek bir cihaz bağlamadan önce uygulamanızı test etmenize olanak tanır.

## <a name="create-the-application"></a>Uygulama oluşturma

Bu hızlı başlangıcı tamamlamak için, **Örnek Contoso** uygulama şablonundan bir Azure IoT Central uygulaması oluşturmalısınız.

Azure IoT Central [Uygulama Yöneticisi](https://aka.ms/iotcentral) sayfasına gidin. Sonra Azure aboneliğinize erişmek için kullandığınız e-posta adresini ve parolayı girin:

![Kuruluş hesabınızı girin](media/quick-deploy-iot-central/sign-in.png)

Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin:

![Azure IoT Central Uygulama Yöneticisi sayfası](media/quick-deploy-iot-central/iotcentralhome.png)

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. **Ücretsiz Deneme Uygulaması** ödeme planını seçin.
1. **Contoso IoT** gibi kolay bir uygulama adı seçin. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL ön ekini daha akılda kalır bir şeyle değiştirebilirsiniz.
1. **Örnek Contoso** uygulama şablonunu seçin.
1. Ardından **Oluştur**’u seçin.

![Azure IoT Central Uygulama Oluştur sayfası](media/quick-deploy-iot-central/iotcentralcreate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, **Soğutmalı Otomat** cihaz şablonunu ve simülasyon cihazlarını içeren, önceden doldurulmuş bir Azure IoT Central uygulaması oluşturdunuz. Bir oluşturucu olarak kendi cihaz şablonlarınızı tanımlama hakkında daha fazla bilgi edinmek için bkz. [Uygulamanızda yeni cihaz şablonu tanımlama](tutorial-define-device-type.md).
