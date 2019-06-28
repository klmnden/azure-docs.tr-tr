---
title: Ticari ortaklar B2B tümleştirmeleri - Azure Logic Apps için ekleyin
description: Azure Logic Apps ile kullanılacak ortaklarını tümleştirme hesabı oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 681f16132c1de2ec5f3b27f80633d32879b0746c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330207"
---
# <a name="add-trading-partners-to-integration-accounts-for-azure-logic-apps"></a>Ticari ortaklar tümleştirme hesapları için Azure Logic Apps ekleyin.

İçinde [Azure Logic Apps](../logic-apps/logic-apps-overview.md), otomatik işletmeler arası (B2B) Tümleştirmesi iş akışlarınızı kullanarak oluşturabileceğiniz bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) logic apps ile. Kuruluşunuz ve diğer temsil etmek için oluşturma ve ortaklarını yapıtları tümleştirme hesabınıza ekleyin. Ortaklar B2B işlemlere katılmasına ve birbirleriyle mesaj alışverişi varlıklardır.

Bu iş ortakları oluşturmadan önce tartışın ve iş ortaklarınızı belirleme ve diğer gönderdiği iletileri doğrulamak hakkında bilgi paylaşmak emin olun. Bu ayrıntılar üzerinde mutabık kaldıktan sonra iş ortaklarının tümleştirme hesabı oluşturmaya hazırsınız.

## <a name="partner-roles-in-integration-accounts"></a>Tümleştirme hesabı iş ortağı rolü

İş ortaklarınızla alınıp verilen iletileri hakkındaki ayrıntıları tanımlamak için oluşturma ve ekleme [sözleşmeleri](../logic-apps/logic-apps-enterprise-integration-agreements.md) tümleştirme hesabınıza yapıt olarak. Anlaşmaları en az iki iş ortaklarının tümleştirme hesabı gerektirir. Her zaman kuruluştur *konak iş ortağı* sözleşmesi. Kuruluşunuz iletileriyle birbiriyle değiştirir kuruluş *Konuk iş ortağı*. Konuk iş ortağı, başka bir şirket veya bir bölümü kendi kuruluşunuzda bile olabilir. Bu iş ortakları ekledikten sonra bir anlaşma oluşturabilirsiniz.

Bir sözleşmede ana iş ortağının açısından gelen ve giden iletileri işlemek için ayrıntıları belirtin. Gelen iletiler için **alma ayarı** nasıl konak iş ortağı Konuk iş ortağı Sözleşmesi'nin gelen iletileri alan belirtin. Giden iletiler için **gönderme ayarları** konak iş ortağı iletileri Konuk iş ortağı nasıl göndereceğini belirtin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , iş ortakları, sözleşmeler ve diğer B2B yapıtlarını depolamak için. Bu tümleştirme hesabı, Azure aboneliğinizle ilişkili olmalıdır.

## <a name="create-partner"></a>İş ortağı oluşturun

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve seçin **tümleştirme hesapları**.

   !["Tümleştirme hesapları" seçin](./media/logic-apps-enterprise-integration-partners/find-integration-accounts.png)

1. Altında **tümleştirme hesapları**, iş ortaklarınızı eklemek istediğiniz tümleştirme hesabı'nı seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-partners/select-integration-account.png)

1. Seçin **iş ortakları** Döşe.

   !["İş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/choose-partners.png)

1. Altında **iş ortakları**, seçin **Ekle**. Altında **ortak ekleme**, aşağıdaki tabloda açıklandığı gibi iş ortağı ayrıntıları sağlayın.

   !["Ekle" yi seçin ve iş ortağı ayrıntıları sağlayın](./media/logic-apps-enterprise-integration-partners/add-partners.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Ad** | Evet | İş ortağı adı |
   | **Niteleyicisi** | Evet | Örneğin, kuruluş, benzersiz iş kimlikleri sağlayan kimlik doğrulama gövdesi **D-U-N-S (Dun & Bradstreet)** . <p>İş ortakları için karşılıklı olarak tanımlanmış iş kimliği seçebilirsiniz. Bu senaryolar için seçin **karşılıklı olarak tanımlanan** EDIFACT için veya **(X12)'karşılıklı olarak tanımlanan** X12 için. <p>RosettaNet için yalnızca belirli **DUNS**, standart olduğu. |
   | **Değer** | Evet | Logic apps alma belgeleri tanımlayan bir değer. <p>RosettaNet için bu değer DUNS numarasına karşılık gelen bir dokuz basamaklı bir sayı olmalıdır. |
   ||||

   > [!NOTE]
   > RosettaNet kullanan iş ortakları için bu iş ortakları oluşturarak ek bilgilerini belirtebilirsiniz ve ardından [sonradan düzenleme](#edit-partner).

1. İşiniz bittiğinde seçin **Tamam**.

   Yeni iş ortağınız artık görünür **iş ortakları** listesi. Ayrıca, **iş ortakları** kutucuğu, geçerli iş ortaklarının sayısını güncelleştirir.

   ![Yeni iş ortağı](./media/logic-apps-enterprise-integration-partners/new-partner.png)

<a name="edit-partner"></a>

## <a name="edit-partner"></a>İş ortağı Düzenle

1. İçinde [Azure portalında](https://portal.azure.com)bulup tümleştirme hesabınızı seçin.
Seçin **iş ortakları** Döşe.

   !["İş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/edit.png)

1. Altında **iş ortakları**, düzenlemek ve istediğiniz iş ortağını seçin **Düzenle**. Altında **Düzenle**, istediğiniz değişiklikleri yapın.

   ![Yapın ve değişikliklerinizi kaydedin](./media/logic-apps-enterprise-integration-partners/edit-partner.png)

   RosettaNet için altında **RosettaNet ortak özellikleri**, bu ek bilgileri belirtebilirsiniz:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İş ortağı sınıflandırma** | Hayır | İş ortağı kuruluş türü |
   | **Tedarik zinciri kod** | Hayır | İş ortağının ve tedarik zinciri kod, örneğin, "Bilgi teknolojisi" veya "Elektronik bileşenlerin" |
   | **İlgili kişi adı** | Hayır | İş ortağının ilgili kişi adı |
   | **E-posta** | Hayır | İş ortağının e-posta adresi |
   | **Faks** | Hayır | İş ortağının faks numarası |
   | **Telefon** | Hayır | İş ortağının telefon numarası |
   ||||

1. İşiniz bittiğinde seçin **Tamam** yaptığınız değişiklikleri kaydedin.

## <a name="delete-partner"></a>İş ortağını Sil

1. İçinde [Azure portalında](https://portal.azure.com)bulup tümleştirme hesabınızı seçin. Seçin **iş ortakları** Döşe.

   !["İş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/choose-partners-to-delete.png)

1. Altında **iş ortakları**, silmek istediğiniz iş ortağını seçin. Seçin **Sil**.

   ![İş ortağını Sil](./media/logic-apps-enterprise-integration-partners/delete-partner.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [sözleşmeleri](../logic-apps/logic-apps-enterprise-integration-agreements.md)