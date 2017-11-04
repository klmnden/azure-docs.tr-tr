---
title: "Bildirimleri yapılandırmak ve e-posta şablonları Azure API Management | Microsoft Docs"
description: "Bildirimleri yapılandırma ve Azure API Management şablonlarında e-posta öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 2029405e4fa05c061cdf7b38fcaa05dd38f9c804
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma
API Management belirli olaylar için bildirimleri yapılandırmak ve yöneticiler ve geliştiriciler API Management örneği ile iletişim kurmak için kullanılan e-posta şablonlarını yapılandırma olanağı sağlar. Bu konuda kullanılabilir olayları için bildirimleri yapılandırmak nasıl gösterir ve bu olaylar için kullanılan e-posta şablonlarını yapılandırma genel bir bakış sağlar.

## <a name="publisher-notifications"></a>Yayımcı bildirimleri yapılandırma
Bildirimleri yapılandırmak için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda. Bu sizi API Management yayımcı portalına götürür.

![Yayımcı portalı][api-management-management-console]

> [!NOTE] 
> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.

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
* Kullanıcı Davet Et
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

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
