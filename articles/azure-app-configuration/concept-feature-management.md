---
title: Azure uygulama yapılandırma özelliği Yönetim | Microsoft Docs
description: Azure uygulama yapılandırması açmak ve isteğe bağlı uygulama özelliklerini kapatmak için nasıl kullanılabileceğini genel bakış
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
ms.openlocfilehash: 304ca08275ccf4bf32d38e3fc89dd5cf07460fa4
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413777"
---
# <a name="feature-management"></a>Özellik yönetimi

Geleneksel olarak yeni bir uygulama özelliği sevkiyat uygulamanın tam bir dağıtım gerektirir. Bir özelliği test etmek için büyük olasılıkla uygulamanızı birden çok kez görünür olduğunda denetim veya görmek kimlerin dağıtmak gerekir. Özellik Yönetimi özellik sürümünü kod geliştirme ayırır ve isteğe bağlı Özellik kullanılabilirliği için hızlı değişiklikler sağlayan modern yazılım geliştirme uygulamadır. Olarak adlandırılan tekniği kullanan *özellik bayrağı* (olarak da bilinen *özelliği Aç/Kapat*, *özellik anahtarı*, vb.) bir özelliğin yaşam döngüsü dinamik olarak yönetmek için. Bu, aşağıdaki sorunları baş geliştiriciler yardımcı olur:

* **Kod dallara ayırma Yönetimi**: Özellik bayrakları, geliştirilmekte olan yeni uygulama işlevselliği sarmalamak için sık sık kullanılır. Bu işlevselliğin "varsayılan olarak gizlidir". Güvenli bir şekilde, tamamlanmamış ancak sevk edebilir ve üretime etkinliği olmayan kalır. Bu yaklaşımı kullanarak adlı *koyu dağıtım*, tüm kodunuzu her geliştirme döngüsü sonuna serbest bırakabilirsiniz. Artık bir özellik alma işlemi için birden fazla nokta nedeniyle birden çok döngüleri arasında herhangi bir kod dal korumak gerekir.
* **Üretimde test**: Özellik bayrakları, üretimde yeni işlevlerine erken erişimi vermek için kullanabilirsiniz. Örneğin, yalnızca takım üyelerinin veya iç beta test uzmanlarına bu erişimi sınırlayabilirsiniz. Bu kullanıcılar, bir test ortamında sanal ya da kısmi bir deneyim yerine tam uygunluklu üretim deneyimi alırsınız.
* **Yayını**: Özellik bayrakları, son kullanıcılar için yeni işlevleri artımlı olarak geri almak için de kullanabilirsiniz. Önce kullanıcı kitlesiyle daha küçük bir yüzdesine hedefin ve söz konusu yüzdesi uygulamada daha fazla güvenle öğrendikten sonra zaman içinde aşamalı olarak artırın.
* **Anlık KILL anahtar**: Özellik bayrakları, yeni işlevsellik serbest için doğal bir güvenlik ağı sağlar. Kendileriyle, yalnızca açma ancak Ayrıca uygulama özelliklerini kolayca kapatın. Hızlı bir şekilde yeniden oluşturun ve uygulamanızı yeniden dağıtmaya gerek kalmadan gerekirse bir özelliği devre dışı bırakma olanağı elde edin.
* **Seçmeli etkinleştirme**: Çoğu özellik bayraklarını onların ilgili işlevleri yalnızca başarıyla çıkarılan kadar mevcut olsa da, bazı uzun sürebilir. Kullanıcıların segmentlere ayırın ve her gruba belirli bir özellik kümesi sunmak için bir yöntem olarak kullanabilirsiniz. Örneğin, belirli bir web tarayıcısında çalışan bir özelliğe sahip. Bu tarayıcı yalnızca kullanıcıların görebileceği ve kullanılmakta olan şekilde özellik bayrağı tanımlayabilirsiniz. Bu yaklaşımın avantajı, kolayca desteklenen tarayıcı listesini daha sonra değiştirmek herhangi bir kod yapmak zorunda kalmadan genişletebilirsiniz emin olmanızdır.

## <a name="basic-concepts"></a>Temel kavramlar

Özellik Yönetimi ile ilgili birkaç yeni koşulları aşağıdadır.

* **Özellik bayrağı**: Bir özellik bayrağını ikili durumuna sahip bir değişkendir *üzerinde* veya *kapalı* ve ilişkili bir kod bloğuna sahiptir. Kod bloğu veya çalışıp çalışmayacağını durumunu belirler.
* **Özellik Yöneticisi**: Bir uygulamadaki tüm özellik bayraklarını yaşam döngüsü işleyen bir uygulama paketi özelliği yöneticisidir. Genellikle, bayraklar ve durumlarını güncelleştirmek önbelleğe alma gibi ek işlevler sağlar.
* **Filtre**: Bir özellik bayrağını durumunu değerlendirmek için bir kural bir filtredir. Kullanıcı grubu, cihaz veya tarayıcı türü, coğrafi konum, zaman penceresi olan tüm örnekleri bir filtre temsil edebilir.

Özellik Yönetimi etkin bir uygulama birlikte çalışan en az iki bileşenden oluşur: özellik bayraklarının kullanılmasını ve ayrı bir depo getiren bir uygulaması, özellik bayrakları ve bunların geçerli durumlarını depolar. Kendi etkileşimleri aşağıda gösterilmiştir.

## <a name="feature-flag-usage-in-code"></a>Kod içinde özellik bayrağı kullanımı

Özellik bayrakları bir uygulamada uygulamak için temel düzeni, basit bir işlemdir. İle kullanılan bir Boolean durumu değişken kavramsallaştırmak bir `if` kodunuzu koşullu deyimde.

```csharp
if (featureFlag) {
    // Run the following code
}
```

Bu durumda, `featureFlag` ayarlanır `True`, eklenen kod bloğu yürütülür; Aksi halde atlanır. Değerini `featureFlag` statik olarak ayarlanabilir:

```csharp
bool featureFlag = true;
```

Bu, belirli kurallara göre de değerlendirilebilir:

```csharp
bool featureFlag = isBetaUser();
```

Biraz daha karmaşık bir özellik bayrağı düzeni içeren bir `else` deyimi de.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

Bu ancak temel düzeni yazılabilir. [Özellik bayrakları ASP.NET Core uygulaması kullanmak](./use-feature-flags-dotnet-core.md) bir basit kod desen Standartlaştırma avantajı gösterir.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Özellik bayrağı depo

Etkili bir şekilde kullanmak için bir uygulamada kullanılan tüm özellik bayraklarını federasyonda gerekir. Bu özellik bayrağı durumlarını değiştirme ve uygulamayı yeniden dağıtmaya gerek olmadan değiştirmenize izin verir. Azure uygulama yapılandırması, özellik bayrakları için merkezi bir havuz olacak şekilde tasarlanmıştır. Farklı türde bir özellik bayraklarını tanımlama ve hızla ve güvenle durumlarını işlemek için kullanabilir. Daha sonra bu özellik bayraklarını kolayca uygulama yapılandırması kitaplıkları için çeşitli programlama dili çerçevesini kullanarak uygulamanızın içinden erişebilirsiniz. [Özellik bayrakları ASP.NET Core uygulaması kullanmak](./use-feature-flags-dotnet-core.md) nasıl özellik yönetim kitaplıkları ve .NET Core uygulaması yapılandırma sağlayıcısı birlikte eksiksiz bir çözüm olarak ASP.NET web uygulamanız için özellik bayraklarını uygulamak için kullanıldığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özellik bayrakları için ASP.NET Core web uygulaması Ekle](./quickstart-feature-flag-aspnet-core.md)  
