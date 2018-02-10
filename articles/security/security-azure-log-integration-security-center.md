---
title: "Güvenlik Merkezi ile Azure günlük tümleştirme | Microsoft Docs"
description: "Azure Güvenlik Merkezi uyarılarını ile günlük tümleştirme çalışma alma hakkında bilgi"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/29/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 9acce21d544a43adcd0c0983771c02c6bb39caec
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-get-your-security-center-alerts-in-azure-log-integration"></a>Güvenlik Merkezi uyarılarını Azure günlük tümleştirme alma

Bu makalede Azure Güvenlik Merkezi tarafından üretilen güvenlik uyarısı bilgileri çıkarmak Azure günlük tümleştirme hizmeti etkinleştirmek için gereken adımları sağlar. ' Ndaki adımları tamamladınız gerekir [başlama](security-azure-log-integration-get-started.md) bu makaledeki adımları gerçekleştirmeden önce makalesi.

## <a name="detailed-steps"></a>Ayrıntılı adımlar

Aşağıdaki adımlar gerekli Azure Active Directory Hizmet sorumlusu oluşturmak ve abonelik için Okuma izinleri hizmet ilkesi atayabilirsiniz:
1. Komut istemi açın ve gidin **c:\Program Files\Microsoft Azure günlük tümleştirme**
2. Komutu çalıştırın``azlog createazureid``

    Bu komut için Azure oturum açma bilgilerinizi ister. Komut sonra oluşturur bir [Azure Active Directory hizmet asıl](../active-directory/develop/active-directory-application-objects.md) oturum açan kullanıcının olduğu bir yönetici, bir ortak yönetici veya bir sahip Azure abonelikleri konak Azure AD kiracıları içinde. Oturum açan kullanıcının yalnızca Konuk kullanıcı olarak Azure AD Kiracı ise komut başarısız olur. Azure Active Directory (AD ile) Azure kimlik doğrulaması yapılır. Azlog tümleştirmesi için bir hizmet sorumlusu oluşturma Azure aboneliklerinden okuma erişimi verilir Azure AD kimlik oluşturur.

3. Ardından, yeni oluşturduğunuz hizmete asıl abonelikte okuyucu erişim atayan bir komut çalışır. Bir Subscriptionıd belirtmezseniz, komut herhangi bir erişim sahip olan tüm abonelikler için hizmet asıl okuyucu rolüne atamak dener. </br></br>
``azlog authorize <SubscriptionID>`` </br> Örneğin </br>
``azlog authorize 0ee55555-0bc4-4a32-a4e8-c29980000000``

    >[!NOTE]
    Authorize komutu hemen sonra çalıştırırsanız uyarıları görebilirsiniz ```createazureid``` komutu. Azure AD hesabının oluşturulduğu ve hesap kullanılabilir olduğunda arasındaki bazı gecikme süresi yok. Yaklaşık 60 saniye çalıştırdıktan sonra beklerseniz ```createazureid``` Bu uyarılar görülmemelidir sonra authorize komutu çalıştırmak için komutu.

4. Denetim günlüğü JSON dosyalarını olduğunu onaylamak için aşağıdaki klasörler denetleyin:
 * **c:\Users\azlog\AzureResourceManagerJson**
 * **c:\Users\azlog\AzureResourceManagerJsonLD** </br></br>
5. Güvenlik Merkezi uyarılarını bunları bulunduğunu doğrulamak için aşağıdaki klasörler denetleyin:</br></br>
 * **c:\Users\azlog\AzureSecurityCenterJson**
 * **c:\Users\azlog\AzureSecurityCenterJsonLD** </br></br>

Yükleme ve yapılandırma sırasında herhangi bir sorunla çalıştırırsanız, lütfen açık bir [destek isteği](/azure-supportability/how-to-create-azure-support-request.md)seçin **günlük tümleştirme** destek isteyen hizmet olarak.

## <a name="next-steps"></a>Sonraki adımlar
Azure günlük tümleştirmesi hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Microsoft Azure günlük tümleştirme Azure günlükleri için](https://www.microsoft.com/download/details.aspx?id=53324) – merkezi ayrıntılı bilgi için sistem gereksinimleri, yükleyip yönergeler Azure günlük tümleştirme.
* [Azure günlük tümleştirme giriş](security-azure-log-integration-overview.md) – Azure günlük tümleştirme, önemli işlevleri ve nasıl çalıştığı için bu belge size tanıtır.
* [Ortak yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – bu blog gönderisine Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesini yapılandırma gösterilmektedir.
* [Azure günlük sık sorulan sorular (SSS) tümleştirme](security-azure-log-integration-faq.md) -bu SSS Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
* [Azure tanılama ve Azure denetim günlükleri için yeni özellikler](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – bu blog gönderisine Azure denetim günlükleri tanıtır ve yardımcı diğer özellikleri Azure kaynaklarınızı işlemleri alın.
