---
title: Azure Stack altyapısını yedekleme hizmeti başvurusu | Microsoft Docs
description: Bu makale, Azure Stack altyapısını yedekleme hizmeti için başvuru bilgileri içerir.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: a7930ea86f7972a6e4abb939fb148d519ca924e9
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416726"
---
# <a name="infrastructure-backup-service-reference"></a>Altyapı Backup hizmeti başvurusu

## <a name="azure-backup-infrastructure"></a>Azure Yedekleme Altyapısı

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portal, Azure Resource Manager'ı oluşturan çok sayıda hizmetten oluşur ve altyapı yönetimi deneyimi. Azure Stack Gereci gibi yönetim deneyimini kullanıma sunulan çözüm işlecine karmaşıklığı azaltmak üzerinde odaklanır.

Altyapı yedeklemeyi yedekleme karmaşıklığı internalize için tasarlanmıştır ve altyapı hizmetleri için verilerin geri yüklenmesi, işleçler sağlayarak çözümü yönetme ve kullanıcılar için bir SLA'yı tutarak odaklanabilirsiniz.

Dış bir paylaşıma yedekleme verileri dışarı aktarma, yedeklemeler aynı sistemde depolanmasını önlemek için gereklidir. Yönetici, bir dış paylaşım gerektiren var olan şirket BC/DR ilkelerine bağlı olarak verilerin depolanacağı konumu belirlemek için esnekliği sunar. 

### <a name="infrastructure-backup-components"></a>Altyapı Backup bileşenleri

Yedekleme Altyapısı aşağıdaki bileşenleri içerir:

 - **Yedekleme Altyapısı denetleyici**  
 Yedekleme Altyapısı Denetleyici ile örneği ve her Azure Stack bulutunda yer alan.
 - **Yedekleme kaynak sağlayıcısı**  
 Yedekleme kaynak sağlayıcısı (Yedekleme RP) için Azure Stack altyapısının temel yedekleme işlevselliği kullanıma sunma kullanıcı arabirimi ve uygulama programı arabirimleri (API) s oluşur.

#### <a name="infrastructure-backup-controller"></a>Yedekleme Altyapısı denetleyici

Altyapı yedekleme hizmeti için bir Azure Stack bulut örneği bir Service Fabric denetleyicisidir. Yedekleme kaynakları bölgesel düzeyi ve yakalama bölgeye özgü verileri bir hizmet AD, CA, Azure Kaynak Yöneticisi'nden en oluşturulur CRP, SRP, NRP, anahtar kasası, RBAC. 

### <a name="backup-resource-provider"></a>Yedekleme kaynak sağlayıcısı

Yedekleme kaynak sağlayıcısı, Azure Stack portalında temel yapılandırmasını ve yedekleme kaynaklar listesi için kullanıcı arabirimi sunar. İşleci kullanıcı arabiriminin aşağıdaki işlemleri gerçekleştirebilirsiniz:

 - Dış depolama konumu, kimlik bilgilerini ve şifreleme anahtarı sağlayarak ilk kez yedeklemeyi etkinleştirme
 - Yedekleme ve durum kaynaklarının oluşturma'nın altında tamamlandı görünümü oluşturan
 - Burada yedekleme verilerini yedekleme denetleyicisi yerleştirir depolama konumu değiştirin
 - Yedekleme denetleyicisi dış depolama konumuna erişmek için kullandığı kimlik bilgilerini değiştirme
 - Yedekleme denetleyicisi yedeklemeleri şifrelemek için kullanılan şifreleme anahtarını değiştir 


## <a name="backup-controller-requirements"></a>Yedekleme denetleyicisi gereksinimleri

Bu bölümde altyapı yedeklemesine önemli gereksinimleri açıklanmaktadır. Azure Stack Örneğiniz için yedeklemeyi etkinleştirme ve ardından geri gerektiğinde dağıtım ve sonraki işlemi sırasında başvurduğu önce bilgileri dikkatlice gözden öneririz.

Gereksinimleri şunları içerir:

  - **Yazılım gereksinimleri** – desteklenen depolama konumlarını ve boyutlandırma kılavuzluğu açıklar. 
  - **Ağ gereksinimleri** – farklı depolama konumları için ağ gereksinimlerini açıklar.  

### <a name="software-requirements"></a>Yazılım gereksinimleri

#### <a name="supported-storage-locations"></a>Desteklenen depolama konumları

| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilen ağ ortamında bir depolama cihazı üzerinde barındırılan SMB dosya paylaşımı | SMB, Azure Stack dağıtıldığı aynı veri merkezinde veya farklı bir veri merkezinde paylaşın. Azure Stack birden fazla aynı dosya paylaşımını kullanabilirsiniz. |
| Azure'da SMB dosya paylaşımı                                                          | Şu anda desteklenmiyor.                                                                                                                                 |
| Azure BLOB Depolama                                                            | Şu anda desteklenmiyor.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Desteklenen SMB sürümleri

| SMB | Sürüm |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>Depolama konumu boyutlandırma 

Altyapı yedekleme denetleyicisi verilerini isteğe bağlı olarak yedekler. En son iki gün ve canlı en fazla yedi güne kadar yedek yedeklemek için önerilir. 

**1811 ve sonraki süreci desteleyen**
| Ortam ölçek | Yedekleme tahmini boyutu | Toplam gereken alan miktarı |
|-------------------|--------------------------|--------------------------------|
| 4-16 düğümleri        | 20 GB                    | 280 GB                        |
| ASDK              | 10 GB                    | 140 GB                        |

**Öncesi 1811**
| Ortam ölçek | Yedekleme tahmini boyutu | Toplam gereken alan miktarı |
|-------------------|--------------------------|--------------------------------|
| 4-16 düğüm ASDK  | 10 GB                     | 140 GB                        |

### <a name="network-requirements"></a>Ağ gereksinimleri
| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilen ağ ortamında bir depolama cihazı üzerinde barındırılan SMB dosya paylaşımı | Azure Stack örneği bağlantısıyla bir ortamda yer alıyorsa, 445 bağlantı noktası gereklidir. Altyapı yedekleme denetleyicisi bağlantı noktası 445 üzerinden SMB dosya sunucusu için bir bağlantı başlatır. |
| Dosya sunucusu FQDN'si kullanmak için ad CESARETLENDİRİCİ çözülebilir olması gerekir             |                                                                                                                                                                                         |

> [!Note]  
> Hiçbir gelen bağlantı noktalarının açılması gerekir.

### <a name="encryption-requirements"></a>Şifreleme gereksinimleri

İçinde 1901 başlayarak, altyapı yedekleme hizmeti ortak anahtara sahip bir sertifika kullanır (. Yedekleme verileri ve sertifikayı özel anahtarla şifrelemek için CER) (. PFX) bulut Kurtarma sırasında yedek verilerin şifresini çözmek için kullanılır.   
 - Sertifika anahtarlarının taşıma için kullanılır ve güvenli bir kimliği doğrulanmış iletişim kurmak için kullanılmaz. Bu nedenle, sertifika otomatik olarak imzalanan bir sertifika olabilir. Azure Stack dış internet erişimi gerekli değildir. Bu nedenle, kök veya bu sertifikanın güven doğrulayın gerekmez.
 
Otomatik olarak imzalanan sertifika, iki parça, bir ortak anahtara sahip ve bir özel anahtarla birlikte gelir:
 - Yedekleme verilerini şifrele: (Dışarı. ortak anahtara sahip sertifika CER dosyası), yedekleme verilerini şifrelemek için kullanılır
 - Yedekleme verilerinin şifresini: (Dışarı. Özel anahtarlı sertifika PFX dosyası) yedek verilerin şifresini çözmek için kullanılır

Sertifikanın ortak anahtarı ile (. CER) iç gizli döndürme tarafından yönetilmiyor. Sertifika döndürmek için yeni bir otomatik olarak imzalanan sertifika oluşturmak ve yeni bir dosya ile Yedekleme ayarlarını güncelleştirme yapmanız gerekir (. CER).  
 - Tüm mevcut yedeklemeler önceki ortak anahtar kullanılarak şifrelenmiş olarak kalır. Yeni yedeklemeleri yeni ortak anahtarı kullanır. 
 
Bulut Kurtarma sırasında kullanılan özel anahtarla sertifikayı (. PFX) tarafından Azure Stack güvenlik nedeniyle kalıcı olmaz. Bu dosya, bulut Kurtarma sırasında açıkça sağlanması gerekir.  

**Geriye dönük uyumluluk modu** içinde 1901 başlayarak, anahtar şifreleme desteği kullanım dışıdır ve gelecek sürümde kaldırılacak. Zaten bir şifreleme anahtarı kullanarak etkin yedek ile 1811 güncelleştirdiyseniz, şifreleme anahtarını kullanmak Azure Stack devam eder. Geriye dönük uyumluluk modu için en az 3 sürüm desteklenecektir. Bundan sonra bir sertifika gerekir. 
 * Şifreleme anahtarı için sertifika güncelleştirilirken bir tek yönlü bir işlemdir.  
 * Tüm mevcut yedeklemeler şifreleme anahtar kullanılarak şifrelenmiş olarak kalır. Yeni yedeklemeleri sertifikayı kullanır. 

## <a name="infrastructure-backup-limits"></a>Yedekleme Altyapısı sınırları

Planlama, dağıtma ve Microsoft Azure Stack örneklerinizin çalışmak gibi bu sınırları göz önünde bulundurun. Bu sınırlar aşağıdaki tabloda açıklanmaktadır.

### <a name="infrastructure-backup-limits"></a>Altyapı yedekleme sınırları
| Sınır tanımlayıcı                                                 | Sınır        | Yorumlar                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme türü                                                      | Yalnızca tam    | Altyapı yedekleme denetleyicisi, yalnızca tam yedeklemeleri destekler. Artımlı yedeklemeler desteklenmez.                                          |
| Zamanlanmış yedeklemeler                                                | Zamanlanmış ve el ile  | Zamanlanmış ve isteğe bağlı yedeklemeler yedekleme denetleyicisi destekler                                                                                 |
| En fazla eş zamanlı yedekleme işleri                                   | 1            | Yedekleme denetleyici örneği başına yalnızca bir etkin yedek iş desteklenir.                                                                  |
| Ağ anahtarı yapılandırmasının                                     | Kapsam içinde değil | Yönetici, OEM araçlar kullanarak ağ anahtarı yapılandırmasının yedeklemeniz gerekir. Azure Stack her OEM satıcısı tarafından sağlanan belgelere bakın. |
| Donanım yaşam döngüsü konak                                          | Kapsam içinde değil | Yönetici, donanım yaşam döngüsü OEM araçlarını kullanarak konak yedeklemeniz gerekir. Azure Stack her OEM satıcısı tarafından sağlanan belgelere bakın.      |
| En fazla sayıda dosya paylaşımı                                    | 1            | Yalnızca bir dosya paylaşımını yedekleme verilerini depolamak için kullanılabilir                                                                                        |
| Uygulama Hizmetleri, işlev, SQL, mysql kaynak sağlayıcı verilerini yedekleme | Kapsam içinde değil | Dağıtma ve yönetme değeri-Microsoft tarafından oluşturulan RPs eklemek için yayımlanan kılavuzuna bakın.                                                  |
| Yedekleme üçüncü taraf kaynak sağlayıcıları                              | Kapsam içinde değil | Dağıtma ve yönetme değeri-üçüncü taraf satıcıları ile oluşturulan RPs eklemek için yayımlanan kılavuzuna bakın.                                          |

## <a name="next-steps"></a>Sonraki adımlar

 - Altyapı Backup hizmeti hakkında daha fazla bilgi için bkz: [altyapı Backup hizmeti ile Azure Stack için yedekleme ve veri kurtarma](azure-stack-backup-infrastructure-backup.md).
