---
title: Bildirimleri yapılandırabilir ve e-posta, Azure API Management şablonlarında | Microsoft Docs
description: Bildirimleri yapılandırabilir ve e-posta şablonlarını Azure API Management hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: apimpm
ms.openlocfilehash: 2a959c9d131c6aa0bdc99450cf2b6f09a5d8bfa7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60528456"
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma
API Management, belirli olaylar için bildirimleri yapılandırabilir ve yöneticiler ve geliştiriciler API Management örneği ile iletişim kurmak için kullanılan e-posta şablonlarını yapılandırma olanağı sağlar. Bu makalede, kullanılabilir olaylar için bildirimleri yapılandırmak nasıl gösterir ve bu olaylar için kullanılan e-posta şablonlarını yapılandırma genel bir bakış sağlar.

## <a name="prerequisites"></a>Önkoşullar

API Management hizmet örneği yoksa şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="publisher-notifications"> </a>Bildirimleri yapılandırma

1. Seçin, **API MANAGEMENT** örneği.
2. Tıklayın **bildirimleri** kullanılabilir bildirimleri görüntülemek için.

    ![Yayımcı bildirimleri][api-management-publisher-notifications]

    Aşağıdaki listede yer alan olay bildirimleri için yapılandırılabilir.

   * **Abonelik isteklerinin (onay alma zorunlu kılındığında)** -kullanıcılar ve belirtilen e-posta alıcılarını onay alma zorunlu kılındığında API ürünleri için abonelik isteklerini hakkında e-posta bildirimleri alırsınız.
   * **Yeni abonelikler** -belirtilen e-posta alıcıları ve kullanıcıların yeni API ürün abonelikleri hakkında e-posta bildirimleri alırsınız.
   * **Uygulama Galerisi istekleri** -yeni uygulamalar için uygulama Galerisi gönderildiğinde belirtilen e-posta alıcıları ve kullanıcıların e-posta bildirimleri alırsınız.
   * **Gizli** -belirtilen e-posta alıcıları ve kullanıcıların e-posta gizli kopya geliştiricileri için gönderilen tüm e-posta alırsınız.
   * **Yeni sorun veya açıklama** - belirtilen e-posta alıcıları ve kullanıcıların yeni bir sorun olduğunda e-posta bildirimleri alırsınız veya yorum Geliştirici portalında gönderildi.
   * **Hesabı Kapat ileti** -hesabınız kapatıldıktan sonra belirtilen e-posta alıcıları ve kullanıcıların e-posta bildirimleri alırsınız.
   * **Yaklaşan abonelik kota sınırını** -abonelik kullanımı, kullanım kotanız dolmak aldığında aşağıdaki e-posta alıcıları ve kullanıcıların e-posta bildirimleri alırsınız.

     Her olay için e-posta adresi metin kutusunu kullanarak e-posta alıcılarını belirtebilirsiniz veya kullanıcıların bir listeden seçebilirsiniz.

3. Gönderilecek e-posta adresleri belirtmek için e-posta adresi metin kutusuna girin. Birden çok e-posta adresi varsa, bunları birbirinden virgülle ayırın.

    ![Bildirim alıcıları][api-management-email-addresses]
4. **Ekle**’ye basın.

## <a name="email-templates"> </a>Bildirim şablonları yapılandırma
API Management, yönetme ve hizmet kullanımı sırasında gönderilen e-posta iletileri için bildirim şablonları sağlar. Aşağıdaki e-posta şablonlarını sağlanır.

* Onaylanan uygulama Galerisi gönderme
* Geliştirici diğer harf
* Geliştirici kota sınırına yaklaşılıyor bildirimi
* Kullanıcı davet et
* Bir sorun için eklenen yeni açıklama
* Alınan yeni sorun
* Yeni Abonelik etkinleştirildi
* Yenilenen abonelik onayı
* Abonelik isteği reddeder.
* Abonelik isteği alındı

Bu şablonlar değiştirilebilir istenen şekilde.

API Management örneğinizin e-posta şablonlarını yapılandırma ve görüntülemek için tıklatın **bildirim şablonları**.

![E-posta şablonları][api-management-email-templates]

Her e-posta şablonu, bir konu düz metin ve HTML biçiminde bir gövde tanımı sahiptir. Her öğe özelleştirilebilir istenen şekilde.

![E-posta şablonu Düzenleyicisi][api-management-email-template]

**Parametreleri** listesini içeren bir parametre listesi olan konusu veya gövdesi içinde eklenen olacaktır e-posta gönderildiğinde, belirtilen değeri değiştirildi. Bir parametre eklemek için Git parametresi burada istediğiniz imleci yerleştirin ve parametre adının sol tarafındaki oka tıklayın.

> [!NOTE] 
> Parametreleri önizleme ya da bir test gönderme gerçek değerlerle değiştirilmedi.

E-posta şablonu değişiklikleri kaydetmek için tıklatın **Kaydet**, değişiklikleri tıklayarak iptal etmek veya **at**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
