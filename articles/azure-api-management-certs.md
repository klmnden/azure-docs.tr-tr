---
title: "Azure yönetim API sertifikası karşıya | Microsoft Docs"
description: "Azure portalı yönetim API sertifikası karşıya öğrenin."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: ad55d71a56657e9cf33c1d33e09c58295206a2ae
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Azure yönetim API'si yönetim sertifikasını karşıya yükle
Yönetim sertifikaları, Azure tarafından sağlanan Klasik dağıtım modeli, kimlik doğrulaması sağlar. Birçok programlar ve araçlar (örneğin, Visual Studio ya da Azure SDK'sı) yapılandırma ve çeşitli Azure hizmetlerine dağıtımını otomatik hale getirmek için bu sertifikaları kullanacak. 

> [!WARNING]
> Dikkatli olun! Bu tür bir sertifika ile ilişkili aboneliği yönetmek için kimlik doğrulamasını herkes izin verin.
>
>

(Dahil olmak üzere otomatik olarak imzalanan sertifika oluşturma) Azure sertifikalar hakkında daha fazla bilgi isterseniz bkz [Azure Cloud Services sertifikalarına genel bakış](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Aynı zamanda [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) Otomasyon amaçları için istemci kodu doğrulanacak.

**Not:** yönetim sertifikaları altında herhangi bir işlem gerçekleştirmek için abonelik ortak bir yönetici olması gerekir. [Daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=849300) yeni Azure Portalı'ndan nasıl Ekle veya Kaldır ortak yöneticileri 

## <a name="upload-a-management-certificate"></a>Bir yönetim sertifikasını karşıya yükle
Bulduktan sonra oluşturulan yönetim sertifikası, (.cer dosyası yalnızca ortak anahtarla) portalına karşıya yükleyebilir. Sertifika Portalı'nda kullanılabilir olduğunda, eşleşen bir sertifika (özel anahtarı) kimseyle Yönetimi API aracılığıyla bağlanmak ve ilişkili aboneliği için kaynak erişim.

1. [Azure Portal](http://portal.azure.com)’da oturum açın.
2. Tıklatın **daha fazla hizmet** alt Azure hizmeti listenin seçip **abonelikleri** içinde _genel_ hizmeti grubu.

    ![Abonelik menüsü](./media/azure-api-management-certs/subscriptions_menu.png)

3. Sertifika ile ilişkilendirmek istediğiniz için doğru aboneliğin seçtiğinizden emin olun.     
4. Doğru abonelik seçtikten sonra basın **yönetim sertifikaları** içinde _ayarları_ grubu.

    ![Ayarlar](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Tuşuna **karşıya** düğmesi.

    ![Sertifikalar sayfasında karşıya yükle](./media/azure-api-management-certs/certificates_page.png)
6. İletişim bilgileri ve tuşuna doldurmak **karşıya**.

    ![Ayarlar](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Sonraki adımlar
Bir aboneliği ile ilişkili bir yönetim sertifikası sahip olduğunuza göre (yerel olarak eşleşen sertifika yükledikten sonra) programlı olarak bağlanabileceğiniz [Klasik dağıtım modeli REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) ve otomatik hale getirme Ayrıca bu abonelikle ilişkili çeşitli Azure kaynakları.
