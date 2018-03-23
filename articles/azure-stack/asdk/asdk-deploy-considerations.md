---
title: Azure yığın Geliştirme Seti (ASDK) dağıtımının önkoşulları | Microsoft Docs
description: Azure yığın Geliştirme Seti (ASDK) için ortamı ve donanım gereksinimlerini gözden geçirin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: f4b55bb3287f67792b3257c3f62256437f5625ca
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-stack-deployment-planning-considerations"></a>Azure yığın dağıtım planlama konuları
Azure yığın Geliştirme Seti (ASDK) dağıtmadan önce Geliştirme Seti ana bilgisayarınız bu makalede açıklanan gereksinimleri karşıladığından emin olun.


## <a name="hardware"></a>Donanım
| Bileşen | Minimum | Önerilen |
| --- | --- | --- |
| Disk sürücüleri: İşletim Sistemi |Sistem bölümü için en az 200 GB'lık kullanılabilir alana sahip 1 işletim sistemi diski (SSD veya HDD) |Sistem bölümü için en az 200 GB'lık kullanılabilir alana sahip 1 işletim sistemi diski (SSD veya HDD) |
| Disk sürücüleri: Genel Geliştirme Seti veri<sup>*</sup>  |4 disk. Her disk, en az 140 GB'lık kapasiteye (SSD veya HDD) sahiptir. Kullanılabilir tüm diskler kullanılır. |4 disk. Her disk, en az 250 GB'lık kapasiteye (SSD veya HDD) sahiptir. Kullanılabilir tüm diskler kullanılır. |
| İşlem: CPU |Çift Yuvalı: 12 Fiziksel Çekirdek (toplam) |Çift Yuvalı: 16 Fiziksel Çekirdek (toplam) |
| İşlem: Bellek |96 GB RAM |128 GB RAM (PaaS kaynak sağlayıcıları desteklemek için en düşük gereksinimdir.)|
| İşlem: BIOS |Hyper-V Etkin (SLAT desteğiyle) |Hyper-V Etkin (SLAT desteğiyle) |
| Ağ: NIC |NIC için Windows Server 2012 R2 Sertifikası gerekir; özelleştirilmiş herhangi bir özellik gerekmez |NIC için Windows Server 2012 R2 Sertifikası gerekir; özelleştirilmiş herhangi bir özellik gerekmez |
| Donanım logosu sertifikası |[Windows Server 2012 R2 için sertifikalıdır](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Windows Server 2016 sertifikalı](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

<sup>*</sup> Birçok eklemeyi düşünüyorsanız, bu kapasite önerilen fazlasını gereksinim [Market öğesi](asdk-marketplace-item.md) azure'dan.

**Veri disk sürücüsü yapılandırması:** tüm veri sürücüleri türü (tüm SAS, tüm SATA veya tüm NVMe) ve kapasite aynı olmalıdır. SAS disk sürücüleri kullanılıyorsa disk sürücülerinin (MPIO kullanılmaz, çok yollu destek sağlanır) tek bir yolla bağlanması gerekir.

**HBA yapılandırma seçenekleri**

* (Tercih edilen) Basit HBA
* RAID HBA - Bağdaştırıcının "geçiş" modunda yapılandırılmış olması gerekir
* RAID HBA - Disklerin Tek Disk ve RAID-0 olarak yapılandırılmış olması gerekir

**Desteklenen veri yolu ve ortam birleşimleri yazın**

* SATA HDD
* SAS HDD
* RAID HDD
* RAID SSD (medya türü belirtilmemiş bilinmeyen ise<sup>*</sup>)
* SATA SSD + SATA HDD
* SAS SSD + SAS HDD
* NVMe

<sup>*</sup> RAID denetleyiciler geçiş yetenek olmadan medya türü tanıyamıyor. Bu tür denetleyicilerini HDD ve SSD belirtilmemiş işaretleyin. Bu durumda, SSD aygıtları önbelleğe alma yerine kalıcı depolama olarak kullanılır. Bu nedenle, bu SSD Geliştirme Seti dağıtabilirsiniz.

**Örnek HBA'lar**: geçiş modunda LSI 9207-8i, LSI-9300-8i veya LSI-9265-8i

Örnek OEM yapılandırmaları kullanılabilir.

## <a name="operating-system"></a>İşletim sistemi
|  | **Gereksinimleri** |
| --- | --- |
| **İşletim sistemi sürümü** |Windows Server 2012 R2 veya sonraki bir sürümü. Dağıtıma başlamadan önce işletim sistemi sürümü Azure yığın yüklemede VHD içine ana bilgisayar önyükleme gibi kritik değildir. İşletim sistemi ve tüm gerekli düzeltme eklerini zaten görüntüsüne tümleşiktir. Tüm anahtarları development Kit'te kullanılan herhangi bir Windows Server örneklerini etkinleştirmek için kullanmayın. |

> [!TIP]
> İşletim sisteminin yükledikten sonra kullanabilirsiniz [Azure yığını için dağıtım denetleyicisi](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınız tüm gereksinimleri karşıladığını onaylamak için.

## <a name="account-requirements"></a>Hesap gereksinimleri
Genellikle, burada, Microsoft Azure bağlanabilirsiniz internet bağlantısıyla Geliştirme Seti dağıtın. Bu durumda, Geliştirme Seti dağıtmak için bir Azure Active Directory (Azure AD) hesabı yapılandırmanız gerekir.

Ortamınız internet'e bağlı değil veya Azure AD kullanmak istemiyorsanız, Active Directory Federasyon Hizmetleri (AD FS) kullanarak Azure yığın dağıtabilirsiniz. Geliştirme Seti kendi AD FS ve Active Directory etki alanı Hizmetleri örnekleri içerir. Bu seçeneği kullanarak dağıtırsanız, önceden hesapları ayarlamanız gerekmez.

>[!NOTE]
AD FS seçeneğini kullanarak dağıtırsanız, Azure AD ile geçiş yapmak için Azure yığını yeniden dağıtmanız gerekir.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory hesapları
Bir Azure AD hesabı kullanarak Azure yığın dağıtmak için dağıtım PowerShell komut dosyasını çalıştırmadan önce bir Azure AD hesabının hazırlamanız gerekir. Bu hesap, Azure AD Kiracı için genel yönetici olur. Sağlamak ve uygulamalar ve hizmet asıl adı Azure Active Directory ve grafik API'si etkileşime tüm Azure yığın hizmetlerini temsilci seçmek için kullanılır. (Bu, daha sonra değiştirebilirsiniz) varsayılan sağlayıcı aboneliğin sahibi da kullanılır. Azure yığın sisteminizin Yönetici portalına bu hesabı kullanarak oturum.

1. İçin en az bir Azure AD directory yönetici olan bir Azure AD hesabı oluşturun. Zaten bir hesabınız varsa bu hesabı kullanabilirsiniz. Aksi takdirde, bir ücretsiz oluşturabilirsiniz [ https://azure.microsoft.com/free/ ](http://azure.microsoft.com/pricing/free/) (Çin'de ziyaret <http://go.microsoft.com/fwlink/?LinkID=717821> yerine). İçin daha sonra planlıyorsanız [Azure yığın Azure ile kaydedin](asdk-register.md), ayrıca bir abonelik bu hesap yeni oluşturulmuş olması gerekir.
   
    Bu kimlik bilgilerini kullanmak için Hizmet Yöneticisi olarak kaydedin. Bu hesabı yapılandırın ve kaynak bulutlarını, kullanıcı hesapları, Kiracı plan, kotalar ve fiyatlandırma yönetin. Portalda web sitesi bulutları, sanal makine özel bulutları ve planlar oluşturup kullanıcı aboneliklerini yönetebilir.
1. En az bir test kullanıcı hesabı, Azure AD'de oluşturun, böylece Kiracı Geliştirme Seti için oturum açabilirsiniz.
   
   | **Azure Active Directory hesabı** | **Destekleniyor mu?** |
   | --- | --- |
   | Geçerli ortak Azure aboneliği ile iş veya Okul hesabı |Evet |
   | Geçerli Genel Azure Aboneliğine sahip Microsoft Hesabı |Evet |
   | Geçerli Çin Azure aboneliği ile iş veya Okul hesabı |Evet |
   | Geçerli US Government Azure aboneliği ile iş veya Okul hesabı |Evet |

## <a name="network"></a>Ağ
### <a name="switch"></a>Anahtar
Kullanılabilir bir bağlantı noktasına Geliştirme Seti makine için bir anahtar.  

Geliştirme Seti makine anahtarı erişim bağlantı noktası veya santral bağlantı noktasına bağlanmayı destekler. Anahtar üzerinde özelleştirilmiş herhangi bir özellik gerekmez. Santral bağlantı noktası kullanıyorsanız veya VLAN kimliği yapılandırmanız gerekiyorsa dağıtım parametresi olarak VLAN kimliğini sağlamanız gerekir.

### <a name="subnet"></a>Alt ağ
Geliştirme Seti makine aşağıdaki alt ağlara bağlanma:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Bu alt Geliştirme Seti ortamında iç ağlar için ayrılmıştır.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Yalnızca IPv4 desteklenir. IPv6 ağı oluşturamazsınız.

### <a name="dhcp"></a>DHCP
Ağ üzerinde NIC'nin bağlanabileceği kullanılabilir bir DHCP sunucusunun olduğundan emin olun. Kullanılabilir bir DHCP sunucusu yoksa ana bilgisayar tarafından kullanılanın dışında ek bir statik IPv4 ağı hazırlamanız gerekir. Dağıtım parametresi olarak bu IP adresini ve ağ geçidini sağlamanız gerekir.

### <a name="internet-access"></a>İnternet erişimi
Azure yığını, doğrudan ya da saydam bir proxy üzerinden Internet erişimi gerektirir. Azure yığın Internet erişimi etkinleştirmek için bir web proxy yapılandırmasını desteklemez. Hem ana bilgisayar IP hem de (DHCP veya statik IP) için MAS BGPNAT01 atanan yeni IP Internet erişimine sahip olmalıdır. 80 ve 443 numaralı bağlantı noktalarını altındaki graph.windows.net ve login.microsoftonline.com etki alanları kullanılır.


## <a name="next-steps"></a>Sonraki adımlar
[ASDK dağıtım paketini karşıdan yükle](asdk-download.md)
