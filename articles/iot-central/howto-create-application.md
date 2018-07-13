---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Bir yönetici olarak bir Azure IOT Central uygulaması oluşturmayı öğrenin.
services: iot-central
ms.service: iot-central
author: tbhagwat3
ms.author: tanmayb
ms.date: 07/09/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 1fbe3ea142e1dd738cd341f57d2b8f48b539ac75
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003775"
---
# <a name="create-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızı oluşturma

Microsoft Azure IOT Central uygulamanızı oluşturduğunuz [uygulama oluşturma](https://apps.microsoftiotcentral.com/create) sayfası. Azure IOT Central bir uygulama oluşturmak için bu sayfadaki tüm alanları doldurun ve ardından **Oluştur**. Aşağıdaki alanların her biri hakkında daha fazla bilgi bulabilirsiniz.

![Uygulama sayfası oluşturma](media\howto-create-application\image1.png)

## <a name="payment-plan"></a>Ödeme planı

Bir deneme veya Ücretli bir uygulama oluşturabilirsiniz. Deneme ve Ücretli uygulamalar hakkında daha fazla bilgi [Azure IOT fiyatlandırma sayfası Merkezi](https://azure.microsoft.com/pricing/details/iot-central/)...

## <a name="application-name"></a>Uygulama Adı

Uygulamanızın ad **Uygulama Yöneticisi** sayfası ve her bir Azure IOT Central uygulaması içinde. Azure IOT Central uygulamanız için herhangi bir ad seçebilirsiniz. Size ve diğer kullanıcılarla kuruluşunuzda anlamlı bir ad seçin.

## <a name="application-url"></a>Uygulama URL'si

Uygulama URL'si uygulamanıza bağlantıdır. Tarayıcınızda yer işareti kaydetmesi veya başkalarıyla paylaşabilirsiniz.

Uygulamanız için bir ad girin, uygulama URL'nizi otomatik olarak üretilir. Tercih ederseniz, uygulamanız için farklı bir URL seçebilirsiniz. Her Azure IOT Central URL, Azure IOT Central içinde benzersiz olmalıdır. Seçtiğiniz URL önceden alınmış bir hata iletisi görürsünüz.

## <a name="directory"></a>Dizin

Yalnızca ücretli uygulamalarında.

Azure IOT Central uygulaması oluşturmak için bir Azure Active Directory kiracısı seçin. Azure Active Directory kiracısı, kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Birden çok Azure aboneliği, tek bir Azure Active Directory kiracısı ile ilişkili olabilir.

Bir Azure Active Directory kiracınız yoksa, bir Azure aboneliği oluşturduğunuzda bir sizin için oluşturulur.

Daha fazla bilgi için bkz. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Azure aboneliği

Bir Azure aboneliği, Azure Hizmetleri örneklerini oluşturmanıza olanak sağlar. Azure IOT Central otomatik olarak erişim iznine sahip olduğunuz tüm Azure abonelikleri bulur ve bunları açılır menüde görüntüler **uygulama oluşturma** sayfası. Yeni bir Azure IOT Central uygulaması oluşturmak için bir Azure aboneliği seçin.

Azure aboneliğiniz yoksa, bir temel oluşturabilirsiniz [Azure kayıt sayfasına](https://aka.ms/createazuresubscription). Azure abonelik oluşturduktan sonra geri gidin **uygulama oluşturma** sayfası. Yeni aboneliğinizi görünür **Azure abonelik** açılır.

Daha fazla bilgi için bkz. [Azure abonelikleri](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Bölge

Yalnızca ücretli uygulamalarında.

Azure IOT Central uygulaması oluşturmak için istediğiniz bölgeyi seçin. Genellikle, en iyi performansı elde etmek için fiziksel olarak cihazlarınıza en yakın bölgeyi seçmeniz gerekir.

Daha fazla bilgi için bkz. [Azure bölgeleri](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#azure-regions).

Azure IOT Central olduğu kullanılabilir bölgeler gördüğünüz [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/) sayfası.

> [!Note]
> Bir bölge seçtikten sonra daha sonra uygulamanızı farklı bir bölgeye taşıyamazsınız.

## <a name="application-template"></a>Uygulama şablonu

Yeni Azure IOT Central uygulamanız için mevcut uygulama şablonlarından birini seçebilirsiniz. Bir uygulama şablonunu gibi cihaz şablonları önceden tanımlanmış öğeleri içerebilir ve yardımcı olması için panoları kullanmaya başlayın.

| Uygulama şablonu | Açıklama |
| -------------------- | ----------- |
| Özel uygulama   | Kendi cihaz şablonlarını ve cihazlarla doldurmak, boş bir uygulama oluşturur. |
| Örnek Contoso       | Basit bağlı bir cihaz için cihaz şablon içeren bir uygulama oluşturur. Azure IOT Central'ı keşfetmeye başlamak için bu şablonu kullanın. |
| Örnek Devkits       | Uygulama cihaz şablonları ile MXChip veya Raspberry Pi bir aygıt bağlanmaya hazır oluşturur. Cihaz geliştiriciyseniz bu cihaz üzerinde kodla denemeler bu şablonu kullanın. |

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure IOT Central uygulaması oluşturmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)