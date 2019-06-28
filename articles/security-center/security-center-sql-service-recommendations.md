---
title: Azure Güvenlik Merkezi'nde Azure veri ve depolama hizmetleri korumaya | Microsoft Docs
description: Bu belge adresleri yardımcı olacak öneriler Azure Güvenlik Merkezi'nde Azure SQL Hizmeti ve veri koruma ve güvenlik ilkelerine uygun haberdar olun.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2019
ms.author: monhaber
ms.openlocfilehash: 2ac0e4ebaafb8b0c9c79e885cecbefc5a65c1823
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275253"
---
# <a name="protect-azure-data-and-storage-services-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Azure veri ve depolama hizmetleri koruyun
Bu konu görüntülemek ve veri ve depolama kaynakları için güvenlik önerileri uygulama gösterilmiştir. Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz edilirken bu önerileri bulundu.

## <a name="view-your-data-security-information"></a>Veri güvenlik bilgilerinizi görüntüleyin

1. İçinde **kaynak güvenlik sağlığı** bölümünde **veri ve depolama kaynaklarınızı**.

   ![Veri ve depolama kaynakları](./media/security-center-monitoring/click-data.png)

    **Veri güvenliği** Sayfası'nın şirket içi veri kaynaklarına yönelik önerilerle birlikte açılır.

     ![Veri Kaynakları](./media/security-center-monitoring/sql-overview.png)

Bu sayfadan şunları yapabilirsiniz:

* Tıklayın **genel bakış** sekmesi düzeltilmesi için tüm veri kaynakları önerileri listeler. 
* Her sekmesine tıklayın ve kaynak türüne göre önerileri görüntüleyin.

    > [!NOTE]
    > Depolama şifrelemesi hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Azure depolama hesabı için şifrelemeyi etkinleştirme](security-center-enable-encryption-for-storage-account.md) bölümünü okuyun.


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Bir veri kaynağında bir öneri Düzelt

1. Kaynak sekmeleri birini bir kaynağa tıklayın. Bilgi sayfası, düzeltilmesi için öneriler listesi açılır.

    ![Kaynak bilgileri](./media/security-center-monitoring/sql-recommendations.png)

2. Bir öneriye tıklayabilir. Öneri sayfası açılır ve görüntüler **düzeltme adımları** öneriyi uygulamak için.

   ![Düzeltme adımları](./media/security-center-monitoring/remediate1.png)

3. Tıklayın **harekete**. Kaynak ayarları sayfası görüntülenir.

    ![Öneriyi etkinleştirme](./media/security-center-monitoring/remediate2.png)

4. İzleyin **düzeltme adımları** tıklatıp **Kaydet**.

## <a name="data-and-storage-recommendations"></a>Veri ve depolama önerileri

|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|Depolama hesabı|20|Güvenli aktarım depolama hesapları için etkinleştirilmiş olmalıdır|Güvenli aktarım depolama hesabınıza yalnızca güvenli bağlantı (HTTPS) gelen istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS sunucusu ve hizmeti arasında kimlik doğrulaması sağlar ve gizlice ve oturum ele geçirme gibi adam-de-ADAM, ağ katmanı saldırılardan Aktarımdaki verileri korur.|
|Redis|20|Yalnızca güvenli bağlantılar Redis Cache'inizi etkinleştirilmelidir.|Azure Cache için SSL aracılığıyla yalnızca bağlantıları Redis için etkinleştirin. Güvenli bağlantı kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.|
|SQL|15|SQL veritabanlarında saydam veri şifrelemesi etkinleştirilmelidir.|Saydam veri şifrelemesi, bekleyen veri koruma ve uyumluluk gereksinimlerini karşılamak etkinleştirin.|
|SQL|15|SQL server denetimi etkinleştirilmelidir|Azure SQL sunucuları için denetimi etkinleştirin. (Yalnızca azure SQL Hizmeti. Sanal makineler üzerinde çalışan SQL dahil değildir.)|
|Data lake analytics|5|Data Lake Analytics için tanılama günlüklerine etkinleştirilmelidir.|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|Data lake store|5|Azure Data Lake Store tanılama günlüklerine etkinleştirilmelidir.|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|SQL|30|Güvenlik açıklarını SQL veritabanlarınızda düzeltilen|SQL güvenlik açığı değerlendirmesi, veritabanınızı güvenlik açıkları için tarar ve yanlış yapılandırmalar, aşırı izinleri ve korumasız hassas veriler gibi en iyi sapmaları kullanıma sunar. Güvenlik açıkları bulundu çözme, veritabanı güvenliği stature büyük ölçüde artırabilir.|
|SQL|20|SQL server için Azure AD Yöneticisi sağlama|Azure AD kimlik doğrulamasını etkinleştirmek için SQL server için Azure AD Yöneticisi sağlayın. Azure AD kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimi veritabanı kullanıcıları ve diğer Microsoft hizmetleri sağlar.|
|Depolama hesabı|15|Güvenlik Duvarı ve sanal ağ yapılandırmaları ile depolama hesaplarının kısıtlanması gerekir|Depolama hesabı güvenlik duvarı ayarlarınızda sınırsız ağ erişimi denetleyin. Bunun yerine, yalnızca izin verilen ağları uygulamalardan depolama hesabına erişebilmesi için ağ kurallarını yapılandırın. Belirli bir Internet ya da şirket içi istemcileri bağlantılara izin vermek için belirli bir Azure sanal ağları gelen trafiği veya genel Internet IP adresi aralıkları için erişim verebilirsiniz.|
|Depolama hesabı|1|Yeni Azure Resource Manager kaynaklarına depolama hesapları geçirilmelidir|Depolama hesaplarınız için yeni Azure Resource Manager v2 gibi güvenlik geliştirmeleri sağlamak için kullanın: yönetilen kimlikleri için anahtar kasasına erişim daha güçlü erişim denetimi (RBAC), daha iyi denetim, Resource Manager tabanlı bir dağıtım ve yönetim erişim gizli ve Azure AD tabanlı kimlik doğrulaması ve etiketleri için destek ve daha kolay güvenlik yönetimi için kaynak grupları.|

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi'nde sanal makinelerinizi koruma](security-center-virtual-machine-recommendations.md)
* [Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
