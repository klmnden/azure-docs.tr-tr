---
title: Azure Marketi için bulut iş ortağı Portalı'nda sanal makinenin Test Sürüşü sekmesini | Microsoft Docs
description: Bir Azure Market VM teklifi oluşturmak için kullanılan Test Sürüşü sekmesi açıklanır.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 97fb21dc390bd365357f6395c72aa282423c83c9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61344620"
---
# <a name="virtual-machine-test-drive-tab"></a>Sanal makine Test Sürüşü sekmesi

<!-- TD: The AMP tree needs a conceptual/business overview of Test Drive. I've deleted all the marketing fluff and most of overview from this topic.   See also https://azure.microsoft.com/blog/azure-marketplace-test-drive/ and https://github.com/Azure/AzureTestDrive/wiki/What-is-a-Test-Drive. --> 

**Test Sürüşü** sekmesinde **yeni teklif** sayfası sağlar, ürününüzün temel özellikler ve avantajları, örnekte gösterildiği uygulamalı kendi kendine yol gösteren bir tanıtımı, müşteri adaylarınıza sağlamak bir standartlaştırılmış senaryo.  Test Sürüşü Test Sürüşü destekleyen teklif türleri için isteğe bağlı bir özelliktir.  Test Sürüşü düzgün şekilde uygulanması gereken varlıklar destekleyen gerektirir.  Daha fazla bilgi için bkz [Azure Marketi'nde Test Sürüşü](https://azure.microsoft.com/blog/azure-marketplace-test-drive/).  <!--TD: Replace with migrated version of Test Drive article! -->

Bu özelliği etkinleştirmek için **Test Sürüşü** sekmesinde **Evet** seçeneğini **Test Sürüşü etkinleştirme**.  **Test Sürüşü** sekmesi düzenlemek için kullanılabilir alanları görüntüler.  Alan adı eklenmiş bir yıldız (*) gerekli olduğunu gösterir.

![Sanal makineler için yeni teklifi formdaki test sürücü sekmesi](./media/publishvm_007.png)

<br/>

Aşağıdaki tabloda, amacı ve bu alanların içeriğini açıklar.


|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
|  *Ayrıntılar*   |  |
| **Açıklama**           | Test Sürüşü senaryonuz için genel bir bakış sağlar. Bu metin, Test Sürüşü sağlanırken kullanıcıya gösterilir. Biçimlendirilmiş içerik sağlamak istiyorsanız bu alan temel HTML destekler.  |
| **Kullanıcı el kitabı**           | Test Sürüşü kullanıcılar çözümünüzü kullanmayı öğrenmenize yardımcı olan ayrıntılı kullanıcı kılavuzuna (.pdf) karşıya yükleyin.  |
| **Test sürücü tanıtım videosu** | Çözümünüzü gösteren bir video karşıya yükleyin.  Bu seçeneği seçerseniz video URL'si (YouTube veya Vimeo barındırılan) video ve (533 x 324 piksel) küçük bir ad sağlamanız gerekir. |
| *Teknik yapılandırma* |  |
| **Örnekler**             | Bölge kullanılabilirliği hem göreli olarak kullanılabilirlik sanal makine örneği belirtin (daha fazla ayrıntı için bilgi simgesine tıklayın).  <br/>Olası eşzamanlı Test Sürüşü oturum aboneliğiniz kota sınırını aşmamalıdır.  İlk olarak hesaplanır: [numarası, seçili bölgeleri] [etkin örnekler] x + [, bölgeler seçili numarası] x [sıcak örnekleri] + [, bölgeler seçili numarası] x [soğuk örnekleri] |
| **Test Sürüşü süresi**   | Saat cinsinden maksimum oturum süresi. Bu süre aşılırsa sonra Test Sürüşü oturumu otomatik olarak sona erer.  |
|**Test sürücü ARM şablonu**| Bu Test Sürüşü ile ilişkili Azure Resource Manager şablonunu karşıya yükleyin. Daha fazla bilgi için [Test Sürüşü için sanal makine dağıtım şablonu dönüştürme](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive). |
| **Erişim bilgileri**    | Azure Resource Manager erişim ve deneme oturum açma bilgileri, basit bir HTML veya düz metin yazılır. |
| *Test Sürüşü dağıtım Abonelik Ayrıntıları* |  |
| **Azure abonelik kimliği** | Oturum açarak alınabilir [Microsoft Azure Portal'da](https://ms.portal.azure.com) tıklayıp **abonelikleri** sol menü çubuğu üzerinde. (Örnek: "a83645ac-1234-5ab6-6789-1h234g764ghty")    Bu tanımlayıcı bir GUID biçiminde olmalıdır `a83645ac-1234-5ab6-6789-1h234g764ghty`.|
| **Azure AD Kiracı kimliği**    | Azure Active Directory Kiracı kimliği  Oturum açarak alınabilir [Microsoft Azure Portal'da](https://ms.portal.azure.com) tıklayıp **Azure Active Directory** sol menü, ardından **özellikleri** Orta menü içinde ardından kopyalama **dizin kimliği** formundan.  Bu tanımlayıcı, aynı zamanda bir GUID olmalıdır.  Boşsa, kuruluşunuz için bir kiracı kimliği oluşturmanız gerekir. |
| **Azure AD uygulama kimliği**       | Kayıtlı Azure VM çözümünüz için tanımlayıcı  |
| **Azure AD uygulama anahtarı**      | Kayıtlı çözümünüz için kimlik doğrulama anahtarı |
|  |  |

<br/>

Sonraki [Market](./cpp-marketplace-tab.md) sekmesi, pazarlama ve yasal bilgi çözümünüzü hakkında sağlayacaktır.
