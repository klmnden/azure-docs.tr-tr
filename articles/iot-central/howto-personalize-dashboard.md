---
title: Azure IOT Central kişisel panolar oluşturma | Microsoft Docs
description: Bir kullanıcı olarak oluşturma ve kişisel panolarınızı yönetme hakkında bilgi edinin.
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: c048ae8c0daba0e467a9243f4dd83f8d95921e10
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502640"
---
# <a name="create-and-manage-personal-dashboards"></a>Oluşturma ve kişisel panoları Yönet

**Pano** uygulamanızı ilk kez gittiğinizde, sayfasıdır. A **Oluşturucu** , uygulamanızdaki tüm kullanıcılar için varsayılan uygulama Panosu tanımlar. Bu varsayılan panoyu, kendi kişiselleştirilmiş uygulama panosu ile değiştirebilirsiniz. Farklı verileri görüntülemek ve bunlar arasında geçiş birçok paneliniz olabilir.

## <a name="create-dashboard"></a>Pano oluşturma

Aşağıdaki ekran görüntüsünde oluşturulan uygulamada gösteren panoyu **örnek Contoso** şablonu. Varsayılan uygulama Panosu kişisel bir pano ile değiştirebilirsiniz. Bunu yapmak için **+ yeni** üst sayfanın sağ.

!["Örnek Contoso" şablonu temel alan uygulamalar için Pano](media/howto-personalize-dashboard/defaultdashboard.png)

Seçme **+ yeni**, Pano Düzenleyicisi açılır. Düzenleyicisinde, panonuzu bir ad verebilir ve kitaplıktan öğe seçti. Kitaplık kutucuk ve Pano özelleştirmek için kullanabileceğiniz Pano temelleri içerir.

![Pano kitaplığı](media/howto-personalize-dashboard/dashboardeditor.png)

Örneğin, ekleyebileceğiniz bir **cihaz ayarlarını ve özelliklerini** biri, yönettiğiniz cihazları için ayarları ve özellikleri değerlerini gösterecek şekilde kutucuk. Bunu yapmak için önce seçin bir **cihaz şablonu** seçip bir **cihaz örneği**. Ardından kutucuğu bir başlık ve select vermek bir **ayarı** veya **özelliği** görüntülenecek. Aşağıdaki ekran görüntüsü gösterildiği **Fan hızı** ayarı için bir kutucuk eklemek için seçili. Seçin **Bitti** panonuza yapılan değişiklik kaydedilemiyor.

!["Cihaz ayrıntıları Yapılandır" form ayarlarına ve özelliklerine ilişkin ayrıntılı](media/howto-personalize-dashboard/dashboardsetting.png)

Kişisel panonuzu görüntüleyin, ile yeni kutucuğu görmek şimdi **Fan hızı** cihazı için:

![Görüntülenen ayarları ve özellikleri için kutucuğun bulunduğu "Pano" sekmesi](media/howto-personalize-dashboard/personaldashboard.png)

Nasıl daha fazla kişisel panolarınızı özelleştirmek keşfetmek için diğer kutucuk türleri Kitaplığı'nda keşfedebilirsiniz.

Azure IOT Central kutucukları kullanma hakkında daha fazla bilgi için bkz. [Pano kutucukları kullanacak](howto-use-tiles.md).

## <a name="manage-dashboards"></a>Panoları Yönet

Kişisel birçok paneliniz olabilir ve bunlar arasında geçiş yapmak veya varsayılan uygulama Panosu seçin:

![Anahtar Panosu](media/howto-personalize-dashboard/switchdashboards.png)

Kişisel panolarınızı düzenleyebilir ve artık ihtiyacınız olanlar Sil:

![Panoyu Sil](media/howto-personalize-dashboard/managedashboards.png)

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma ve yönetme kişisel panolar öğrendiniz, şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Uygulama tercihlerinizi yönetmeyi öğrenin](howto-manage-preferences.md)
