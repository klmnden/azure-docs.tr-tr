---
title: Azure storage'da veri kopyalama | Microsoft Docs
description: Microsoft Azure depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik için çoğaltılır. Yerel olarak yedekli depolama (LRS), bölgesel olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) çoğaltma seçenekleri içerir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/08/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 3d6d3184c2a17e397e5bbba9fff6cf9c3de9b73e
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218500"
---
# <a name="azure-storage-replication"></a>Azure Storage çoğaltma

Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır. Azure depolama çoğaltma, geçici donanım hataları, ağ veya bölgesel elektrik kesintileriyle, çok büyük doğal felaketlere ve benzeri arasında planlanmış ve planlanmamış olaylardan korunur verilerinizi kopyalar. Aynı bölge içinde Bölgesel veri merkezleri arasında ve bölgeler arasında bile verilerinizi aynı veri merkezinde çoğaltmayı seçebilirsiniz.

Çoğaltma işlemi, hata durumunda bile depolama hesabınızın [Depolama için Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) uymasını sağlar. Azure Depolama'nın dayanıklılık ve kullanılabilirlikle ilgili sağladığı garantiler hakkında bilgi edinmek için SLA'ya göz atın.

## <a name="choosing-a-replication-option"></a>Bir çoğaltma seçeneği belirleyerek

Bir depolama hesabı oluşturduğunuzda şu çoğaltma seçeneklerinden birini seçebilirsiniz:

* [Yerel olarak yedekli depolama (LRS)](storage-redundancy-lrs.md)
* [Bölgesel olarak yedekli depolama (ZRS)](storage-redundancy-zrs.md)
* [Coğrafi olarak yedekli depolama (GRS)](storage-redundancy-grs.md)
* [Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

Aşağıdaki tabloda, dayanıklılık ve kullanılabilirlik her çoğaltma stratejinizi olay (veya benzer bir etkisi olması durumunda) verilen tür için sağlayacak kapsamını hızlı bir genel bakış sağlar.

| Senaryo                                                                                                 | LRS                             | ZRS                              | GRS                                  | RA-GRS                               |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Bir veri merkezi içinde düğüm kullanılamazlık                                                                 | Evet                             | Evet                              | Evet                                  | Evet                                  |
| Tüm veri merkezinde (Bölgesel veya bölgesel olmayan) kullanılamaz                                           | Hayır                              | Evet                              | Evet                                  | Evet                                  |
| Bir bölge çapında kesinti                                                                                     | Hayır                              | Hayır                               | Evet                                  | Evet                                  |
| Bölge genelinde kullanım dışı kalması durumunda (bir bölgede uzaktan, coğrafi olarak çoğaltılmış) verilerinize okuma erişimi | Hayır                              | Hayır                               | Hayır                                   | Evet                                  |
| Sağlamak üzere tasarlanmış \_ \_ belirli bir yıl boyunca nesnelerin dayanıklılık                                          | en az % 99,999999999 (11 9) | en az % 99,9999999999 (12 9) | en az % 99,99999999999999 (16 9) | en az % 99,99999999999999 (16 9) |
| Desteklenen depolama hesabı türleri                                                                   | GPv1, GPv2 ve Blob                | GPv2                             | GPv1, GPv2 ve Blob                     | GPv1, GPv2 ve Blob                     |
| Okuma istekleri için kullanılabilirlik SLA'sı | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,99 oranında (seyrek erişimli katman için % 99,9) |
| Yazma isteklerine ilişkin kullanılabilirlik SLA'sı | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) |

Fiyatlandırma bilgileri her yedekliliği seçeneği için bkz: [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/). 

Azure depolama hakkında bilgi için dayanıklılık ve kullanılabilirlik garanti eder için bkz: [Azure depolama SLA'sı](https://azure.microsoft.com/support/legal/sla/storage/).

> [!NOTE]
> Premium depolama yalnızca yerel olarak yedekli depolama (LRS) destekler. Premium depolama hakkında daha fazla bilgi için bkz: [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../../virtual-machines/windows/premium-storage.md).

## <a name="changing-replication-strategy"></a>Çoğaltma stratejisi değiştirme
Biz kullanarak depolama hesabınızın çoğaltma stratejinizi değiştirmenize izin [Azure portalında](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), veya çok [ Azure istemci kütüphaneleri](https://docs.microsoft.com/azure/index?view=azure-dotnet#pivot=sdkstools). Depolama hesabınızın çoğaltma türünü değiştirme, süresini sonuçlanmaz.

   > [!NOTE]
   > Şu anda ZRS için hesabınızı dönüştürün için portalı veya API kullanamazsınız. Hesabınızın çoğaltma için ZRS dönüştürmek istiyorsanız, bkz. [bölgesel olarak yedekli depolama (ZRS)](storage-redundancy-zrs.md) Ayrıntılar için.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Hesabımın çoğaltma stratejinizi değiştirme maliyetlerin var mı?
Bu dönüştürme path değişkeninize bağlıdır. Ucuz en pahalı yedeklilik teklife sıralama LRS, ZRS, GRS ve RA-GRS sahibiz. Örneğin, *gelen* şeye LRS belirlenen ek ücretleri ödemesi daha karmaşık bir yedeklilik düzeyini devam ediyor. Giden *için* GRS veya RA-GRS bir çıkış bant genişliği ücret doğurur çünkü verilerinizi (bölgenizde birincil), uzak ikincil bölgeye çoğaltılır. Bu tek seferlik bir ücret ilk kurulum sırasında dir. Veri kopyalandıktan sonra daha fazla dönüştürme ücretlendirme yoktur. Tüm yeni çoğaltmak için yalnızca ücretlendirilirsiniz veya mevcut verileri güncelleştirme. Bant genişliği ücretlerine ilişkin daha fazla ayrıntı için bkz. [Azure depolama fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

LRS için GRS değiştirirseniz, hiçbir ek ücret yoktur, ancak ikincil konumdan, çoğaltılan veriler silinir.

## <a name="see-also"></a>Ayrıca bkz.

- [Yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](storage-redundancy-lrs.md)
- [Bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar](storage-redundancy-zrs.md)
- [Coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md)
- [Azure depolama ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md)
- [RA-GRS depolama kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure depolama yedekliliği seçenekleri ve okuma erişimli coğrafi olarak yedekli depolama ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP belgesi - Azure Depolama: Yüksek oranda kullanılabilir depolama ile bulut hizmeti güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
