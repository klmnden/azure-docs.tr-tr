---
title: Azure IOT merkezi uygulamalarınız için cihaz şablonu sürüm anlama | Microsoft Docs
description: Yeni sürümler oluşturarak ve canlı bağlı aygıtlarınızı etkileyen olmadan cihaz şablonlarınızı yineleme
services: iot-central
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/19/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 3a399b6b54f8da9c48b4bdfe7e98a527262bb174
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="create-a-new-device-template-version"></a>Yeni bir cihaz şablonu sürüm oluşturma

Microsoft Azure IOT merkezi IOT uygulamaların hızlı geliştirme sağlar. Hızlı bir şekilde ekleme, düzenleme veya ölçümleri, ayarları veya özellikleri silmek, aygıt şablon tasarımları üzerinde yineleyebilirsiniz. Bu değişikliklerden bazıları şu anda bağlı aygıtlar için zorlayıcı olabilir. Azure IOT Orta Bu önemli değişiklikler tanımlar ve güvenli bir şekilde bu güncelleştirmeler cihazlara dağıtmak için bir yol sağlar.

Oluşturduğunuzda bir aygıt şablon sürüm numarasına sahip. Varsayılan olarak, 1.0.0 sürüm numarasıdır. Bir aygıt şablon düzenleyin ve bu değişiklik etkileyebilir bağlı aygıtlar canlı, Azure IOT merkezi yeni bir cihaz şablonu sürüm oluşturmak isteyip istemediğinizi sorar.

> [!NOTE]
> Bir aygıt şablonu oluşturma hakkında daha fazla bilgi edinmek için [aygıt şablon ayarlama](howto-set-up-template.md)

## <a name="changes-that-prompt-a-version-change"></a>Bir sürüm sor değişiklik

Genel ayarları veya aygıt şablonun özelliklerini değişiklikleri bir sürüm değişikliği ister.

> [!NOTE]
> Cihaz şablona yapılan değişiklikler yeni bir sürüm hiçbir aygıt oluşturulmasında istemez veya en büyük bir aygıt bağlı değil.

Aşağıdaki listede, yeni bir sürüm gerektirebilir kullanıcı eylemleri açıklanmaktadır:

* Özellikler (gerekli)
    * Ekleme veya gerekli bir özellik silme
    * Bir özellik, ileti göndermek için cihazlar tarafından kullanılan alan adı alan adını değiştirme.
*  Özellikler (isteğe bağlı)
    * İsteğe bağlı bir özellik silme
    * Bir özellik, ileti göndermek için cihazlar tarafından kullanılan alan adı alan adını değiştirme.
    * İsteğe bağlı bir özellik için gerekli bir özellik değiştirme
*  Ayarlar
    * Ekleme veya bir ayar silme
    * İleti gönderme ve alma için cihazlar tarafından kullanılan alan adı bir ayar alan adını değiştirme.

## <a name="what-happens-on-version-change"></a>Sürüm değişiminde ne olur?

Sürüm değişiklik yapıldığında kuralları ve cihaz panolar ne olur?

**Kuralları** özellikleri bağımlı koşulları içerebilir. Bir veya daha fazla bu özellikleri kaldırdıysanız, bu kurallar, yeni cihaz şablonu sürümü bozuk. Belirli kurallar gidin ve kuralları düzeltmek için koşullar güncelleştirin. Önceki sürümü için kurallar ile herhangi bir etkisi çalışması gerekir.

**Cihaz panolar** döşeme çeşitli türlerde içerebilir. Bazı kutucuklar ayarları ve özellikleri içeriyor olabilir. Bir özellik veya bir parçasında kullanılan ayar kaldırıldığında, döşeme tamamen veya kısmen bozulur. Döşeme gidin ve da döşeme kaldırarak veya kutucuğun içeriği güncelleştirme sorunu düzeltin.

## <a name="migrate-a-device-across-device-template-versions"></a>Bir aygıt cihaz şablonu sürümleri arasında geçirme

Birden fazla sürümünü cihaz şablonu oluşturabilirsiniz. Zaman içinde bu cihaz şablonları kullanarak birden çok bağlı cihazları sahip olur. Cihazları başka bir aygıt şablonunuzu bir sürümünden geçirebilirsiniz. Aşağıdaki adımlar bir aygıtı geçirmek nasıl açıklamaktadır:

1. Git **Explorer** sayfası.
1. Başka bir sürüme geçiş yapmanız cihazı seçin.
1. Seçin **aygıt geçirmek**.
1. Cihaza geçirmek ve seçmek için sürüm numarasını seçin **geçirme**.

![Bir aygıt geçirme](media\howto-version-devicetemplate\pick-version.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızda cihaz şablonu sürümleri kullanmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Telemetri kuralları oluşturma](howto-create-telemetry-rules.md)