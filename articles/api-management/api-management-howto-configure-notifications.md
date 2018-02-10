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
ms.date: 02/02/2018
ms.author: apimpm
ms.openlocfilehash: 228cbb103e13c478bea460bb04de43d6480bc60e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma
API Management belirli olaylar için bildirimleri yapılandırmak ve yöneticiler ve geliştiriciler API Management örneği ile iletişim kurmak için kullanılan e-posta şablonlarını yapılandırma olanağı sağlar. Bu makale kullanılabilir olayları için bildirimleri yapılandırmak nasıl gösterir ve bu olaylar için kullanılan e-posta şablonlarını yapılandırma genel bir bakış sağlar.

## <a name="prerequisites"></a>Önkoşullar

API Management hizmet örneği yoksa, aşağıdaki hızlı başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="publisher-notifications"></a>Bildirimleri yapılandırma

1. Seçin, **API MANAGEMENT** örneği.
2. Tıklatın **bildirimleri** kullanılabilir bildirim görüntülemek için.

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

3. Bildirim almak için e-posta adresleri belirtmek için e-posta adresi metin kutusuna girin. Birden çok e-posta adresi varsa, bunları ayrı virgülle ayırın.

    ![Bildirim alıcılarını][api-management-email-addresses]
4. **Ekle**’ye basın.

## <a name="email-templates"></a>Bildirim şablonlarını yapılandırma
API Management yönetme ve hizmet kullanarak esnasında gönderilen e-posta iletileri için bildirim şablonları sağlar. Aşağıdaki e-posta şablonlarını sağlanır.

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

API Management Örneğiniz için e-posta şablonlarını yapılandırma ve görüntülemek için tıklatın **bildirim şablonları**.

![E-posta şablonları][api-management-email-templates]

Düz metin biçiminde bir konu ve gövde tanımı'nda HTML biçiminde her e-posta şablonu yok. Her öğe özelleştirilebilir istenen şekilde.

![E-posta şablonu Düzenleyicisi][api-management-email-template]

**Parametreleri** listesi parametrelerinin listesini içeren hangi olduğunda konusu ya da gövdesi eklenen olacaktır e-posta gönderildiğinde, belirtilen değeri değiştirildi. Bir parametre eklemek için imleci gitmek için parametre istediğiniz yere yerleştirin ve parametre adının solundaki oka tıklayın.

> [!NOTE] 
> Parametreleri önizleme ya da bir test gönderme asıl değerlerle değiştirilmedi.

E-posta şablonuna yapılan değişiklikleri kaydetmek için tıklatın **kaydetmek**, veya değişiklikleri tıklatın iptal etmek için **atmak**.
 

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
