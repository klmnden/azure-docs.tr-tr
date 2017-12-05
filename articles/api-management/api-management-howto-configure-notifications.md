---
title: "Bildirimleri yapılandırmak ve e-posta şablonları Azure API Management | Microsoft Docs"
description: "Bildirimleri yapılandırma ve Azure API Management şablonlarında e-posta öğrenin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: ec560bbab3caf4cde090ed3c9a47ccc0afcb2492
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma
API Management belirli olaylar için bildirimleri yapılandırmak ve yöneticiler ve geliştiriciler API Management örneği ile iletişim kurmak için kullanılan e-posta şablonlarını yapılandırma olanağı sağlar. Bu konuda kullanılabilir olayları için bildirimleri yapılandırmak nasıl gösterir ve bu olaylar için kullanılan e-posta şablonlarını yapılandırma genel bir bakış sağlar.

## <a name="publisher-notifications"></a>Yayımcı bildirimleri yapılandırma
Bildirimleri yapılandırmak için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda. Bu sizi API Management yayımcı portalına götürür.

![Yayımcı portalı][api-management-management-console]

> [!NOTE] 
> Henüz bir API Management hizmeti örneği oluşturmadıysanız, bkz: [bir API Management hizmet örneği oluşturma][Create an API Management service instance].

Tıklatın **bildirimleri** gelen **API Management** kullanılabilir bildirim görüntülemek için sola menüsünde.

![Yayımcı bildirimleri][api-management-publisher-notifications]

Aşağıdaki listede olayların bildirimleri için yapılandırılabilir.

* **(Onay gerektiren) abonelik istekler** -belirtilen e-posta alıcıları ve kullanıcıların onay gerektiren API ürünleri için abonelik isteklerini hakkında e-posta bildirimlerini alacak.
* **Yeni abonelikler** -belirtilen e-posta alıcıları ve kullanıcıların yeni API ürün abonelikleri hakkında e-posta bildirimlerini alacak.
* **Uygulama Galerisi isteklerini** -yeni uygulamalar için uygulama Galerisi gönderildiğinde belirtilen e-posta alıcıları ve kullanıcıların e-posta bildirimlerini alacak.
* **Gizli** -belirtilen e-posta alıcıları ve kullanıcıların e-posta gizli kopya geliştiricilerine gönderilen tüm e-posta alacaksınız.
* **Yeni bir sorun veya yorum** - belirtilen e-posta alıcıları ve kullanıcıların yeni bir sorun olduğunda e-posta bildirimlerini alacak veya yorum Geliştirici portalında gönderildi.
* **Hesabı Kapat iletisi** -bir hesap kapatıldığında belirtilen e-posta alıcıları ve kullanıcıların e-posta bildirimlerini alacak.
* **Yaklaşan abonelik kota sınırına** -abonelik kullanım kullanım kotanız dolmak aldığında aşağıdaki e-posta alıcıları ve kullanıcıların e-posta bildirimlerini alacak.

Her olay için e-posta adresi metin kutusunu kullanarak e-posta alıcılarını belirtebilirsiniz veya kullanıcıların bir listeden seçebilirsiniz.

Bildirim almak için e-posta adresleri belirtmek için e-posta adresi metin kutusuna girin. Birden çok e-posta adresi varsa, bunları ayrı virgülle ayırın.

![Bildirim alıcılarını][api-management-email-addresses]

Bilgi verilecek kullanıcıları belirtmek için tıklatın **alıcı ekleyin**, kullanıcıların bildirilmesini ve tıklatın yanındaki kutuyu **Tamam**.

> [!NOTE] 
> Yalnızca Yöneticiler listesinde görüntülenir.


Bildirim alıcılarını yapılandırdıktan sonra tıklatın **kaydetmek** güncelleştirilmiş bildirim alıcılarını uygulamak için.

> [!NOTE] 
> Merkezden giderseniz **yayımcı bildirimleri** yayımcı portalına Uyarılar sekmesi, kaydedilmemiş değişiklikler var. durumunda.


## <a name="email-templates"></a>E-posta şablonlarını yapılandırma
API Management yönetme ve hizmet kullanarak esnasında gönderilen e-posta iletileri için e-posta şablonları sağlar. Aşağıdaki e-posta şablonlarını sağlanır.

* Onaylanan uygulama Galerisi gönderme
* Geliştirici diğer harf
* Bildirim yaklaşan Geliştirici kota sınırı
* Kullanıcı davet et
* Yeni açıklama soruna eklendi
* Alınan yeni sorun
* Yeni Abonelik etkinleştirildi
* Yenilenen abonelik onayı
* Abonelik isteği reddettiğinde
* Abonelik isteği alındı

Bu şablonlar değiştirilebilir istenen şekilde.

API Management Örneğiniz için e-posta şablonlarını yapılandırma ve görüntülemek için tıklatın **bildirimleri** gelen **API Management** sol ve select menüsünde **e-posta şablonlarını**sekmesi.

![E-posta şablonları][api-management-email-templates]

Görüntülemek veya belirli bir şablon değiştirmek için dosyayı seçin **şablonları** aşağı açılan liste.

![E-posta şablonları listesi][api-management-email-templates-list]

Düz metin biçiminde bir konu ve gövde tanımı'nda HTML biçiminde her e-posta şablonu yok. Her öğe özelleştirilebilir istenen şekilde.

![E-posta şablonu Düzenleyicisi][api-management-email-template]

**Parametreleri** listesi parametrelerinin listesini içeren hangi olduğunda konusu ya da gövdesi eklenen olacaktır e-posta gönderildiğinde, belirtilen değeri değiştirildi. Bir parametre eklemek için imleci gitmek için parametre istediğiniz yere yerleştirin ve parametre adının solundaki oka tıklayın.

Tıklatın **Önizleme** veya **bir sınama Gönder** nasıl e-posta arayın veya bir test e-posta Gönder görmek için.

> [!NOTE] 
> Parametreleri önizleme ya da bir test gönderme asıl değerlerle değiştirilmedi.

E-posta şablonuna yapılan değişiklikleri kaydetmek için tıklatın **kaydetmek**, veya değişiklikleri tıklatın iptal etmek için **iptal**.
 

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
