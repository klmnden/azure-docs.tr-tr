---
title: Ticari ortaklar B2B tümleştirmeleri - Azure Logic Apps için ekleme | Microsoft Docs
description: Ticari ortaklar tümleştirme hesabı için Azure Logic Apps Enterprise Integration Pack ile oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.date: 07/08/2016
ms.openlocfilehash: 137ed89c276338b534cad8fdf81ec31b5e5610b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60846081"
---
# <a name="add-trading-partners-for-integration-accounts-in-azure-logic-apps-with-enterprise-integration-pack"></a>Azure Logic Apps Enterprise Integration Pack ile tümleştirme hesapları için ticaret iş ortakları ekleme

İş ortakları, işletmeler arası (B2B) işlemlere katılmasına ve birbirlerine arasında mesaj alışverişi varlıklardır. Siz ve bu işlem başka bir kuruluştaki temsil eden iş ortakları oluşturmadan önce her ikisi gerekir tanımlar ve diğer tarafından gönderilen iletileri doğrulama bilgileri paylaşabilir. Bu detayları tartışabilirler ve iş ilişkiniz başlamaya hazır sonra her iki temsil etmek için iş ortaklarının tümleştirme hesabı oluşturabilirsiniz.

## <a name="what-roles-do-partners-play-in-your-integration-account"></a>Hangi rollerin iş ortakları tümleştirmesi hesabınızdaki yürütülüyor?

İş ortakları arasında değiş tokuş iletiyle ilgili ayrıntılı tanımlamak için bu iş ortakları arasında sözleşmeleri oluşturun. Bir anlaşma oluşturmadan önce ancak, en az iki iş ortağı tümleştirmesi hesabınıza eklemiş olmanız gerekir. Kuruluşunuzun bir parçası kabul ettiğiniz sözleşmenin olmalıdır **konak iş ortağı**. Diğer ortak veya **Konuk iş ortağı** yapan kuruluşunuz iletileriyle kuruluşu temsil eder. Konuk iş ortağı, başka bir şirket veya bir bölümü kendi kuruluşunuzda bile olabilir.

Bu iş ortakları ekledikten sonra bir anlaşma oluşturabilirsiniz.

Gönderip ayarları açısından barındırılan iş ortağının yerleştirilir. Örneğin, nasıl barındırılan iş ortağı Konuk ortak gönderilen iletileri alan bir sözleşmenin alma ayarları belirleyin. Benzer şekilde, nasıl barındırılan iş ortağı Konuk iş ortağı iletileri gönderir. anlaşma Gönder ayarlarını belirtin.

## <a name="create-partner"></a>İş ortağı oluşturun

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. Altında **tümleştirme hesapları**, iş ortaklarınızı eklemek istediğiniz tümleştirme hesabı'nı seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Seçin **iş ortakları** Döşe.

   !["İş ortakları" seçin](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Altında **iş ortakları**, seçin **Ekle**.

   !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. İş ortağınız için bir ad girin ve ardından bir **niteleyicisi**. Girin bir **değer** uygulamalarınızı almak belgelerini tanımlamak için. İşiniz bittiğinde seçin **Tamam**.

   ![İş ortağı ayrıntıları ekleyin](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Seçin **iş ortakları** yeniden Döşe.

   !["İş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   Yeni iş ortağınız görünür. 

   ![Yeni iş ortağı görüntüle](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="edit-partner"></a>İş ortağı Düzenle

1. İçinde [Azure portalında](https://portal.azure.com)bulup tümleştirme hesabınızı seçin. Seçin **iş ortakları** Döşe.

   !["İş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/edit.png)

2. Altında **iş ortakları**, düzenlemek istediğiniz iş ortağını seçin.

   ![İş ortağı silmek için işaretleyin](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. Altında **güncelleştirme iş ortağı**, istediğiniz değişiklikleri yapın.
Bitirdikten sonra seçin **Kaydet**. 

   ![Yapın ve değişikliklerinizi kaydedin](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   Yaptığınız değişiklikleri iptal etmek için işaretleyin **at**.

## <a name="delete-partner"></a>İş ortağını Sil

1. İçinde [Azure portalında](https://portal.azure.com)bulup tümleştirme hesabınızı seçin. Seçin **iş ortakları** Döşe.

   !["İş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/delete.png)

2. Altında **iş ortakları**, silmek istediğiniz iş ortağını seçin.
Seçin **Sil**.

   ![İş ortağını Sil](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Sözleşmeleri hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmalar hakkında bilgi edinin")  

