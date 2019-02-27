---
title: Azure depolama alanında tehditleri izleme
description: Azure depolama Gelişmiş tehdit hesabı etkinliğinde anomalileri algılayın ve hesabınıza erişmek için zararlı olabilecek girişimleri hakkında bilgi Koruması'nı yapılandırın.
services: storage
author: rmatchoro
ms.service: storage
ms.topic: article
ms.date: 09/24/2018
ms.author: ronmat
ms.manager: shaik
ms.openlocfilehash: 00de38aa7309179b92ff65f009f8aa780f60c284
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883700"
---
# <a name="azure-storage-advanced-threat-protection"></a>Gelişmiş tehdit koruması, azure depolama

Azure depolama Gelişmiş tehdit koruması, hesap etkinliği anormallikleri algılar ve hesabınıza erişmek için zararlı olabilecek girişimleri, size bildirir. Bu koruma katmanı tehditlerle Uzman güvenlik veya güvenlik izleme sistemlerine yönetmek zorunda kalmadan olanak tanır.

Tehditleri etkinliğinde bozukluklar ortaya çıktığında tetikleyen güvenlik uyarıları tanımlayarak çıkarılır. Bu uyarılar tümleştirin [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) şüpheli etkinlik ve tehdit araştırma ve düzeltme konusunda öneriler ayrıntılarını içerir. 

> [!NOTE]
> Azure depolama Gelişmiş tehdit koruması, şu anda yalnızca Blob hizmeti için kullanılabilir. Güvenlik Uyarıları, Azure Güvenlik Merkezi ile tümleşiktir ve abonelik yöneticilerine e-postayla gönderilir.

Azure depolama Gelişmiş tehdit koruması okuma tanılama günlükleri alır, yazma ve silme istekleri için tehdit algılama için Blob hizmeti. Gelişmiş tehdit koruması uyarılardan araştırmak için şunları yapmanız [tanılama günlüklerini yapılandırmak](storage-monitor-storage-account.md#configure-logging) günlükleri Blob hizmetinin tüm düzeylerini sağlamak.

## <a name="set-up-advanced-threat-protection-in-the-portal"></a>Gelişmiş tehdit Koruması ' Portalı'nda ayarlayın

1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com/).

2. Korumak istediğiniz Azure depolama hesabı yapılandırma sayfasına gidin. İçinde **ayarları** sayfasında **Gelişmiş tehdit koruması**.

3. İçinde **Gelişmiş tehdit koruması** yapılandırma dikey penceresi
    * Kapatma **ON** Gelişmiş *tehdit koruması*
    * Tıklayın **Kaydet** yeni veya güncelleştirilmiş Gelişmiş tehdit koruması ilkeyi kaydedin.

![Azure Gelişmiş tehdit koruması depolama üzerinde Aç](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

## <a name="explore-anomalies"></a>Anomalileri araştırın

Depolama etkinliği anormallikleri meydana geldiğinde, Şüpheli güvenlik olayı hakkında bilgi içeren bir e-posta bildirimi alırsınız. Olay ayrıntıları içerir:

* anomali yapısı
* Depolama hesabı adı
* Depolama türü
* Olay saati

E-posta, ayrıca olası nedenleri hakkında ayrıntılar içerir ve önerilen araştırıp olası tehdidi azaltmak için Eylemler.

![Gelişmiş tehdit koruması uyarı e-posta, azure depolama](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Gözden geçirin ve Azure Güvenlik Merkezi'nden kullanıcının geçerli güvenlik uyarılarınızı yönetme [güvenlik uyarıları kutucuğu](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts). Belirli bir uyarıya tıklayarak ayrıntıları ve Eylemler için geçerli tehdit araştırma ve gelecekteki tehditleri adresleme sağlar.

![Gelişmiş tehdit koruması uyarı e-posta, azure depolama](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Koruma uyarıları

Erişmek veya depolama hesapları yararlanmak için sıra dışı ve zararlı olabilecek girişimleri tarafından uyarılar oluşturulur. Bu olaylar aşağıdaki uyarılar tetikleyebilirsiniz:

* **Olağan dışı bir konumdan erişim**: Bir depolama hesabına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Örneğin, ne zaman birisi bir depolama hesabı olağan dışı bir coğrafi konumdan eriştiğini. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya Geliştirici Bakımı işlemi) algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan, dış bir saldırgan, vb.) algılar.

* **Olağan dışı veri ayıklama**: Bir depolama hesabından veri ayıklama düzeninde bir değişiklik olduğunda bu uyarı tetiklenir. Örneğin, birisi olağan dışı bir depolama hesabındaki verilerin miktarını eriştiğini durumunda. Bazı durumlarda uyarı güvenli işlemleri (Bakım etkinliği) algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (veri Sızdırma/ihlal, yetkisiz veri aktarımını) algılar.

* **Olağan dışı anonim erişim:** Bir depolama hesabına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Örneğin, birisi, anonim olarak bir depolama hesabı eriştiğini varsayalım. Bazı durumlarda, uyarı, genel okuma erişimini kullanarak yasal bir erişimini algılar. Diğer durumlarda, uyarı, bir kapsayıcı ve bloblarını genel okuma erişimini yararlanan yetkisiz erişim algılar.

* **Beklenmeyen Sil:** Bu uyarı, bir depolama hesabındaki depolama hesabının geçmiş analize dayalı bir veya daha fazla beklenmeyen silme işlemleri meydana geldiğinde tetiklenir. Örneğin, birisi gerçekleştirilen varsayalım. bir *DeleteBlob* kullanarak yeni bir uygulama işlemi ve yeni bir IP adresi. Bazı durumlarda uyarı güvenli işlemleri (yönetici farklı bir tarayıcı iş seyahat ederken kullanılan) algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (bir saldırganın verileri silme) algılar. 
 
* **Erişim izni değiştir:** Bir depolama hesabına erişim izni beklenmeyen bir değişiklik olduğunda bu uyarı tetiklenir. Örneğin, birisi için yeni bir uygulama kullanarak bir depolama hesabı ve yeni bir IP adresinden erişim izni değiştirildi varsayalım. Bazı durumlarda uyarı güvenli işlemleri (yönetici farklı bir tarayıcı iş seyahat ederken kullanılan) algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (örneğin, bir hesap erişim elde etmiştir ayrıcalıkları artırma bir saldırganın) algılar. 

* **Azure bulut hizmeti paketini karşıya yükleyin:** Bir Azure bulut hizmeti paketi beklenmeyen bir karşıya yükleme olduğunda bu uyarı tetiklenir (*.cspkg* dosyası) bir depolama hesabına. Örneğin, düşünelim bir *.cspkg* dosyanın karşıya yüklendiği yeni bir IP adresi. Bazı durumlarda uyarı güvenli işlemleri algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (örneğin, kötü amaçlı bir hizmetin bir dağıtım için paket karşıya yüklenen bir bulut hizmeti) algılar.    
   

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure depolama hesaplarındaki günlükleri](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)