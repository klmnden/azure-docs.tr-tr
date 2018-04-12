---
title: Azure Dosyaları Yedeklemesinde sorun giderme
description: Bu makalede, Azure dosya paylaşımlarınızın korunması sırasında oluşan sorunlarla ilgili sorun giderme bilgileri verilmektedir.
services: backup
ms.service: backup
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: 2e067e0a1f673480bc08abfee61d2b1b2c92f885
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-problems-backing-up-azure-files"></a>Azure Dosyaları’nı yedekleme sırasında karşılaşılan sorunları giderme
Aşağıdaki tablolarda listelenen bilgilerle Azure Dosyaları yedeklemesi kullanılırken karşılaşılan sorunları ve hataları giderebilirsiniz.

## <a name="preview-boundaries"></a>Önizleme sınırları
Azure Dosyaları’nı yedekleme Önizleme sürümündedir. Aşağıdaki yedekleme senaryoları, Azure dosya paylaşımları için desteklenmemektedir:
- [Bölgesel olarak yedekli depolama](../storage/common/storage-redundancy-zrs.md) (ZRS) veya [okuma erişimli coğrafi olarak yedekli depolama](../storage/common/storage-redundancy-grs.md) (RA-GRS) çoğaltması ile Depolama Hesaplarında Azure dosya paylaşımlarını koruma.
- Sanal Ağların etkin olduğu Depolama Hesaplarında Azure dosya paylaşımlarını koruma.
- PowerShell veya CLI kullanarak Azure dosya paylaşımlarını yedekleme.

### <a name="limitations"></a>Sınırlamalar
- Günlük en fazla #Zamanlanan-yedekleme 1’dir.
- Günlük en fazla #İsteğe-Bağlı-yedekleme 4’tür.
- Kurtarma Hizmetleri kasanızdaki Yedeklemelerin yanlışlıkla silinmesini önlemek için Depolama Hesabı’ndaki kaynak kilitlerini kullanın.
- Azure Backup tarafından oluşturulan anlık görüntülerin silmeyin. Anlık görüntülerin silinmesi, Kurtarma Noktalarının kaybolması veya Geri Yükleme işlemlerinin başarısız olmasıyla sonuçlanabilir

## <a name="configuring-backup"></a>Yedeklemeyi yapılandırma
Aşağıdaki tablo, yedeklemenin yapılandırılmasına yöneliktir:

| Yedeklemeyi yapılandırma | Geçici çözüm veya çözümleme ipuçları |
| ------------------ | ----------------------------- |
| Azure dosya paylaşımına yönelik yedeklemeyi yapılandırmak için Depolama Hesabımı bulamıyorum | <ul><li>Bulma işlemi tamamlanana kadar bekleyin. <li>Depolama hesabından herhangi bir Dosya paylaşımının zaten başka bir Kurtarma Hizmetleri kasası ile korunup korunmadığını denetleyin. **Not**: Bir Depolama Hesabındaki tüm dosya paylaşımları tek bir Kurtarma Hizmetleri kasası kapsamında korunabilir. <li>Desteklenmeyen herhangi bir Depolama Hesabında Dosya paylaşımının mevcut olmadığından emin olun.|
| Portal durumlarında hata oluştu. Depolama hesaplarını bulma işlemi başarısız oldu. | Aboneliğiniz iş ortağı hesabıysa (CSP etkin) hatayı yoksayın. Aboneliğinizde CSP etkin değilse ve depolama hesaplarınız bulunamıyorsa desteğe başvurun.|
| Seçilen Depolama Hesabı doğrulaması veya kaydı başarısız oldu.| İşlemi yeniden deneyin, sorun devam ederse desteğe başvurun.|
| Seçilen Depolama Hesabında Dosya paylaşımları listelenemedi veya bulunamadı. | <ul><li> Kaynak Grubunda Depolama Hesabının mevcut olduğundan (ve silinmediğinden veya kasadaki son doğrulamadan/kayıttan sonra taşınmadığından) emin olun.<li>Korumak istediğiniz Dosya paylaşımının silinmemiş olduğundan emin olun. <li>Depolama Hesabının, Dosya paylaşımı yedeklemesi için desteklenen bir depolama hesabı olduğundan emin olun.<li>Dosya paylaşımının, aynı Kurtarma Hizmetleri kasasında zaten korumalı olup olmadığını denetleyin.|
| Yedekleme Dosyası paylaşım yapılandırması (veya koruma ilkesi yapılandırması) başarısız oluyor. | <ul><li>Sorunun devam edip etmediğini görmek için işlemi yeniden deneyin. <li> Korumak istediğiniz Dosya paylaşımının silinmemiş olduğundan emin olun. <li> Aynı anda birden çok Dosya paylaşımını korumaya çalışıyorsanız ve dosya paylaşımlarının bir kısmı başarısız oluyorsa başarısız Dosya paylaşımları için yedeklemeyi yapılandırmayı tekrar deneyin. |
| Bir Dosya paylaşımının koruması kaldırıldıktan sonra Kurtarma Hizmetleri kasası silinemiyor. | Azure portalında **Yedekleme Altyapısı** > **Depolama hesapları**’nı açın ve Kurtarma Hizmetleri kasasından depolama hesabını kaldırmak için **Kaydı Kaldır**’a tıklayın.|


## <a name="error-messages-for-backup-or-restore-job-failures"></a>Başarısız Yedekleme veya Geri Yükleme işlemleri için hata iletileri

| Hata iletileri | Geçici çözüm veya çözümleme ipuçları |
| -------------- | ----------------------------- |
| Dosya paylaşımı bulunamadığından işlem başarısız oldu. | Korumak istediğiniz Dosya paylaşımının silinmemiş olduğundan emin olun.|
| Depolama hesabı bulunamadı veya desteklenmiyor. | <ul><li>Depolama hesabının Kaynak Grubunda mevcut olduğundan ve silinmediğinden veya son doğrulamadan sonra Kaynak Grubundan kaldırılmadığından emin olun. <li> Depolama hesabının, Dosya paylaşımı yedeklemesi için desteklenen bir Depolama hesabı olduğundan emin olun.|
| Bu dosya paylaşımı için anlık görüntü sayısı üst sınırına ulaştınız, eski görüntülerin süresi dolduktan sonra yenilerini almanız mümkün olacaktır. | <ul><li> Bu hata, bir Dosya için birden fazla isteğe bağlı yedekleme oluşturduğunuzda oluşabilir. <li> Azure Backup tarafından alınanlar da dahil olmak üzere, Dosya paylaşımı başına 200 anlık görüntü sınırı söz konusudur. Zamanlanmış olan daha eski yedeklemeler (veya anlık görüntüler) otomatik olarak temizlenir. Üst sınıra ulaşılırsa isteğe bağlı yedeklemelerin (veya anlık görüntülerin) silinmesi gerekir.<li> Azure Dosyaları portalından isteğe bağlı yedeklemeleri (Azure dosya paylaşımı anlık görüntülerini) silin. **Not**: Azure Backup tarafından oluşturulan anlık görüntüleri silerseniz kurtarma noktalarını kaybedersiniz. |
| Depolama hizmeti azaltması nedeniyle Dosya paylaşımını yedekleme veya geri yükleme başarısız oldu. Bunun sebebi, depolama hizmetinin, söz konusu depolama hesabına yönelik diğer istekleri işlemekle meşgul olmasından kaynaklanıyor olabilir.| Bir süre sonra işlemi yeniden deneyin. |
| Hedef Dosya Paylaşımı Bulunamadı hatası ile geri yükleme başarısız oldu. | <ul><li>Seçilen Depolama Hesabının mevcut olduğundan ve Hedef Dosya paylaşımının silinmediğinden emin olun. <li> Depolama Hesabının, Dosya paylaşımı yedeklemesi için desteklenen bir depolama hesabı olduğundan emin olun. |
| Sanal Ağların etkin olduğu Depolama Hesaplarında Azure Dosyaları için Azure Backup şu an için desteklenmemektedir. | Başarılı yedeklemeler ve geri yükleme işlemleri için Depolama Hesabınızda Sanal Ağları devre dışı bırakın. |
| Depolama hesabının Kilitli durumda olması nedeniyle Yedekleme veya Geri Yükleme işleri başarısız oldu. | Depolama Hesabı üzerindeki kilidi kaldırın veya okuma kilidi yerine silme kilidini kullanarak işlemi yeniden deneyin. |
| Başarısız dosyaların sayısı, eşik değerinden daha fazla olduğu için kurtarma başarısız oldu. | <ul><li> Kurtarma hatası nedenleri bir dosyada listelenmektedir. (Yol, İş ayrıntılarında belirtilir.) Lütfen hataları giderin ve yalnızca başarısız dosyalar için geri yükleme işlemini yeniden deneyin. <li> Dosya geri yükleme hatalarının sık karşılaşılan nedenleri şunlardır: <br/> - Başarısız dosyaların o sırada kullanımda olması, <br/> - Üst dizinde başarısız dosyayla aynı ada sahip bir dizinin mevcut olması. |
| Hiçbir dosya kurtarılamadığı için kurtarma başarısız oldu. | <ul><li> Kurtarma hatası nedenleri bir dosyada listelenmektedir. (Yol, İş ayrıntılarında belirtilir.) Hataları giderin ve yalnızca başarısız dosyalar için geri yükleme işlemlerini yeniden deneyin. <li> Dosya geri yükleme hatasının sık karşılaşılan nedenleri şunlardır: <br/> - Başarısız dosyaların o sırada kullanımda olması. <br/> - Üst dizinde başarısız dosyayla aynı ada sahip bir dizinin mevcut olması. |
| Kaynaktaki dosyalardan biri mevcut olmadığından geri yüklemenin başarısız olmaktadır. | <ul><li> Seçilen öğeler kurtarma noktası verilerinde mevcut değildir. Dosyaları kurtarmak için doğru dosya listesini sağlayın. <li> Kurtarma noktasına karşılık gelen dosya paylaşımı anlık görüntüsü el ile silindi. Farklı bir kurtarma noktası seçin ve geri yükleme işlemini yeniden deneyin. |
| Aynı hedefe yönelik olarak devam eden bir Kurtarma işi mevcuttur. | <ul><li>Dosya paylaşımı yedeklemesi, aynı hedef Dosya paylaşımına yönelik paralel kurtarmayı desteklememektedir. <li>Mevcut kurtarma işleminin tamamlanmasını bekleyip tekrar deneyin. Kurtarma Hizmetleri kasasında bir kurtarma işi bulamazsanız aynı abonelikte yer alan diğer Kurtarma Hizmetleri kasalarını denetleyin. |
| Hedef dosya paylaşımı dolu olduğundan geri yükleme işlemi başarısız olmuştur. | Hedef dosya paylaşımı için boyut kotasını geri yükleme verilerine uyacak şekilde artırın ve işlemi yeniden deneyin. |
| Bir veya daha fazla dosyayı kurtarma başarısız oldu. Daha fazla bilgi için, yukarıda verilen yoldaki başarısız dosya listesini kontrol edin. | <ul> <li> Kurtarma hatasının nedenleri dosyada listelenmektedir (yol, İş ayrıntılarında belirtilir), nedenleri giderin ve yalnızca başarısız dosyalar için geri yükleme işlemini yeniden deneyin. <li> Dosya geri yükleme hatalarının sık karşılaşılan nedenleri şunlardır: <br/> - Başarısız dosyaların o sırada kullanımda olması. <br/> - Üst dizinde başarısız dosyalarla aynı ada sahip bir dizinin mevcut olması. |

## <a name="see-also"></a>Ayrıca Bkz.
Azure dosya paylaşımlarının yedeklenmesi hakkında daha fazla bilgi için bkz.:
- [Azure dosya paylaşımlarını yedekleme](backup-azure-files.md)
- [Azure dosya paylaşımlarını yedekleme ile ilgili SSS](backup-azure-files-faq.md)
