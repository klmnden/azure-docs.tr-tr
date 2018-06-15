---
title: Azure Market görüntülerini için güvenlik önerilerini | Microsoft Docs
description: Bu makalede Pazar yerinde dahil görüntüleri için öneriler
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: barclayn
ms.openlocfilehash: 4ae36f87c29975c82bb99f713893a9dc78a249e6
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
ms.locfileid: "23931138"
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Azure Market görüntüleri için güvenlik önerileri

Her bir çözümü için aşağıdaki güvenlik yapılandırma önerileri ile uyumlu olmasını öneririz. Bu, yüksek düzeyde güvenlik için iş ortağı çözümü Azure Marketi görüntülerinde korumak yardımcı olur.

Bu öneriler, ayrıca görüntüleri Azure marketi'ndeki sahip olmayan kuruluşlarda yararlı olabilir. Aşağıdaki tablolarda bulunan yönergeleri karşı şirketinizin Windows ve Linux görüntü yapılandırmalarını denetleyin isteyebilirsiniz.

## <a name="open-source-based-images"></a>Açık kaynak tabanlı görüntüler

|||
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Kategori**                                                 | **Onay**                                                                                                                                                                                                                                                                              |
| Güvenlik                                                     | Tüm en son düzeltme eklerini Linux dağıtım yüklenir.                                                                                                                                                                                                              |
| Güvenlik                                                     | VM görüntüsü belirli Linux dağıtım güvenliğini sağlamak için endüstri yönergeleri izlediyseniz.                                                                                                                                                                                     |
| Güvenlik                                                     | Tutma en az ayak izini yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktaları tarafından saldırı yüzeyini sınırlayın.                                                                                                                                               |
| Güvenlik                                                     | Kaynak kodu ve sonuçta elde edilen VM görüntüsü kötü amaçlı yazılımlar için tarama.                                                                                                                                                                                                                                   |
| Güvenlik                                                     | VHD görüntüsü yalnızca etkileşimli oturum açma belirleyebilmesini varsayılan parolalarına sahip olmaması gereken kilitli hesapları içerir; hiçbir arka kapı.                                                                                                                                           |
| Güvenlik                                                     | Gibi bir güvenlik duvarı uygulaması, bunlar üzerinde işlevsel olarak uygulama dayanır sürece güvenlik duvarı kuralları devre dışıdır.                                                                                                                                                                             |
| Güvenlik                                                     | Tüm hassas bilgiler gibi bilinen hosts dosyasını, günlük dosyalarını ve gereksiz sertifikaları test SSH anahtarları VHD görüntüsü kaldırılmıştır.                                                                                                                                       |
| Güvenlik                                                     | LVM değil kullanılan önerilir.                                                                                                                                                                                                                                            |
| Güvenlik                                                     | Gerekli kitaplıkları'nın en son sürümleri dahil olmalıdır: </br> -OpenSSL v1.0 veya daha büyük </br> -Python 2.5 veya üstü (Python 2.6 + önemle önerilir) </br> -Zaten yüklü değilse Python pyasn1 paketi </br> -d.OpenSSL v 1.0 veya üzeri                                                                |
| Güvenlik                                                     | Bash/Kabuk geçmiş girişleri temizlenmelidir                                                                                                                                                                                                                                             |
| Ağ                                                   | SSH sunucusu varsayılan olarak eklenmelidir. Kümesine SSH Koru Canlı aşağıdaki seçeneğiyle sshd config: ClientAliveInterval 180                                                                                                                                                        |
| Ağ                                                   | Görüntü herhangi bir özel ağ yapılandırması içermemesi gerekir. Resolv.conf Sil:`rm /etc/resolv.conf`                                                                                                                                                                                |
| Dağıtım                                                   | En son Azure Linux aracısı yüklü olmalıdır </br> -Aracı kullanarak RPM ya da Deb paketi yüklü olmalıdır.  </br> -El ile yükleme işlemi de kullanabilir, ancak Yükleyici paketlerini önerilen ve tercih edilen. </br> -Aracıyı el ile github'da depodan yüklüyorsanız, ilk kopyalama `waagent` dosya `/usr/sbin` ve (kök olarak) çalıştırın: </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Aracı yapılandırma dosyası konumunda yerleştirilecek `/etc/waagent.conf`.    |
| Dağıtım                                                   | Azure destek ortaklarımızın gereken ve işletim sistemi disk bağlama bulut depolama için yeterli zaman aşımı sağlayın, çıktı seri konsolu ile sağlayabilir sağlar. Görüntü aşağıdaki parametreler için çekirdek önyükleme satırı eklemiş olmanız gerekir:`console=ttyS0 earlyprintk=ttyS0 rootdelay=300` |
| Dağıtım                                                   | İşletim sistemi disk üzerindeki değiştirme bölüm yok. Takas yerel kaynak diskteki oluşturma Linux aracısı tarafından istenebilir.         |
| Dağıtım                                                   | Tek kök bölüm işletim sistemi diski için oluşturduğunuz önerilir.      |
| Dağıtım                                                   | yalnızca 64-bit işletim sistemi için.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server tabanlı görüntüler

|||
|-------------| -------------------------|
| **Kategori**                                                     | **Onay**                                                                                                                                                                |
| Güvenlik                                                         | Güvenli bir işletim sistemi temel görüntü kullanın. Windows Server tabanlı herhangi bir görüntü kaynağı için kullanılan VHD Microsoft Azure tarafından sağlanan Windows Server işletim sistemi görüntüleri arasında olmalıdır. |
| Güvenlik                                                         | Tüm son güvenlik güncelleştirmelerini yükleyin.                                                                                                                                     |
| Güvenlik                                                         | Uygulamalar, yönetici, kök ve yönetici gibi sınırlı kullanıcı adları bir bağımlılık olmamalıdır                                                                |
| Güvenlik                                                         | BitLocker Sürücü Şifrelemesi işletim sistemi sabit diskinde desteklenmiyor. BitLocker veri diskler üzerinde kullanılabilir.                                                            |
| Güvenlik                                                         | En az ayak yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktalarını etkin tutma tarafından saldırı yüzeyini sınırlayın.                         |
| Güvenlik                                                         | Kaynak kodu ve sonuçta elde edilen VM görüntüsü kötü amaçlı yazılımlar için tarama.                                                                                                                     |
| Güvenlik                                                         | Otomatik güncelleştirme için Windows Server görüntülerini güvenlik güncelleştirmesini ayarlayın.                                                                                                                |
| Güvenlik                                                         | VHD görüntüsü yalnızca etkileşimli oturum açma belirleyebilmesini varsayılan parolalarına sahip olmaması gereken kilitli hesapları içerir; hiçbir arka kapı.                             |
| Güvenlik                                                         | Gibi bir güvenlik duvarı uygulaması, bunlar üzerinde işlevsel olarak uygulama dayanır sürece güvenlik duvarı kuralları devre dışıdır.                                                               |
| Güvenlik                                                         | Tüm hassas bilgileri VHD görüntüden kaldırılmıştır. Örneğin, ana dosya, günlük dosyalarını ve gereksiz sertifikaları kaldırılması gerekir.                                              |
| Dağıtım                                                       | yalnızca 64-bit işletim sistemi için.                            |
