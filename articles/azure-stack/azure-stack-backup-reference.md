---
title: "Azure hizmet yığını altyapı yedekleme başvurusu | Microsoft Docs"
description: "Bu makale Azure yığın altyapı yedekleme hizmeti için başvuru bilgileri içerir."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 4e6e0a52b2c55239e38757223f54e5e94dc98c42
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="infrastructure-backup-service-reference"></a>Altyapısı yedekleme hizmeti başvurusu

## <a name="azure-backup-infrastructure"></a>Azure Yedekleme Altyapısı

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Portal, Azure Resource Manager oluşturan birçok hizmetlerini Azure yığın oluşur ve altyapı yönetimi deneyimi. Azure yığın Gereci gibi yönetim deneyimini çözüm işleci için kullanıma sunulan karmaşıklığını azaltması üzerine odaklanır.

Altyapı yedekleme yedekleme karmaşıklığını internalize için tasarlanmıştır ve altyapı hizmetleri için verileri geri yükleme, işleçler sağlama çözümü yönetme ve kullanıcılara bir SLA koruma odaklanabilirsiniz.

Dış bir paylaşıma yedekleme verileri dışarı aktarma, aynı sistemde yedeklemelerini depolamak önlemek için gereklidir. Bir dış paylaşım gerektiren yönetici mevcut şirket BC/DR ilkelerine bağlı olarak verileri depolamak nereye belirlemek için esneklik sunar. 

### <a name="infrastructure-backup-components"></a>Altyapı yedekleme bileşenleri

Altyapı yedekleme aşağıdaki bileşenleri içerir:

 - **Altyapı yedek denetleyicisi**  
 Altyapı yedekleme denetleyicisi ile örneği ve her Azure yığın bulutta bulunur.
 - **Yedekleme kaynak sağlayıcısı**  
 Azure yığın altyapısı için temel yedekleme işlevini gösterme kullanıcı arabirimi ve uygulama programı arabirimlerini (API) s yedekleme kaynak sağlayıcısı (Yedekleme RP) oluşur.

#### <a name="infrastructure-backup-controller"></a>Altyapı yedek denetleyicisi

Altyapı yedekleme hizmeti için bir Azure yığın bulut örneği Service Fabric denetleyicisidir. Bir bölgesel düzeyinde ve yakalama bölgeye özgü hizmet veri AD, CA, Azure Kaynak Yöneticisi'nden, oluşturulan yedekleme kaynakları CRP, SRP, NRP, KeyVault, RBAC. 

### <a name="backup-resource-provider"></a>Yedekleme kaynak sağlayıcısı

Yedekleme kaynak sağlayıcısı temel yapılandırmasını ve yedekleme kaynaklar listesi için Azure yığın Portalı'nda kullanıcı arabirimi sunar. İşleci, kullanıcı arabiriminde aşağıdaki işlemleri gerçekleştirebilirsiniz:

 - Harici depolama konumu, kimlik bilgileri ve şifreleme anahtarı sağlayarak ilk kez yedeklemeyi etkinleştirme
 - Yedekleme ve durum kaynaklarının oluşturma'nın altında oluşturulan görüntüleme tamamlandı
 - Yedek denetleyicisi yedekleme verilerinin nerede yerleştirir depolama konumunu değiştirme
 - Yedek denetleyicisi harici depolama konuma erişmek için kullandığı kimlik bilgilerini değiştirme
 - Yedekleme denetleyicisi yedeklemeleri şifrelemek için kullanılan şifreleme anahtarını değiştirin 


## <a name="backup-controller-requirements"></a>Yedekleme denetleyicisi gereksinimleri

Bu bölümde altyapı yedekleme için önemli gereksinimler açıklanır. Önce Azure yığın Örneğiniz için yedeklemeyi etkinleştirin ve sonra geri gerektiği gibi dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatle gözden geçirmenizi öneririz.

Gereksinimleri şunları içerir:

  - **Yazılım gereksinimleri** – desteklenen depolama konumlarını ve boyutlandırma kılavuzluğu açıklar. 
  - **Ağ gereksinimleri** – farklı depolama konumları ağ gereksinimlerini açıklar.  

### <a name="software-requirements"></a>Yazılım gereksinimleri

#### <a name="supported-storage-locations"></a>Desteklenen depolama konumları

| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilir ağ ortamında bir depolama aygıtı üzerinde barındırılan SMB dosya paylaşımı | SMB paylaşımı Azure yığın dağıtıldığı aynı veri merkezinde veya farklı bir veri merkezinde. Birden çok Azure yığın örnekleri aynı dosya paylaşımını kullanabilirsiniz. |
| SMB dosya paylaşımında Azure                                                          | Şu anda desteklenmiyor.                                                                                                                                 |
| Azure BLOB Depolama                                                            | Şu anda desteklenmiyor.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Desteklenen SMB sürümleri

| SMB | Sürüm |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>Depolama konumu boyutlandırma 

Altyapı yedekleme denetleyicisi isteğe bağlı olarak verileri yedekler. En son iki kez gün ve canlı en fazla yedi gün yedeklemeleri yedeklemek için önerilir. 

| Ortam ölçek | Yedekleme tahmini boyutu | Gerekli alanının toplam miktarını |
|-------------------|--------------------------|--------------------------------|
| 4-12 düğümler        | 10 GB                     | 140 GB                          |

### <a name="network-requirements"></a>Ağ gereksinimleri
| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilir ağ ortamında bir depolama aygıtı üzerinde barındırılan SMB dosya paylaşımı | Azure yığın örneği güvenlik duvarı içindeki bir ortamda bulunuyorsa 445 bağlantı noktası gereklidir. Altyapı yedekleme denetleyicisi 445 bağlantı noktası üzerinden SMB dosya sunucusu için bir bağlantı başlatır. |
| Dosya sunucusu FQDN'si kullanmak için ad CESARETLENDİRİCİ çözülebilir olması gerekir             |                                                                                                                                                                                         |

> [!Note]  
> Hiçbir gelen bağlantı noktalarının açılması gerekir.


## <a name="infrastructure-backup-limits"></a>Altyapı yedekleme sınırları

Planlamak, dağıtmak ve Microsoft Azure yığın örneklerinizi çalıştırmak gibi bu sınırları göz önünde bulundurun. Aşağıdaki tabloda bu sınırları açıklanmaktadır.

### <a name="infrastructure-backup-limits"></a>Altyapı yedekleme sınırları
| Sınır tanımlayıcı                                                 | Sınır        | Yorumlar                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme türü                                                      | Yalnızca tam    | Altyapı yedekleme denetleyicisi yalnızca tam yedeklemeleri destekler. Artımlı yedeklemeler desteklenmez.                                          |
| Zamanlanmış yedeklemeleri                                                | Yalnızca el ile  | Yedek denetleyicisi şu anda yalnızca isteğe bağlı yedeklemeler destekler                                                                                 |
| En fazla eş zamanlı yedekleme işleri                                   | 1            | Yedekleme denetleyici örneği başına yalnızca bir etkin yedek iş desteklenir.                                                                  |
| Ağ anahtarı yapılandırması                                     | Kapsamda değil | Yönetici, OEM araçlarını kullanarak ağ anahtarı yapılandırması yedeklemeniz gerekir. Azure yığınının her OEM satıcısı tarafından sağlanan belgelere başvurun. |
| Donanım yaşam döngüsü ana bilgisayar                                          | Kapsamda değil | Yönetici, donanım yaşam döngüsü ana OEM araçları kullanarak yedeklemeniz gerekir. Azure yığınının her OEM satıcısı tarafından sağlanan belgelere başvurun.      |
| En fazla sayıda dosya paylaşımı                                    | 1            | Yalnızca bir dosya paylaşımı yedek verileri depolamak için kullanılabilir                                                                                        |
| Uygulama Hizmetleri, işlev, SQL, mysql kaynak sağlayıcı veri yedekleme | Kapsamda değil | Dağıtma ve yönetme değeri-Microsoft tarafından oluşturulan RPs eklemek için yayımlanan kılavuzuna bakın.                                                  |
| Yedekleme üçüncü taraf kaynak sağlayıcıları                              | Kapsamda değil | Dağıtma ve yönetme değeri-üçüncü taraf satıcılar tarafından oluşturulan RPs eklemek için yayımlanan kılavuzuna bakın.                                          |

## <a name="next-steps"></a>Sonraki adımlar

 - Altyapı Backup hizmeti hakkında daha fazla bilgi için bkz: [altyapı Backup hizmeti ile Azure yığını için yedekleme ve veri kurtarma](azure-stack-backup-infrastructure-backup.md).