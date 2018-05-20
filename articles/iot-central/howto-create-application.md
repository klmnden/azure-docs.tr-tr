---
title: Azure IOT Merkezi uygulama oluşturma | Microsoft Docs
description: Bir yönetici olarak Azure IOT merkezi bir uygulama oluşturma.
services: iot-central
author: TanmayBhagwat
ms.author: tanmayb
ms.date: 03/20/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 83879c6b81985f61b9fcff665e9f764c1346592e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="create-your-azure-iot-central-application"></a>Azure IOT Orta uygulamanızı oluşturma

Microsoft Azure IOT merkezi uygulamanızdan oluşturduğunuz [uygulama oluştur](https://apps.microsoftiotcentral.com/create) sayfası. Azure IOT merkezi bir uygulama oluşturmak için bu sayfadaki tüm alanları doldurun ve ardından **oluşturma**. Bu makalede alanların her biri hakkında daha fazla bilgi vardır.

![Uygulama sayfası oluşturma](media\howto-create-application\image1.png)

## <a name="payment-plan"></a>Ödeme planı

Bir deneme ya da Ücretli bir uygulama oluşturabilirsiniz. Bu sayfada deneme aboneliği ve Ücretli uygulamalar hakkında daha fazla bilgi edinin.

## <a name="application-name"></a>Uygulama Adı

Uygulamanızın ad **Uygulama Yöneticisi** sayfa ve her Azure IOT merkezi bir uygulama içinde. Azure IOT merkezi uygulamanız için herhangi bir ad seçebilirsiniz. Size ve diğerleri için kuruluşunuzda anlamlı bir ad seçin.

## <a name="application-url"></a>Uygulama URL'si

Uygulama URL'si uygulamanıza bağlantıdır. Tarayıcınızda bir yer işareti kaydetmesi veya başkalarıyla paylaşabilirsiniz.

Uygulamanız için bir ad girin, uygulama URL'sini otomatik olarak üretilir. Tercih ederseniz, uygulamanız için farklı bir URL seçebilirsiniz. Her Azure IOT merkezi URL'si benzersiz olmalıdır. Seçtiğiniz URL zaten kullanımda değilse, hata iletisi görürsünüz.

## <a name="directory"></a>Dizin

Yalnızca ücretli uygulamalarında.

Azure IOT merkezi bir uygulama oluşturmak için bir Azure Active Directory Kiracı seçin. Bir Azure Active Directory Kiracı Kullanıcı kimlikleri, kimlik bilgileri ve diğer kuruluş bilgilerini içerir. Birden çok Azure aboneliği tek bir Azure Active Directory Kiracı ile ilişkilendirilebilir.

Bir Azure Active Directory Kiracı yoksa, bir Azure aboneliği oluşturduğunuzda, sizin için oluşturulur.

Daha fazla bilgi için bkz: [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Azure aboneliği

Bir Azure aboneliği, Azure Hizmetleri örneklerini oluşturmanıza olanak sağlar. Azure IOT Orta otomatik olarak erişiminiz tüm Azure abonelikleri bulur ve bunları açılır listede görüntüler **uygulama oluştur** sayfası. Yeni bir Azure IOT merkezi uygulaması oluşturmak için Azure aboneliği seçin.

Bir Azure aboneliğiniz yoksa, bu sayfada oluşturabilirsiniz. Azure aboneliği oluşturduktan sonra geri gittiğinizde **uygulama oluştur** sayfası. Yeni Abonelik görünür **Azure aboneliği** açılır.

Daha fazla bilgi için bkz: [Azure abonelikleri](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Bölge

Yalnızca ücretli uygulamalarında.

Burada Azure IOT merkezi uygulamanızı oluşturmak istediğiniz bölgeyi seçin. Genellikle, en iyi performansı elde etmek, cihazlara fiziksel olarak en yakın bölgeyi seçmeniz gerekir.

Daha fazla bilgi için bkz: [Azure bölgeleri](https://docs.microsoft.com/en-us/azure/guides/developer/azure-developer-guide#azure-regions).

Azure IOT merkezi olduğu bulunan bölgelere görebilirsiniz [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/) sayfası.

> [!Note]
> Bir bölge seçtikten sonra daha sonra farklı bir bölgeye uygulamanıza taşıyamazsınız.

## <a name="application-template"></a>Uygulama şablonu

Yeni Azure IOT merkezi uygulamanız için kullanılabilir uygulama şablonlarından birini seçebilirsiniz. Bir uygulama şablonunu aygıt şablonlar gibi önceden tanımlanmış öğeleri içerebilir ve yardımcı olması için panolar başlayın:

| Uygulama şablonu | Açıklama |
| -------------------- | ----------- |
| Özel uygulama   | Kendi cihaz şablonları ve aygıtlarını doldurmak boş bir uygulama oluşturur. |
| Örnek Contoso       | Basit bağlı bir aygıt için bir aygıt şablonu içeren bir uygulama oluşturur. Azure IOT merkezi keşfetmeye başlamak için bu şablonu kullanın. |
| Örnek Devkits       | Uygulama aygıt şablonlarıyla MXChip veya Raspberry Pi'yi bir aygıt bağlanmaya hazır oluşturur. Cihaz geliştiriciyseniz bu cihazların birinde koduyla denemeler bu şablonu kullanın. |

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi bir uygulama oluşturmak öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)