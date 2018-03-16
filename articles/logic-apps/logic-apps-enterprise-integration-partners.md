---
title: "İş ortakları için işletmeden işletmeye (B2B) iletileri - Azure mantıksal uygulamaları oluşturma | Microsoft Docs"
description: "İş ortaklarının Kurumsal tümleştirme paketi ve Logic Apps ile tümleştirme hesabınızı eklemeyi öğrenin"
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 17f15c49e0f8137d5f11c57fa600588cda791c28
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Ekleyin veya iş iş akışınızı sözleşmelerde ortakları güncelleştirin

İşletmeden işletmeye (B2B) işlemlere katılmasına ve birbirleri arasındaki iletileri exchange varlıklar ortaklarıdır. Ve bu işlemler başka bir kuruluştaki temsil eden iş ortakları oluşturabilmeniz için önce her ikisi gerekir tanımlar ve diğer tarafından gönderilen iletileri doğrular bilgileri paylaşabilir. Bu ayrıntıları ele almaktadır ve iş ilişkiniz başlamak hazır sonra her iki temsil etmek için tümleştirme hesabınızı ortakları oluşturabilirsiniz.

## <a name="what-roles-do-partners-play-in-your-integration-account"></a>Hangi rollerin ortakları tümleştirme hesabınızda yürütülüyor?

İş ortakları arasında alınıp ileti ayrıntılarını tanımlamak için bu iş ortakları arasında anlaşmaları oluşturun. Bir anlaşma oluşturmadan önce ancak, en az iki iş ortağı tümleştirme hesabınıza eklemiş olmanız gerekir. Kuruluşunuz anlaşmanın bir parçası olmalıdır **ana iş ortağı**. Diğer ortağı veya **Konuk iş ortağı** iletileri kuruluşunuz ile alış verişleri kuruluşu temsil eder. Konuk iş ortağı, başka bir şirket veya hatta bir bölüm veya kendi kuruluşunuzdaki olabilir.

Bu iş ortaklarının ekledikten sonra bir anlaşma oluşturabilirsiniz.

Gönderip ayarları açısından bakıldığında barındırılan ortağının yerleştirilir. Örneğin, bir anlaşma alma ayarlarında nasıl barındırılan ortak bir konuk ortağından gönderilen iletileri alan belirler. Benzer şekilde, nasıl barındırılan iş ortağı Konuk iş ortağı iletileri gönderir anlaşmasında gönderme ayarlarını belirtin.

## <a name="create-partner"></a>İş ortağı oluşturun

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Azure ana menüde seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. Altında **tümleştirme hesapları**, ortaklarınızın eklemek istediğiniz tümleştirme hesabı seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Seçin **ortakları** döşeme.

   !["Partners" seçin](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Altında **ortakları**, seçin **Ekle**.

   !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. İş ortağınız için bir ad girin ve ardından bir **niteleyicisi**. Girin bir **değeri** uygulamalarınızı alma belgelerini tanımlamak için. İşiniz bittiğinde seçin **Tamam**.

   ![İş ortağı ayrıntılarını Ekle](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Seçin **ortakları** yeniden döşeme.

   !["Partners" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   Yeni iş ortağınız artık görünür. 

   ![Görünüm yeni iş ortağı](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="edit-partner"></a>İş ortağı Düzenle

1. İçinde [Azure portal](https://portal.azure.com), bulma ve tümleştirme hesabınızı seçin. Seçin **ortakları** döşeme.

   !["Partners" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/edit.png)

2. Altında **ortakları**, düzenlemek istediğiniz iş ortağını seçin.

   ![Silmek için iş ortağı seçin](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. Altında **güncelleştirme iş ortağı**, istediğiniz değişiklikleri yapın.
Bitirdikten sonra seçin **kaydetmek**. 

   ![Yapın ve değişikliklerinizi kaydedin](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   Yaptığınız değişiklikleri iptal etmek için seçin **atmak**.

## <a name="delete-partner"></a>İş ortağı Sil

1. İçinde [Azure portal](https://portal.azure.com), bulma ve tümleştirme hesabınızı seçin. Seçin **ortakları** döşeme.

   !["Partners" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/delete.png)

2. Altında **ortakları**, silmek istediğiniz iş ortağını seçin.
Seçin **silmek**.

   ![İş ortağı Sil](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Anlaşmaları hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmaları hakkında bilgi edinin")  

