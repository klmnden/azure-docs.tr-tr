---
title: Azure Depolama için Gelişmiş Tehdit Koruması
description: Azure depolama Gelişmiş tehdit hesabı etkinliğinde anomalileri algılayın ve hesabınıza erişmek için zararlı olabilecek girişimleri hakkında bilgi Koruması'nı yapılandırın.
services: storage
author: rmatchoro
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: monhaber
ms.manager: shaik
ms.openlocfilehash: 78338ece1bc70d8410bd71183a34aaf1a52f2d1b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58904126"
---
# <a name="advanced-threat-protection-for-azure-storage"></a>Azure Depolama için Gelişmiş Tehdit Koruması

Azure Depolama için Gelişmiş Tehdit Koruması, depolama hesaplarına erişmeye veya güvenlik açıklarından yararlanmaya yönelik sıra dışı, zararlı olabilecek girişimleri algılayan güvenlik zekasına sahip ek bir güvenlik katmanı sağlar. Bu koruma katmanı tehditlerle Uzman güvenlik veya güvenlik izleme sistemlerine yönetmek zorunda kalmadan olanak tanır. 

Güvenlik Uyarıları, anomalileri etkinliğinde meydana geldiğinde tetiklenir.  Bu güvenlik uyarıları ile tümleşik olduğu [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ve ayrıca şüpheli etkinlik ve öneriler tehdit araştırma ve düzeltme konusunda ayrıntılarıyla abonelik yöneticilerine e-postayla gönderilir.

> [!NOTE]
> * Gelişmiş tehdit koruması için Azure depolama şu anda yalnızca Blob Depolama alanı için kullanılabilir.
> * Fiyatlandırma ayrıntıları, 30 günlük ücretsiz deneme sürümü dahil olmak üzere bkz [Azure Güvenlik Merkezi fiyatlandırma sayfasına]( https://azure.microsoft.com/en-us/pricing/details/security-center/).
> * ATP Azure depolama özelliği için şu anda Azure devlet kurumları ve bağımsız bulut bölgelerinde kullanılabilir değil.

Azure depolama için Gelişmiş tehdit koruması, okuma, yazma ve silme isteği tehdit algılama için Blob Depolama için tanılama günlükleri alır. Gelişmiş tehdit koruması uyarılardan araştırmak için depolama analizi günlük kaydı kullanarak ilgili depolama etkinliğini görüntüleyebilirsiniz. Daha fazla bilgi için bkz. nasıl [depolama analizi günlük tutmayı yapılandırma](storage-monitor-storage-account.md#configure-logging).

## <a name="set-up-advanced-threat-protection"></a>Gelişmiş tehdit korumasını ayarlama 

### <a name="using-the-portal"></a>Portalı kullanma

1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com/).

2. Korumak istediğiniz Azure depolama hesabı yapılandırma sayfasına gidin. İçinde **ayarları** sayfasında **Gelişmiş tehdit koruması**.

3. İçinde **Gelişmiş tehdit koruması** yapılandırma dikey penceresi
    * Kapatma **ON** Gelişmiş *tehdit koruması*
    * Tıklayın **Kaydet** yeni veya güncelleştirilmiş Gelişmiş tehdit koruması ilkeyi kaydedin. (Görüntüde örneğin yalnızca fiyatlarıdır.)

![Azure Gelişmiş tehdit koruması depolama üzerinde Aç](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="using-azure-security-center"></a>Azure Güvenlik Merkezi'ni kullanma
Standart katmana abone olduğunuzda Azure Güvenlik Merkezi'nde Gelişmiş tehdit koruması depolama hesaplarınızı ayarlanır. Daha fazla bilgi için [Güvenlik Merkezi'nin standart katmanında Gelişmiş güvenlik yükseltme](https://docs.microsoft.com/azure/security-center/security-center-pricing). (Görüntüde örneğin yalnızca fiyatlarıdır.)

![ASC standart katmanda](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing.png)

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

Etkin. Gelişmiş tehdit koruması ile bir Azure depolama hesabı dağıtmak için bir Azure Resource Manager şablonu kullanın.
Daha fazla bilgi için [Gelişmiş tehdit koruması ile depolama hesabı](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/).

### <a name="using-azure-policy"></a>Azure İlkesi'ni kullanma

Belirli bir abonelik veya kaynak grubu altında depolama hesapları arasında Gelişmiş tehdit Koruması'nı etkinleştirmek için bir Azure İlkesi'ni kullanın.

1. Azure yemek **ilke - tanımlar** sayfası.

1. Arama **dağıtma Gelişmiş tehdit koruması depolama hesaplarında** ilkesi.

     ![Arama İlkesi](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)
  
1. Azure abonelik veya kaynak grubunu seçin.

    ![Abonelik veya Grup Seç](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. İlkeyi atayın.

    ![İlke tanımları sayfası](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="using-rest-api"></a>REST API kullanma
Oluşturmak, güncelleştirmek veya belirli bir depolama hesabına için Gelişmiş tehdit koruması ayarı almak için REST API komutlarını kullanın.

* [Gelişmiş tehdit koruması - oluşturma](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Gelişmiş tehdit koruması - Al](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

Aşağıdaki PowerShell cmdlet'lerini kullanın:

  * [Gelişmiş tehdit korumasını etkinleştirin](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
  * [Gelişmiş tehdit koruması](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
  * [Gelişmiş tehdit koruması devre dışı bırak](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

## <a name="explore-security-anomalies"></a>Güvenlik anormallikleri keşfedin

Depolama etkinliği anormallikleri meydana geldiğinde, Şüpheli güvenlik olayı hakkında bilgi içeren bir e-posta bildirimi alırsınız. Olay ayrıntıları içerir:

* Anomali yapısı
* Depolama hesabı adı
* Olay saati
* Depolama türü
* Olası nedenler 
* Araştırma adımları
* Düzeltme adımları


E-posta, ayrıca olası nedenleri hakkında ayrıntılar içerir ve önerilen araştırıp olası tehdidi azaltmak için Eylemler.

![Gelişmiş tehdit koruması uyarı e-posta, azure depolama](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Gözden geçirin ve Azure Güvenlik Merkezi'nden kullanıcının geçerli güvenlik uyarılarınızı yönetme [güvenlik uyarıları kutucuğu](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts). Belirli bir uyarıya tıklayarak ayrıntıları ve Eylemler için geçerli tehdit araştırma ve gelecekteki tehditleri adresleme sağlar.

![Gelişmiş tehdit koruması uyarı e-posta, azure depolama](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Koruma uyarıları

Erişmek veya depolama hesapları yararlanmak için sıra dışı ve zararlı olabilecek girişimleri tarafından uyarılar oluşturulur. Bu olaylar aşağıdaki uyarılar tetikleyebilirsiniz:

### <a name="anomalous-access-pattern-alerts"></a>Anormal erişim düzeni uyarıları

* **Olağan dışı bir konumdan erişim**: Bir depolama hesabına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Örneğin, ne zaman birisi bir depolama hesabı olağan dışı bir coğrafi konumdan eriştiğini.
Olası nedenler:
   * Bir saldırgan, depolama hesabınıza erişme
   * Bir kullanıcının gönderdiğini, depolama hesabınıza yeni bir konumdan eriştiğini
 
* **Uygulama Anomali**: Bu uyarı, olağandışı bir uygulama bu depolama hesabı eriştiğini belirtir. Olası nedenler:
   * Bir saldırgan, yeni bir uygulama kullanarak depolama hesabınıza eriştiğini.
   * Bir kullanıcının gönderdiğini, depolama hesabınıza erişmek için yeni bir uygulama/tarayıcı kullandı.

* **Anonim erişim**: Bu uyarı, bir depolama hesabına erişim deseninde değişiklik olduğunu belirtir. Örneğin, bu hesap yapıldı (yani herhangi bir kimlik doğrulaması) anonim olarak erişilebilir, beklenmeyen olduğu Bu hesapta en son erişim düzeni ile karşılaştırıldığında.
Olası nedenler:
   * Bir kapsayıcı için genel okuma erişimini saldırgan.
   * Meşru bir kullanıcı veya uygulama bir kapsayıcı için genel okuma erişimini kullandı.

### <a name="anomalous-extractupload-alerts"></a>Uyarılar anormal extract/karşıya yükleme

* **Veri Sızdırma**: Bu uyarı, olağan dışı derecede büyük bir veri miktarını bu depolama kapsayıcısındaki son etkinliklere göre çıkartılan belirtir. Olası nedenler:
   * Bir saldırganın bir kapsayıcıdan büyük miktarda veri ayıklanan. (Örneğin: veri Sızdırma/ihlal, yetkisiz veri aktarımı)
   * Olağan dışı bir veri miktarı, meşru bir kullanıcı veya uygulama bir kapsayıcı ayıklanan. (Örneğin: Bakım etkinliği)

* **Beklenmeyen silme**: Bu uyarı, bir veya daha fazla beklenmeyen silme işlemleri Bu hesapta en son etkinlik ile karşılaştırıldığında bir depolama hesabı oluştu belirtir. Olası nedenler:
   * Bir saldırganın veri depolama hesabınızdan silindi.
   * Bir kullanıcının gönderdiğini, olağan dışı bir silme işlemi gerçekleştirdi.

* **Azure bulut hizmeti paketi yükleme**: Bu uyarı, bir Azure bulut hizmeti paketi (.cspkg dosyası) bir depolama hesabına Bu hesapta en son etkinlik ile karşılaştırıldığında, olağan dışı bir şekilde yüklendiğini belirtir. Olası nedenler: 
   * Bir saldırgan, depolama hesabınızdan bir Azure bulut hizmeti'ne kadar kötü amaçlı kod dağıtmak hazırlanıyor.
   * Bir kullanıcının gönderdiğini yasal hizmet dağıtımı için hazırlama.

### <a name="suspicious-storage-activities-alerts"></a>Depolama şüpheli etkinlikleri uyarıları

* **Erişim izni Değiştir**: Bu uyarı, bu depolama kapsayıcısındaki erişim izni olağan dışı bir şekilde değiştiğini gösterir. Olası nedenler: 
   * Bir saldırgan, kendi güvenlik düzeyini düşürmek için kapsayıcı izinlerini değişti.
   * Bir kullanıcının gönderdiğini, kapsayıcı izinlerini değişti.

* **Erişim incelemesi**: Bu uyarı, bu hesapta en son etkinlik ile karşılaştırıldığında, olağan dışı bir şekilde bir depolama hesabına erişim izinlerini geçersiz inceledikten olmadığını belirtir. Olası nedenler: 
   * Bir saldırgan için gelecekteki bir saldırı keşif gerçekleştirdi.
   * Bir kullanıcının gönderdiğini depolama hesabında bakım gerçekleştirdi.

* **Veri keşfi**: Bu uyarı, BLOB veya bir depolama hesabındaki kapsayıcıları Bu hesapta en son etkinlik ile karşılaştırıldığında, olağan dışı bir şekilde numaralandırılmış olduğunu belirtir. Olası nedenler: 
   * Bir saldırgan için gelecekteki bir saldırı keşif gerçekleştirdi.
   * Veri depolama hesabında meşru bir kullanıcı veya uygulama mantığı incelediniz.






## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure depolama hesaplarındaki günlükleri](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)
