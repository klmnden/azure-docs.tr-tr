---
title: Azure IOT Central uygulamalarınız için cihaz şablonu sürüm anlama | Microsoft Docs
description: Yeni sürümler oluşturarak ve canlı bağlı cihazlarınızın etkilemeden cihaz şablonlarınızı yineleme
author: sandeeppujar
ms.author: sandeepu
ms.date: 03/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d4f9617a5c2ba6f6cf8dc261845aa98e33d70a55
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59281786"
---
# <a name="create-a-new-device-template-version"></a>Yeni bir cihaz şablon sürümü oluşturma

Azure IOT Central, IOT uygulamaların hızlı geliştirilmesini sağlar. Cihaz şablonu tasarımlarınızı, ekleme, düzenleme veya ölçüleri, ayarları ve özellikleri silme hızla yineleyebilirsiniz. Bu değişikliklerden bazıları şu anda bağlı cihazlar için zorlayıcı olabilir. Azure IOT Central, bu bozucu değişiklikleri tanımlar ve bu güncelleştirmelerin cihazlara güvenli bir şekilde dağıtmak için bir yol sağlar.

Oluşturduğunuz cihaz şablonu bir sürüm numarasına sahip. Varsayılan olarak, 1.0.0 sürüm numarasıdır. Bir cihaz şablonu düzen ve bu değişiklik etkileyebilir bağlı cihazlar live, Azure IOT Central, yeni bir cihaz şablonu sürüm oluşturmanızı ister.

> [!NOTE]
> Bir cihaz şablonu oluşturma hakkında daha fazla bilgi için [bir cihaz şablonu ayarlama](howto-set-up-template.md)

## <a name="changes-that-prompt-a-version-change"></a>Bir sürüm değişikliği iste değişiklikleri

Genel değişiklik ayarlarını veya cihaz şablonunuzun özellikler bir sürüm değişikliği ister.

> [!NOTE]
> Cihaz şablona yapılan değişiklikler yeni bir sürüm hiçbir cihaz oluşturmak için istemde bulunmayın veya çoğu, bir cihaz bağlı değil.

Aşağıdaki listede yeni bir sürüm gerekli olabilecek kullanıcı eylemleri açıklanmaktadır:

* Özellikler (gerekli)
    * Ekleme veya gerekli bir özellik silme
    * Bir özellik, cihazlarınızın iletileri göndermek için kullanılan alan adı alan adının değiştirilmesi.
*  (İsteğe bağlı) özellikleri
    * İsteğe bağlı bir özelliği silme
    * Bir özellik, cihazlarınızın iletileri göndermek için kullanılan alan adı alan adının değiştirilmesi.
    * İsteğe bağlı bir özellik için gerekli bir özellik değiştirme
*  Ayarlar
    * Ekleme veya bir ayarı silme
    * Bir ayar, cihazlarınızın iletileri gönderip almak için kullanılan alan adı alan adının değiştirilmesi.

## <a name="what-happens-on-version-change"></a>Sürüm değişiklik ne olacak?

Bir sürüm değişikliği olduğunda kuralları ve cihaz panolar için ne olur?

**Kuralları** özellikleri bağımlı olan koşullar içerebilir. Bir veya daha fazla bu özellik kaldırılırsa, bu kurallar, yeni cihaz şablon sürümü bozuk durumda. Belirli kurallar gidin ve koşulları kuralları düzeltmek için güncelleştirin. Önceki sürümünüz için kuralları, herhangi bir etkisi ile çalışması gerekir.

**Cihaz panolar** birkaç kutucuk türleri içerebilir. Bazı kutucuklar, ayarları ve özellikleri içerebilir. Bir özellik ya da bir kutucukta kullanılan ayar kaldırıldığında, kutucuğu tamamen veya kısmen bozulur. Kutucuğuna gidin ve kutucuğu kaldırarak veya kutucuk içeriğini güncelleştirme sorunu düzeltin.

## <a name="migrate-a-device-across-device-template-versions"></a>Bir cihaz, cihaz şablonu sürümleri arasında geçirme

Birden çok sürümünü cihaz şablonu oluşturabilirsiniz. Zamanla, bu cihaz şablonlarını kullanarak birden çok bağlı cihazlar gerekir. Cihazları başka bir cihaz şablonunuzu bir sürümden diğerine geçirebilirsiniz. Aşağıdaki adımlar, bir cihaz geçirileceği açıklanır:

1. Git **Device Explorer** sayfası.
1. Başka bir sürüme geçirmek istediğiniz cihazı seçin.
1. Seçin **cihaz geçirme**.
1. İstediğiniz cihaza geçmek ve aboneliği seçin sürüm numarasını seçin **geçirme**.

![Bir cihaz geçirme](media/howto-version-devicetemplate/pick-version.png)

## <a name="next-steps"></a>Sonraki adımlar

Cihaz şablonu sürümleri Azure IOT Central, uygulamanızda kullanmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Telemetri kuralları oluşturma](howto-create-telemetry-rules.md)