---
title: "Azure mantıksal uygulamaları sertifikalarla B2B iletileri güvenli | Microsoft Docs"
description: "B2B iletileri Kurumsal tümleştirme paketi ile güvenli hale getirmek için sertifikaları Ekle"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 708bdcddede71186c48ae7d4034cc9df0bd61391
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="secure-b2b-messages-with-certificates"></a>Sertifikalar ile güvenli B2B iletiler

Bazen B2B iletişimi gizli tutmanız gerekir. Tümleştirme uygulamalarını, özellikle mantıksal uygulamalar, kuruluşunuz için B2B iletişimi korumanıza yardımcı olması için tümleştirme hesabınıza sertifikalar ekleyebilirsiniz. Sertifikalar elektronik iletişimin katılımcıları kimliğini doğrulayan dijital belgelerdir.
Sertifikaları güvenli iletişim aşağıdaki şekillerde yardımcı:

* İleti içeriği şifrele
* İletileri dijital olarak imzala  

Bu sertifikalar Kurumsal tümleştirme uygulamalarınızda kullanabilirsiniz:

* Ortak Sertifikalar, sertifika yetkilisinden (CA) satın alınması gerekir.
* Özel sertifikaları kendiniz uygulayabilirsiniz. Bu sertifikaları otomatik olarak imzalanan sertifikalar da adlandırılır.

## <a name="upload-a-public-certificate"></a>Ortak sertifikasını karşıya yükle

Kullanılacak bir *ortak sertifika* B2B özelliklerine sahip logic apps içinde ilk sertifikayı tümleştirme hesabınıza yüklemeniz gerekir. Özelliklerinde tanımladıktan sonra [anlaşmaları](logic-apps-enterprise-integration-agreements.md) oluşturduğunuz, sertifika B2B iletilerinizi korumanıza yardımcı olması kullanılabilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Azure ana menüde seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabınızı Bul](media/logic-apps-enterprise-integration-certificates/overview-1.png)  

3. Altında **tümleştirme hesapları**, sertifikanın eklemek istediğiniz tümleştirme hesabı seçin.

   ![Sertifika eklemek istediğiniz tümleştirme hesabı seçin](media/logic-apps-enterprise-integration-certificates/overview-3.png)  

4. Seçin **sertifikaları** döşeme.  

   !["Sertifikalar" seçin](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

5. Altında **sertifikaları**, seçin **Ekle**.

   !["Ekle" yi seçin](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

6. Altında **sertifika Ekle**, sertifikanızı için ayrıntıları sağlayın.
   
   1. Sertifikanızı girin **adı**. Sertifika türü için **ortak**.

   2. Sağ tarafında **sertifika** kutusunda, klasör simgesini seçin. 
   Bulun ve karşıya yüklemek istediğiniz sertifika dosyası seçin. 
   İşiniz bittiğinde seçin **Tamam**.

      ![Ortak sertifikasını karşıya yükle](media/logic-apps-enterprise-integration-certificates/certificate-3.png)

   Azure, seçiminizi doğrulandıktan sonra sertifikanızı karşıya yükleme.

   ![Yeni sertifika bakın](media/logic-apps-enterprise-integration-certificates/certificate-4.png) 

## <a name="upload-a-private-certificate"></a>Özel bir sertifika karşıya yükle

Kullanılacak bir *özel sertifika* B2B özelliklerine sahip logic apps içinde ilk sertifikayı tümleştirme hesabınıza yüklemeniz gerekir. Ayrıca önce eklemeniz bir özel anahtara sahip olmanız gerekir [Azure anahtar kasası](../key-vault/key-vault-get-started.md). 

Özelliklerinde tanımladıktan sonra [anlaşmaları](logic-apps-enterprise-integration-agreements.md) oluşturduğunuz, sertifika B2B iletilerinizi korumanıza yardımcı olması kullanılabilir.

> [!NOTE]
> Özel sertifikaları için görünmesi karşılık gelen bir ortak sertifika eklediğinizden emin olun [AS2 sözleşmesi](logic-apps-enterprise-integration-as2.md) gönderip ayarları imzalama ve iletileri şifrelemek için.

1. [Özel anahtarınızı Azure anahtar Kasası'na eklemek](../key-vault/key-vault-get-started.md#add) ve sağlayan bir **anahtar adı**.
   
2. Azure anahtar kasası işlemleri gerçekleştirmek için Azure Logic Apps yetkilendirin. Logic Apps hizmet sorumlusuna erişim vermek için PowerShell komutunu kullanın [kümesi AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), örneğin:

   `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. [Azure Portal](https://portal.azure.com) oturum açın.

4. Azure ana menüde seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabınızı Bul](media/logic-apps-enterprise-integration-certificates/overview-1.png) 

5. Altında **tümleştirme hesapları**, sertifikanın eklemek istediğiniz tümleştirme hesabı seçin.

6. Seçin **sertifikaları** döşeme.  

   ![Sertifikaları döşeme seçin](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

7. Altında **sertifikaları**, seçin **Ekle**.   

   ![Ekle düğmesini seçin](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

8. Altında **sertifika Ekle**, sertifikanızı için ayrıntıları sağlayın.
   
   1. Sertifikanızı girin **adı**. Sertifika türü için **özel**.

   2. Sağ tarafında **sertifika** kutusunda, klasör simgesini seçin. 
   Bulun ve karşıya yüklemek istediğiniz sertifika dosyası seçin. 
   Ayrıca, seçin **kaynak grubu**, **anahtar kasası**, ve **anahtar adı**. 
   İşiniz bittiğinde seçin **Tamam**.

      ![Sertifika ekleme](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)

   Azure, seçiminizi doğrulandıktan sonra sertifikanızı karşıya yükleme.

   ![Yeni sertifika bakın](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B sözleşmesi oluşturma](logic-apps-enterprise-integration-agreements.md)
