---
title: Azure storage'da veri yedekliği | Microsoft Docs
description: Microsoft Azure depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik için çoğaltılır. Yedeklilik seçenekleri yerel olarak yedekli depolama (LRS), bölgesel olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) içerir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/18/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 078c62913b903eafe9e0fcfcef4189f5ca735d0f
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002817"
---
# <a name="azure-storage-redundancy"></a>Azure depolama yedekliliği

Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır. Azure depolama, verilerinizi, geçici donanım hataları, ağ veya bölgesel elektrik kesintileriyle ve çok büyük doğal afetler gibi planlanan ve planlanmayan olayları, korunur kopyalar. Aynı bölge içinde Bölgesel veri merkezleri, veya bölgeler arasındaki coğrafi olarak ayrılmış verilerinizi aynı veri merkezinde çoğaltmayı seçebilirsiniz.

Çoğaltma işlemi, hata durumunda bile depolama hesabınızın [Depolama için Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) uymasını sağlar. Azure Depolama'nın dayanıklılık ve kullanılabilirlikle ilgili sağladığı garantiler hakkında bilgi edinmek için SLA'ya göz atın.

Azure depolama, Döngüsel artıklık denetimleri (CRC) kullanarak depolanan verilerin bütünlüğünü düzenli olarak doğrular. Veri bozulması algılanırsa, yedek verileri kullanarak onarıldı. Azure depolama, depolama veya veri alma, veri paketlerinin Bozulması algılamak için tüm ağ trafiğini sağlama de hesaplar.

## <a name="choosing-a-redundancy-option"></a>Bir yedeklik seçeneği seçme

Bir depolama hesabı oluşturduğunuzda, yedeklilik şunlardan birini seçebilirsiniz:

* [Yerel olarak yedekli depolama (LRS)](storage-redundancy-lrs.md)
* [Alanlar arası yedekli depolama (ZRS)](storage-redundancy-zrs.md)
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
| Desteklenen depolama hesabı türleri                                                                   | GPv2, GPv1, Blob                | GPv2                             | GPv2, GPv1, Blob                     | GPv2, GPv1, Blob                     |
| Okuma istekleri için kullanılabilirlik SLA'sı | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,99 oranında (seyrek erişimli katman için % 99,9) |
| Yazma isteklerine ilişkin kullanılabilirlik SLA'sı | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) | En az % 99,9 (seyrek erişimli katman için % 99) |

Fiyatlandırma bilgileri her yedekliliği seçeneği için bkz: [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/). 

Azure depolama hakkında bilgi için dayanıklılık ve kullanılabilirlik garanti eder için bkz: [Azure depolama SLA'sı](https://azure.microsoft.com/support/legal/sla/storage/).

> [!NOTE]
> Premium depolama yalnızca yerel olarak yedekli depolama (LRS) destekler.

## <a name="changing-replication-strategy"></a>Çoğaltma stratejisi değiştirme
Kullanarak, depolama hesabınızın çoğaltma stratejinizi değiştirebilirsiniz [Azure portalında](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), veya biri [Azure istemcisi kitaplıkları](https://docs.microsoft.com/azure/index#pivot=sdkstools). Depolama hesabınızın çoğaltma türünü değiştirme, süresini sonuçlanmaz.

   > [!NOTE]
   > Şu anda ZRS için hesabınızı dönüştürün için portalı veya API kullanamazsınız. Hesabınızın çoğaltma için ZRS dönüştürmek istiyorsanız, bkz. [bölgesel olarak yedekli depolama (ZRS)](storage-redundancy-zrs.md) Ayrıntılar için.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Hesabımın çoğaltma stratejinizi değiştirme maliyetlerin var mı?
Bu dönüştürme path değişkeninize bağlıdır. Ucuz en pahalı yedeklilik teklife sıralama LRS, ZRS, GRS ve RA-GRS sahibiz. Örneğin, *gelen* şeye LRS belirlenen ek ücretleri ödemesi daha karmaşık bir yedeklilik düzeyini devam ediyor. Giden *için* GRS veya RA-GRS bir çıkış bant genişliği ücret doğurur çünkü verilerinizi (bölgenizde birincil), uzak ikincil bölgeye çoğaltılır. Bu tek seferlik bir ücret ilk kurulum sırasında dir. Veri kopyalandıktan sonra daha fazla dönüştürme ücretlendirme yoktur. Tüm yeni çoğaltmak için yalnızca ücretlendirilirsiniz veya mevcut verileri güncelleştirme. Bant genişliği ücretlerine ilişkin daha fazla ayrıntı için bkz. [Azure depolama fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

LRS için GRS depolama hesabınızı dönüştürün, hiçbir ek ücret yoktur, ancak ikincil konumdan, çoğaltılan veriler silinir.

GRS veya LRS için RA-GRS depolama hesabınızın dönüştürürseniz, bu hesabı ek 30 gün boyunca dönüştürülüp dönüştürülmediğini tarih ötesinde RA-GRS faturalandırılır.

## <a name="see-also"></a>Ayrıca bkz.

- [Yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](storage-redundancy-lrs.md)
- [Bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar](storage-redundancy-zrs.md)
- [Coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md)
- [Azure depolama ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md)
- [RA-GRS depolama kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure depolama yedekliliği seçenekleri ve okuma erişimli coğrafi olarak yedekli depolama](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP belgesi - Azure Depolama: Güçlü tutarlılık ile yüksek oranda kullanılabilir bulut depolama hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
