---
title: Bir Azure Hizmet Yönetim sertifikasını karşıya yükle | Microsoft Docs
description: Azure portalında Hizmet Yönetim sertifikası karşıya yüklemeyi öğrenin.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeconnoc
ms.openlocfilehash: 014a26c2500959502eeb1c50d3f311584c1ad84e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60742977"
---
# <a name="upload-an-azure-service-management-certificate"></a>Bir Azure Hizmet Yönetim sertifikasını karşıya yükleyin
Yönetim sertifikaları, Azure tarafından sağlanan Klasik dağıtım modeli ile kimlik doğrulaması sağlar. Birçok programları ve Araçları (örneğin, Visual Studio ya da Azure SDK'sı) bu sertifikaları yapılandırma ve çeşitli Azure Hizmetleri dağıtımını otomatikleştirmek için kullanın. 

> [!WARNING]
> Dikkat et! Bu tür bir sertifika ile ilişkili aboneliği yönetmek için onlarla kimliğini doğrulayan herkes izin verin.
>
>

Azure sertifikaları (dahil olmak üzere otomatik olarak imzalanan sertifika oluşturma) hakkında daha fazla bilgi istiyorsanız bkz [Azure Cloud Services sertifikalarına genel bakış](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Ayrıca [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) istemci kodu, Otomasyon amacıyla kimlik doğrulaması için.

**Not:** Yönetim sertifikaları altında'herhangi bir işlemi gerçekleştirmek için aboneliğin ortak yönetici olmanız gerekir. [Daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=849300) yeni Azure Portalı'ndan nasıl Ekle veya Kaldır ortak Yöneticiler 

## <a name="upload-a-management-certificate"></a>Bir yönetim sertifikasını karşıya yükleyin
Sonra bir yönetim sertifikası oluşturulan, (yalnızca ortak anahtarı içeren .cer dosyası) portalına karşıya yükleyebilirsiniz. Sertifika Portalı'nda kullanılabilir olduğunda, eşleşen bir sertifika (özel anahtarı) olan herkes yönetim API'si bağlanabilir ve ilgili abonelik için kaynaklara erişim.

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Tıklayın **tüm hizmetleri** alt Azure hizmeti listenin seçip **abonelikleri** içinde _genel_ hizmeti grubu.

    ![Abonelik menü](./media/azure-api-management-certs/subscriptions_menu.png)

3. Sertifika ile ilişkilendirmek istediğiniz doğru aboneliği seçmek emin olun.     
4. Doğru abonelik seçtikten sonra basın **yönetim sertifikaları** içinde _ayarları_ grubu.

    ![Ayarlar](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Tuşuna **karşıya** düğmesi.

    ![Sertifikalar sayfasında karşıya yükleme](./media/azure-api-management-certs/certificates_page.png)
6. İletişim bilgileri ve ENTER tuşuna doldurmak **karşıya**.

    ![Ayarlar](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Sonraki adımlar
Bir abonelikle ilişkili bir yönetim sertifikası edindikten sonra (yerel olarak eşleşen sertifika yükledikten sonra) programlı bir şekilde bağlanabilirsiniz [Klasik dağıtım modeli REST API](/azure/#pivot=sdkstools) ve otomatik hale getirin Bu abonelikle ilişkili çeşitli Azure kaynakları.
