---
title: "Azure Güvenlik Merkezi uyarılarını Azure günlük tümleştirme ile tümleştirme | Microsoft Docs"
description: "Bu makalede Azure günlük tümleşme Güvenlik Merkezi uyarılarını tümleştirme ile çalışmaya başlamanıza yardımcı olur."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: d2d088d3-d38d-47ff-a062-c78e0fd59226
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: terrylan
ms.openlocfilehash: d13e5b87c446e587091551b22d80fe568d5d8093
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="integrating-azure-security-center-alerts-with-azure-log-integration"></a>Azure Güvenlik Merkezi uyarılarını Azure günlük tümleştirme ile tümleştirme
Çok sayıda güvenlik işlemleri ve olay yanıtı takımlar önceliklendirmek ve güvenlik uyarıları İnceleme için başlangıç noktası olarak bir güvenlik bilgileri ve Olay yönetimi (SIEM) çözümünü kullanır. Azure günlük Tümleştirmesi ile Azure Güvenlik Merkezi uyarılarını SIEM çözümünüzle birlikte tümleştirebilirsiniz.

Azure günlük tümleştirme şu anda HP ArcSight, Splunk ve IBM QRadar destekler.

## <a name="what-logs-can-i-integrate"></a>Hangi günlükleri ı tümleştirebilir miyim?
Azure her hizmet için ayrıntılı günlük kaydını üretir. Bu günlükler olarak kategorilere ayrılır:

* **Denetim/Yönetim günlüklerini** Azure Kaynak Yöneticisi oluşturma, güncelleştirme ve silme işlemleri görünürlük sağlar. Bu denetim düzlemi olayları Azure etkinlik günlüklerinde çıkmış
* **Veri düzlemi günlüklerini** bir Azure kaynağı kullanılırken tetiklenen olayları görünürlük sağlar. Güvenlik olay bilgileri Olay Görüntüleyicisi'nin güvenlik kanaldan alabileceğiniz Windows olay günlüğü örneğidir. (Bir sanal makine veya bir Azure hizmeti tarafından oluşturulan) veri düzlemi olaylar tarafından Azure tanılama günlükleri çıkmış.

Azure günlük tümleştirme şu anda tümleştirmesini destekler:

* Azure VM günlükleri
* Azure denetim günlükleri
* Azure Güvenlik Merkezi uyarıları

## <a name="install-azure-log-integration"></a>Azure günlük tümleştirme yükleyin
Karşıdan [Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324).

Azure günlük tümleştirme hizmeti yüklü olduğu makinede telemetri verileri toplar.  Toplanan telemetri verileri şöyledir:

* Azure günlük tümleştirme yürütülmesi sırasında oluşan özel durumlar
* Sorgular ve işlenen olayların sayısı hakkında ölçümleri
* Komut satırı seçenekleri hakkında hangi Azlog.exe kullanılan istatistikleri

> [!NOTE]
> Bu seçenek kaldırarak telemetri veri koleksiyonunu devre dışı bırakabilirsiniz.
>
>

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Azure denetim günlükleri ve Güvenlik Merkezi uyarıları tümleştirme
1. Bir komut istemi açın ve **cd** içine **c:\Program Files\Microsoft Azure günlük tümleştirme**.
2. Çalıştırma **azlog createazureid** komutu oluşturmak için bir [Azure Active Directory hizmet asıl](../active-directory/active-directory-application-objects.md) Azure aboneliklerini konak Azure Active Directory (AD) kiracılar içinde.

    Azure oturum açma için istenir.

   > [!NOTE]
   > Abonelik sahibi veya aboneliğin ortak yöneticisi olmanız gerekir.
   >
   >

    Azure kimlik doğrulaması Azure AD üzerinden yapılır.  Azure günlük tümleştirmesi için bir hizmet sorumlusu oluşturma Azure aboneliklerinden okuma erişimi verilir Azure AD kimlik oluşturur.
3. Çalıştırma **azlog yetkilendirmek <SubscriptionID>**  2. adımda oluşturduğunuz hizmet asıl abonelik okuyucusu erişimi atamak için komutu. Belirtmediyseniz bir **Subscriptionıd**, sonra da hizmet sorumlusu tüm abonelikleri olan erişim okuyucu rolüne atanır.

   > [!NOTE]
   > Çalıştırırsanız uyarıları görebilirsiniz **yetkilendirmek** hemen sonra komut **createazureid** komutu. Azure AD hesabının oluşturulduğu ve hesap kullanılabilir olduğunda arasındaki bazı gecikme süresi yok. Yaklaşık 10 saniye çalıştırdıktan sonra beklerseniz **createazureid** çalıştırmak için komut **yetkilendirmek** Bu uyarılar görülmemelidir sonra komutu.
   >
   >
4. Denetim günlüğü JSON dosyalarını olduğunu onaylamak için aşağıdaki klasörler denetleyin:

   * **c:\Users\azlog\AzureResourceManagerJson**
   * **c:\Users\azlog\AzureResourceManagerJsonLD**
5. Güvenlik Merkezi uyarılarını bunları bulunduğunu doğrulamak için aşağıdaki klasörler denetleyin:

   * **c:\Users\azlog\ AzureSecurityCenterJson**
   * **c:\Users\azlog\AzureSecurityCenterJsonLD**
6. SIEM dosya iletici bağlayıcı uygun klasöre yapılandırın. Yordamı kullanmakta olduğunuz SIEM göre değişir.

## <a name="next-steps"></a>Sonraki adımlar
Azure etkinlik günlükleri ve özellik tanımları hakkında daha fazla bilgi için bkz:

* [Resource Manager ile işlemleri denetleme](../azure-resource-manager/resource-group-audit.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.
