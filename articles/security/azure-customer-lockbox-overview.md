---
title: Microsoft Azure müşteri kasa
description: Müşteri kasa Microsoft müşteri verilerine erişebileceği gerekebilir, bulut sağlayıcısı erişimi denetlemenizi sağlayan Microsoft Azure için teknik genel açıklama.
author: cabailey
ms.service: security
ms.topic: article
ms.author: cabailey
manager: barbkess
ms.date: 05/07/2019
ms.openlocfilehash: 468e392cd2c45d79cbb24f8d737a6e83fbcd2725
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65079278"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Microsoft Azure müşteri kasa

> [!NOTE]
> Bu özelliği kullanmak için kuruluşunuzun olmalıdır bir [Azure destek planı](https://azure.microsoft.com/support/plans/) asgari düzeyde ile **Geliştirici**.

Microsoft Azure müşteri kasa gözden geçirin ve müşteri veri erişim isteklerini onaylayabilir veya müşteriler için bir arabirim sağlar. Bir destek isteği sırasında müşteri verilerine erişmek için bir Microsoft mühendisi gereken yere durumlarda kullanılır.

Bu makale, nasıl müşteri kasa istekleri başlatılan, izlenen ve daha sonra gözden geçirmeler ve denetimler için depolanan kapsar.

Müşteri kasa artık genel kullanıma sunuldu ve sanal makine için Uzak Masaüstü erişimi için şu anda etkin.

## <a name="workflow"></a>İş akışı

Aşağıdaki adımlar, bir müşteri kasa isteği için tipik bir iş akışı özetlemektedir.

1. Birisi bir kuruluşta kendi Azure iş yükü ile ilgili bir sorun var.

2. Bu kişi sorunu giderir, ancak bunu düzeltilemiyor sonra bunlar bir destek bileti açın [Azure portalında](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). Anahtar Azure müşteri destek mühendisine atanmış.

3. Bir Azure destek mühendisi hizmet isteği inceler ve sorunu çözmek için sonraki adımlar belirler.

4. Standart araçları ve telemetri kullanarak sorunu gidermeye destek mühendisine olamaz, sonraki adım, tam zamanında (JIT) Erişim hizmetini kullanarak yükseltilmiş izinlere ilişkin istek sağlamaktır. Bu istek, özgün destek mühendisinden olabilir. Veya, Azure DevOps ekibine sorun ilerletilmiş olduğundan farklı mühendisinden olabilir.

5. Sonra erişim Azure mühendisi, Just-ın-Time istek gönderilmeden hizmet hesabı faktöründe gibi alma isteği değerlendirir:
    - Kaynak kapsamı
    - İstek sahibinin yalıtılmış bir kimlik veya multi-Factor authentication kullanarak olup
    - İzin düzeyleri
    
    JIT kurala göre bu istek bir iç Microsoft onaylayanlara onay içerebilir. Örneğin, onaylayanın müşteri destek lideri veya Yöneticisi olabilir.

6. İstek müşteri verilerine doğrudan erişim gerektirdiğinde, bir müşteri kasa isteği başlatılır. Örneğin, Uzak Masaüstü erişimi için bir müşterinin sanal makine.
    
    İstek artık bulunduğu bir **müşteri bildirim** durumu, erişim vermeden önce müşteri onayı bekleniyor.

7. Müşteri kuruluştaki yükleyen kullanıcının [sahip rolü](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-rbac-roles) için Azure aboneliği hakkında beklemedeki erişim isteği bildirmek için Microsoft, e-posta alır. Müşteri kasa istekler için bu kişi belirlenmiş onaylayanın anlamına gelir.
    
    Örnek e-posta:
    
    ![Azure müşteri kasa - e-posta bildirimi](./media/azure-customer-lockbox/customer-lockbox-email-notification.png)

8. E-posta bildirimi bir sayfaya bağlantı verilmektedir **müşteri kasa** Azure portalındaki dikey penceresinde. Bu bağlantıyı kullanarak, belirlenmiş onaylayanın Azure portalında bekleyen herhangi bir müşteri kasa için kuruluş olan istek görüntülemek için oturum açtığında:
    
    ![Azure müşteri kasa - giriş sayfası](./media/azure-customer-lockbox/customer-lockbox-landing-page.png)
    
   İstek, dört gün boyunca müşteri kuyruğunda kalır. Bu süreden sonra otomatik olarak erişim isteği sona ve Microsoft mühendisleri için erişim verilir.

9. Bekleyen istek ayrıntılarını almak için atanan onaylayan kasa istekten seçebilirsiniz **bekleyen istekler**:
    
    ![Azure müşteri kasa - bekleyen isteği görüntüleyin](./media/azure-customer-lockbox/customer-lockbox-pending-requests.png)

10. Atanan onaylayan de seçebilirsiniz **hizmet isteği kimliği** özgün kullanıcı tarafından oluşturulan destek bileti isteği görüntülemek için. Bu bilgileri Microsoft Support neden ilgilendiğini için bağlam ve bildirilen sorunu geçmişini sağlar. Örneğin:
    
    ![Azure müşteri kasa - destek bileti isteği görüntüleyin](./media/azure-customer-lockbox/customer-lockbox-support-ticket.png)

11. İstek gözden geçirdikten sonra belirlenmiş onaylayanın belirlediği **Onayla** veya **Reddet**:
    
    ![Azure müşteri kasa - select Onayla veya Reddet](./media/azure-customer-lockbox/customer-lockbox-approval.png)
    
    Sonucu olarak Seçimi:
    - **Onaylama**:  Microsoft mühendislik için erişim izni verilir. Sekiz saatte bir varsayılan süre için erişim izni verilir.
    - **Reddetme**: Microsoft mühendisi tarafından yükseltilmiş erişim isteğinin reddedildiğini ve başka hiçbir işlem yapılmaz.

Denetim amacıyla gerçekleştirilen bu iş akışında eylemleri oturum açmış olan [müşteri kasa istek günlükleri](#auditing-logs).

## <a name="auditing-logs"></a>Denetim günlükleri

Müşteri kasa günlükler, etkinlik günlüğünde depolanır. Azure portalında **etkinlik günlüklerini** müşteri kasa istekleriyle ilgili denetim bilgilerini görüntülemek için. Gibi belirli eylemler için filtreleyebilirsiniz:
- **Kasa isteği reddet**
- **Kasa isteği oluştur**
- **Kasa isteği Onayla**
- **Kasa isteği süre sonu**

Örneğin:

![Azure müşteri kasa - etkinlik günlükleri](./media/azure-customer-lockbox/customer-lockbox-activitylogs.png)

## <a name="supported-services-and-scenarios"></a>Desteklenen hizmetler ve senaryolar

Şu anda aşağıdaki hizmetler ve senaryolar Genel Müşteri kasa için kullanım aşamasındadır.

### <a name="remote-desktop-access-to-virtual-machines"></a>Sanal makine için Uzak Masaüstü erişimi

Müşteri kasa sanal makinelere Uzak Masaüstü erişimi istekleri için şu anda etkin. Aşağıdaki iş yükleri desteklenir:
- Platform (PaaS) - sürüm 1 hizmeti
- (Iaas) - Windows ve Linux (yalnızca Azure Resource Manager) hizmet olarak altyapı
- Sanal makine ölçek kümesi - Windows ve Linux

> [!NOTE]
> Iaas Klasik'ten örnekleri müşteri kasa tarafından desteklenmiyor. Iaas Klasik'ten örnekleri üzerinde çalışan iş yüklerini varsa, bunları Klasikten Resource Manager dağıtım modelleri için geçiş öneririz. Yönergeler için [Iaas kaynaklarının Klasik modelden Azure Resource Manager'a Platform destekli geçişe](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

#### <a name="detailed-audit-logs"></a>Ayrıntılı denetim günlükleri

Uzak Masaüstü erişimi gerektiren senaryolar için Microsoft mühendisi tarafından gerçekleştirilen eylemleri gözden geçirmek için Windows Olay Günlükleri'ni kullanabilirsiniz. Olay günlüklerinizde toplamak ve analiz için çalışma alanınızı veri kopyalamak için Azure Güvenlik Merkezi'ni kullanmayı düşünün. Daha fazla bilgi için [Azure Güvenlik Merkezi'nde veri toplamayı](../security-center/security-center-enable-data-collection.md).

## <a name="exclusions"></a>İstisnalar

Müşteri kasa istekleri aşağıdaki mühendislik desteği senaryolarda tetiklenen değildir:

- Bir Microsoft mühendisi, standart işletim yordamlarını dışında kalan bir etkinlik yapmanız gerekir. Örneğin, veya beklenmeyen zor veya öngörülemez senaryolarda hizmetlerinde geri yükleyin.

- Bir Microsoft mühendisi, sorun giderme işleminin parçası olarak Azure platformu erişir ve yanlışlıkla müşteri verilerine erişebilir. Örneğin, Azure ağ ekibi bir paket yakalama ağ aygıtındaki sonuçlanan sorun giderme gerçekleştirir. Ancak, Aktarımdaki ederken müşteri şifreli veriler, mühendislik veri okunamıyor.

## <a name="next-steps"></a>Sonraki adımlar

Müşteri kasa kullanılabilir olan tüm müşteriler için otomatik olarak bir [Azure destek planı](https://azure.microsoft.com/support/plans/) asgari düzeyde ile **Geliştirici**.

Bir uygun destek planına sahip olduğunuzda, herhangi bir işlem sizin tarafınızdan müşteri kasa etkinleştirmek için gereklidir. Bu eylem, kuruluşunuzdaki bir kişiden Dosyalanan bir destek bileti ilerleme gerekirse, müşteri kasa istekleri bir Microsoft mühendisi tarafından başlatılır.
