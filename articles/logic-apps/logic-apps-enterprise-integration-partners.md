---
title: "İş ortakları için işletmeden işletmeye (B2B) iletileri - Azure mantıksal uygulamaları oluşturma | Microsoft Docs"
description: "İş ortaklarının Kurumsal tümleştirme paketi ve Logic Apps ile tümleştirme hesabınızı eklemeyi öğrenin"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
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
ms.openlocfilehash: 950cb449b53f400f0f0f860caf5415bbb5212269
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Ekleyin veya iş iş akışınızı sözleşmelerde ortakları güncelleştirin

İşletmeden işletmeye (B2B) işlemlere katılmasına ve birbirleri arasındaki iletileri exchange varlıklar ortaklarıdır. Ve bu işlemler başka bir kuruluştaki temsil eden iş ortakları oluşturabilmeniz için önce her ikisi gerekir tanımlar ve diğer tarafından gönderilen iletileri doğrular bilgileri paylaşabilir. Bu ayrıntıları ele almaktadır ve iş ilişkiniz başlamak hazır sonra her iki temsil etmek için tümleştirme hesabınızı ortakları oluşturabilirsiniz.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Hangi rollerin ortakları tümleştirme hesabınız var mı?

İş ortakları arasında alınıp ileti ayrıntılarını tanımlamak için bu iş ortakları arasında anlaşmaları oluşturun. Bir anlaşma oluşturmadan önce ancak, en az iki iş ortağı tümleştirme hesabınıza eklemiş olmanız gerekir. Kuruluşunuz anlaşmanın bir parçası olmalıdır **ana iş ortağı**. Diğer ortağı veya **Konuk iş ortağı** iletileri kuruluşunuz ile alış verişleri kuruluşu temsil eder. Konuk iş ortağı, başka bir şirket veya hatta bir bölüm veya kendi kuruluşunuzdaki olabilir.

Bu iş ortaklarının ekledikten sonra bir anlaşma oluşturabilirsiniz.

Gönderip ayarları açısından bakıldığında barındırılan ortağının yerleştirilir. Örneğin, bir anlaşma alma ayarlarında nasıl barındırılan ortak bir konuk ortağından gönderilen iletileri alan belirler. Benzer şekilde, nasıl barındırılan iş ortağı Konuk iş ortağı iletileri gönderir anlaşmasında gönderme ayarlarını belirtin.

## <a name="how-to-create-a-partner"></a>Bir iş ortağı oluşturmak nasıl?

1. Azure portalında seçin **Gözat**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Filtre Arama kutusuna **tümleştirme**seçeneğini belirleyip **tümleştirme hesapları** sonuçlar listesinde.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Ortaklarınızın eklemek istediğiniz tümleştirme hesabı seçin.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Seçin **ortakları** döşeme.

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. İş ortakları dikey penceresinde, seçin **Ekle**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. İş ortağınız için bir ad girin ve ardından bir **niteleyicisi**. Son olarak, girin bir **değeri** uygulamalarınızla gelen belgeleri tanımlamaya yardımcı olmak için.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. İş ortağı oluşturma işlemi için ilerleme durumunu görmek için seçin *zil* bildirim simgesi.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. Yeni iş ortaklarınıza başarıyla eklendiğini doğrulamak için şunu seçin **ortakları** döşeme.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    İş ortakları döşeme seçtikten sonra yeni eklenen iş ortakları ortakları dikey penceresinde de görürsünüz.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a>Var olan iş ortakları tümleştirme hesabınızda düzenleme

1. Seçin **ortakları** döşeme.
2. İş ortakları dikey penceresi açıldıktan sonra düzenlemek istediğiniz iş ortağını seçin.
3. Üzerinde **güncelleştirme iş ortağı** döşeme, istediğiniz değişiklikleri yapın.
4. Bitirdikten sonra seçin **kaydetmek**, yaptığınız değişiklikleri iptal etmek için seçin **atmak**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a>Bir iş ortağı silme

1. Seçin **ortakları** döşeme.
2. İş ortağı dikey penceresi açıldıktan sonra silmek istediğiniz iş ortağını seçin.
3. Seçin **silmek**.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Anlaşmaları hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmaları hakkında bilgi edinin")  

