---
title: "Azure dosya eşitleme (Önizleme) sorunlarını giderme | Microsoft Docs"
description: "Azure dosya eşitleme ile ilgili genel sorunları giderin."
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
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: 7562e43f58f303ea34a08b8b9e056a0c3d0c10d0
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme) sorunlarını giderme
Esneklik, performans ve uyumluluk bir şirket içi dosya sunucusunun tanırken kuruluşunuzun dosya paylaşımları Azure dosyalarında merkezileştirmek için Azure dosya eşitleme (Önizleme) kullanın. Azure dosya eşitleme, Windows Server Hızlı Azure dosya paylaşımınıza önbelleğine dönüştürür. SMB ve NFS FTPS çeşitli verilerinize yerel olarak erişmek için Windows Server üzerinde kullanılabilir herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gerektiği kadar önbellekleri olabilir.

Bu makalede ve Azure dosya eşitleme dağıtımınıza karşılaşabileceğiniz sorunları gidermek amacıyla tasarlanmıştır. Biz de sorunun daha kapsamlı bir araştırma gerekiyorsa sistemden önemli günlükleri toplamak açıklar. Sorunuzun yanıtını görmüyorsanız, bize (sırayla yükselen) aşağıdaki kanallar aracılığıyla başvurabilirsiniz:

1. Bu makalede Açıklamalar bölümüne.
2. [Azure depolama Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft destek. Azure portalında yeni bir destek isteği oluşturmak için **yardımcı** sekmesine **Yardım + Destek** düğmesine tıklayın ve ardından **yeni destek isteği**.

## <a name="agent-installation-and-server-registration"></a>Aracı yükleme ve sunucu kaydı
<a id="agent-installation-failures"></a>**Aracı yükleme hatalarını giderme**  
Yükseltilmiş bir komut isteminde Azure dosya eşitleme aracı yüklemesi başarısız olursa aracı yükleme işlemi sırasında oturum açmak için aşağıdaki komutu çalıştırın:

```
StorageSyncAgent.msi /l*v Installer.log
```

Yükleme hatanın nedenini belirlemek için installer.log gözden geçirin. 

> [!Note]  
> Aracı yüklemesi Microsoft Update'i kullanmak için makinenize ayarlamak ve Windows Update hizmeti çalışmıyor başarısız olur.

<a id="agent-installation-websitename-failure"></a>**Aracı yüklemesi bu hata ile başarısız olur: "Depolama alanı eşitleme Aracısı Sihirbazı erken sona erdi."**  
IIS Web sitesi varsayılan adı değişirse, bu sorun ortaya çıkabilir. Bu sorunu çözmek için IIS varsayılan Web sitesi olarak "Default Web Site" yeniden adlandırın ve yükleme işlemini yeniden deneyin. Aracısı'nın gelecek bir güncelleştirmede sorun düzeltilecektir. 

<a id="server-registration-missing"></a>**Azure portalında kayıtlı sunucuları altındaki Server listelenmeyen**  
Bir sunucu altında listelenmemişse **kayıtlı sunucuları** depolama eşitleme hizmeti için:
1. Kaydetmek istediğiniz sunucuya oturum açın.
2. Dosya Gezgini'ni açın ve (varsayılan konumu C:\Program Files\Azure\StorageSyncAgent:) depolama eşitleme Aracısı yükleme dizinine gidin. 
3. ServerRegistration.exe çalıştırın ve sunucuyu bir depolama eşitleme hizmeti ile kaydetmek için sihirbazı tamamlayın.

<a id="server-already-registered"></a>**Sunucu kaydı Azure dosya eşitleme aracı yükleme işlemi sırasında aşağıdaki iletisini görüntüler: "Bu sunucusu zaten kayıtlı"** 

!["Sunucu zaten kayıtlı" hatası ile sunucu kaydı iletişim kutusunun ekran görüntüsü iletisi](media/storage-sync-files-troubleshoot/server-registration-1.png)

Sunucunun bir depolama eşitleme hizmeti daha önceden kaydedilen bu ileti görüntülenir. Geçerli depolama eşitleme hizmeti sunucunun kaydını silmek ve ardından yeni bir depolama eşitleme hizmeti ile kaydetmek için bölümünde açıklanan adımları [Azure dosya eşitleme sahip bir sunucu kaydını](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Altında sunucu listelenmemişse **kayıtlı sunucuları** depolama eşitleme hizmeti kaydını kaldırmak istediğiniz sunucuda aşağıdaki PowerShell komutlarını çalıştırın:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Sunucu kümesinin parçası ise, isteğe bağlı kullanabilirsiniz *sıfırlama StorageSyncServer - CleanClusterRegistration* ayrıca küme kaydını kaldırmak için parametre.

<a id="web-site-not-trusted"></a>**Bir sunucuyu kaydettirmek, çok sayıda "güvenilmeyen web site" yanıtları bakın. Neden?**  
Bu sorun oluşur zaman **Gelişmiş Internet Explorer güvenlik** İlkesi, sunucu kaydı sırasında etkindir. Doğru bir şekilde devre dışı bırakma hakkında daha fazla bilgi için **Gelişmiş Internet Explorer güvenlik** İlkesi bkz [Azure dosya eşitleme ile kullanmak için Windows Server hazırlama](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) ve [Azure dosya dağıtma Eşitleme (Önizleme)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Eşitleme Grup Yönetimi
<a id="cloud-endpoint-using-share"></a>**Bulut uç noktası oluşturma başarısız olur, bu hata: "Belirtilen Azure dosya paylaşımı zaten farklı bir CloudEndpoint tarafından kullanılıyor"**  
Azure dosya paylaşımı zaten başka bir bulut uç noktası tarafından kullanılıyorsa bu sorun oluşur. 

Bu iletiyi görürseniz ve Azure dosya paylaşımı şu anda bir bulut uç noktası tarafından kullanılmadığından Azure Dosya paylaşımındaki Azure dosya eşitleme meta verileri temizlemek için aşağıdaki adımları tamamlayın:

> [!Warning]  
> Şu anda bir bulut uç noktası tarafından kullanılmakta olan bir Azure dosya paylaşımında meta verileri silme Azure dosya eşitleme işlemlerinin başarısız olmasına neden olur. 

1. Azure portalında, Azure dosya paylaşımına gidin.  
2. Azure dosya paylaşımına sağ tıklayın ve ardından **Düzenle meta verileri**.
3. Sağ **SyncService**ve ardından **silmek**.

<a id="cloud-endpoint-authfailed"></a>**Bulut uç noktası oluşturma başarısız olur, bu hata: "AuthorizationFailed"**  
Kullanıcı hesabınız bir bulut uç noktası oluşturmak için yeterli hakları yoksa bu sorun oluşur. 

Bir bulut uç noktası oluşturmak için kullanıcı hesabınızın aşağıdaki Microsoft Authorization izinleri olması gerekir:  
* Şunu okuyun: rol tanımı Al
* Yazma: Özel rol tanımı güncelle
* Şunu okuyun: rol ataması Al
* Yazma: rol ataması oluşturma

Aşağıdaki yerleşik roller gerekli Microsoft Authorization izinlere sahip:  
* Sahip
* Kullanıcı Erişimi Yöneticisi

Kullanıcı hesabı rolünüz gerekli izinlere sahip olup olmadığını belirlemek için:  
1. Azure portalında seçin **kaynak grupları**.
2. Depolama hesabının bulunduğu kaynak grubunu seçin ve ardından **erişim denetimi (IAM)**.
3. Seçin **rol** (örneğin, sahibi veya katkıda) kullanıcı hesabınız için.
4. İçinde **kaynak sağlayıcısı** listesinde **Microsoft Authorization**. 
    * **Rol ataması** olmalıdır **okuma** ve **yazma** izinleri.
    * **Rol tanımı** olmalıdır **okuma** ve **yazma** izinleri.

<a id="server-endpoint-createjobfailed"></a>**Sunucu uç noktası oluşturma başarısız olur, bu hata: "MgmtServerJobFailed" (hata kodu:-2134375898)**                                                                                                                           
Sunucu bitiş noktası yolu sistem birimi ve bulut üzerinde ise bu sorun oluşur katmanlama etkindir. Bulut katmanlandırma sistem biriminde desteklenmiyor. Sistem biriminde sunucusu uç noktası oluşturmak için bulut sunucusu uç noktası oluşturulurken katmanlama devre dışı bırakın.

<a id="server-endpoint-deletejobexpired"></a>**Sunucu uç noktasını silme başarısız olursa bu hata: "MgmtServerJobExpired"**                
Sunucu çevrimdışı veya ağ bağlantısı yok, bu sorun oluşur. Sunucu artık kullanılabilir değilse, sunucunun hangi sunucu uç noktalarını siler portalında kaydını silin. Sunucu uç noktalarını silmek için açıklanan adımları izleyin [Azure dosya eşitleme sahip bir sunucu kaydını](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**I bir dosya doğrudan my Azure dosya paylaşımı SMB üzerinden veya portal üzerinden oluşturduysanız, ne kadar dosya eşitleme gruptaki sunucular için eşitleme için sürer?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Bir sunucuda eşitleme başarısız**  
Bir sunucuda eşitleme başarısız olursa:
1. Sunucusu uç noktası Azure portalında bir Azure dosya paylaşımına eşitlemek istediğiniz dizinin var olduğundan emin olun:
    
    ![Bulut uç noktasına ve Azure portalında sunucusu uç noktası ile eşitleme grubunun ekran görüntüsü](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

2. Olay Görüntüleyicisi'nde çalıştığını ve tanılama olay uygulamalar ve Services\Microsoft\FileSync\Agent altında bulunan günlüklerini gözden geçirin.
    1. Sunucunun Internet bağlantısına sahip olduğunu doğrulayın.
    2. Azure dosya eşitleme hizmetinin sunucuda çalıştığını doğrulayın. Bunu yapmak için Hizmetler MMC ek bileşenini açın ve (FileSyncSvc) depolama eşitleme aracısı hizmetinin çalıştığını doğrulayın.

<a id="replica-not-ready"></a>**Eşitleme başarısız olursa bu hata: "0x80c8300f - çoğaltma gerekli işlemi gerçekleştirmek hazır değil"**  
Bu sorun, veri içeren bir bulut uç noktası oluşturun ve bir Azure dosya paylaşımı kullanırsanız beklenir. (24 saate kadar sürebilir) Azure dosya paylaşımında çalıştıran değişiklik algılama işi sona erdiğinde, eşitleme düzgün çalışmasını başlamanız gerekir.

<a id="broken-sync-files"></a>**Eşitlemesi başarısız tek tek dosya sorunlarını giderme**  
Tek tek dosyaların eşitlemesi başarısız oluyorsa:
1. Olay Görüntüleyicisi'nde çalıştığını ve tanılama olay uygulamalar ve Services\Microsoft\FileSync\Agent altında bulunan günlüklerini gözden geçirin.
2. Dosya tanımlayıcıları hiçbir açık olduğunu doğrulayın.

    > [!NOTE]
    > Azure dosya eşitleme, VSS anlık görüntüleri tanımlayıcıları açık dosyaları eşitlemek için düzenli aralıklarla alır.

Şu anda başka bir abonelik veya taşıma için kaynak taşıma desteklemiyoruz için farklı bir Azure AD Kiracı.  Abonelik için farklı bir kiracı geçerse, Azure dosya paylaşımı için sahiplik değişikliği göre hizmetimizi erişilemez duruma gelir. Kiracı değiştirdiyseniz, sunucu uç noktaları ve bulut uç noktası silmeniz gerekir (bkz: eşitleme Grup Yönetimi yeniden kullanılacak Azure dosya paylaşımı temizlemek nasıl yönergeler için bölüm) ve eşitleme grubunu yeniden oluşturun.

## <a name="cloud-tiering"></a>Bulut katmanlaması 
Bulutta hataları için iki yol katmanlama:

- Dosyaları katmanı Azure dosya eşitleme başarısız Azure dosyaları bir dosyaya katmanı çalışır, yani başarısız olabilir.
- Dosyaları (StorageSync.sys) başarısız katmanlı bir dosyaya erişmek için kullanıcı girişimi olduğunda verilerini indirmek için Azure dosya eşitleme dosya sistemi filtresi anlamına geri çağırma için başarısız olabilir.

İki ana sınıf ya da hatası yolu oluşabilecek hatalar şunlardır:

- Bulut depolama hataları
    - *Geçici depolama hizmet kullanılabilirliği sorunlarını*. Bkz: [Azure Storage için hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/storage/v1_2/) daha fazla bilgi için.
    - *Erişilemez Azure dosya paylaşımı*. Sildiğinizde bu hata genellikle, Azure dosya paylaşımı hala bulut uç noktasına bir eşitleme grubundaki oluşur.
    - *Erişilemez depolama hesabı*. Bulut uç noktasına bir eşitleme grubunda olan bir Azure dosya paylaşımı hala sahipken depolama hesabını silmek bu hata genellikle olur. 
- Sunucu hataları 
    - *Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) yüklenmedi*. İstekleri katmanlama/geri çağırma yanıtlamak için Azure dosya eşitleme dosya sistemi filtresi yüklenmesi gerekir. Değil yüklenen filtre çeşitli nedenlerle oluşabilir, ancak yönetici, el ile kaldırıldığında, en yaygın nedeni olur. Azure dosya eşitleme dosya sistemi filtresi sırasında her zaman Azure dosya eşitleme için düzgün çalışması için yüklenmesi gerekir.
    - *Eksik, bozuk veya aksi bozuk yeniden ayrıştırma noktası*. Bir özel veri yapısı iki bölümden oluşan bir dosyada bir ayrıştırma noktasıdır:
        1. Azure dosya eşitleme dosya sistemi filtresi (StorageSync.sys) dosyasına GÇ bazı eylem yapmanız gerekebilir işletim sistemini gösterir yeniden ayrıştırma etiketi. 
        2. Yeniden ayrıştırma veri URI ilişkili bulut uç noktası (Azure dosya paylaşımı) dosyasının dosya sistemi filtresi için gösterir. 
        
        Ayrıştırma noktası bozulabilir en yaygın yönetici etiketi veya verisini değiştirmek dener yoludur. 
    - *Ağ bağlantısı sorunları*. Katmanı veya bir dosya geri çekmek için sunucunun internet bağlantısı olması gerekir.

Aşağıdaki bölümlerde bulut katmanlama sorunlarını gidermek ve bir sorunun bir bulut depolama sorunu veya bir sunucu sorunu olup olmadığını belirlemek nasıl yapılacağını belirtin.

<a id="files-fail-tiering"></a>**Katmanı başarısız dosya sorunlarını giderme**  
Azure dosyaları katmanı dosyaları başarısız olursa:

1. Dosyaların Azure dosya paylaşımı mevcut olduğunu doğrulayın.

    > [!NOTE]
    > Katmanlı önce bir dosya için Azure dosya paylaşımının eşitlenen gerekir.
2. Olay Görüntüleyicisi'nde çalıştığını ve tanılama olay uygulamalar ve Services\Microsoft\FileSync\Agent altında bulunan günlüklerini gözden geçirin.
    1. Sunucunun Internet bağlantısına sahip olduğunu doğrulayın. 
    2. Azure dosya eşitleme filtre sürücüleri (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
        - Yükseltilmiş bir komut isteminde çalıştırmak `fltmc`. StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücüleri listelendiğini doğrulayın.

<a id="files-fail-recall"></a>**Çağrılmaya başarısız dosya sorunlarını giderme**  
Dosyaları çağrılmaya başarısız olursa:
1. Olay Görüntüleyicisi'nde çalıştığını ve tanılama olay uygulamalar ve Services\Microsoft\FileSync\Agent altında bulunan günlüklerini gözden geçirin.
    1. Dosyaların Azure dosya paylaşımı mevcut olduğunu doğrulayın.
    2. Sunucunun Internet bağlantısına sahip olduğunu doğrulayın. 
    3. Azure dosya eşitleme filtre sürücüleri (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
        - Yükseltilmiş bir komut isteminde çalıştırmak `fltmc`. StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücüleri listelendiğini doğrulayın.

<a id="files-unexpectedly-recalled"></a>**Beklenmedik bir sunucuda geri çekilen dosyalar sorun giderme**  
Atla Çevrimdışı özniteliği saygı ve bu dosyaların içeriğini okuma Atla sürece virüsten koruma, yedekleme ve çok sayıda dosya okuma diğer uygulamaları istenmeyen geri çekme neden. Ürünler için çevrimdışı dosyalar atlanıyor bu seçenek yardımcı destekleyen kaçının istenmeyen geri çekme virüsten koruma taraması veya yedekleme işleri gibi işlemleri sırasında.

Çevrimdışı dosyaları okuma atlamak için kendi çözümünü yapılandırma konusunda bilgi edinmek için yazılım satıcınıza başvurun.

İstenmeyen geri çekme ne zaman dosya Gezgini'nde dosyaları tarama gibi diğer senaryolarda de oluşabilir. Bulut katmanlı dosyalarını dosya Gezgini'nde sunucusuna olan bir klasörü açma içinde istenmeyen geri çekme neden olabilir. Virüsten koruma çözümünü sunucuda etkinse, bu daha yüksektir.

## <a name="general-troubleshooting"></a>Genel sorun giderme
Azure dosya eşitleme ile bir sunucuda sorunlarla karşılaşırsanız, aşağıdaki adımları tamamlayarak başlayın:
1. Olay Görüntüleyicisi'nde çalıştığını ve tanılama olay günlüklerini gözden geçirin.
    - Eşitleme, katmanlama ve geri çağırma sorunları, uygulamalar ve Services\Microsoft\FileSync\Agent tanılama ve işletimsel olay günlüklerine kaydedilir.
    - Bir sunucu (örneğin, yapılandırma ayarlarını) yönetimiyle ilgili sorunlar, uygulamalar ve Services\Microsoft\FileSync\Management altında çalıştığını ve tanılama olay günlüklerine kaydedilir.
2. Azure dosya eşitleme hizmetinin sunucuda çalıştığını doğrulayın:
    - Hizmetler MMC ek bileşenini açın ve (FileSyncSvc) depolama eşitleme aracısı hizmetinin çalıştığını doğrulayın.
3. Azure dosya eşitleme filtre sürücüleri (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
    - Yükseltilmiş bir komut isteminde çalıştırmak `fltmc`. StorageSync.sys ve StorageSyncGuard.sys dosya sistemi filtre sürücüleri listelendiğini doğrulayın.

Sorun giderilmezse AFSDiag aracını çalıştırın:
1. (Örneğin, C:\Output) AFSDiag çıkış kaydedileceği bir dizin oluşturun.
2. Yükseltilmiş bir PowerShell penceresi açın ve ardından (her komuttan sonra Enter tuşuna basın) aşağıdaki komutları çalıştırın:

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Azure dosya eşitleme çekirdek modu için izleme düzeyi, girin **1** (Aksi takdirde, daha ayrıntılı izlemeler oluşturmak için belirtilmediği sürece), ve ardından Enter tuşuna basın.
4. Azure dosya eşitleme kullanıcı modu için izleme düzeyi, girin **1** (Aksi takdirde, daha ayrıntılı izlemeler oluşturmak için belirtilmediği sürece), ve ardından Enter tuşuna basın.
5. Sorunu yeniden oluşturun. İşlemi tamamladığınızda, girin **D**.
6. Günlükleri içeren bir .zip dosyası ve izleme dosyaları, belirtilen çıkış dizinine kaydedilir.

## <a name="see-also"></a>Ayrıca bkz.
- [Azure dosyaları sık sorulan sorular](storage-files-faq.md)
- [Windows Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
- [Linux Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
