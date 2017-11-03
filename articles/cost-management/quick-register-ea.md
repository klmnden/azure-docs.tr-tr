---
title: "Azure Kurumsal anlaşmanıza Azure maliyet yönetimi ile kaydetme | Microsoft Docs"
description: "Azure maliyeti Yönetimi Cloudyn tarafından ile kaydetmek için Kurumsal Anlaşma kullanın."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 41a9df712b07253d9f5f9db8542fb9917592320f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Bir Azure Kurumsal anlaşmasına ve görünüm veri maliyet kaydetme

Azure maliyeti Yönetimi Cloudyn tarafından ile kaydetmek için Azure Kurumsal anlaşmasına kullanın. Kaydınızı Cloudyn portalına erişim sağlar. Bu hızlı başlangıç ayrıntıları kayıt işlemini Cloudyn deneme aboneliği oluşturmak ve Cloudyn portalında oturum açmak için gerekli. Ayrıca, maliyet verileri hemen görüntüleme başlatmak nasıl gösterir.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

- http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-trial-registration"></a>Bir deneme kaydı oluşturun

1. Azure portalında tıklatın **Yönetimi maliyeti + faturalama** Hizmetler listesinde.
2. Altında **genel bakış**, tıklatın **Yönetimi maliyeti**  
    ![Yönetim sayfasında maliyet](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Üzerinde **Yönetimi maliyeti** sayfasında **Yönetimi maliyeti Git** Cloudyn kayıt sayfasına yeni bir pencerede açmak için.
4. Cloudyn portal deneme kaydı sayfasında, şirketinizin adını yazın ve ardından **Azure kuruluş kayıt Yöneticisi**.  
    ![Deneme kaydı](./media/quick-register-ea/trial-reg.png)
5. Enterprise Portal kayıt API anahtarınızı girin. Anahtarınızı kullanışlı yoksa tıklatın [Enterprise Portal](https://ea.azure.com) bağlamak ve aşağıdaki adımları uygulayın:
  1. Azure Kurumsal Web sitesi için oturum açın ve'ı tıklatın **raporları**, tıklatın **API erişim tuşu** ve birincil anahtarınızı kopyalayın.  
    ![EA API anahtarı](./media/quick-register-ea/ea-key.png)
  3. Kayıt sayfasına geri dönün ve API anahtarınızı yapıştırın.
6. Kullanım hükümlerini kabul etmeniz ardından anahtarınızı doğrulayın. Tıklatın **sonraki** Cloudyn Azure kaynak veri toplamak üzere yetkilendirmek için. Toplanan verileri kullanım, performans, faturalama ve aboneliklerinizi etiket verilerini içerir.  
    ![anahtar doğrulama](./media/quick-register-ea/ea-key-validated.png)
7. Altında **diğer Paydaşlar davet**, kullanıcıların e-posta adreslerini yazarak ekleyebilirsiniz. Tamamlandığında, tıklayın **sonraki**. Cloudyn için eklenen tüm faturalama verileriniz için yaklaşık iki saat sürer.
8. Tıklatın **Cloudyn için Git** Cloudyn Portalı'nı açmak için ve daha sonra **bulut hesap yönetimi** sayfasında, kayıtlı EA hesap bilgilerinizi görmeniz gerekir.

Kurumsal Anlaşma kaydetme hakkında öğretici bir video izlemek için bkz: [bilgisayarınızı EA kayıt kimliği bulmak ve API anahtarını Azure maliyeti Yönetimi Cloudyn tarafından kullanım için nasıl](https://youtu.be/u_phLs_udig).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç Azure Kurumsal anlaşmasına bilgilerinizi maliyet yönetimi ile kaydetmek için kullanılır. Ayrıca Cloudyn Portalı'na imzalanmış ve maliyet verileri görüntüleme başlatıldı. Azure maliyeti Yönetimi Cloudyn tarafından hakkında daha fazla bilgi için maliyet yönetimi için öğreticisi için devam edin.

> [!div class="nextstepaction"]
> [Gözden geçirme kullanım ve maliyetler](./tutorial-review-usage.md)
