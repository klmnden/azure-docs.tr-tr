---
title: Azure Stack güvenlik denetimleri anlama
description: Hizmet Yöneticisi olarak Azure Stack için uygulanan güvenlik denetimleri hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2018
ms.author: patricka
ms.openlocfilehash: 8b478c1ba60df679d69d5fced660836c16079e6a
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727099"
---
# <a name="azure-stack-infrastructure-security-posture"></a>Azure Stack altyapısını güvenlik durumu

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Güvenlik hususlarının yanı sıra uyumluluk düzenlemelerini kullanarak karma Bulutlar için ana sürücüleri arasındadır. Azure Stack, bu senaryolar için tasarlanmıştır. Bu makalede, Azure Stack için güvenlik denetimleri açıklanmaktadır.

İki güvenlik duruşunu katmanları Azure Stack'te bir arada. Donanım bileşenleri kadar Azure Resource Manager içeren Azure Stack altyapısının ilk katmanıdır. İlk katman, yönetici ve Kiracı portalı içerir. Yönetilen kiracılar tarafından oluşturulan ve dağıtılan iş yüklerinin ikinci katman oluşur. İkinci Katman sanal makine ve uygulama hizmetleri web siteleri gibi öğeleri içerir.

## <a name="security-approach"></a>Güvenlik yaklaşımı

Azure Stack güvenlik duruşu modern tehditlere karşı korumak için tasarlanmıştır ve gereksinimlerden en önemli uyumluluk standartları karşılamak amacıyla oluşturulmuştur. Sonuç olarak, Azure Stack altyapısının güvenlik duruşunu iki yapı taşları üzerinde oluşturulmuştur:

 - **İhlal varsayımı**  
Sistem zaten ihlal varsayımına başlayarak, odaklanmak *algılama ve İhlallerin etkisini sınırlandırma* karşı saldırıları önlemek yalnızca çalışıyor. 
 - **Varsayılan olarak sağlamlaştırılmış**  
Altyapı iyi tanımlanmış donanım ve yazılım, Azure Stack üzerinde çalıştığından sunucu *sağlar, yapılandırır ve güvenlik özellikleri doğrular* varsayılan olarak.

Azure Stack tümleşik bir sistem halinde teslim edildiğinden, Azure Stack altyapısının güvenlik duruşunu Microsoft tarafından tanımlanır. Tıpkı Azure'da kiracılar Kiracı iş yüklerini güvenlik duruşunu sorumludur. Bu belge, Azure Stack altyapısının güvenlik açısından duruşunu temel bilgi sağlar.

## <a name="data-at-rest-encryption"></a>Veri bekleyen şifreleme
Tüm Azure Stack altyapı ve Kiracı verileri, Bitlocker kullanılarak, bekleme sırasında şifrelenir. Bu şifreleme, fiziksel kaybedilmesi veya çalınması Azure Stack depolama bileşenleri karşı korur. Daha fazla bilgi için [verileri Azure Stack'te bekleyen şifreleme](azure-stack-security-bitlocker.md).

## <a name="data-in-transit-encryption"></a>Veri aktarım sırasında şifreleme
Azure Stack altyapı bileşenleri, TLS 1.2 ile şifrelenmiş kanalları kullanarak iletişim kurar. Şifreleme sertifikaları Self altyapısı tarafından yönetilir. 

Tüm dış altyapı uç noktaları, REST uç noktalarını ya da Azure Stack portalı gibi güvenli iletişim için TLS 1.2 desteği. Şifreleme sertifikaları, bir üçüncü taraf veya Kurumsal Sertifika yetkilisi için bu Uç noktalara sağlanmalıdır. 

Otomatik olarak imzalanan sertifikalar bu dış uç noktaları için kullanılabilir, ancak Microsoft bunları kullanılmamasını önerir. 

## <a name="secret-management"></a>Gizli dizi Yönetimi
Azure Stack altyapısının parolalar gibi gizli dizileri birçok işlev için kullanır. 24 saatte bir döndürme Group-Managed hizmet hesapları, çünkü çoğu, sık sık otomatik olarak döndürülür.

Group-Managed hizmet hesapları, bir komut dosyası ayrıcalıklı uç noktasını el ile döndürülebilen olmayan diğer gizli dizileri.

## <a name="code-integrity"></a>Kod bütünlüğü
Azure Stack yapar en son Windows Server 2016 ' kullanan güvenlik özellikleri. Windows Defender cihaz uygulama beyaz listesini sağlayan ve sağlar, yalnızca Azure Stack altyapısının içinde kod çalıştırır yetkili koruyucusu, bunlardan biridir. 

Yetkili kod, Microsoft veya OEM iş ortağı tarafından imzalanır. İmzalı yetkili kod Microsoft tarafından tanımlanan bir ilkede belirtilen izin verilen yazılım listesi dahil edilir. Diğer bir deyişle, yalnızca Azure Stack altyapısının içinde çalıştırmak için onaylanmış yazılımlar çalıştırılabilir. Yetkisiz kod yürütme girişimi engellenir ve bir denetim oluşturulur.

Device Guard İlkesi de üçüncü taraf aracılarını veya yazılımını Azure Stack altyapısının çalışmasını engeller.

## <a name="credential-guard"></a>Credential Guard
Başka bir Windows Server 2016 güvenlik Azure Stack'te Azure Stack altyapısını kimlik Pass--Hash ve Pass--Ticket saldırılara karşı korumak için kullanılan Windows Defender Credential Guard özelliğidir.

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma
Azure Stack'te (Hyper-V konakları ve sanal makineler) her bir bileşeni, Windows Defender virüsten koruma ile korunur.

Birbirine bağlı senaryolarda, virüsten koruma tanımı ve motor güncelleştirmelerini günde birden çok kez uygulanır. Bağlantısı kesilmiş senaryolarda, kötü amaçlı yazılımdan koruma güncelleştirmeleri aylık Azure Stack güncelleştirmelerin bir parçası uygulanır. Daha fazla bilgi için [Windows Defender virüsten koruma Azure Stack'te güncelleştirme](azure-stack-security-av.md).

## <a name="constrained-administration-model"></a>Kısıtlı yönetim modeli
Azure Stack yönetiminde, her biri belirli bir amaca sahip üç giriş noktaları kullanılarak denetlenir: 
1. [Yönetici portalı](azure-stack-manage-portals.md) noktası tıklama deneyimindeki için günlük yönetimi işlemleri sağlar.
2. Azure Resource Manager PowerShell ve Azure CLI tarafından kullanılan bir REST API aracılığıyla Yönetici portalı tüm yönetim işlemlerini gösterir. 
3. Belirli düşük düzey işlemler, örneğin veri merkezi tümleştirmesi veya senaryoları desteklemek için Azure Stack adlı bir PowerShell uç noktasını kullanıma sunar [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md). Bu uç nokta yalnızca izin verilenler listesinde cmdlet'ler kümesi sunan ve yoğun olarak denetlenir.

## <a name="network-controls"></a>Ağ denetimleri
Azure Stack altyapısının, birden çok ağ erişim denetimi listesi (ACL) katmanlarla sunulur. ACL, altyapı bileşenleri için yetkisiz erişimi önlemek ve kendi işlevi için gerekli olan yollar için altyapı iletişimi sınırlayabilirsiniz. 

Ağ ACL'leri, üç katmanda uygulanır:
1.  Raf üstü anahtarları
2.  Yazılım tanımlı ağ
3.  Konak ve sanal makine işletim sistemi güvenlik duvarları

## <a name="regulatory-compliance"></a>Mevzuata uyumluluk

Azure Stack biçimsel bir değerlendirme üçüncü taraf bağımsız bir denetleme firmasının geçti. Sonuç olarak, Azure Stack altyapısının çeşitli önemli uyumluluk standartları ilgili denetimleri nasıl karşıladığını üzerinde belgeleri kullanılabilir. Belgeler, çeşitli personeli ve işlem ilgili denetimleri de dahil olmak üzere standartları nedeniyle Azure Stack bir sertifika değil. Bunun yerine, müşteriler kendi sertifika alma süreci hızlı giriş yapmak için bu belgeleri kullanabilir.

Değerlendirmeler, aşağıdaki standartları şunlardır:

- [PCI-DSS](https://www.pcisecuritystandards.org/pci_security/) ödeme kartı sektörüyle yöneliktir.
- [CSA bulut denetim matrisi](https://cloudsecurityalliance.org/group/cloud-controls-matrix/#_overview) FedRAMP Orta, ISO27001, HIPAA, HITRUST, ITAR, NIST SP800-53 ve diğerleri dahil olmak üzere birden çok standartları kapsamlı bir eşlemedir.
- [FedRAMP yüksek](https://www.fedramp.gov/fedramp-releases-high-baseline/) kamu müşterileri için.

Uyumluluk belgeleri bulunabilir [Microsoft hizmet güveni portalı](https://servicetrust.microsoft.com/ViewPage/Blueprint). Uyumluluk kılavuzları, korumalı bir kaynaktır ve Azure bulut hizmeti kimlik bilgilerinizle oturum açmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure stack'teki dizilerinizin döndürme hakkında bilgi edinin](azure-stack-rotate-secrets.md)
- [PCI-DSS ve CSA-CCM belgeleri için Azure Stack](https://servicetrust.microsoft.com/ViewPage/TrustDocuments)
- [Azure Stack için Savunma Bakanlığı ve NIST belgeleri](https://servicetrust.microsoft.com/ViewPage/Blueprint)
