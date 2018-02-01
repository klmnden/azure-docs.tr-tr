---
title: "Azure Maliyet Yönetimi ile Azure Kurumsal Anlaşmanızı kaydetme | Microsoft Docs"
description: "Cloudyn Azure Maliyet Yönetimi ile kaydetmek için Kurumsal Anlaşmanızı kullanın."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 0f157b465a9da266481be8d208fc18307cd3bb16
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Azure Kurumsal Anlaşma kaydetme ve maliyet verilerini görüntüleme

Cloudyn Azure Maliyet Yönetimi ile kaydetmek için Azure Kurumsal Anlaşmanızı kullanın. Kaydınız Cloudyn portalına erişim sağlar. Bu hızlı başlangıçta bir Cloudyn deneme aboneliği oluşturmak ve Cloudyn portalında oturum açmak için gereken kayıt işlemleri açıklanmaktadır. Ayrıca nasıl maliyet verilerini hemen görüntülemeye başlayabileceğinizi gösterir.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

- http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-trial-registration"></a>Deneme kaydı oluşturma

1. Azure portalında, hizmetler listesinde **Maliyet Yönetimi + Faturalama**’ya tıklayın.
2. **Genel Bakış** altında, **Maliyet Yönetimi**’ne tıklayın  
    ![Maliyet Yönetimi sayfası](./media/quick-register-ea/cost-mgt-billing-service.png)
3. **Maliyet Yönetimi** sayfasında, Cloudyn kayıt sayfasını yeni bir pencerede açmak için **Maliyet Yönetimi’ne gidin**.
4. Cloudyn portal deneme kayıt sayfasında, şirketinizin adını yazıp **Azure Kuruluş Kayıt Yöneticisi**’ni seçin.  
    ![deneme kaydı](./media/quick-register-ea/trial-reg.png)
5. Kurumsal Portal kayıt API anahtarınızı girin. Anahtarınız elinizin altında değilse, [Kurumsal Portal](https://ea.azure.com) bağlantısına tıklayıp aşağıdaki adımları uygulayın:
  1. Azure Kurumsal web sitesinde oturum açıp **Raporlar**’a ve ardından **API Erişim Anahtarı**’na tıklayın ve birincil anahtarınızı kopyalayın.  
    ![EA API anahtarı](./media/quick-register-ea/ea-key.png)
  3. Kayıt sayfasına dönüp API anahtarınızı yapıştırın.
6. Kullanım Koşulları’nı kabul edip anahtarınızı doğrulayın. Cloudyn’i Azure kaynak verilerini toplamak için yetkilendirmek üzere **İleri**’ye tıklayın. Toplanan veriler aboneliklerinizden kullanım, performans, faturalama ve etiket verilerini içerir.  
    ![anahtar doğrulama](./media/quick-register-ea/ea-key-validated.png)
7. **Diğer paydaşları davet et** altında, e-posta adreslerini yazarak kullanıcıları ekleyebilirsiniz. İşlem tamamlandığında **İleri**’ye tıklayın. Tüm faturalama verilerinizin Cloudyn’e aktarılması yaklaşık iki saat sürer.
8. Cloudyn portalını açmak için **Cloudyn’e git**’e tıklayın, **Bulut Hesap Yönetimi** sayfasında, kayıtlı EA hesap bilgilerinizi görmeniz gerekir.

Kurumsal Anlaşmanızı kaydetme hakkında öğretici bir video izlemek için, bkz. [Cloudyn Azure Maliyet Yönetimi’nde Kullanmak için EA Kayıt Kimliğinizi ve API Anahtarınızı Bulma](https://youtu.be/u_phLs_udig).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Maliyet Yönetimi ile kaydolmak için Azure Kurumsal Anlaşmanızı kullandınız. Ayrıca Cloudyn portalında oturum açarak maliyet verilerini görüntülemeye başladınız. Cloudyn tarafından Azure Maliyet Yönetimi hakkında daha fazla bilgi almak için, Maliyet Yönetimi öğreticisi ile devam edin.

> [!div class="nextstepaction"]
> [Kullanımı ve maliyetleri gözden geçirme](./tutorial-review-usage.md)
