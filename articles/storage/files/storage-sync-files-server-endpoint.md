---
title: Bir Azure dosya eşitleme sunucusu uç noktası Ekle/Kaldır | Microsoft Docs
description: Bir Azure dosyaları dağıtımını planlarken dikkate almanız gerekenler öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 31bb71f016dd7f9dd37c766ece25caf8f300754b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686962"
---
# <a name="addremove-an-azure-file-sync-server-endpoint"></a>Bir Azure dosya eşitleme sunucusu uç noktası Ekle/Kaldır
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu Windows sunucularınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

A *sunucu uç noktası* üzerinde belirli bir konumu temsil eden bir *kayıtlı sunucu*, bir sunucu birim veya birim kökünde bir klasör gibi. Kendi ad alanları (örneğin, F:\sync1 ve F:\sync2) çakışan, birden çok sunucu uç noktaları aynı birim üzerinde bulunabilir. Bulut katmanlaması ilkeleri her sunucu uç noktası için ayrı ayrı yapılandırabilirsiniz. Sunucu uç noktası var olan bir dosya kümesi ile bir sunucu konumu bir eşitleme grubuna eklerseniz, bu dosyaları diğer uç noktalardan eşitleme grubu zaten bulunan diğer dosyaları ile birleştirilir.

Bkz: [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md) Azure dosya eşitleme uçtan uca dağıtma hakkında daha fazla bilgi için.

## <a name="prerequisites"></a>Önkoşullar
Sunucu uç noktası oluşturmak için önce aşağıdaki ölçütleri karşılandığından emin olmalısınız: 
- Azure dosya eşitleme aracısının yüklü ve kayıtlı sunucu. Azure dosya eşitleme aracısı yüklemek için yönergeler bulunabilir [kaydı/unregister sunucu bir Azure dosya eşitleme ile](storage-sync-files-server-registration.md) makalesi. 
- Depolama eşitleme hizmeti dağıtıldığını emin olun. Bkz: [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md) depolama eşitleme hizmeti dağıtma hakkında daha fazla ayrıntı için. 
- Bir eşitleme grubuna dağıtıldığını emin olun. Bilgi edinmek için nasıl [bir eşitleme grubu oluşturma](storage-sync-files-deployment-guide.md#create-a sync-group-and-a-cloud-endpoint).
- Sunucu internet'e bağlı olduğundan ve Azure erişilebilir olduğundan emin olun. Hizmetimiz ile sunucu arasındaki tüm iletişimi için 443 numaralı bağlantı noktasını kullanın.

## <a name="add-a-server-endpoint"></a>Sunucu uç noktası ekleme
Sunucu uç noktası eklemek için istenen eşitleme grubuna gidin ve "sunucu uç noktası Ekle"'yi seçin.

![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

Aşağıdaki bilgileri altında gerekli **sunucusu uç noktası ekleme**:

- **Kayıtlı sunucu**: Sunucu veya sunucu uç noktasını oluşturmak için küme adı.
- **Yol**: Eşitleme grubunun bir parçası olarak eşitlenmesi gereken Windows Server'da yolu.
- **Bulut Katmanlaması**: Bir anahtar etkinleştirme veya devre dışı bulut katmanlaması. Etkin olduğunda, katmanlama olacak bulut *katmanı* Azure dosya paylaşımlarınızın dosyaları. Bu şirket içi dosya paylaşımlarını veri kümesine tam bir kopyasını yerine bir önbellek sunucunuzdaki alan verimliliğini yönetmenize yardımcı olmak için dönüştürür.
- **Birim boş alanı**: sunucu uç noktasını bulunduğu birimde ayırmak için boş alan miktarı. Birim boş alanı, tek bir sunucu uç noktası olan bir birimde % 50'si ayarlanırsa, örneğin, kabaca veri miktarının yarısına Azure dosyaları'na katmanlanmış olmaz. Bağımsız olarak, ister bulut katmanlamayı etkin olduğunda, Azure dosya paylaşımınızı, verilerin tam bir kopyasını her zaman eşitleme grubunda sahip.

Seçin **Oluştur** sunucu uç noktası eklemek için. Bir eşitleme grubuna bir ad alanı içindeki dosyalar artık eşitlenmiş tutulacak. 

## <a name="remove-a-server-endpoint"></a>Sunucu uç noktası Kaldır
Azure dosya eşitleme için belirtilen sunucu uç noktası kullanmayı Sonlandırabileceğiniz isterse, sunucu uç noktasını kaldırabilirsiniz. 

> [!Warning]  
> Kaldırarak ve sunucu uç noktası için bir Microsoft mühendisi tarafından açıkça belirtilmediği sürece yeniden eşitleme, bulut katmanlandırma veya herhangi bir Azure dosya eşitleme yönüyle ile ilgili sorunları gidermek çalışmayın. Sunucu uç noktası kaldırma zararlı bir işlemdir ve "sunucu uç noktasını, hangi eşitlenmiş hatalara neden olabilecek yeniden sonra sunucu uç noktasını içinde katmanlı dosyalar için Azure dosya paylaşımında konumlarına bağlanır değil". Ayrıca unutmayın, sunucu uç noktası ad dışında var olan katmanlı dosyalar kalıcı olarak kaybolur. Katmanlı dosyalar, içinde olsa bile sunucu uç noktası mevcut bulut katmanlaması hiçbir zaman etkinleştirilmemiş.

Sunucu uç noktasını kaldırmadan önce tüm katmanlı dosyalar çekilir emin olmak için bulut katmanlaması sunucu uç noktasında devre dışı bırakın ve ardından ad alanınızdaki sunucu uç noktasını tüm katmanlı dosyalar çağırmak için aşağıdaki PowerShell cmdlet'ini yürütün:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Note]  
> Barındırma sunucusu yerel birim katmanlı veriler tüm çağırmak için yeterli boş alana sahip değilse `Invoke-StorageSyncFileRecall` cmdlet'i başarısız.  

Sunucu uç noktasını kaldırmak için:

1. Burada sunucunuzun kayıtlı depolama eşitleme hizmetine gidin.
2. İstenen eşitleme grubuna gidin.
3. Depolama eşitleme hizmeti eşitleme grubunda istediğiniz sunucu uç noktasını kaldırın. Bu eşitleme grubunu bölmesinde ilgili sunucu uç noktasını sağ tıklayarak gerçekleştirilebilir.

    ![Sunucu uç noktası bir eşitleme grubundan kaldırılıyor](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosya eşitleme ile bir sunucu kaydı/unregister](storage-sync-files-server-registration.md)
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme İzleyicisi](storage-sync-files-monitoring.md)
