---
title: "Azure dosya eşitleme (Önizleme) sorunlarını giderme | Microsoft Docs"
description: "Azure dosya eşitleme ile ilgili genel sorunları giderme"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 6bc5f2da2b8628671037b9257db746e73cd3afad
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme) sorunlarını giderme
Azure Dosya Eşitleme (önizleme) aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede, sorun giderme ve Azure dosya eşitleme dağıtımınıza karşılaşılan sorunları çözmenize yardımcı olmak için tasarlanmıştır. Başarısız olan bu kılavuz daha kapsamlı bir araştırma sorunların yardımcı olmak için sistem önemli günlükleri toplamak nasıl gösterilmektedir. Yanıt sorunuzun yanıtını burada görmüyorsanız, (sipariş yükselen içinde) aşağıdaki kanallar aracılığıyla Bize Ulaşın çekinmeyin:

1. Bu makalede Açıklamalar bölümüne.
2. [Azure depolama Forumu](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: yeni bir destek servis talebi oluşturmak için gezinme "Yardım + destek için" sekmesinde Azure portalda ve "Yeni destek isteği"'i tıklatın.

## <a name="agent-installation-and-server-registration"></a>Aracı yükleme ve sunucu kaydı
<a id="agent-installation-failures"></a>**Aracı yükleme hatalarını giderme**  
Azure dosya eşitleme aracı yüklemesi başarısız olursa, aracı yükleme sırasında günlük kaydını etkinleştirmek için yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırın:

```
StorageSyncAgent.msi /l*v Installer.log
```

Yükleme başarısız sonra nedenini belirlemek için installer.log gözden geçirin. 

> [!Note]  
> Aracı yüklemesi, Microsoft Update'i kullanmayı seçerseniz ve Windows Update hizmeti çalışmadığı başarısız olur.

<a id="server-registration-missing"></a>**Sunucu kayıtlı sunucuları'nın altında Azure Portalı'nda listelenmez**  
Bir sunucu için bir depolama eşitleme hizmeti kayıtlı sunucuları'nın altında listede yoksa, aşağıdaki adımları gerçekleştirin:
1. Kaydetmek istediğiniz sunucuya oturum açın.
2. Dosya Gezgini'ni açın ve depolama eşitleme Aracısı yükleme dizinine gidin (varsayılan konum `C:\Program Files\Azure\StorageSyncAgent`). 
3. ServerRegistration.exe çalıştırın ve sunucuyu bir depolama eşitleme hizmeti ile kaydetmek için sihirbazı izleyin.

<a id="server-already-registered"></a>**Sunucu kaydı Azure dosya eşitleme Aracısı'nı yükledikten sonra aşağıdaki iletisini görüntüler: "Bu sunucusu zaten kayıtlı"**  
!["Sunucu zaten kayıtlı" hatası ile sunucu kaydı iletişim kutusunun ekran görüntüsü iletisi](media/storage-sync-files-troubleshoot/server-registration-1.png)

Sunucu, daha önce bir depolama eşitleme hizmetine kayıtlı değilse bu ileti görüntülenir. Geçerli depolama eşitleme hizmeti sunucuyla ve yeni bir depolama eşitleme hizmeti ile kaydetme kaydını silmek için adımlarını izleyin [Azure dosya eşitleme sahip bir sunucu kaydını](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Sunucu, kayıtlı sunucuları depolama eşitleme hizmetinin altında listelenmemişse kaydını kaldırmak istediğiniz sunucuda aşağıdaki PowerShell komutlarını çalıştırın:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Sunucu kümesinin parçası olmadığını ise, isteğe bağlı bir `Reset-StorageSyncServer -CleanClusterRegistration` ayrıca küme kayıt kaldıracak parametresi.

<a id="web-site-not-trusted"></a>**Bir sunucuya kaydedilirken çok sayıda "güvenilmeyen web site" yanıtları neden alıyorum?**  
Bu hata oluşur **Gelişmiş Internet Explorer güvenlik** İlkesi, sunucu kaydı sırasında etkindir. Düzgün bir şekilde devre dışı bırakma hakkında daha fazla bilgi için **Gelişmiş Internet Explorer güvenlik** İlkesi bkz [hazırlama Windows sunucularıyla kullanmak için Azure dosya eşitleme](storage-sync-files-deployment-guide.md#prepare-windows-servers-for-use-with-azure-file-sync) ve [Azure dosya dağıtma Eşitleme (Önizleme)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Eşitleme Grup Yönetimi
<a id="cloud-endpoint-using-share"></a>**Bulut uç noktası oluşturma, şu hata ile başarısız olur: "Belirtilen Azure dosya paylaşımı zaten farklı bir CloudEndpoint tarafından kullanılıyor"**  
Azure dosya paylaşımı zaten başka bir bulut uç noktası tarafından kullanılıyorsa bu hata oluşur. 

Bu hatayı alıyorsunuz ve Azure dosya paylaşımı şu anda bir bulut uç noktası tarafından kullanılmakta değil, Azure Dosya paylaşımındaki Azure dosya eşitleme meta verileri temizlemek için aşağıdaki aşağıdaki adımları gerçekleştirin:

> [!Warning]  
> Meta veriler şu anda bir bulut uç noktası tarafından kullanılmakta olan bir Azure dosya paylaşımında silinmesi, Azure dosya eşitleme işlemlerinin başarısız olmasına neden olur. 

1. Azure Portalı'nda, Azure dosya paylaşımına gidin.  
2. Azure dosya paylaşımı sağ tıklatın ve seçin **meta verileri düzenleme**
3. SyncService sağ tıklatın ve seçin **silmek**.

<a id="cloud-endpoint-authfailed"></a>**Bulut uç noktası oluşturma hatası ile başarısız olur: AuthorizationFailed**  
Kullanıcı hesabınız bir bulut uç noktası oluşturmak için yeterli hakları yoksa bu sorun oluşur. 

Bir bulut uç noktası oluşturmak için kullanıcı hesabınızın aşağıdaki Microsoft Authorization izinleri olması gerekir:  
* Şunu okuyun: rol tanımı Al
* Yazma: Özel rol tanımı güncelle
* Şunu okuyun: rol ataması Al
* Yazma: rol ataması oluşturma

Aşağıdaki yerleşik roller uygun Microsoft Authorization izinlere sahip:  
* Sahip
* Kullanıcı Erişimi Yöneticisi

Kullanıcı hesabı rolünüz uygun izinleri olup olmadığını belirlemek için aşağıdakileri gerçekleştirin:  
* Tıklatın **kaynak grupları** Azure portalında
* Depolama hesabının bulunduğu kaynak grubu seçin ve tıklatın **erişim denetimi (IAM)**
* Tıklatın **rol** (örn., sahibi, katkıda bulunan) kullanıcı hesabınız için.
* İçinde **kaynak sağlayıcısı** listesinde **Microsoft Authorization** 
* **Rol ataması** olmalıdır **okuma** ve **yazma izinleri**
* **Rol tanımı** olmalıdır **okuma** ve **yazma izinleri**

<a id="cloud-endpoint-deleteinternalerror"></a>**Uç nokta silme hatası ile başarısız olur: MgmtInternalError**  
Azure dosya paylaşımı veya depolama hesabı bulut uç noktası silmeden önce sildiyseniz Bu sorun ortaya çıkabilir. Gelecek bir güncelleştirmede bu sorun düzeltilecektir ve bulut uç noktası silinebilir.

Bu sorunun oluşmasını önlemek için Azure dosya paylaşımı veya depolama hesabını silmeden önce bulut uç noktası silin.

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Bir dosyayı doğrudan my Azure dosya paylaşımı SMB üzerinden veya portal üzerinden oluşturdum. Dosya eşitleme gruptaki sunucular için eşitlenen kadar ne kadar?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Bir sunucu üzerinde çalışmıyor eşitleme ile ilgili sorunları giderme**  
Bir sunucuda eşitleme başarısız olursa aşağıdakileri gerçekleştirin:
- Bir Azure dosya paylaşımına eşitlemek istediğiniz dizin için Azure portalında bir sunucusu uç noktası var olduğunu doğrulayın:
    
    ![Azure portalında hem bulut hem de sunucusu uç noktası ile eşitleme grubunun bir ekran görüntüsü](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

- Altında bulunan Olay Görüntüleyicisi ile işlemsel ve tanılama olay günlüklerini gözden geçirin `Applications and Services\Microsoft\FileSync\Agent`.
- Sunucunun internet bağlantısına sahip olduğunu doğrulayın.
- Azure dosya hizmeti Hizmetler MMC ek bileşenini açarak sunucu üzerinde çalışan eşitleme ve (FileSyncSvc) ve depolama eşitleme aracısı hizmetinin çalıştığını doğrulayın.

<a id="replica-not-ready"></a>**Eşitleme hatası ile başarısız olur: 0x80c8300f - çoğaltma gerekli işlemi gerçekleştirmek hazır değil**  
Bu hata, bir bulut uç noktası oluşturun ve verileri içeren bir Azure dosya paylaşımı kullanırsanız beklenir. (24 saate kadar sürebilir) Azure dosya paylaşımında değişikliği Algılama işlemi tamamlandıktan sonra eşitleme düzgün çalışmaya başlamanız gerekir.

<a id="broken-sync-files"></a>**Eşitleme başarısız tek dosyalar ile ilgili sorunları giderme**  
Tek tek dosyaların eşitleme başarısız oluyorsa, aşağıdakileri gerçekleştirin:
- Altında işlemsel ve tanılama olay günlüklerini gözden geçirin `Applications and Services\Microsoft\FileSync\Agent` Olay Görüntüleyicisi'nde
- Dosyada hiçbir açık tanıtıcıların olduğundan emin olun
    - Not: Azure dosya eşitleme düzenli aralıklarla dosyaları açık tanıtıcıların ile eşitlemek için VSS anlık görüntüleri sürer

## <a name="cloud-tiering"></a>Bulut katmanlaması 
<a id="files-fail-tiering"></a>**Katmanı tutulamadı dosyaları ile ilgili sorunları giderme**  
Azure dosyaları katmanı dosyalar başarısız olursa, aşağıdaki adımları gerçekleştirin:

- Azure dosya paylaşımı dosyaları mevcut doğrulayın
    - Not: Bu katmanlı önce bir dosya için Azure dosya paylaşımının senkronize gerekir
- İşlemsel ve tanılama olay altında bulunan günlüklerini gözden geçirin `Applications and Services\Microsoft\FileSync\Agent` Olay Görüntüleyicisi'nde
- Sunucunun Internet bağlantısı olduğunu doğrulayın 
- Azure dosya eşitleme filtre sürücüleri (StorageSync.sys & StorageSyncGuard.sys) çalıştıran doğrulayın
    - Yükseltilmiş bir komut istemi açın, çalıştırmak `fltmc` ve StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücüleri listelendiğini doğrulayın

<a id="files-fail-recall"></a>**Çağrılmaya başarısız dosyaları ile ilgili sorunları giderme**  
Dosyaları çağrılmaya başarısız olursa, aşağıdaki adımları gerçekleştirin:
- İşlemsel ve tanılama olay altında bulunan günlüklerini gözden geçirin `Applications and Services\Microsoft\FileSync\Agent` Olay Görüntüleyicisi'nde
- Azure dosya paylaşımında dosyaları var olduğunu doğrulayın
- Sunucunun Internet bağlantısı olduğunu doğrulayın 
- Azure dosya eşitleme filtre sürücüleri (StorageSync.sys & StorageSyncGuard.sys) çalıştıran doğrulayın
    - Yükseltilmiş bir komut istemi açın, çalıştırmak `fltmc` ve StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücüleri listelendiğini doğrulayın

<a id="files-unexpectedly-recalled"></a>**Bir sunucu üzerinde beklenmedik bir şekilde çekilen dosyaları ile ilgili sorunları giderme**  
Çevrimdışı özniteliği saygı ve bu dosyaların içeriğini okuma Atla sürece virüsten koruma, yedekleme ve çok sayıda dosya okuma diğer uygulamaları istenmeyen geri çekme neden olur. Bunu destekleyen bu ürünler için çevrimdışı dosyalar atlanıyor istenmeyen geri çekme virüsten koruma taramaları veya yedekleme işleri gibi işlemleri gerçekleştirirken önlemeye yardımcı olur.

Çevrimdışı dosyaları okuma atlamak için yazılım satıcınıza kendi çözümünü yapılandırma ile ilgili başvurun.

Daha fazla istenmeyen geri çekme, dosya Gezgini'ndeki dosyaları tarama gibi diğer senaryolarda oluşabilir. Virüsten koruma sunucuda etkinse, bulut katmanlı dosyaları dosya Gezgini'nde sunucu üzerinde bir klasör açılarak istenmeyen geri çekme böylece daha neden olabilir.

## <a name="general-troubleshooting"></a>Genel sorun giderme
Azure dosya eşitleme ile bir sunucuda sorunlarla karşılaşırsanız, aşağıdaki işlemleri gerçekleştirerek başlatın:
- Olay Görüntüleyicisi'nde tanılama ve işlemsel olay günlüklerini gözden geçirin
    - Katmanlama, eşitleme ve geri çağırma sorunları tanılama ve işletimsel olay günlüklerinde altında günlüğe kaydedilir`Applications and Services\Microsoft\FileSync\Agent`
    - Bir sunucu (örneğin, yapılandırma ayarlarını) yönetme konuları altında tanılama ve işletimsel olay günlüklerine kaydedilir`Applications and Services\Microsoft\FileSync\Management`
- Azure dosya eşitleme hizmetinin sunucuda çalıştığını doğrulayın
    - Hizmetler MMC ek bileşenini açın ve (FileSyncSvc) depolama eşitleme aracı hizmetinin çalıştığını doğrulayın
- Azure dosya eşitleme filtre sürücüleri (StorageSync.sys & StorageSyncGuard.sys) çalıştıran doğrulayın
    - Yükseltilmiş bir komut istemi açın, fltmc çalıştırın ve StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücüleri listelendiğini doğrulayın

Yukarıdaki adımları gerçekleştirdikten sonra sorun giderilmezse, aşağıdaki adımları gerçekleştirerek AFSDiag aracını çalıştırın:
1. AFSDiag çıkış (örneğin, c:\output) kaydetmek için kullanılan bir dizin oluşturun.
2. Yükseltilmiş bir PowerShell penceresi açın ve (isabet, her komutun ardından enter) aşağıdaki komutları çalıştırın:

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: use the path created in step 1
    ```

3. Azure dosya eşitleme çekirdek modu izleme düzeyi için 1 (daha ayrıntılı izlemeler oluşturmak için belirtilmediği sürece) girin ve ENTER tuşuna basın.
4. Azure dosya eşitleme kullanıcı modu izleme düzeyi için 1 (daha ayrıntılı izlemeler oluşturmak için belirtilmediği sürece) girin ve ENTER tuşuna basın.
5. Sorunu yeniden oluşturun ve bittiğinde D tuşuna basın.
6. Günlükleri ve izleme dosyaları içeren bir .zip dosyası belirtilen çıktı dizinine olacaktır.

## <a name="see-also"></a>Ayrıca bkz.
- [Azure dosyaları sık sorulan sorular](storage-files-faq.md)
- [Windows Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
- [Linux Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
