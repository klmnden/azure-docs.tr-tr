---
title: Azure Stack güvenlik denetimleri anlama | Microsoft Docs
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
ms.date: 09/12/2018
ms.author: patricka
ms.openlocfilehash: 048a2e8204b3b8776b5a7e0e425dbc5fdf3d504c
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719027"
---
# <a name="azure-stack-infrastructure-security-posture"></a>Azure Stack altyapısını güvenlik durumu

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Güvenlik hususlarının yanı sıra uyumluluk düzenlemelerini kullanarak karma Bulutlar için ana sürücüleri arasındadır. Azure Stack bu senaryolar için tasarlanmıştır ve Azure Stack zaten yerinde denetimleri anlamak önemlidir.

İki güvenlik duruşunu katmanları Azure Stack'te bir arada. Donanım bileşenleri kadar Azure Resource Manager içeren Azure Stack altyapısının ilk katmanıdır. İlk katman, yönetici ve Kiracı portalı içerir. Yönetilen kiracılar tarafından oluşturulan ve dağıtılan iş yüklerinin ikinci katman oluşur. İkinci Katman sanal makine ve uygulama hizmetleri web siteleri gibi öğeleri içerir.

## <a name="security-approach"></a>Güvenlik yaklaşımı

Azure Stack güvenlik duruşu modern tehditlere karşı korumak için tasarlanmıştır ve gereksinimlerden en önemli uyumluluk standartları karşılamak amacıyla oluşturulmuştur. Sonuç olarak, Azure Stack altyapısının güvenlik duruşunu iki yapı taşları üzerinde oluşturulmuştur:

 - **İhlal varsayımı**  
Sistem zaten ihlal varsayımına başlayarak, odaklanmak *algılama ve İhlallerin etkisini sınırlandırma* karşı saldırıları önlemek yalnızca çalışıyor. 
 - **Varsayılan olarak sağlamlaştırılmış**  
Altyapı iyi tanımlanmış donanım ve yazılım, Azure Stack üzerinde çalıştığından sunucu *sağlar, yapılandırır ve güvenlik özellikleri doğrular* varsayılan olarak.

Azure Stack tümleşik bir sistem halinde teslim edildiğinden, Azure Stack altyapısının güvenlik duruşunu Microsoft tarafından tanımlanır. Tıpkı Azure'da kiracılar Kiracı iş yüklerini güvenlik duruşunu sorumludur. Bu belge, Azure Stack altyapısının güvenlik açısından duruşunu temel bilgi sağlar.

## <a name="data-at-rest-encryption"></a>Veri bekleyen şifreleme
Tüm Azure Stack altyapı ve Kiracı verileri, Bitlocker kullanılarak, bekleme sırasında şifrelenir. Bu şifreleme, fiziksel kaybedilmesi veya çalınması Azure Stack depolama bileşenleri karşı korur. 

## <a name="data-in-transit-encryption"></a>Veri aktarım sırasında şifreleme
Azure Stack altyapı bileşenleri, TLS 1.2 ile şifrelenmiş kanalları kullanarak iletişim kurar. Şifreleme sertifikaları Self altyapısı tarafından yönetilir. 

Tüm dış altyapı uç noktaları, REST uç noktalarını ya da Azure Stack portalı gibi güvenli iletişim için TLS 1.2 desteği. Şifreleme sertifikaları, bir üçüncü taraf veya Kurumsal Sertifika yetkilisi için bu Uç noktalara sağlanmalıdır. 

Otomatik olarak imzalanan sertifikalar bu dış uç noktaları için kullanılabilir, ancak Microsoft bunları kullanılmamasını önerir. 

## <a name="secret-management"></a>Gizli dizi Yönetimi
Azure Stack altyapısının parolalar gibi gizli dizileri birçok işlev için kullanır. 24 saatte bir döndürme Grup yönetilen hizmet hesapları, çünkü çoğu, sık sık otomatik olarak döndürülür.

Grup yönetilen hizmet hesapları, bir komut dosyası ayrıcalıklı uç noktasını el ile döndürülebilen olmayan diğer gizli dizileri.

## <a name="code-integrity"></a>Kod bütünlüğü
Azure Stack yapar en son Windows Server 2016 ' kullanan güvenlik özellikleri. Windows Defender cihaz uygulama beyaz listesini sağlayan ve sağlar, yalnızca Azure Stack altyapısının içinde kod çalıştırır yetkili koruyucusu, bunlardan biridir. 

Yetkili kod Microsoft veya OEM iş ortağı tarafından imzalanır ve Microsoft tarafından tanımlanan bir ilkede belirtilen izin verilen yazılım listesine eklenir. Diğer bir deyişle, yalnızca Azure Stack altyapısının içinde çalıştırmak için onaylanmış yazılımlar çalıştırılabilir. Yetkisiz kod yürütme girişimi engellenir ve bir denetim oluşturulur.

Device Guard İlkesi de üçüncü taraf aracılarını veya yazılımını Azure Stack altyapısının çalışmasını engeller.

## <a name="credential-guard"></a>Credential Guard
Başka bir Windows Server 2016 güvenlik Azure Stack'te Azure Stack altyapısını kimlik Pass--Hash ve Pass--Ticket saldırılara karşı korumak için kullanılan Windows Defender Credential Guard özelliğidir.

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma
Azure Stack'te (Hyper-V konakları ve sanal makineler) her bir bileşeni, Windows Defender virüsten koruma ile korunur.

Birbirine bağlı senaryolarda, virüsten koruma tanımı ve motor güncelleştirmelerini günde birden çok kez uygulanır. Bağlantısı kesilmiş senaryolarda, kötü amaçlı yazılımdan koruma güncelleştirmeleri aylık Azure Stack güncelleştirmelerin bir parçası uygulanır. Bkz: [Windows Defender virüsten koruma Azure Stack'te güncelleştirme](azure-stack-security-av.md) daha fazla bilgi için.

## <a name="constrained-administration-model"></a>Kısıtlı yönetim modeli
Azure Stack yönetiminde, her biri belirli bir amaca sahip üç giriş noktaları kullanılarak denetlenir: 
1. [Yönetici portalı](azure-stack-manage-portals.md) noktası tıklama deneyimindeki için günlük yönetimi işlemleri sağlar.
2. Azure Resource Manager PowerShell ve Azure CLI tarafından kullanılan bir REST API aracılığıyla Yönetici portalı tüm yönetim işlemlerini gösterir. 
3. Belirli düşük düzey işlemler, örneğin veri merkezi tümleştirmesi veya senaryoları desteklemek için Azure Stack adlı bir PowerShell uç noktasını kullanıma sunar [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md). Bu uç nokta yalnızca izin verilenler listesinde cmdlet'ler kümesi sunan ve yoğun olarak denetlenir.

## <a name="network-controls"></a>Ağ denetimleri
Azure Stack altyapısının, birden çok ağ erişim denetimi List(ACL) katmanlarla sunulur. ACL, altyapı bileşenleri için yetkisiz erişimi önlemek ve kendi işlevi için gerekli olan yollar için altyapı iletişimi sınırlayabilirsiniz. 

Ağ ACL'leri, üç katmanda uygulanır:
1.  Raf üstü anahtarları
2.  Yazılım tanımlı ağ
3.  Konak ve sanal makine işletim sistemi güvenlik duvarları

## <a name="next-steps"></a>Sonraki adımlar

- [Azure stack'teki dizilerinizin döndürme hakkında bilgi edinin](azure-stack-rotate-secrets.md)
- [PCI-DSS ve CSA-CCM belgeleri için Azure Stack](https://servicetrust.microsoft.com/ViewPage/TrustDocuments)
- [Azure Stack için Savunma Bakanlığı ve NIST belgeleri](https://servicetrust.microsoft.com/ViewPage/Blueprint)
