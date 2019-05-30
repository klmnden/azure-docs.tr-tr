---
title: Azure uygulama yapılandırma özelliği Yönetim | Microsoft Docs
description: Azure uygulama yapılandırması açmak ve isteğe bağlı uygulama özelliklerini kapatmak için nasıl kullanılabileceğini genel bakış.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 46f39e87e4e4cf115cbc1fceeabf0dab38fade28
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393372"
---
# <a name="feature-management-overview"></a>Özellik yönetimine genel bakış

Geleneksel olarak, yeni bir uygulama özelliği sevkiyat uygulamanın tam bir dağıtım gerektirir. Bir özelliği test etmek için büyük olasılıkla uygulamanızı birden çok kez özellik görünür olduğunda denetim veya görmek kimlerin dağıtmanız gerekir.

Özellik Yönetimi özellik sürümünü kod geliştirme ayırır ve isteğe bağlı Özellik kullanılabilirliği için hızlı değişiklikler sağlayan modern yazılım geliştirme bir uygulamadır. Olarak adlandırılan tekniği kullanan *özellik bayrakları* (olarak da bilinen *özelliğini değiştirir*, *özellik anahtarları*, vb.) dinamik olarak bir özelliğin yaşam döngüsünü yönetmek için.

Özellik Yönetimi ile ilgili olarak aşağıdaki sorunlardan baş geliştiriciler yardımcı olur:

* **Kod dallara ayırma Yönetimi**: Özellik bayrakları, geliştirilmekte olan yeni uygulama işlevselliği sarmalamak için sık sık kullanılır. Bu işlevselliğin "varsayılan olarak gizlidir". Sahip olsa bile özelliği, güvenli bir şekilde sevk edebilir tamamlanmamış, ve üretimde etkin olmayan kalır. Bu yaklaşımı kullanarak adlı *koyu dağıtım*, tüm kodunuzu her geliştirme döngüsü sonuna serbest bırakabilirsiniz. Artık herhangi bir kod dal bir özellik alma işlemi için birden fazla döngüsü nedeniyle arasında birden çok döngüleri korumak gerekir.
* **Üretimde test**: Özellik bayrakları, üretimde yeni işlevlerine erken erişimi vermek için kullanabilirsiniz. Örneğin, yalnızca takım üyeleri veya iç beta test uzmanlarına bu erişimi sınırlayabilirsiniz. Bu kullanıcılar, bir test ortamında sanal ya da kısmi bir deneyim yerine tam uygunluklu üretim deneyimi alırsınız.
* **Yayını**: Özellik bayrakları, artımlı olarak son kullanıcılarınıza yeni işlevleri geri almak için de kullanabilirsiniz. Önce kullanıcı kitlesiyle daha küçük bir yüzdesine hedefin ve söz konusu yüzdesi uygulamada daha fazla güvenle öğrendikten sonra zaman içinde aşamalı olarak artırın.
* **Anlık KILL anahtar**: Özellik bayrakları, yeni işlevsellik serbest için doğal bir güvenlik ağı sağlar. Bunları ile kolayca uygulama özellikleri açıp kapatabilirsiniz. Gerekirse, hızlı bir şekilde bir özellik yeniden oluşturma ve uygulamanızı yeniden dağıtmanıza gerek kalmadan devre dışı bırakabilirsiniz.
* **Seçmeli etkinleştirme**: Çoğu özellik bayraklarını onların ilgili işlevleri yalnızca başarıyla çıkarılan kadar mevcut olsa da, bazı uzun sürebilir. Özellik bayrakları, kullanıcıların segmentlere ayırın ve her gruba belirli bir özellik kümesi sunmak için kullanabilirsiniz. Örneğin, yalnızca belirli web tarayıcısı üzerinde çalışan bir özelliğe sahip. Böylece, tarayıcının yalnızca kullanıcıların görebileceği ve kullanılmakta olan bir özellik bayrağını tanımlayabilirsiniz. Bu yaklaşımda, kolayca desteklenen tarayıcı listesini daha sonra herhangi bir kod değişikliği yapmadan genişletebilirsiniz.

## <a name="basic-concepts"></a>Temel kavramlar

Özellik yönetimiyle ilgili birkaç yeni şartları şunlardır:

* **Özellik bayrağı**: Bir özellik bayrağını ikili durumuna sahip bir değişkendir *üzerinde* veya *kapalı*. Özellik bayrağı, bir ilişkili kod bloğu de vardır. Kod bloğunun olmasından veya özellik bayrağını durumunu tetikler.
* **Özellik Yöneticisi**: Bir uygulamadaki tüm özellik bayraklarını yaşam döngüsü işleyen bir uygulama paketi özelliği yöneticisidir. Özellik Yöneticisi genellikle özellik bayraklarını önbelleğe alma ve durumlarını güncelleştirme gibi ek işlevsellik sağlar.
* **Filtre**: Bir özellik bayrağını durumunu değerlendirmek için bir kural bir filtredir. Bir kullanıcı grubu, bir cihaz veya tarayıcı türü, coğrafi bir konum ve bir zaman penceresinde bir filtre temsil edebilen tüm örnekleridir.

Özellik Yönetimi etkin bir uygulama birlikte çalışan en az iki bileşenden oluşur:

* Yapan bir uygulamada özellik bayrakları kullanın.
* Özellik bayraklarını ve bunların geçerli durumlarını depolar ayrı bir depo.

Bu bileşenlerin nasıl etkileşim aşağıdaki örneklerde gösterilmiştir.

## <a name="feature-flag-usage-in-code"></a>Kod içinde özellik bayrağı kullanımı

Özellik bayrakları bir uygulamada uygulamak için temel düzeni, basit bir işlemdir. İle kullanılan bir Boolean durumu değişkeni olarak bir özellik bayrağını düşünebilirsiniz bir `if` kodunuzu koşullu deyimde:

```csharp
if (featureFlag) {
    // Run the following code
}
```

Bu durumda, `featureFlag` ayarlanır `True`çevrelenmiş kod bloğu yürütülen; Aksi takdirde, halde atlanır. Değerini ayarlayabilirsiniz `featureFlag` statik olarak, aşağıdaki kod örneğinde olduğu gibi:

```csharp
bool featureFlag = true;
```

Ayrıca, belirli kurallara göre bayrağın durumunu değerlendirebilirsiniz:

```csharp
bool featureFlag = isBetaUser();
```

Biraz daha karmaşık bir özellik bayrağı düzeni içeren bir `else` deyimi de:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

Ancak bu davranış temel deseni yazabilirsiniz. [Özellik bayrakları ASP.NET Core uygulaması kullanmak](./use-feature-flags-dotnet-core.md) bir basit kod desen Standartlaştırma avantajları gösterilir. Örneğin:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Özellik bayrağı depo

Özellik bayraklarını etkili bir şekilde kullanmak için bir uygulamada kullanılan tüm özellik bayraklarını federasyonda gerekir. Bu yaklaşım özellik bayrağı durumlarını değiştirme ve uygulamayı yeniden dağıtmaya gerek olmadan değiştirmenize izin verir.

Azure uygulama yapılandırması, özellik bayrakları için merkezi bir havuz olacak şekilde tasarlanmıştır. Farklı türde bir özellik bayraklarını tanımlama ve hızla ve güvenle durumlarını işlemek için kullanabilirsiniz. Ardından, bu özellik bayraklarını uygulamanızdan kolayca erişmek için çeşitli programlama dili çerçeveler için uygulama yapılandırma kitaplıklarını da kullanabilirsiniz.

[Özellik bayrakları ASP.NET Core uygulaması kullanmak](./use-feature-flags-dotnet-core.md) özellik yönetim kitaplıkları ve .NET Core uygulaması yapılandırma sağlayıcısı birlikte ASP.NET web uygulamanız için özellik bayraklarını uygulamak için nasıl kullanılacağını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özellik bayrakları için ASP.NET Core web uygulaması Ekle](./quickstart-feature-flag-aspnet-core.md)  
