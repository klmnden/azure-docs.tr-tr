---
title: Sertifikalar - Azure Logic Apps B2B iletileri güvenli hale getirme | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile B2B iletileri güvenli hale getirmek için sertifikalar ekleme
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, LADocs
manager: jeconnoc
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.suite: integration
ms.topic: article
ms.date: 08/17/2018
ms.openlocfilehash: 38bc1615c0849a33ddfa5790a66fc05d681ce339
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66167137"
---
# <a name="secure-b2b-messages-with-certificates"></a>Sertifikaları güvenli B2B iletileri

B2B iletişim gizli tutmak gerektiğinde, B2B iletişim, kuruluşunuz için tümleştirme uygulamaları, özellikle logic apps, tümleştirme hesabınıza sertifikaları ekleyerek güvenliğini sağlayabilirsiniz. Sertifikalar elektronik iletişimleri katılımcılar kimlikleri denetlemek ve bu şekilde iletişimi güvenli hale getirmeye yardımcı dijital belgelerdir:

* İleti içeriğini şifreleyin.
* İletileri imzala. 

Bu sertifikalar, Kurumsal tümleştirme uygulamalarınızda kullanabilirsiniz:

* [Ortak Sertifikalar](https://en.wikipedia.org/wiki/Public_key_certificate), hangi bir genel internet'ten satın almalısınız [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority) ancak herhangi bir anahtarı gerektirmez. 

* Özel sertifikaları veya [ *otomatik olarak imzalanan sertifikalar*](https://en.wikipedia.org/wiki/Self-signed_certificate), oluşturma ve kendiniz sorun ancak de özel anahtarlar gerektirir. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="upload-a-public-certificate"></a>Bir ortak sertifika karşıya yükleme

Kullanılacak bir *ortak sertifika* B2B özelliklerine sahip logic apps'te, önce sertifika tümleştirme hesabınıza yüklemeniz gerekir. Özelliklerinde tanımladıktan sonra [sözleşmeleri](logic-apps-enterprise-integration-agreements.md) oluşturduğunuz, B2B iletilerinizi güvenli hale getirmeye yardımcı olmak sertifika kullanılabilir.

1. [Azure Portal](https://portal.azure.com) oturum açın. Ana Azure menüsünde **tüm kaynakları**. Arama kutusuna tümleştirme hesap adınızı girin ve ardından istediğiniz tümleştirme hesabı seçin.

   ![Bul ve tümleştirme hesabınızı seçin](media/logic-apps-enterprise-integration-certificates/select-integration-account.png)  

2. Altında **bileşenleri**, seçin **sertifikaları** Döşe.

   !["Sertifikalar" seçin](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

3. Altında **sertifikaları**, seçin **Ekle**. Altında **sertifika Ekle**, sertifikanız için bu ayrıntıları sağlayın. İşiniz bittiğinde seçin **Tamam**.

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Ad** | <*Sertifika adı*> | Bu örnekte "publicCert" Sertifikanızın adı | 
   | **Sertifika türü** | Genel | Sertifikanızın türü |
   | **Sertifika** | <*Sertifika dosya adı*> | Bulmak ve yüklemek istediğiniz sertifika dosyasını seçmek için klasör simgesine yanındaki seçin **sertifika** kutusu. |
   ||||

   !["Ekle" öğesini seçin, sertifika ayrıntılarını sağlayın](media/logic-apps-enterprise-integration-certificates/public-certificate-details.png)

   Azure, Azure seçiminizi doğruladıktan sonra sertifikanız karşıya yüklenir.

   ![Yeni sertifika Azure görüntüler](media/logic-apps-enterprise-integration-certificates/new-public-certificate.png) 

## <a name="upload-a-private-certificate"></a>Özel bir sertifika yükleyin

Kullanılacak bir *özel sertifika* B2B özelliklerine sahip logic apps'te, önce sertifika tümleştirme hesabınıza yüklemeniz gerekir. Ayrıca ilk eklediğiniz bir özel anahtara sahip gerekir [Azure anahtar kasası](../key-vault/key-vault-get-started.md). 

Özelliklerinde tanımladıktan sonra [sözleşmeleri](logic-apps-enterprise-integration-agreements.md) oluşturduğunuz, B2B iletilerinizi güvenli hale getirmeye yardımcı olmak sertifika kullanılabilir.

> [!NOTE]
> Özel sertifikaları için görünür karşılık gelen bir ortak sertifika eklediğinizden emin olun [AS2 anlaşması](logic-apps-enterprise-integration-as2.md) **gönderip** imzalama ve iletileri şifrelemek için ayarlar.

1. [Özel anahtarınızı Azure anahtar Kasası'na ekleme](../key-vault/certificate-scenarios.md#import-a-certificate) ve sağlayan bir **anahtar adı**.
   
2. Azure Logic Apps, Azure anahtar kasası işlemleri gerçekleştirmek için yetkilendirin. Mantıksal uygulamalar hizmet sorumlusu için erişim vermek için PowerShell komutunu kullanın. [kümesi AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy), örneğin:

   `Set-AzKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. [Azure Portal](https://portal.azure.com) oturum açın. Ana Azure menüsünde **tüm kaynakları**. Arama kutusuna tümleştirme hesap adınızı girin ve ardından istediğiniz tümleştirme hesabı seçin.

   ![Tümleştirme hesabı bulunamadı](media/logic-apps-enterprise-integration-certificates/select-integration-account.png) 

4. Altında **bileşenleri**, seçin **sertifikaları** Döşe.  

   ![Sertifikaları kutucuğu seçin](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

5. Altında **sertifikaları**, seçin **Ekle**. Altında **sertifika Ekle**, sertifikanız için bu ayrıntıları sağlayın. İşiniz bittiğinde seçin **Tamam**.

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Ad** | <*Sertifika adı*> | Bu örnekte "privateCert" Sertifikanızın adı | 
   | **Sertifika türü** | Özel | Sertifikanızın türü |
   | **Sertifika** | <*Sertifika dosya adı*> | Bulmak ve yüklemek istediğiniz sertifika dosyasını seçmek için klasör simgesine yanındaki seçin **sertifika** kutusu. | 
   | **Kaynak Grubu** | <*Tümleştirme hesabının kaynak grubu*> | Bu örnekte, "MyResourceGroup" olan tümleştirme hesabının kaynak grubu | 
   | **Anahtar Kasası** | <*Key vault adı*> | Azure anahtar kasasının adı |
   | **Anahtar adı** | <*anahtar adı*> | Anahtarınızı kişinin adı |
   ||||

   !["Ekle" öğesini seçin, sertifika ayrıntılarını sağlayın](media/logic-apps-enterprise-integration-certificates/private-certificate-details.png)

   Azure, Azure seçiminizi doğruladıktan sonra sertifikanız karşıya yüklenir.

   ![Yeni sertifika Azure görüntüler](media/logic-apps-enterprise-integration-certificates/new-private-certificate.png) 

## <a name="next-steps"></a>Sonraki adımlar

* [B2B sözleşmesi oluşturma](logic-apps-enterprise-integration-agreements.md)
