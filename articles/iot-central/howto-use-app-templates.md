---
title: Azure IOT Central uygulama şablonlarını kullanma | Microsoft Docs
description: Operatör cihazı kullanmak nasıl Azure IOT Central uygulamanızda ayarlar.
author: dominicbetts
ms.author: dobett
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a26f70c5a61f3855a3de991072a7e84103e87b69
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499227"
---
# <a name="use-application-templates"></a>Uygulama şablonlarını kullanma

Bu makalede nasıl, oluşturma ve uygulama şablonlarını kullanma çözüm Yöneticisi olarak.

Azure IOT Central bir uygulama oluşturduğunuzda, yerleşik bir örnek şablonlar vardır. Bu gibi durumlarda, kendi uygulama şablonları da var olan IOT Central uygulamalar oluşturabilirsiniz. Yeni uygulamalar oluştururken, kendi uygulama şablonları kullanabilirsiniz.

Bir uygulama şablonu oluşturduğunuzda, mevcut uygulamanızı aşağıdaki öğeleri içerir:

- Pano düzeni ve tanımladığınız tüm kutucuklar dahil olmak üzere varsayılan uygulama Panosu.
- Ölçümler, ayarları, özellikleri, komutları ve Pano dahil olmak üzere cihaz şablonları.
- Kuralları. Tüm kural tanımlarını dahil edilir. Bununla birlikte, e-posta eylemleri dışındaki Eylemler dahil edilmez.
- Cihaz, kendi koşulları ve panolar gibi ayarlar.

> [!WARNING]
> Bir Pano belirli cihazlarla ilgili bilgileri görüntüleyen kutucuklar içerir. ardından bu kutucukları Göster **istenen kaynak bulunamadı** yeni uygulama. Yeni uygulamanızda cihazlar hakkındaki bilgileri görüntülemek için bu kutucukları yeniden yapılandırmanız gerekir.

Bir uygulama şablonu oluşturduğunuzda, aşağıdaki öğeleri dahil değildir:

- Cihazlar
- Kullanıcılar
- İş tanımları
- Verileri sürekli dışarı aktarma tanımları

Bu öğeleri el ile bir uygulama şablondan oluşturulan tüm uygulamalar ekleyin.

## <a name="create-an-application-template"></a>Bir uygulama şablonunu oluşturma

Varolan bir IOT Central uygulamasından bir uygulama şablonunu oluşturmak için:

1. Git **Yönetim** uygulamanızı bölümünde.
1. Seçin **uygulama şablonu dışarı aktarma**.
1. Üzerinde **uygulama şablonu dışarı aktarma** sayfasında, bir ad ve şablonunuz için bir açıklama girin.
1. Seçin **dışarı** uygulama şablonu oluşturmak için. Artık kopyalayabilirsiniz **paylaşılabilir bağlantı** , sağlayan biri şablondan yeni bir uygulama oluşturmak:

![Bir uygulama şablonunu oluşturma](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>Bir uygulama şablonunu kullanma

Yeni bir IOT Central uygulaması oluşturmak için bir uygulama şablonunu kullanmak için önceden oluşturulmuş ihtiyacınız **paylaşılabilir bağlantı**. Yapıştırma **paylaşılabilir bağlantı** tarayıcınızın adres çubuğuna. **Uygulama oluşturma** sayfasında seçilen özel uygulama şablonunuzla birlikte görüntülenir:

![Bir şablondan uygulama oluşturma](media/howto-use-app-templates/create-app.png)

Ödeme planınız seçin ve formdaki diğer alanları doldurun. Ardından **Oluştur** uygulama şablondan yeni bir IOT Central uygulaması oluşturmak için.

## <a name="manage-application-templates"></a>Uygulama şablonlarını yönetme

Üzerinde **uygulama şablonu dışarı aktarma** sayfasında, silebilir veya uygulama şablonu güncelleştirin.

Bir uygulama şablonunu silerseniz, daha önce oluşturulmuş bir paylaşılabilir bağlantı artık yeni uygulamalar oluşturmak için kullanabilirsiniz.

Uygulama şablonunuzu güncelleştirmek için şablon adı veya açıklamayı değiştirmek **uygulama şablonu dışarı aktarma** sayfası. Ardından **dışarı** düğmesini tekrar. Bu işlem yeni bir dizi oluşturur **paylaşılabilir bağlantı** ve tüm önceki geçersiz kılar **paylaşılabilir bağlantı** URL'si.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Şablonları kullanılacağını öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [yönetme IOT Central Azure portalından](howto-manage-iot-central-from-portal.md)
