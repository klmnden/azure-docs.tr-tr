---
title: "Kurumsal tümleştirme paketi ile sertifikaları kullanarak | Microsoft Docs"
description: "Kurumsal tümleştirme paketi sertifikaları kullanmayı öğrenin | Azure mantıksal uygulamaları"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0357e67a8920a57b2ab8b79ebd8dd3a64d888478
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Sertifikalar ve Enterprise Integration Pack hakkında bilgi edinin
## <a name="overview"></a>Genel Bakış
Enterprise Integration B2B iletişimin güvenliğini sağlamak için sertifikaları kullanır. Enterprise Integration uygulamalarınızda iki sertifika türünü kullanarak şunları yapabilirsiniz:

* Ortak Sertifikalar, sertifika yetkilisinden (CA) satın alınması gerekir.
* Özel sertifikaları kendiniz uygulayabilirsiniz. Bu sertifikaları otomatik olarak imzalanan sertifikalar da adlandırılır.

## <a name="what-are-certificates"></a>Sertifikalar nelerdir?
Katılımcıların elektronik iletişimin kimliğini doğrulamak ve ayrıca elektronik iletişimleri güvenli dijital belgeleri sertifikalardır.

## <a name="why-use-certificates"></a>Sertifikaları neden kullanılır?
Bazen B2B iletişimleri gizli tutulması gerekir. Enterprise Integration iki yolla bu iletişimin güvenliğini sağlamak için sertifikaları kullanır:

* İleti içeriğini şifreleyerek
* İletileri imzalamak tarafından  

## <a name="upload-a-public-certificate"></a>Ortak sertifikasını karşıya yükle

Kullanılacak bir *ortak sertifika* B2B özellikleriyle logic apps içinde önce tümleştirme hesabınızda sertifikayı karşıya yüklemek gerekir.  

Bir sertifika yükledikten sonra nesnelerin özelliklerini tanımlarken B2B iletilerinizi korumanıza yardımcı olması kullanılabilir [anlaşmaları](logic-apps-enterprise-integration-agreements.md) oluşturduğunuz.  

Azure portalında oturum açtıktan sonra ortak sertifikalarınızı tümleştirme hesabınızda karşıya yükleme için ayrıntılı adımlar şunlardır:

1. Seçin **tüm hizmetleri** ve girin **tümleştirme** filtre arama kutusuna. Seçin **tümleştirme hesapları** sonuçları listesinden     
![Gözat seçin](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Sertifika eklemek istediğiniz tümleştirme hesabı seçin.  
![Sertifika eklemek istediğiniz tümleştirme hesabı seçin](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Seçin **sertifikaları** döşeme.  
![Sertifikaları kutucuk seçin](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. İçinde **sertifikaları** açıldığında seçin dikey **Ekle** düğmesi.   
![Ekle düğmesini seçin](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Girin bir **adı** sertifika ve sertifika türü seçin **ortak** gelen açılır.  
6. klasör simgesine sağ tarafında seçin **sertifika** metin kutusu. Dosya seçiciyi açıldığında, bulma ve tümleştirme hesabınıza karşıya yüklemek istediğiniz sertifika dosyası seçin.
7. Sertifikayı seçin ve ardından **Tamam** dosya Seçici. Bu doğrular ve tümleştirme hesabınıza sertifikayı yükler.
8. Son olarak, tekrar **sertifika Ekle** dikey penceresinde, select **Tamam** düğmesi.  
![Tamam düğmesine seçin](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Seçin **sertifikaları** döşeme. Yeni eklenen sertifika görmeniz gerekir.  
![Yeni sertifika bakın](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Özel bir sertifika karşıya yükle

Kullanılacak bir *özel sertifika* B2B özellikleriyle logic apps içinde özel bir sertifika tümleştirme hesabınız için aşağıdaki adımları uygulayarak yükleyebilirsiniz

1. [Anahtar Kasası'na özel anahtarınızı karşıya](../key-vault/key-vault-get-started.md "anahtar kasası hakkında bilgi edinin") ve sağlayan bir **anahtar adı** 
   
   > [!TIP]
   > Anahtar kasası işlemleri gerçekleştirmek için Logic Apps yetkilendirmeniz gerekir. Aşağıdaki PowerShell komutunu kullanarak Logic Apps hizmet sorumlusuna erişim izni verebilir: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Önceki adımda ayırdıktan sonra özel bir sertifika tümleştirme hesabını ekleyin.

Azure portalında oturum açtıktan sonra özel sertifikalarınızı tümleştirme hesabınızda karşıya yükleme için ayrıntılı adımlar şunlardır:  
 
1. Sertifika eklemek ve seçmek istediğiniz tümleştirme hesabı seçin **sertifikaları** döşeme.  
![Sertifikaları kutucuk seçin](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. İçinde **sertifikaları** açıldığında seçin dikey **Ekle** düğmesi.   
![Ekle düğmesini seçin](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Girin bir **adı** sertifika ve sertifika türü seç **özel** gelen açılır.   
4. klasör simgesine sağ tarafında seçin **sertifika** metin kutusu. Dosya seçiciyi oturum açtığında, tümleştirme hesabınıza karşıya yüklemek istediğiniz ilgili ortak sertifikayı bulun.   
   
   > [!Note]
   > Özel bir sertifika eklenirken göstermek için karşılık gelen ortak sertifika eklemek önemli olduğunu [AS2 sözleşmesi](logic-apps-enterprise-integration-as2.md) gönderip ayarları imzalama ve iletileri şifrelemek için.
   > 
   >   

5. Seçin **kaynak grubu**, **anahtar kasası**, **anahtar adı** seçip **Tamam** düğmesi.  
![Sertifika Ekle](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Seçin **sertifikaları** döşeme. Yeni eklenen sertifika görmeniz gerekir.
![Yeni sertifika bakın](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [B2B sözleşmesi oluşturma](logic-apps-enterprise-integration-agreements.md)  
* [Anahtar kasası hakkında daha fazla bilgi](../key-vault/key-vault-get-started.md "anahtar kasası hakkında bilgi edinin")  

