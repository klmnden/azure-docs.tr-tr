---
title: Azure Active Directory denetim günlükleri ile Azure günlük tümleştirmesi | Microsoft Docs
description: Azure günlük tümleştirmesi hizmeti yüklemek ve Azure denetim günlükleri günlüklerinden tümleştirme hakkında bilgi edinin
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 01/14/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: db065c78008e47326155e9e2b3a0f65031ec4cd9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60478438"
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Azure Active Directory denetim günlüklerini tümleştirme

Azure Active Directory (Azure AD) denetim olayları, Azure Active Directory'de oluştu ayrıcalıklı Eylemler belirlemenize yardımcı olur. Gözden geçirerek izleyebileceğiniz olay türleri görebilirsiniz [Azure Active Directory Denetim Raporu olayları](../active-directory/reports-monitoring/concept-audit-logs.md).


>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/01/2019 tarafından kullanımdan kaldırılacaktır. AzLog yüklemeleri, 27 Haziran 2018'de devre dışı bırakıldı. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında rehberlik için [SIEM araçlarla tümleştirmek için kullanım Azure İzleyici](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Azure Active Directory Tümleştirme adımları denetim günlükleri

> [!NOTE]
> Bu makaledeki adımlarda çalışmadan önce gözden geçirmeniz gerekir [başlama](security-azure-log-integration-get-started.md) makalesini inceleyin ve ilgili adımları tamamlayın.

1. Bir komut istemi açın ve şu komutu çalıştırın:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Şu komutu çalıştırın: 
 
   ``azlog createazureid``

   Bu komut için Azure oturum açma bilgilerinizi ister. Komut daha sonra Azure Active Directory Hizmet sorumlusu oturum açma kullanıcı bir yönetici, ortak yönetici veya sahibi olduğu Azure abonelikleri barındıran Azure AD kiracılarıyla oluşturur. Yalnızca Konuk kullanıcı Azure AD kiracısında oturum açan kullanıcı olduğundan komut başarısız olur. Azure kimlik doğrulamasını Azure AD gerçekleştirilir. Azure günlük tümleştirmesi için hizmet sorumlusu oluşturma, Azure aboneliklerinden okumak üzere erişim verilen Azure AD kimliğini oluşturur.

3. Kiracı kimliğinizi sağlamak için aşağıdaki komutu çalıştırın Komutu çalıştırmak için Kiracı yönetici rolünün üyesi olmanız gerekir.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Örnek:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Azure Active Directory denetim günlüğü JSON dosyalarını bunları oluşturulduğunu onaylamak için aşağıdaki klasörleri denetleyin:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

Aşağıdaki video bu makalede ele alınan adımları göstermektedir:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Güvenlik bilgileri ve Olay yönetimi (SIEM) sistemi JSON dosyalarındaki bilgileri getirme ayrıntılı yönergeler için SIEM satıcınıza başvurun.

Topluluk Yardımı, aracılığıyla [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Bu forum birbirine sorular ve yanıtlar, ipuçları ve püf noktaları ile desteklemek için Azure günlük tümleştirmesi topluluğundaki kişiler sağlar. Ayrıca, Azure günlük tümleştirmesi takım bu Forumu izler ve mümkün olduğunda yardımcı olur.

Ayrıca açabileceğiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Seçin **günlük tümleştirmesi** destek isteme hizmet olarak.

## <a name="next-steps"></a>Sonraki adımlar
Azure günlük tümleştirmesi hakkında daha fazla bilgi için bkz:

* [Microsoft Azure günlük tümleştirmesi Azure günlükleri](https://www.microsoft.com/download/details.aspx?id=53324): Bu İndirme Merkezi sayfasında, ayrıntıları, sistem gereksinimleri ve yükleme yönergeleri için Azure günlük tümleştirmesi sağlar.
* [Azure günlük tümleştirmesine giriş](security-azure-log-integration-overview.md): Bu makalede Azure günlük tümleştirmesi, önemli işlevleri ve nasıl çalıştığını tanıtılmaktadır.
* [Azure günlük tümleştirmesi SSS](security-azure-log-integration-faq.md): Bu makalede, Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
* [Yeni özellikler için Azure tanılama ve Azure denetim günlükleri](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Bu blog gönderisinde size Azure denetim günlükleri tanıtır ve yardımcı olan diğer özellikleri, Azure kaynaklarınızın işlemleri Öngörüler.
