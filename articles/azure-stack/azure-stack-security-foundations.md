---
title: "Azure yığın güvenlik denetimleri anlama | Microsoft Docs"
description: "Hizmet Yöneticisi olarak Azure yığına uygulanan güvenlik denetimleri hakkında bilgi edinin"
services: azure-stack
documentationcenter: 
author: Heathl17
manager: byronr
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 106fcf7b0edc095a52e82d58ad48a73084b65d1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-infrastructure-security-posture"></a>Azure yığın altyapı güvenlik yaklaşımı

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Karma bulut kullanmak için ana sürücüleri güvenlik konuları ve uyumluluk düzenlemeleri arasındadır. Azure yığın bu senaryoları için tasarlanmıştır ve Azure yığın benimsenmesi zaman denetimlerin zaten anlamak önemlidir.

Azure yığınında bir arada iki güvenlik tutumunu katman vardır. İlk katman donanım bileşenleri tamamen kadar Azure Resource Manager duruma geçtiğinde ve yönetici ve Kiracı portalı içerir Azure yığın altyapı oluşur. İkinci katman, kiracılar oluşturmak, dağıtmak ve yönetmek iş yükü oluşur ve sanal makine ya da uygulama hizmetleri web siteleri gibi şeyleri içerir.  

## <a name="security-approach"></a>Güvenlik yaklaşımı
Azure yığını ile güvenlik tutumunu modern tehditlere karşı korumak için tasarlanmıştır ve ana uyumluluk standartlarını gereksinimlerden karşılamak için oluşturuldu. Sonuç olarak, güvenlik tutumunu Azure yığın altyapısının üzerinde iki ayaklar yerleşik olarak bulunur:

 - **İhlali varsayalım.** Sistem zaten ihlal edildiği varsayılır başlayarak, biz odaklanmak *algılama ve ihlallerinden etkilerini sınırlayarak* karşı saldırıları önlemek yalnızca çalışıyor. 
 - **Varsayılan olarak sıkı.**  İyi tanımlanmış donanım ve yazılım, altyapı çalışır olduğundan biz *etkinleştirmek, yapılandırmak ve güvenlik özellikleri doğrulama* , genellikle bırakılır uygulamak için müşteriler için.

Azure yığın tümleşik bir sistem olarak teslim edildiğinden Azure yığın altyapısının güvenlik tutumunu Microsoft tarafından tanımlanır.  Gibi Azure'da, kiracılar, Kiracı iş yüklerini güvenlik duruşunu tanımlamak için sorumludur. Bu belge, Azure yığın altyapı güvenlik yaklaşımı hakkında temel bilgi sağlar.

## <a name="data-at-rest-encryption"></a>Veri bekleyen şifreleme
Tüm Azure yığın altyapı ve Kiracı verileri Bitlocker kullanılarak bekleme sırasında şifrelenir. Bu şifreleme fiziksel kaybedilmesi veya çalınması Azure yığın depolama bileşenlerinin karşı korur. 

## <a name="data-in-transit-encryption"></a>Veri aktarım sırasında şifreleme
Azure yığın altyapı bileşenlerini TLS 1.2 ile şifrelenmiş kanalları kullanarak iletişim kurar. Şifreleme sertifikaları altyapısı tarafından otomatik olarak yönetilir. 

Tüm dış altyapı uç noktaları, REST uç noktaları ya da Azure yığın portalı gibi güvenli iletişimler için TLS 1.2 destekler. Şifreleme sertifikaları, bir üçüncü taraf veya sertifika yetkilisi, kuruluşunuz için bu uç noktaları sağlanmalıdır. 

Bu dış uç noktalar için otomatik olarak imzalanan sertifikalar kullanılabilir, ancak Microsoft bunları kullanılmamasını önerir. 

## <a name="secret-management"></a>Gizlilik Yönetimi
İşlev için parolalar gibi gizli çok sayıda Azure yığın altyapısını kullanır. Bunların çoğu otomatik olarak her 24 saatte döndürme Grup yönetilen hizmet hesapları olduklarından sık döndürülür.

Grup yönetilen hizmet hesapları el ile ayrıcalıklı uç noktada bir betik ile döndürülüp olmayan kalan gizli.

## <a name="code-integrity"></a>Kod bütünlüğü
Azure yığın yapar en son Windows Server 2016 kullanmak güvenlik özellikleri. Bunları Windows Defender'ın cihaz uygulamaları güvenilir listesine ekleme sağlayan ve sağlar, yalnızca Azure yığın altyapısındaki kodu çalıştırır yetkili koruyucusu, biridir. 

Yetkili kod Microsoft veya OEM ortağı tarafından imzalanır ve Microsoft tarafından tanımlanan bir ilke belirtilen izin verilen yazılım listesi dahil. Diğer bir deyişle, Azure yığın altyapısında çalıştırmak için onaylı yazılım çalıştırılabilir. Yetkisiz kod yürütmek için tüm girişimler engellenir ve bir denetim oluşturulur.

Cihaz koruyucusu İlkesi de üçüncü taraf aracılarını veya yazılımını Azure yığın altyapısında çalışmasını engeller.

## <a name="credential-guard"></a>Kimlik bilgisi koruma
Başka bir Windows Server 2016 güvenlik Azure yığınında Azure yığın altyapı kimlik Pass--Hash ve geçişi anahtar saldırılarından korumak için kullanılan Windows Defender'ın kimlik bilgisi koruma özelliğidir.

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma
Her bileşen Azure yığınında (Hyper-V konakları ve sanal makineler), Windows Defender virüsten korunur.

## <a name="constrained-administration-model"></a>Kısıtlanmış yönetim modeli
Azure yığınında yönetimi, her biri belirli bir amaca sahip üç giriş noktaları kullanımı ile kontrol edilir: 
1. [Yönetici portalı](azure-stack-manage-portals.md) günlük yönetimi işlemleri için noktası tıklatma deneyimi sağlar.
2. Azure Resource Manager PowerShell ve Azure CLI tarafından kullanılan bir REST API'si aracılığıyla Yönetici portalı'nı tüm yönetim işlemlerini gösterir. 
3. Belirli düşük düzey işlemler, örneğin veri merkezi tümleştirme veya senaryoları desteklemek için Azure yığın adlı bir PowerShell uç noktasını kullanıma sunar [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md). Yalnızca Güvenilenler listesine kümesi cmdlet'leri Bu uç noktasını kullanıma sunar ve yoğun olarak denetlenir.

## <a name="network-controls"></a>Ağ denetimleri
Azure yığın altyapı birden çok ağ erişim denetimi List(ACL) katman ile birlikte gelir.  ACL'ler altyapı bileşenleri için yetkisiz erişimi engellemek ve altyapı iletişimi kendi çalışması için gerekli olan yollara kısıtlayabilirsiniz. 

Ağ ACL'leri üç katmanında uygulanır:
1.  Raf üstündeki geçer
2.  Yazılım tanımlı ağ
3.  Konak ve VM işletim sistemi güvenlik duvarları 


