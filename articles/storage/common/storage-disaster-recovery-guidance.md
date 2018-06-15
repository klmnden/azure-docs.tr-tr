---
title: Azure Storage kesinti durumunda yapmanız gerekenler | Microsoft Docs
description: Azure Storage kesinti durumunda yapmanız gerekenler
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: tamram
ms.openlocfilehash: 3c313025917bba06675d3b2d844a6740fab89fbc
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30323160"
---
# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Bir Azure depolama kesinti oluşursa yapmanız gerekenler
Microsoft, biz sabit hizmetlerimizle her zaman kullanılabilir olduğundan emin olmak için çalışır. Bazen bizim denetim etkisi bize bir veya daha fazla bölgelerde planlanmayan hizmet kesintileri neden yolla zorlar. Bu nadir örnekleri işlemenize yardımcı olmak için Azure depolama hizmetleri için aşağıdaki üst düzey Kılavuzu sunuyoruz.

## <a name="how-to-prepare"></a>Hazırlama
Her müşterinin kendi olağanüstü durum kurtarma planını hazırlaması kritik öneme sahiptir. Genellikle bir depolama kesintisinden kurtarma çaba çalışır duruma uygulamalarınızda yeniden etkinleştirmek için işlem personeli ve otomatik yordamları içerir. Lütfen kendi olağanüstü durum kurtarma planınızı oluşturmak için aşağıdaki Azure belgelerine bakın:

* [Kullanılabilirlik denetim listesi](https://docs.microsoft.com/azure/architecture/checklist/availability)
* [Azure için dayanıklı uygulamalar tasarlama](https://docs.microsoft.com/azure/architecture/resiliency/)
* [Azure Site Recovery hizmeti](https://azure.microsoft.com/services/site-recovery/)
* [Azure Depolama çoğaltması](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Azure yedekleme hizmeti](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Nasıl algılanacağını
Abone olmak için Azure hizmet durumunu belirlemek için önerilen yöntem olduğu [Azure hizmet sağlığı panosunu](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Bir depolama kesinti oluşursa yapmanız gerekenler
Bir veya daha fazla depolama hizmetleri bir veya daha fazla bölgelerinde aynı geçici olarak kullanılamıyor, dikkate almanız gereken iki seçenek vardır. Lütfen verilerinizi anında erişim üzerinde isterse seçeneği 2 göz önünde bulundurun.

### <a name="option-1-wait-for-recovery"></a>Seçenek 1: Kurtarma için bekleyin
Bu durumda, herhangi bir eyleminiz gereklidir. Titizlikle Azure hizmeti kullanılabilirlik geri yüklemek için çalışıyoruz. Hizmet durumunu izleyebilirsiniz [Azure hizmet sağlığı panosunu](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Seçenek 2: ikincil veri kopyalama
Seçerseniz [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (depolama hesapları için önerilir), okuma erişimi verilerinizi ikincil bölgesinden gerekir. Araçları gibi kullanabilir [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)ve [Azure veri hareketi Kitaplığı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) veri unimpacted bir bölgede başka bir depolama hesabı ikincil bölgesinden kopyalayın ve hem okuma hem kullanılabilirlik için bu depolama hesabı, uygulamalarınızı noktası.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Bir depolama yük devretme oluşursa beklenmesi gerekenler
Seçerseniz [coğrafi olarak yedekli depolama (GRS)](storage-redundancy-grs.md) veya [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (önerilen), Azure Storage verilerinizin iki bölgelerde (birincil ve ikincil) dayanıklı tutar. Her iki bölgelerde, Azure Storage verilerinizin birden çok çoğaltma sürekli tutar.

Bölgesel bir olağanüstü durumda birincil bölge etkilediğinde, biz öncelikle bu bölgede hizmeti geri yüklemeye çalışır. Olağanüstü durum ve bazı nadir durumlarda, kendi etkileri doğasına bağlı biz birincil bölge geri yükleme mümkün olmayabilir. Bu noktada, biz bir coğrafi yük devretme işlemini gerçekleştirecek. Bölgeler arası veri çoğaltma ikincil bölge'ye henüz çoğaltılmamış değişiklikler kaybolabilir mümkün olacak şekilde, bir gecikme içerebileceği zaman uyumsuz bir işlemdir. Sorgulayabileceğiniz ["Son eşitleme zamanı" değerini depolama hesabınızın](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) çoğaltma durumu hakkında bilgi almak için.

Depolama yük devretme coğrafi deneyimi ilgili noktaları birkaç:

* Depolama coğrafi yük devretme Azure depolama ekibi tarafından yalnızca tetiklenecek – gerekli müşteri eylemi yok.
* Var olan Depolama Hizmeti uç noktalarınızı BLOB'lar, tablolar, kuyruklar ve dosya için aynı yük devretme sonrasında kalır; Microsoft tarafından sağlanan DNS girişini birincil bölgesinden ikincil bölge'ye geçiş yapmak için güncelleştirilmesi gerekir.  Microsoft, bu güncelleştirme coğrafi yük devretme işleminin bir parçası olarak otomatik olarak gerçekleştirir.
* Önce ve coğrafi-yük devretme sırasında depolama hesabınıza etkisi, olağanüstü durum nedeniyle yazma erişimine sahip olmaz ancak depolama hesabınız RA-GRS yapılandırılmışsa, ikincil hala okuyabilirsiniz.
* Yük devretme coğrafi tamamlandı ve DNS değişiklikleri yayılan, okuma ve yazma erişimi, depolama hesabınıza sürdürülecek; başka bir işaret ne ikincil uç noktanız olması için kullanılır. 
* GRS veya RA-GRS depolama hesabı için yapılandırılmış varsa yazma erişimi gerekir unutmayın. 
* Sorgulayabileceğiniz ["Son coğrafi yük devretme zaman" değerini depolama hesabınızın](https://msdn.microsoft.com/library/azure/ee460802.aspx) daha fazla bilgi almak için.
* Yük devretme sonrasında depolama hesabınız tam olarak çalışan ancak bir "bozuk" durumunda olarak gerçekte hiçbir coğrafi çoğaltma olası sahip bir tek başına bölge barındırılır. Bu riski azaltmak için özgün birincil bölge geri ve özgün durumuna geri yüklemek için bir coğrafi yeniden yapmak. Özgün birincil bölge kurtarılamaz ise, biz başka bir ikincil bölge ayırır.
  Azure Storage coğrafi çoğaltma'nın altyapısı hakkında daha fazla ayrıntı için depolama ekibi blogu makale hakkında lütfen [artıklık seçenekleri ve RA-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

## <a name="best-practices-for-protecting-your-data"></a>Verilerinizi korumak için en iyi uygulamalar
Düzenli olarak depolama verileri yedeklemek için bazı önerilen yaklaşım vardır.

* VM diskleri – [Azure Backup hizmeti](https://azure.microsoft.com/services/backup/) , Azure sanal makineler tarafından kullanılan VM diskleri yedeklenir.
* Blok blobları – Oluştur bir [anlık görüntü](https://msdn.microsoft.com/library/azure/hh488361.aspx) her bir blok blobunda veya başka bir bölge kullanarak başka bir depolama hesabı BLOB kopyalama [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), veya [Azure veri hareketi Kitaplığı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Tabloları – kullanın [AzCopy](storage-use-azcopy.md) tablo verileri başka bir bölgede başka bir depolama hesabı verilecek.
* Dosyalar – kullanmak [AzCopy](storage-use-azcopy.md) veya [Azure PowerShell](storage-powershell-guide-full.md) başka bir bölgede başka bir depolama hesabı dosyalarınızı kopyalamak için.

RA-GRS özelliğini tam olarak yararlanmasını uygulamaları oluşturma hakkında daha fazla bilgi için lütfen kullanıma [tasarlama yüksek oranda kullanılabilir RA-GRS depolama kullanan uygulamalar](../storage-designing-ha-apps-with-ragrs.md)

