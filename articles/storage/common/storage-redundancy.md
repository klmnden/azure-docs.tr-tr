---
title: Azure Storage veri çoğaltması | Microsoft Docs
description: Microsoft Azure depolama hesabınızdaki veriler dayanıklılık ve yüksek kullanılabilirlik için çoğaltılır. Çoğaltma seçenekleri yerel olarak yedekli depolama (LRS), bölge olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) içerir.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: 6c2c6979d56eb19ff2ba4fb647c7c51e52e51ac6
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076223"
---
# <a name="azure-storage-replication"></a>Azure Storage çoğaltma

Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır. Azure Storage çoğaltma verilerinizi kopyalar, böylece planlanan ve planlanmayan olaylardan geçici donanım arızalarında, ağ veya güç kesintileri, yoğun doğal afetler ve benzeri arasında değişen korumalı. Verilerinizi aynı veri merkezi içinde aynı bölge içinde zonal veri merkezleri üzerinden ve hatta bölgeler üzerinden çoğaltmak seçebilirsiniz.

Çoğaltma işlemi, hata durumunda bile depolama hesabınızın [Depolama için Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) uymasını sağlar. Azure Depolama'nın dayanıklılık ve kullanılabilirlikle ilgili sağladığı garantiler hakkında bilgi edinmek için SLA'ya göz atın.

## <a name="choosing-a-replication-option"></a>Bir çoğaltma seçeneği seçme

Bir depolama hesabı oluşturduğunuzda şu çoğaltma seçeneklerinden birini seçebilirsiniz:

* [Yerel olarak yedekli depolama (LRS)](storage-redundancy-lrs.md)
* [Bölgesel olarak yedekli depolama (ZRS)](storage-redundancy-zrs.md)
* [Coğrafi olarak yedekli depolama (GRS)](storage-redundancy-grs.md)
* [Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

Aşağıdaki tabloda dayanıklılık ve her çoğaltma stratejisi belirli bir türde olay (veya benzer etkisi olay) sağlayacak kullanılabilirlik kapsamını hızlı bir bakış sağlar.

| Senaryo                                                                                                 | LRS                             | ZRS                              | GRS                                  | RA-GRS                               |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Bir veri merkezi içinde düğüm kullanılamazlık                                                                 | Evet                             | Evet                              | Evet                                  | Evet                                  |
| Bütün bir veri merkezi (zonal veya zonal olmayan) kullanılamaz                                           | Hayır                              | Evet                              | Evet                                  | Evet                                  |
| Bir bölge genelinde kesinti                                                                                     | Hayır                              | Hayır                               | Evet                                  | Evet                                  |
| Bölge genelinde olarak kullanım dışı kalması durumunda (bir bölgede uzaktan, coğrafi olarak çoğaltılmış) verilerinize okuma erişimi | Hayır                              | Hayır                               | Hayır                                   | Evet                                  |
| Belirli bir yılda nesnelerin ___ dayanıklılık sağlamak üzere tasarlanmış                                          | en az %99.999999999 (11 9'ın) | en az %99.9999999999 (12 9'ın) | en az %99.99999999999999 (16 9'ın) | en az %99.99999999999999 (16 9'ın) |
| Desteklenen depolama hesap türleri                                                                   | GPv1, GPv2, Blob                | GPv2                             | GPv1, GPv2, Blob                     | GPv1, GPv2, Blob                     |

Bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) farklı artıklık seçenekleri hakkında bilgi fiyatlandırma için.

> [!NOTE]
> Premium depolama yalnızca yerel olarak yedekli depolama (LRS) destekler. Premium depolama hakkında daha fazla bilgi için bkz: [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../../virtual-machines/windows/premium-storage.md).

## <a name="changing-replication-strategy"></a>Çoğaltma stratejisi değiştirme
Biz kullanarak depolama hesabınızın çoğaltma stratejisi değiştirmenize izin [Azure portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), ya da çok birini [ Azure istemci kitaplıkları](https://docs.microsoft.com/azure/index?view=azure-dotnet#pivot=sdkstools). Depolama hesabınızın çoğaltma türünü değiştirme süresini neden değil.

   > [!NOTE]
   > Şu anda, hesabınız için ZRS dönüştürmek için Portal veya API kullanamazsınız. Hesabınızın çoğaltma için ZRS dönüştürmek istiyorsanız, bkz: [bölge olarak yedekli depolama (ZRS)](storage-redundancy-zrs.md) Ayrıntılar için.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Hesabımı ait çoğaltma stratejisi değiştirme maliyetlerin var mı?
Dönüştürme yolda bağlıdır. Ucuz en pahalı artıklık teklifine sıralama LRS, ZRS, GRS ve RA-GRS sunuyoruz. Örneğin, *gelen* herhangi bir şeye LRS, ek ücretlere erişimliye, daha karmaşık bir artıklık düzeyini devam ediyor. Giden *için* GRS veya RA-GRS erişimliye bir çıkış bant genişliği ücret çünkü verilerinizi (bölgenizde birincil) uzaktan, ikincil bölge'ye çoğaltılır. İlk kurulum sırasında tek seferlik bir ücret budur. Verileri kopyaladıktan sonra başka hiçbir dönüştürme ücretleri vardır. Yalnızca herhangi bir yeni çoğaltmak için sizden ücret alınır veya var olan verilere güncelleştirmeler. Bant genişliği ücretleri hakkında daha fazla bilgi için bkz: [Azure depolama Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/storage/blobs/).

LRS için GRS değiştirirseniz, ek bir maliyet yoktur, ancak, çoğaltılmış verileri ikincil konumdan silinir.

## <a name="see-also"></a>Ayrıca bkz.

- [Yerel olarak yedekli depolama (LRS): Azure Storage için düşük maliyetli veri artıklığı](storage-redundancy-lrs.md)
- [Bölge olarak yedekli depolama (ZRS): yüksek oranda kullanılabilir Azure Storage uygulamaları](storage-redundancy-zrs.md)
- [Coğrafi olarak yedekli depolama (GRS): Azure Storage için çapraz bölge çoğaltma](storage-redundancy-grs.md)
- [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md)
- [RA-GRS depolama kullanarak yüksek oranda kullanılabilir uygulamalar tasarlama](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure Depolama artıklık seçenekleri ve okuma erişimi coğrafi olarak yedekli depolama ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP belgesi - Azure Storage: Yüksek oranda kullanılabilir depolama sahip bulut hizmeti güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
