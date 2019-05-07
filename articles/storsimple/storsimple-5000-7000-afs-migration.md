---
title: Veri Storsimple'dan 5000-7000 Serisi için Azure dosya eşitleme taşıma | Microsoft Docs
description: Azure dosya eşitleme (AFS) için StorSimple 5000/7000 serisinden veri geçirmeyi açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: b46e9ee8fc3e14981a01cc2425a8ce55d06c5a9a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150747"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-azure-file-sync"></a>Azure dosya eşitleme için StorSimple 5000-7000 serisinden veri geçirme

> [!IMPORTANT]
> 9 Temmuz 2019, StorSimple 5000/7000 Serisi (EOS) destek durumu sonuna ulaşacak. StorSimple 5000/7000 Serisi müşteriler belgede açıklanan alternatifleri birine geçiş öneririz.

Veri geçişi veri depolama tek bir konumdan diğerine taşınmasını işlemidir. Bu verilerin bir kuruluşun geçerli bir CİHAZDAN başka bir cihaz için tam kopyalayarak gerektirir — tercihen kesintiye veya etkin uygulamalar devre dışı bırakma olmadan — ve ardından yeni cihaz için tüm giriş/çıkış (g/ç) etkinlikleri yeniden yönlendirme. 

StorSimple 5000 ve 7000 Serisi depolama cihazları içinde Temmuz 2019 hizmet sonuna ulaşacak. Bu, Microsoft artık donanım ve yazılım Temmuz 2019 sonra için StorSimple 5000/7000 Serisi destekleyebilen olacağını gösterir. Bu cihazları kullanan müşteriler, azure'da diğer karma depolama çözümleri için StorSimple verilerine geçirmeniz gerekir. Bu makale, bir StorSimple 5000/7000 Serisi CİHAZDAN verileri Azure dosya eşitleme (AFS) geçişini kapsar.

## <a name="intended-audience"></a>Hedef kitle

Bu makalede, bilgi teknolojisi (BT) uzmanları ve StorSimple 5000/7000 Serisi cihazlar datacentre dağıtıp sorumlu olan bilgi çalışanları için tasarlanmıştır. Dosya sunucusu iş yükleri (ile Windows Server) için StorSimple cihazlarını kullanan müşteriler bu geçiş yolu özellikle çekici fark edebilirsiniz. Düşünüyorsanız de kuruluşunuz için Azure dosya eşitleme özellikleri çalışmaz ve bu makalede, bu çözümleri Storsimple'dan taşıma anlamanıza yardımcı olacaktır.

## <a name="migration-considerations"></a>Geçiş konuları

Bu işlem, depolama için StorSimple birimini kullanarak bir Windows dosya paylaşımı yapılandıran müşteriler için çalışır. Azure dosya eşitleme StorSimple 5000/7000'den geçirme verilerini içerir, dosya paylaşım konumunu dönüştürme bir [sunucu uç noktası](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) ve ardından yeni konumunuzu olacak başka bir uç bağlı sürücüleri yerel olarak kullanma. 

Aşağıdaki noktaları için AFS taşırken bulundurulmalıdır:

1. Azure dosyaları şu anda bir 5 TB/paylaşma kısıtlaması yok. Bu kısıtlama, Azure dosya eşitleme yayılmış birden çok Azure dosya paylaşımlarında veri kullanarak üstesinden gelebilir. Daha fazla bilgi için gözden [veri büyüme modelini Azure dosyaları dağıtımını](https://docs.microsoft.com/azure/storage/files/storage-files-planning).
2. Bu geçiş tüm birincil veri kümesi için bir şirket içi cihaz indirir CİHAZDAN şirket içi veri kopyalama gerçekleştirilir. Bu aktarım uyum sağlamak için yeterli bant genişliği olduğundan emin olun.
3. Bu işlem, önceden oluşturulmuş anlık görüntüleri korumaz. Yalnızca, birincil veri geçirir. İşlem, ilişkili bant genişliği şablonları veya yedekleme ilkelerini de korumaz. [Azure Yedekleme'yi](https://docs.microsoft.com/azure/backup/backup-azure-files) paylaşımı verileri Azure dosya geçirildikten sonra yedekleme ilkeleri ayarlamak için.
4. StorSimple, birinci taraf donanımı sağlar. Ancak, Azure dosyaları/Azure dosya eşitleme ile yerel önbelleği olarak kendi yerel Windows sunucu donanımı kullanıyorsanız. Veri kümesi, tercih ettiğiniz yerel olarak koru üzere yeterli depolama kapasitesine sahip emin olmanız gerekir. Katmanlama ve önkoşul boş alan hedef ayarı hakkında daha fazla bilgi için gözden geçirmek için nasıl [Azure dosya eşitleme dağıtırken sunucu uç noktası oluşturma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal). 
5. Gözden geçirme [Azure dosya eşitleme için fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/files/) Storsimple'dan değiştikçe. Yinelenenleri kaldırma ve sıkıştırma gibi StorSimple AFS yok.

## <a name="migration-prerequisites"></a>Geçiş önkoşulları

Burada, eski 5000 veya 7000 Serisi Cihazınızı için Azure dosya eşitleme geçiş Önkoşullar bulacaksınız. Başlamadan önce olduğundan emin olun:

- Bir StorSimple 5000/7000 Serisi cihaz çalışan yazılım sürümü v2.1.1.518 veya daha sonra erişimi. Önceki sürümleri desteklenmez. Web kullanıcı Arabirimi StorSimple cihazınızın sağ üst köşesindeki çalıştırılan yazılım sürümü görüntülemelidir.  
    Lütfen Cihazınızı v2.1.1.518 çalışmıyorsa, sisteminiz için gerekli en düşük sürümü yükseltin. Ayrıntılı yönergeler için başvurmak [v2.1.1.518 için sisteminizi yükseltin](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518).
- Kaynak cihazda çalışmakta olan tüm etkin yedekleme işlerini denetleyin. Bu StorSimple veri koruma konsol konağı işleri içerir. Geçerli işlerinin tamamlanmasını bekleyin. 
- Bir Windows Server konağının erişimi için StorSimple 5000-7000 Serisi cihaz bağlı. Ana bilgisayar desteklenen bir Windows Server sürümünü açıklandığı çalıştırmalıdır [Azure dosya eşitleme birlikte çalışabilirlik](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning).
- StorSimple birimleri konakta bağlanır ve dosya paylaşımları içeren.
- Ana bilgisayar, yerel olarak önbelleğe alınan verileri tutmak için yeterli yerel depolama alanına sahiptir.
- Azure dosya eşitleme dağıtmak için kullanacağınız Azure aboneliği düzeyinde erişim sahibi. Sahibi veya yönetici düzeyi izinler yoksa eşitleme grubunuz için bir bulut uç noktası oluştururken sorunlarla karşılaşabilirsiniz.
- Erişim bir [genel amaçlı v2 depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-account-overview) ile eşitlemek için istediğiniz bir Azure dosya paylaşımı. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
  - Nasıl yapılır [bir Azure dosya paylaşımı oluşturma](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

## <a name="migration-process"></a>Geçiş işlemi

Storsimple'dan verileri geçirmeyi 5000-7000 AFS için iki adımlı bir işlemdir:
1.  StorSimple birimlerini bir Azure dosyaları'na bağlı olduğu şirket içi dosya sunucusundan verileri çoğaltma paylaşın.  Çoğaltma, yüklediğiniz bir AFS aracı gerçekleştirilir.
2.  StorSimple cihaz bağlantısını kesin. Yerel diskler, ardından yerel önbelleği olarak davranır.

### <a name="migration-steps"></a>Geçiş adımları

Azure dosya eşitleme paylaşımına StorSimple birimlerde yapılandırılmış Windows dosya paylaşımına geçirmek için aşağıdaki adımları gerçekleştirin. 
1.  Bu adımlar StorSimple birimlerini burada bağlanan veya farklı bir sistem kullanın aynı Windows Server ana bilgisayarında gerçekleştirin. 
    - [Windows Server'ın Azure dosya eşitleme ile kullanmak üzere hazırlama](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#prepare-windows-server-to-use-with-azure-file-sync).
    - [Azure dosya eşitleme Aracısı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#install-the-azure-file-sync-agent).
    - [Depolama eşitleme hizmetini dağıtma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#deploy-the-storage-sync-service). 
    - [Windows Server depolama eşitleme hizmeti ile kaydetme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#register-windows-server-with-storage-sync-service). 
    - [Bir eşitleme grubuna ve bir bulut uç noktası oluşturma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#create-a-sync-group-and-a-cloud-endpoint). Eşitleme grubu konaktan geçirilmesi gerekiyor her Windows dosya paylaşımı için yapılması gerekir.
    - [Sunucu uç noktası oluşturma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal#create-a-server-endpoint). Dosya Paylaşımı verilerinizi içeren bir StorSimple biriminin yolunu yolunu belirtin. StorSimple birim sürücüdür. Örneğin, `J`, ve verilerinizi bulunan `J:/<myafsshare>`, ardından sunucu uç noktası bu yolu ekleyin. Bırakın **katmanlama** olarak **devre dışı bırakılmış**.
2.  Dosya sunucusu eşitleme işlemi tamamlanana kadar bekleyin. Verilen eşitleme grubundaki her sunucu için emin olun:
    - Denenen son eşitlemenin hem karşıya yükleme ve indirme için zaman damgası, son.
    - Karşıya yükleme ve indirme için yeşil durumudur.
    - **Eşitleme etkinliği** çok az sayıda gösterir ya da eşitlemek için kalan dosya yok.
    - **Dosyaları değil eşitleniyor** hem karşıya yükleme ve indirme 0'dır.
    Sunucu eşitleme tamamlandığında daha fazla bilgi için Git [Azure dosya eşitleme sorunlarını giderme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#how-do-i-know-if-my-servers-are-in-sync-with-each-other). Eşitleme gün cinsinden veri boyutu ve bant genişliğine bağlı olarak birkaç saat sürebilir. Eşitleme tamamlandıktan sonra tüm verilerinizi güvenli bir şekilde Azure dosya paylaşımı olduğu. 
3.  Paylaşımlara StorSimple birimlere gidin. Bir paylaşımı, sütuna sağ tıklayıp seçin **özellikleri**. Paylaşım izinleri altında Not **güvenlik**. Bu izinleri, sonraki adımda yeni bir paylaşım el ile uygulanması gerekecektir.
4.  Aynı Windows Server konağının ya da farklı bir kullanmadığınıza bağlı olarak, sonraki adımlar farklı olacaktır.

    Bu adımı atlayın ve farklı bir Windows Server ana bilgisayar kullanıyorsanız, sonraki adıma gidin. AFS aynı Windows dosya sunucusu kullanıyorsanız, artık birkaç dakika kapalı kalma süresi yaşar. 
    - **Kapalı kalma süresi başlar** -oluşturduğunuz sunucu uç noktasını silme *1F adım*. 
    - Verilerin ileri giderek bulunmasını istediğiniz yol ile yeni bir sunucu uç noktası oluşturun.
    - Sunucu uç noktası (Bu işlem birkaç dakika sürebilir) sağlıklı gösterir. sonra bu yeni konumda veri görürsünüz. Artık bu yeni konumdaki dosyaları sunmak için Windows Server konağınızda yapılandırabilirsiniz. -  **Kapalı kalma süresi sona erer**.
5.  Ardından Azure dosya eşitleme için başka bir Windows dosya sunucusu kullanıyorsanız, herhangi bir kesintiyle karşılaşmayacaksınız. 
    - Başka bir sunucu uç noktası yoluyla StorSimple cihazı yerine bir önbellek olarak kullanmaya hazırsınız yerel depolama ekleyin. 
    - Yeni Sunucu dosyalarında birkaç dakika içinde görmeye olacaktır. Herhangi bir zamanda StorSimple Cihazınızı bu yeni konuma konaktaki geçiş yapmak ücretsizdir.

    > [!TIP] 
    > Bir BT uğramasını azaltmak için değiştiriyor gibi bu yeni dosya paylaşımı aynı ada ve aynı yol ile yapılandırmayı göz önünde bulundurun. DFS-N kullanıyorsanız, bu yapılandırmada değişiklik yapmanızı gerektirebilir.
6.  İçinde belirtilenlerle paylaşım izinlerini yeniden *3. adım*.

Veri geçişi sırasında herhangi bir sorun yaşarsanız lütfen [Microsoft Destek'e başvur](storsimple-8000-contact-microsoft-support.md). 



## <a name="next-steps"></a>Sonraki adımlar

AFS sizin için doğru çözüm değilse, bilgi nasıl [veri bir StorSimple 5000-7000 Serisi 8000 serisi bir cihaza geçirme](storsimple-8000-migrate-from-5000-7000.md).

