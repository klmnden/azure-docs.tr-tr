---
title: "Oluştur, bağlantı, silmek veya bir tümleştirme hesap Azure logic apps içinde taşımak | Microsoft Docs"
description: "Bir tümleştirme hesap oluşturma ve mantıksal uygulamalarınızı Bağla"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: b238ef8cf9d1328913334a92c042584334d81e3c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="what-is-an-integration-account"></a>Bir tümleştirme hesabı nedir?

Bir tümleştirme hesap tümleştirme uygulamalarını, erişim ve iş ortakları, anlaşmalar, haritalar, şemalar, sertifikalar ve benzeri ticari B2B yapıları, örneğin, yönetmek için özellikle mantıksal uygulamalar, kuruluşunuz için bir yol sağlar. Bu erişimi sağlamak için tümleştirme hesabınızı mantıksal uygulamanızı tümleştirme hesabı ve mantığı uygulamaya sahip olduğundan emin olduktan sonra bağlantı *aynı Azure konumuna*.

## <a name="create-an-integration-account"></a>Tümleştirme hesabı oluşturma

1. [Azure portalı](http://portal.azure.com "Azure portalı") oturumunu açın. 

2. Ana Azure menüsünden seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabı oluşturma](./media/logic-apps-enterprise-integration-accounts/account-1.png)

3. Sayfanın en üstünde seçin **Ekle**.

   ![Seçin ekleme](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Tümleştirme hesabınızın adını ve kullanmak istediğiniz Azure aboneliğini seçin. Oluşturabilir ya da yeni bir **kaynak grubu** veya varolan bir kaynak grubu seçin. Seçin bir **konumu** tümleştirme hesabınızı barındırmak için ve bir **fiyatlandırma katmanı**. Hazır olduğunuzda, seçin **oluşturma**.

   ![Tümleştirme hesabının ayrıntılarını verin](./media/logic-apps-enterprise-integration-accounts/account-4.png)

   Azure tümleştirme hesabınızda bir dakika içinde tamamlanmalıdır seçilen konum sağlar.

5. Sayfayı yenileyin. Yeni tümleştirme hesabınızı bakın.

   ![Yeni tümleştirme hesabınız görüntülenir](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Ardından, mantıksal uygulamanızı oluşturduğunuz tümleştirme hesabı bağlayın. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Bir mantıksal uygulama için bir tümleştirme hesap bağlantı

İş ortakları, anlaşmalar, eşlemeleri ve şemaları tümleştirme hesabınızdaki ticaret gibi B2B yapılarına logic apps erişim vermek için mantıksal uygulamanızı tümleştirme hesabı bağlayın. 

1. Azure portalında mantıksal uygulamanızı seçin ve mantığı uygulamanızın konumunu denetleyin.

   ![Mantıksal uygulamanızı seçin, konumunu denetleyin](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. Altında **ayarları**seçin **tümleştirme hesabını**.

   !["Tümleştirme hesabı" seçin](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Gelen **tümleştirme hesabı seçme** listesinde, mantıksal uygulamanızı bağlamak istediğiniz tümleştirme hesabını seçin. Bağlama işlemini tamamlamak için seçin **kaydetmek**.

   ![Tümleştirme hesabınızı seçin](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

   Tümleştirmenize hesabına mantıksal uygulamanızı bağlanır ve tümleştirme hesabınızdaki tüm yapıları mantıksal uygulamanızı kullanılabilir olduğunu gösteren bir bildirim alır.

   ![Mantıksal uygulamanızı tümleştirme hesabınıza bağlı](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Mantıksal uygulamanızı tümleştirme hesabınıza bağlı, B2B bağlayıcılar mantıksal uygulamanızı kullanabilirsiniz. XML doğrulaması ve düz dosya kodlama/kod çözme bazı ortak B2B bağlayıcıları içerir.  

## <a name="delete-your-integration-account"></a>Tümleştirme hesabınızı silme

1. Ana Azure menüsünden seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabınızı Bul](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Silmek istediğiniz tümleştirme hesabı seçin.

    ![Silmek için tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-accounts/account-5.png)

3. Menüsünde, **silmek**.

    !["Sil"'i seçin](./media/logic-apps-enterprise-integration-accounts/delete.png)

4. Tümleştirme hesabını silmek için seçiminizi onaylayın.

## <a name="move-your-integration-account"></a>Tümleştirme hesabınızı taşıma

Bir tümleştirme hesabı başka bir Azure abonelik veya kaynak grubuna taşımak için şu adımları izleyin. Tümleştirme hesabını taşıdıktan sonra yeni kaynak kimlikleri kullanmak için tüm betikler güncelleştirdiğinizden emin olun.

1. Ana Azure menüsünden seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabınızı Bul](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Taşımak istediğiniz tümleştirme hesabı seçin. Altında **ayarları**, seçin **özellikleri**.

   ![Taşımak için tümleştirme hesabı seçin. Ayarlarda, Özellikler'i seçin.](./media/logic-apps-enterprise-integration-accounts/move.png)

3. Kaynak grubu veya tümleştirme hesabınızla ilişkili Azure abonelik değiştirin.

   ![Kaynak grubu Değiştir veya değişiklik abonelik seçin](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Anlaşmaları hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmaları hakkında bilgi edinin")  

