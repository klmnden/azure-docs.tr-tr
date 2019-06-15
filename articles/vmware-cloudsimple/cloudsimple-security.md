---
title: CloudSimple Hizmetleri için güvenlik
description: Paylaşılan sorumluluk modelleri için CloudSimple hizmetlerinin güvenliğini açıklanmaktadır
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/27/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f5f3fe32e03a9a2bb0186854a83917f8918c6647
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358131"
---
# <a name="cloudsimple-security-overview"></a>CloudSimple güvenliğine genel bakış

Bu makalede güvenlik CloudSimple hizmet, altyapı ve veri merkezine nasıl uygulandığını genel bir bakış sağlar.  Veri koruma ve güvenlik, ağ güvenliği ve güvenlik açıkları ve düzeltme eklerini nasıl yönetileceğini hakkında bilgi edinin.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk

VMware CloudSimple çözümüyle Azure güvenlik için paylaşılan sorumluluk modeli kullanır. Güvenilir bulut güvenliğinin müşteriler ve Microsoft Paylaşılan sorumlulukları bir hizmet sağlayıcısı olarak sağlanır. Bu matris sorumluluk daha yüksek güvenlik sağlar ve tek hata noktaları ortadan kaldırır. 

## <a name="azure-infrastructure"></a>Azure altyapı 

Azure altyapı güvenlik konuları veri merkezleri ve donanım konumunu içerir. 

### <a name="datacenter-security"></a>Veri Merkezi güvenliği 

Microsoft, tasarlama, oluşturma ve Azure'ı destekleyen fiziksel tesisler çalıştırma için ayrılan tüm bir bölme vardır. Bu takım, fiziksel güvenlik durumu resim sürdürmeye yatırım. Fiziksel güvenlik hakkında daha fazla bilgi için bkz: [Azure özellikleri, şirket içi ve fiziksel güvenlik](https://docs.microsoft.com/azure/security/azure-physical-security).

### <a name="equipment-location"></a>Donanım konumu

Azure veri merkezi konumlarında özel Bulutlarınızın çalıştıran çıplak metal donanım ekipman barındırılır.  Bu donanım olduğu kafesleri bulunur erişim elde etmek için biyometrik tabanlı iki öğeli kimlik doğrulamayı gerektirir.

## <a name="dedicated-hardware"></a>Özel donanım

CloudSimple hizmetin bir parçası olarak, tüm CloudSimple müşteriler diğer Kiracı donanımdan fiziksel olarak izole edilmiş bağlı yerel disklere sahip adanmış çıplak metal konakları alın. Vsan'ı ile bir ESXi hiper yöneticisine her düğüm üzerinde çalışır. Düğümler, adanmış müşteri VMware vCenter ve NSX yönetilir. Donanım kiracılar arasında paylaşılması değil, ek bir yalıtım ve güvenlik koruma katmanı sağlar.

## <a name="data-security"></a>Veri güvenliği

Müşteriler, Denetim ve verilerine sahipliğini tutun. Veri Hizmetleri Müşteri verilerinin, müşterinin sorumluluğundadır.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Bekleyen veriler ve iç ağ içinde hareket halindeki veriler için veri koruma

Özel bulut ortamı bölgesinde, bekleyen veri için vSAN şifreleme kullanabilirsiniz. vSAN şifreleme, kendi sanal ağ ya da şirket içi VMware dış anahtar yönetim sunucuları (KMS) sertifikası ile çalışır.  Veri şifreleme anahtarları kendiniz denetler. Özel bulut içinde hareket halindeki veriler için vSphere (VMotion'ı trafiği dahil) tüm vmkernel trafiği için kablo üzerinden verilerin şifrelenmesi destekler.

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Ortak ağlara taşımak için gereken veriler için veri koruma

Ortak ağlar taşıyan verileri korumak için IPSec ve SSL VPN tünelleri, özel Bulutlar için oluşturabilirsiniz. AES 128 bayt ve 256 bayt dahil olmak üzere genel şifreleme yöntemleri desteklenir. (Kimlik doğrulama, yönetici erişimi ve müşteri verilerini dahil) Aktarımdaki verileri standart şifreleme mekanizmaları (SSH, TLS 1.2 ve güvenli RDP) ile şifrelenir. Hassas bilgileri taşımaları iletişim standart şifreleme mekanizmalarını kullanır.

### <a name="secure-disposal"></a>Güvenli elden çıkarma 

CloudSimple hizmetinizi süresi dolana veya sözleşme sona erdirildi, kaldırma veya verilerinizi silme sorumlu olursunuz. CloudSimple silin veya ölçüde, bazı veya tüm kişisel verileri korumak için geçerli yasalar tarafından CloudSimple gereklidir dışında müşteri anlaşması'nda sağlanan tüm müşteri verileri döndürmek için iş Birliği yapacaktır. Gerekirse tüm kişisel verileri korumak CloudSimple verileri arşivlemek ve müşteri verilerini bir daha fazla işlenmesini önlemek için uygun önlemleri uygulayın.

### <a name="data-location"></a>Veri Konumu

Özel Bulutlarınızın ayarlanırken, bunlar dağıtılacağı Azure bölgesi seçin. Veri geçişi veya site dışında yedekleme gerçekleştirdiğiniz sürece, VMware sanal makine verilerini, fiziksel veri merkezlerinden taşınmaz. Ayrıca, iş yüklerini barındırmak ve gereksinimlerinize uygun durumlarda verilerin birden çok Azure bölgeleri içinde depolamak.

Özel bulut hiper yakınsama düğümleri içinde yerleşik olan müşteri verileri, Kiracı yöneticisinin açık bir eylem olmadan konumları çapraz değil. Bu iş yükünüz yüksek oranda kullanılabilir bir şekilde uygulamak için sizin sorumluluğunuzdur.

### <a name="data-backups"></a>Veri yedekleri
CloudSimple yedeklemek veya müşteri verileri arşivleme değil. VCenter ve NSX verilerin düzenli yedekleme Yönetimi sunucularının yüksek kullanılabilirliğini sağlamak için CloudSimple gerçekleştirir. Yedeklemeden önce vCenter kaynakta VMware API'lerini kullanarak tüm veriler şifrelenir. Şifrelenmiş veriler taşındığını ve Azure BLOB içinde. Yedeklemeler için şifreleme anahtarları azure'da CloudSimple sanal ağda çalışıyor yüksek güvenlikli CloudSimple yönetilen kasasında depolanır.

## <a name="network-security"></a>Ağ Güvenliği

Ağ güvenlik katmanları CloudSimple çözüm kullanır.

### <a name="azure-edge-security"></a>Azure uç güvenlik

CloudSimple Hizmetleri, Azure tarafından sağlanan temel ağ güvenliği üzerinde oluşturulur. Azure, savunma teknikleri algılama ve anormal giriş veya çıkış trafiği desenlerini ve dağıtılmış hizmet engelleme (DDoS) saldırıları ilişkili ağ tabanlı saldırılar için zamanında yanıt için geçerlidir. Bu güvenlik denetimi, özel bulut ortamları ve CloudSimple tarafından geliştirilen denetim düzlemi yazılımı için geçerlidir.

### <a name="segmentation"></a>Segmentlere ayırma

Özel bulut ortamınızdaki kendi özel ağlara erişimi kısıtlama mantıksal olarak ayrı katman 2 ağları CloudSimple hizmet vardır. Bir Güvenlik Duvarı'nı kullanarak, özel bulut ağları daha iyi koruyabilirsiniz. CloudSimple Portal içi özel bulut trafiği, arası özel bulut trafik, İnternet'e Genel trafiği de dahil olmak üzere, tüm ağ trafiğini ENİ ve NS ağ trafiği denetimleri kuralları tanımlayabilir ve ağ trafiği şirket içi IPSec VPN üzerinden izin verir veya ExpressRoute bağlantısı.

## <a name="vulnerability-and-patch-management"></a>Güvenlik Açığı ve düzeltme eki yönetimi 

Yönetilen VMware yazılım (ESXi, vCenter ve NSX) düzenli güvenlik düzeltme eki uygulama için CloudSimple sorumludur.

## <a name="identity-and-access-management"></a>Kimlik ve Erişim Denetimi

Müşteriler için çok faktörlü kimlik doğrulama ve SSO olarak tercih edilen kullanarak kendi Azure hesabı (Azure AD) kimlik doğrulaması yapabilir. Azure portalından kimlik bilgilerini girmeden CloudSimple portalı başlatabilirsiniz.

CloudSimple kimlik kaynağı isteğe bağlı yapılandırma özel bulut vCenter destekler. Kullanabileceğiniz bir [şirket içi kimlik kaynağı](https://docs.azure.cloudsimple.com/set-vcenter-identity), özel bulut için yeni bir kimlik kaynağı veya [Azure AD'ye](https://docs.azure.cloudsimple.com/azure-ad).

Varsayılan olarak, müşteriler günlük işlemlerini vCenter içinde özel bulut için gereken ayrıcalıklar verilir. Bu izin düzeyi vCenter yönetici erişimi içermez. Yönetici erişimi geçici olarak gerekiyorsa yapabilecekleriniz [, ayrıcalıkları yükseltmek](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges) yönetim görevleri tamamlarken sınırlı bir süre için.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [Azure'da CloudSimple hizmeti oluşturma](quickstart-create-cloudsimple-service.md)
* Bilgi edinmek için nasıl [özel bulut oluşturma](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Bilgi edinmek için nasıl [bir özel bulut ortamı yapılandırma](quickstart-create-private-cloud.md)
