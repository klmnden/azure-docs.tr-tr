---
title: Azure API Management örneği oluşturma | Microsoft Docs
description: Yeni bir Azure API Management örneği oluşturmak için bu öğreticideki adımları izleyin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cflower
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 7fb4182c0b5149a9006a30ad34782ad968e16758
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38595597"
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Yeni bir Azure API Management hizmeti örneği oluşturma

Azure API Management (APIM), kuruluşların kendi veri ve hizmet potansiyellerini ortaya çıkarmak üzere API’leri dış, iş ortağı ve iç geliştiricilere yayımlamalarına yardımcı olur. API Management; geliştirici katılımı, iş öngörüleri, analizler, güvenlik ve koruma aracılığıyla başarılı bir API programı yürütmeye ilişkin temel uzmanlıklar sağlar. APIM, herhangi bir yerde barındırılan mevcut arka uç hizmetleri için modern API ağ geçitleri oluşturmanıza ve yönetmenize olanak sağlar. Daha fazla bilgi için [Genel Bakış](api-management-key-concepts.md) konusuna bakın.

Bu hızlı başlangıç, Azure portalını kullanarak yeni bir API Management örneği oluşturma adımlarını açıklar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

![yeni örnek](./media/get-started-create-service-instance/get-started-create-service-instance-created.png)

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-new-service"></a>Yeni hizmet oluşturma

1. [Azure portalı](https://portal.azure.com/)’nda **Kaynak oluştur** > **Kurumsal Tümleştirme** > **API yönetimi**’ni seçin.

    Alternatif olarak, **Yeni**’yi seçip arama kutusuna `API management` yazabilir ve Enter tuşuna basabilirsiniz. **Oluştur**’a tıklayın.

2. **API Management hizmeti** penceresine ayarları girin.

    ![yeni örnek](./media/get-started-create-service-instance/get-started-create-service-instance-create-new.png)

    | Ayar      | Önerilen değer  | Açıklama              |
    | ------------ |  ------- | ---------------------------------|
    |**Ad**|API Management hizmetiniz için benzersiz bir ad| Ad daha sonra değiştirilemez. Hizmet adı, *{name}.azure-api.net* biçiminde varsayılan bir etki alanı adı oluşturmak için kullanılır. Özel bir etki alanı adı kullanmak istiyorsanız bkz. [Özel etki alanı yapılandırma](configure-custom-domain.md). <br/> Hizmet adı, hizmete ve ilgili Azure kaynağına başvurmak için kullanılır.|
    |**Abonelik**|Aboneliğiniz | Bu yeni hizmet örneğini barındıran abonelik oluşturulur. Erişiminizin bulunduğu farklı Azure abonelikleri arasından abonelik seçebilirsiniz.|
    |**Kaynak Grubu**|*apimResourceGroup*|Yeni veya var olan bir kaynak seçebilirsiniz. Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../azure-resource-manager/resource-group-overview.md#resource-groups) daha fazla bilgi edinin.|
    |**Konum**|*Batı ABD*|Yakınınızdaki coğrafi bölgeyi seçin. Açılır listede yalnızca kullanılabilir API Management hizmet bölgeleri görünür. |
    |**Kuruluş adı**|Kuruluşunuzun adı|Bu ad, geliştirici portalının başlığı ve bildirim e-postalarının göndereni gibi birkaç yerde kullanılır.|
    |**Yönetici e-postası**|*admin@org.com*|**API Management**’tan tüm bildirimlerin gönderileceği e-posta adresini ayarlayın.|
    |**Fiyatlandırma katmanı**|*Geliştirici*|Hizmeti değerlendirmek için **Geliştirici** katmanını ayarlayın. Bu katman, üretim kullanımı için değildir. API Management katmanlarını ölçeklendirme hakkında daha fazla bilgi için bkz. [yükseltme ve ölçeklendirme](upgrade-and-scale.md).|
3. **Oluştur**’u seçin.

    > [!TIP]
    > Bir API Management hizmetinin oluşturulması genellikle 20 ile 30 dakika arasında sürer. **Panoya Sabitle** öğesinin seçilmesi, yeni oluşturulan hizmetin bulunmasını kolaylaştırır.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, aşağıdaki adımları izleyerek kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz:


1. Azure portalında ![oku seçin](./media/get-started-create-service-instance/arrow.png).
2. **Kaynak grupları**’nı seçin.
3. Kaynak grubunuzu bulun.
4. ". . ." öğesine tıklayıp grubunuzu silin.

![temizle](./media/get-started-create-service-instance/cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk API’nizi içeri aktarma ve yayımlama](import-and-publish.md)
