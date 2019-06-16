---
title: Azure Market görüntüleri için güvenlik önerilerini | Microsoft Docs
description: Bu makalede, Pazar yerinde dahil görüntüleri için öneriler sağlanır.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: barclayn
ms.openlocfilehash: fa521b81c95f7c0556b082e5487848ef4d7ecaf7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60444121"
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Azure Market görüntüleri için güvenlik önerileri

Her çözüm aşağıdaki güvenlik yapılandırma önerileri ile uyumlu öneririz. Bu durum, yüksek düzeyde bir iş ortağı çözümü görüntüleri Azure Market'te güvenlik korumasına yardımcı olur.

Bu önerileri görüntüleri Azure Market'te olmayan kuruluşlar için yararlı olabilir. Şirketinizin Windows ve Linux görüntü yapılandırmaları aşağıdaki tablolarda bulunan yönergeleri karşı denetlemek isteyebilirsiniz:

## <a name="open-source-based-images"></a>Açık kaynak tabanlı görüntüler

|||
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Kategori**                                                 | **Onay**                                                                                                                                                                                                                                                                              |
| Güvenlik                                                     | Tüm en son düzeltme eklerini Linux dağıtımı için yüklenir.                                                                                                                                                                                                              |
| Güvenlik                                                     | Sanal makine görüntüsü için ilgili Linux dağıtımına güvenliğini sağlamak için endüstri yönergeleri izlediyseniz.                                                                                                                                                                                     |
| Güvenlik                                                     | Saldırı yüzeyini tutma minimum ayak izini yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktaları sınırlayın.                                                                                                                                               |
| Güvenlik                                                     | Kaynak kodu ve sonuçta elde edilen sanal makine görüntüsü kötü amaçlı yazılım için tarama.                                                                                                                                                                                                                                   |
| Güvenlik                                                     | VHD görüntüsü yalnızca etkileşimli oturum açma izin varsayılan parolalarına sahip olmaması gereken kilitli hesaplar içerir; arka kapı yok.                                                                                                                                           |
| Güvenlik                                                     | Güvenlik duvarı kuralları devre dışıdır, bu sürece bir güvenlik duvarı Gereci gibi bunlar üzerinde işlevsel olarak uygulama kullanır.                                                                                                                                                                             |
| Güvenlik                                                     | Tüm önemli bilgileri gibi test SSH anahtarları, ana dosya, günlük dosyalarını ve gereksiz sertifikaları bilinen bir VHD görüntüsü kaldırılmıştır.                                                                                                                                       |
| Güvenlik                                                     | LVM'yi kullanılmaz, önerilir.                                                                                                                                                                                                                                            |
| Güvenlik                                                     | Gerekli kitaplıkların en son sürümlerine dahil edilmelidir: </br> -OpenSSL v1.0 veya üzeri </br> -Python 2.5 veya üstü (Python 2.6 + kesinlikle önerilir) </br> -Zaten yüklü değilse Python pyasn1 paketi </br> -d.OpenSSL v 1.0 veya üstü                                                                |
| Güvenlik                                                     | Bash/Kabuk geçmişi girdileri temizlenmelidir                                                                                                                                                                                                                                             |
| Ağ                                                   | SSH sunucusu varsayılan olarak eklenmelidir. Sshd config seçeneğiyle SSH keep alive ayarlayın: Satırını Clientaliveınterval 180                                                                                                                                                        |
| Ağ                                                   | Görüntü, herhangi bir özel ağ yapılandırması içermemelidir. Resolv.conf silin: `rm /etc/resolv.conf`                                                                                                                                                                                |
| Dağıtım                                                   | En son Azure Linux aracısı yüklü olması gerekir </br> -Aracıyı RPM veya Deb paketini kullanarak yüklenmesi gerekir.  </br> -El ile yükleme işlemini de kullanabilirsiniz, ancak Eğer yükleyici paketleri önerilen ve tercih edilen. </br> -Aracının GitHub deposundan el ile yükleme, ilk kopyalama `waagent` dosyasını `/usr/sbin` ve (kök) çalıştırın: </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Aracı yapılandırma dosyasını yerleştirilmiş olması `/etc/waagent.conf`.    |
| Dağıtım                                                   | Azure destek ortaklarımızla birlikte seri konsol gerektiği ve işletim sistemi disk bağlama bulut depolama için yeterli zaman aşımı sağlamak çıkışı sağlamasına olanak tanır. Görüntü aşağıdaki parametreleri Kernel önyükleme satırına eklemiş olmanız gerekir: `console=ttyS0 earlyprintk=ttyS0 rootdelay=300` |
| Dağıtım                                                   | İşletim sistemi diski üzerinde takas bölümü yok. Takas Linux Aracısı kullanılarak yerel kaynak oluşturma için istenebilir.         |
| Dağıtım                                                   | İşletim sistemi diski için tek bir kök bölümü oluşturmanız önerilir.      |
| Dağıtım                                                   | yalnızca 64 bit işletim sistemi.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server tabanlı görüntüler

|||
|-------------| -------------------------|
| **Kategori**                                                     | **Onay**                                                                                                                                                                |
| Güvenlik                                                         | Güvenli bir işletim sistemi temel görüntüsü kullanın. Microsoft Azure sağlanan Windows Server işletim sistemi görüntüleri Windows Server tabanlı herhangi bir görüntü kaynağı için kullanılan VHD olmalıdır. |
| Güvenlik                                                         | Tüm son güvenlik güncelleştirmelerini yükleyin.                                                                                                                                     |
| Güvenlik                                                         | Uygulamalar, kısıtlı kullanıcı adları yönetici, kök ve yönetici gibi bir bağımlılık olmamalıdır                                                                |
| Güvenlik                                                         | BitLocker Sürücü Şifrelemesi, işletim sistemi sabit diskinde desteklenmiyor. BitLocker, veri diskleri üzerinde kullanılabilir.                                                            |
| Güvenlik                                                         | Saldırı yüzeyini en az alana yayılma olanağıyla yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri tutarak ve bağlantı noktaları etkin ağ sınırlayın.                         |
| Güvenlik                                                         | Kaynak kodu ve sonuçta elde edilen sanal makine görüntüsü kötü amaçlı yazılım için tarama.                                                                                                                     |
| Güvenlik                                                         | Otomatik güncelleştirme için Windows Server görüntüleri Güvenlik Güncelleştirmesi'ni ayarlayın.                                                                                                                |
| Güvenlik                                                         | VHD görüntüsü yalnızca etkileşimli oturum açma izin varsayılan parolalarına sahip olmaması gereken kilitli hesaplar içerir; arka kapı yok.                             |
| Güvenlik                                                         | Güvenlik duvarı kuralları devre dışıdır, bu sürece bir güvenlik duvarı Gereci gibi bunlar üzerinde işlevsel olarak uygulama kullanır.                                                               |
| Güvenlik                                                         | Tüm önemli bilgileri VHD görüntüsünden kaldırılmıştır. Örneğin, ana dosya, günlük dosyalarını ve gereksiz sertifikaları kaldırılmalıdır.                                              |
| Dağıtım                                                       | yalnızca 64 bit işletim sistemi.                            |
