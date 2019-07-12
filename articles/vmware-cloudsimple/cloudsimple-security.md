---
title: CloudSimple Hizmetleri için güvenlik
description: Paylaşılan sorumluluk modelleri için CloudSimple hizmetlerinin güvenliğini açıklanmaktadır
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/27/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 325915aae43c905236910382f650730a6daa127a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595334"
---
# <a name="cloudsimple-security-overview"></a>CloudSimple güvenliğine genel bakış

Bu makalede nasıl güvenlik Azure VMware çözümü CloudSimple hizmet, altyapı ve veri merkezi tarafından uygulanan genel bir bakış sağlar. Veri koruma ve güvenlik, ağ güvenliği ve güvenlik açıkları ve düzeltme eklerini nasıl yönetileceğini hakkında bilgi edinin.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk

VMware CloudSimple çözümüyle Azure güvenlik için paylaşılan sorumluluk modeli kullanır. Güvenilir bulut güvenliğinin müşteriler ve Microsoft Paylaşılan sorumlulukları bir hizmet sağlayıcısı olarak sağlanır. Bu matris sorumluluk daha yüksek güvenlik sağlar ve tek hata noktaları ortadan kaldırır.

## <a name="azure-infrastructure"></a>Azure altyapı 

Azure altyapı güvenlik konuları veri merkezleri ve donanım konumunu içerir.

### <a name="datacenter-security"></a>Veri Merkezi güvenliği 

Microsoft, tasarlama, oluşturma ve Azure'ı destekleyen fiziksel tesisler çalıştırma için ayrılan tüm bir bölme vardır. Bu takım, fiziksel güvenlik durumu resim sürdürmeye yatırım. Fiziksel güvenlik hakkında daha fazla bilgi için bkz. [Azure özellikleri, şirket içi ve fiziksel güvenlik](https://docs.microsoft.com/azure/security/azure-physical-security).

### <a name="equipment-location"></a>Donanım konumu

Azure veri merkezi konumlarında özel bulutlarınızın çalıştıran çıplak bilgisayar donanım ekipmanını barındırılır. Biyometrik tabanlı iki öğeli kimlik doğrulama, ekipman bulunduğu kafesleri bulunur erişim elde etmek için gereklidir.

## <a name="dedicated-hardware"></a>Özel donanım

CloudSimple hizmetin bir parçası olarak, tüm CloudSimple müşteriler diğer Kiracı donanımdan fiziksel olarak izole edilmiş bağlı yerel disklere sahip adanmış çıplak konakları alın. Vsan'ı ile bir ESXi hiper yöneticisine her düğüm üzerinde çalışır. Düğümler, müşteri ayrılmış VMware vCenter ve NSX yönetilir. Donanım kiracılar arasında paylaşılması değil, ek bir yalıtım ve güvenlik koruma katmanı sağlar.

## <a name="data-security"></a>Veri güvenliği

Müşteriler, Denetim ve verilerine sahipliğini tutun. Veri Hizmetleri Müşteri verilerinin, müşterinin sorumluluğundadır.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Bekleyen veriler ve iç ağ içinde hareket halindeki veriler için veri koruma

Özel bulut ortamı bölgesinde, bekleyen veri için vSAN şifreleme kullanabilirsiniz. vSAN şifreleme, kendi sanal ağ ya da şirket içi VMware sertifikalı dış anahtar yönetim sunucuları (KMS) ile çalışır. Veri şifreleme anahtarları kendiniz denetler. Özel bulut içinde hareket halindeki veriler için vSphere vMotion trafiği içeren tüm VMkernel trafiği için kablo üzerinden verilerin şifrelenmesi destekler.

### <a name="data-protection-for-data-thats-required-to-move-through-public-networks"></a>Ortak ağlara taşımak için gereken veriler için veri koruması

Ortak ağlar taşıyan verileri korumak için IPSec ve SSL VPN tünelleri için özel bulutlarınızın oluşturabilirsiniz. AES 128 bayt ve 256 bayt dahil olmak üzere genel şifreleme yöntemleri desteklenir. Kimlik doğrulama, yönetici erişimi ve müşteri verilerini içeren, Taşınmakta olan veriler güvenli bir RDP SSH ve TLS 1.2 gibi standart şifreleme mekanizmaları ile şifrelenir. Hassas bilgileri taşımaları iletişim standart şifreleme mekanizmalarını kullanır.

### <a name="secure-disposal"></a>Güvenli elden çıkarma 

CloudSimple hizmetinizi süresi dolana veya sözleşme sona erdirildi, kaldırma veya verilerinizi silme sorumlu. CloudSimple silin veya ölçüde, bazı veya tüm kişisel verileri korumak için geçerli yasalar tarafından CloudSimple gereklidir dışında müşteri anlaşması'nda sağlanan tüm müşteri verileri döndürmek için sizinle birlikte cooperates. Gerekirse tüm kişisel verileri korumak CloudSimple veri arşivler ve müşteri verilerini bir daha fazla işlenmesini önlemek için uygun önlemleri uygular.

### <a name="data-location"></a>Veri konumu

Özel bulutlarınızın ayarladığınızda, burada dağıtılan Azure bölgesini seçin. Veri geçişi veya site dışında yedekleme gerçekleştirdiğiniz sürece, VMware sanal makine verilerini, fiziksel veri merkezlerinden taşınmaz. Ayrıca iş yüklerini barındırmak ve gereksinimlerinize uygun durumlarda verilerin birden çok Azure bölgeleri içinde depolamak.

Özel bulut hiper yakınsanmış düğümler yerleşik müşteri verileri, Kiracı yöneticisinin açık bir eylem olmadan konumları çapraz değil. Bu iş yükünüz yüksek oranda kullanılabilir bir şekilde uygulamak için sizin sorumluluğunuzdur.

### <a name="data-backups"></a>Veri yedekleri
CloudSimple yedeklemek veya müşteri verileri arşivleme değil. VCenter ve NSX verilerin düzenli yedekleme Yönetimi sunucularının yüksek kullanılabilirliğini sağlamak için CloudSimple gerçekleştirir. Yedeklemeden önce tüm verilerin vCenter kaynakta VMware API'leri kullanılarak şifrelenir. Şifrelenmiş veriler aktarılabilen ve bir Azure blob içinde depolanır. Yedeklemeler için şifreleme anahtarları azure'da CloudSimple sanal ağ içinde çalıştırılan son derece güvenli bir yönetilen CloudSimple kasasında depolanır.

## <a name="network-security"></a>Ağ güvenliği

Ağ güvenlik katmanları CloudSimple çözüm kullanır.

### <a name="azure-edge-security"></a>Azure uç güvenlik

CloudSimple Hizmetleri, Azure tarafından sağlanan temel ağ güvenliği üzerinde oluşturulur. Azure, savunma teknikleri algılama ve anormal giriş veya çıkış trafiği desenlerini ve dağıtılmış hizmet engelleme (DDoS) saldırıları ilişkili ağ tabanlı saldırılar için zamanında yanıt için geçerlidir. Bu güvenlik denetimi, özel bulut ortamları ve CloudSimple tarafından geliştirilen denetim düzlemi yazılımı için geçerlidir.

### <a name="segmentation"></a>Segmentlere ayırma

Özel bulut ortamınızdaki kendi özel ağlara erişimi kısıtlama mantıksal olarak ayrı katman 2 ağları CloudSimple hizmet vardır. Özel bulut ağlarınızı bir Güvenlik Duvarı'nı kullanarak daha iyi koruyabilirsiniz. Doğu-Batı ve Kuzey-Güney ağ trafiği denetim kuralları içi özel bulut trafiği, arası özel bulut trafiği, genel Internet trafiği ve şirket içi ağ trafiğini içeren tüm ağ trafiği için tanımladığınız CloudSimple Portalı'nda IPSec VPN veya Azure ExpressRoute bağlantısı üzerinden.

## <a name="vulnerability-and-patch-management"></a>Güvenlik Açığı ve düzeltme eki yönetimi 

CloudSimple düzenli güvenlik ESXi ve vCenter NSX gibi yönetilen VMware yazılımının düzeltme eki uygulama için sorumludur.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

Müşteriler, Azure hesabı (Azure Active Directory) için çok faktörlü kimlik doğrulama ve SSO olarak tercih edilen kullanarak kimlik doğrulaması yapabilir. Azure portalından kimlik bilgilerini girmeden CloudSimple portalı başlatabilirsiniz.

CloudSimple kimlik kaynağı isteğe bağlı yapılandırma özel bulut vCenter destekler. Kullanabileceğiniz bir [şirket içi kimlik kaynağı](https://docs.azure.cloudsimple.com/set-vcenter-identity), özel bulut için yeni bir kimlik kaynağı veya [Azure Active Directory](https://docs.azure.cloudsimple.com/azure-ad).

Varsayılan olarak, müşteriler, özel bulut içinde vCenter günlük işlemlerini için gerekli ayrıcalıklara verilir. Bu izin düzeyi vCenter yönetici erişimi içermez. Yönetici erişimi geçici olarak gerekiyorsa yapabilecekleriniz [, ayrıcalıkları yükseltmek](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges) yönetim görevleri tamamlarken sınırlı bir süre için.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [Azure'da CloudSimple hizmet oluşturma](quickstart-create-cloudsimple-service.md).
* Bilgi edinmek için nasıl [özel bulut oluşturma](https://docs.azure.cloudsimple.com/create-private-cloud/).
* Bilgi edinmek için nasıl [bir özel bulut ortamı yapılandırma](quickstart-create-private-cloud.md).
