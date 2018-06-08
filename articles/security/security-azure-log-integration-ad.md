---
title: Azure Active Directory denetim günlükleri ile Azure günlük tümleştirme | Microsoft Docs
description: Azure günlük tümleştirme hizmeti yüklemek ve Azure denetim günlükleri günlüklerinden tümleştirme hakkında bilgi edinin
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 06/06/2018
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: d7ec3a3fe26600e69a7e4f511c01006a97cbe12c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839490"
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Azure Active Directory denetim günlüklerini tümleştirme

Azure Active Directory (Azure AD) denetim olayları Azure Active Directory'de oluştu ayrıcalıklı Eylemler belirlemenize yardımcı olur. Gözden geçirerek izleyebilirsiniz olay türlerini görebilirsiniz [Azure Active Directory Denetim Raporu olayları](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).


>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 01/06/2019 tarafından kullanım dışı kalacaktır. AzLog yüklemeleri 27 Haz 2018 tarafından devre dışı bırakılacak. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında yönergeler için [SIEM araçları ile tümleştirmek için kullanım Azure İzleyicisi](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/preview/?cdn=disable) 

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Azure Active Directory Tümleştirme adımları denetim günlükleri

> [!NOTE]
> Bu makaledeki adımları denemeden önce gözden geçirmeniz gerekir [başlama](security-azure-log-integration-get-started.md) makalesi ve ilgili adımları tamamlayın.

1. Komut istemi açın ve şu komutu çalıştırın:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Şu komutu çalıştırın: 
 
   ``azlog createazureid``

   Bu komut için Azure oturum açma bilgilerinizi ister. Komutu daha sonra bir Azure Active Directory oturum açma kullanıcı bir yönetici, bir ortak yönetici veya sahibi olduğu Azure abonelikleri barındıran Azure AD kiracılar hizmet sorumlusu oluşturur. Oturum açma kullanıcı yalnızca Konuk kullanıcı olarak Azure AD kiracısı ise komut başarısız olur. Azure kimlik doğrulaması Azure AD üzerinden yapılır. Azure günlük tümleştirmesi için bir hizmet sorumlusu oluşturma Azure aboneliklerinden okuma erişimi verilir Azure AD kimlik oluşturur.

3. Kiracı kimliğinizi sağlamak için aşağıdaki komutu çalıştırın Komutu çalıştırmak için Kiracı yöneticisi rolünün üyesi olması gerekir.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Örnek:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Azure Active Directory denetim günlüğü JSON dosyalarını bunları oluşturulduğunu doğrulamak için aşağıdaki klasörler denetleyin:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

Aşağıdaki video bu makalede ele alınan adımları gösterir:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Güvenlik bilgileri ve Olay yönetimi (SIEM) sistem bilgileri JSON dosyalarında getiren ayrıntılı yönergeler için SIEM satıcınıza başvurun.

Topluluk Yardım ile de kullanılabilir [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Bu forumda birbirine soruları yanıtlar, ipuçları ve püf noktaları ile desteklemek için Azure günlük tümleştirme topluluk kişilere sağlar. Ayrıca, Azure günlük tümleştirme takım Bu forumda izler ve mümkün olduğunca yardımcı olur.

Ayrıca açabilirsiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Seçin **günlük tümleştirme** destek isteyen hizmet olarak.

## <a name="next-steps"></a>Sonraki adımlar
Azure günlük tümleştirmesi hakkında daha fazla bilgi için bkz:

* [Microsoft Azure günlük tümleştirme Azure günlükleri için](https://www.microsoft.com/download/details.aspx?id=53324): Bu Yükleme Merkezi sayfası ayrıntıları, sistem gereksinimleri ve yükleme yönergeleri için Azure günlük tümleştirme sağlar.
* [Azure günlük tümleştirme giriş](security-azure-log-integration-overview.md): Bu makalede Azure günlük tümleştirme, önemli işlevleri ve nasıl çalıştığı tanıtılır.
* [Azure günlük tümleştirme SSS](security-azure-log-integration-faq.md): Bu makalede Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
* [Azure tanılama ve Azure için yeni özellikler denetim günlüklerini](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Bu blog gönderisine Azure denetim günlükleri tanıtır ve yardımcı diğer özellikleri Azure kaynaklarınızı işlemleri alın.
