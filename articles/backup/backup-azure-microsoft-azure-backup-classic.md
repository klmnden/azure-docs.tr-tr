---
title: "Klasik Azure portalında iş yüklerini yedeklemek için Azure yedekleme sunucusu kullanın | Microsoft Docs"
description: "Azure yedekleme sunucusu kullanarak iş yüklerini yedeklemeye ortamınızı doğru şekilde hazırlandığından emin olun"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: "Azure backup sunucusu; Yedekleme kasası"
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cwatson
ms.openlocfilehash: 3f22ad12c966f0e8d5a77c2060711d32dfddbc94
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="preparing-to-back-up-workloads-using-azure-backup-server"></a>Azure Backup Sunucusu kullanarak iş yüklerini yedeklemeye hazırlama
> [!div class="op_single_selector"]
> * [Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup sunucusu (Klasik)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (Klasik)](backup-azure-dpm-introduction-classic.md)
>
>

Bu makalede, Azure yedekleme sunucusu kullanarak iş yüklerini yedeklemeye için ortamınızı hazırlama hakkında ' dir. Azure yedekleme Sunucusu'nu kullanarak, Hyper-V sanal makineleri, Microsoft SQL Server, SharePoint Server, Microsoft Exchange ve Windows istemcileri gibi uygulama iş yükleri tek bir konsoldan koruyabilirsiniz.

> [!WARNING]
> Azure Backup sunucusu iş yükü yedeklemesi için Data Protection Manager (DPM) işlevselliği devralır. Bu özelliklerden bazıları için DPM belgelerine işaretçileri bulacaksınız. Ancak Azure yedekleme sunucusu bant üzerindeki koruma sağlamaz veya System Center ile tümleştirme.
>
>

## <a name="1-windows-server-machine"></a>1. Windows Server makine
![step1](./media/backup-azure-microsoft-azure-backup/step1.png)

Azure yedekleme sunucusu Başlarken hazırlarken ve çalışırken doğrultusunda ilk adımı, Windows Server makinesinde sağlamaktır.

| Konum | En düşük gereksinimler | Ek yönergeler |
| --- | --- | --- |
| Azure |Azure Iaas sanal makinesi<br><br>A2 Standart: 2 Çekirdek, 3.5GB RAM |Windows Server 2012 R2 Datacenter basit galeri görüntüsü ile başlayabilirsiniz. [Azure yedekleme Server'ı (DPM) kullanılarak Iaas iş yüklerini koruyorsanız](https://technet.microsoft.com/library/jj852163.aspx) pek çok küçük farklar vardır. Makaleyi tamamen makineyi dağıtmadan önce okuduğunuzdan emin olun. |
| Şirket içi |Hyper-V VM<br> VMWare VM<br> ya da fiziksel bir ana bilgisayar<br><br>2 Çekirdek ve 4GB RAM |Windows Server yinelenenleri kaldırma kullanılarak DPM depolama alanında yinelenenleri kaldırma. Hakkında daha fazla bilgi [DPM ve yinelenenleri kaldırma](https://technet.microsoft.com/library/dn891438.aspx) Hyper-V VM dağıtıldığında birlikte çalışır. |

> [!NOTE]
> Azure yedekleme sunucusu Windows Server 2012 R2 Datacenter olan bir makinede yüklü önerilir. Önkoşullar çok otomatik olarak Windows işletim sisteminin en son sürümü ile ele alınmıştır.
>
>

Azure yedekleme sunucusu bir etki alanına düşünüyorsanız, fiziksel sunucu veya sanal makine etki alanına Azure yedekleme sunucusu yazılımı yüklenmeden önce katılmaları önerilir. Bir Azure yedekleme sunucusu dağıtıldıktan sonra yeni bir etki alanına taşıma *desteklenmiyor*.

## <a name="2-backup-vault"></a>2. Yedekleme kasası
![step2](./media/backup-azure-microsoft-azure-backup/step2.png)

Azure'a yedekleme verileri göndermek ya da yerel olarak tutmak isteyip Azure yedekleme sunucusu bir kasaya kayıtlı olması gerekir. Yeni bir Azure yedekleme kullanıcısıysanız ve Azure yedekleme sunucusu kullanmak istiyorsanız, bu makalenin - Azure portal sürümü olup [Azure yedekleme sunucusu kullanarak iş yüklerini Yedeklemeye hazırlanma](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> Mart 2017’den itibaren Backup kasaları oluşturmak için klasik portalı kullanamayacaksınız.
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> 30 Kasım 2017 sonra yedekleme kasaları oluşturmak için PowerShell kullanmanız mümkün olmaz. **30 Kasım 2017 tarafından**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>



## <a name="3-software-package"></a>3. Yazılım paketi
![Adım 3](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-the-software-package"></a>Yazılım paketini indirme
Benzer kimlik bilgileri kasası, Microsoft Azure yedekleme için uygulama iş yüklerini indirebilirsiniz **hızlı başlangıç sayfasında** yedekleme kasasının.

1. Tıklatın **uygulama iş yükleri (buluta diske Disk)**. Bu yazılım paketi indirdiğiniz İndirme Merkezi sayfasından sayfasına yönlendirileceksiniz.

    ![Microsoft Azure yedekleme Hoş Geldiniz ekranı](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. **İndir**’e tıklayın.

    ![İndirme Merkezinden 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Tüm dosyaları seçin ve tıklatın **sonraki**. Microsoft Azure yedekleme indirme sayfasından gelen tüm dosyaları indirin ve tüm dosyaları aynı klasöre yerleştirin.
   ![İndirme Merkezinden 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    İndirme boyutu tüm dosyaların birlikte > 3 G olduğundan, yüklemeyi tamamlamak 60 dakika kadar sürebilir bağlantı 10 MB/sn üzerinde indirin.

### <a name="extracting-the-software-package"></a>Yazılım paketi ayıklanıyor
Tüm dosyaları indirdikten sonra tıklayın **MicrosoftAzureBackupInstaller.exe**. Bu başlatır **Microsoft Azure yedekleme Kurulum Sihirbazı'nı** Kurulum dosyaları sizin belirttiğiniz bir konuma ayıklayın. Sihirbaza devam edin ve tıklayın **ayıklamak** ayıklama işlemine başlamak için düğmesi.

> [!WARNING]
> Kurulum dosyalarını ayıklamak için en az 4GB boş disk alanı gereklidir.
>
>

![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Ayıklama işlemi tamamlandı, istemcinin ayıklanan başlatmak için kutusunu işaretlediğinizde *setup.exe* Microsoft Azure yedekleme sunucusu yüklemeye başlamak için **son** düğmesi.

### <a name="installing-the-software-package"></a>Yazılım paketi yükleniyor
1. Tıklatın **Microsoft Azure yedekleme** Kurulum Sihirbazı'nı başlatmak için.

    ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Hoş Geldiniz ekranında tıklatın **sonraki** düğmesi. Bu, olanak sürer *önkoşul denetler* bölümü. Bu ekranda tıklayın **denetleyin** Azure yedekleme sunucusu için donanım ve yazılım önkoşullarının karşılanıp karşılanmadığını varsa belirlemek için düğmesini. Tüm Önkoşullar silinmiş başarıyla karşılanır makine gereksinimlerini karşıladığını belirten bir ileti görürsünüz. Tıklayın **sonraki** düğmesi.

    ![Azure yedekleme sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure yedekleme sunucusu SQL Server Standard gerektirir ve uygun SQL Server ikili gereken Azure yedekleme sunucusu yükleme paketi ile birlikte gelen sunar. Yeni bir Azure yedekleme sunucusu yüklemesine başlatırken seçeneğini seçmeniz gerekir **yeni SQL Server örneği bu kurulum ile yükleme** tıklatıp **denetle ve Yükle** düğmesi. Önkoşullar başarıyla yüklendikten sonra tıklayın **sonraki**.

    ![Azure yedekleme sunucusu - SQL denetimi](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Makineyi yeniden başlatmak için bir öneri bir arıza durumunda bunu ve tıklayın **yeniden denetle**.

   > [!NOTE]
   > Azure yedekleme sunucusu uzak bir SQL Server örneği ile çalışmaz. Azure yedekleme sunucusu tarafından kullanılan örnek yerel olması gerekir.
   >
   >

4. Microsoft Azure yedekleme sunucusu dosyaların yüklenmesi için bir konum belirtin ve tıklatın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Azure yedekleme bir zorunluluk karalama konumdur. Karalama konumun buluta yedeklenmesi için planlanan veri % 5'en az olduğundan emin olun. Disk koruması için ayrı diskin yükleme işlemi tamamlandıktan sonra yapılandırılması gerekir. Depolama havuzları hakkında daha fazla bilgi için bkz: [depolama havuzlarını yapılandırın ve disk depolama](https://technet.microsoft.com/library/hh758075.aspx).
5. Kısıtlı yerel kullanıcı hesapları için güçlü bir parola sağlayın ve tıklatın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Kullanmak mı istediğinizi seçin *Microsoft Update* güncelleştirmeleri denetlemek ve tıklayın **sonraki**.

   > [!NOTE]
   > Windows Update Microsoft Update, Windows ve Microsoft Azure yedekleme sunucusu gibi diğer ürünleri için güvenlik ve önemli güncelleştirmeler sunar yönlendirmek olması önerilir.
   >
   >

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Gözden geçirme *ayarların özeti* tıklatıp **yükleme**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. Yükleme aşamada gerçekleşir. İlk aşamada Microsoft Azure kurtarma Hizmetleri Aracısı sunucusuna yüklenir. Sihirbaz aynı zamanda Internet bağlantısını denetler. Internet bağlantısı olup olmadığını yüklemeye devam edebilirsiniz, aksi durumda, Internet'e bağlanmak için proxy ayrıntılarını sağlamanız gerekir.

    Sonraki adım, Microsoft Azure kurtarma Hizmetleri Aracısı yapılandırmaktır. Yapılandırmasının parçası olarak yedekleme kasasına makine kaydetmek için kasa kimlik bilgileri olduğunuz vermeniz gerekir. Ayrıca, şirket içi ve Azure arasında gönderilen verilerin şifreleme/şifre çözme için bir parola sağlar. Otomatik olarak bir parola oluşturmak veya kendi en az 16 karakter parola girin. Aracı yapılandırılana kadar sihirbaza devam edin.

    ![Azure yedekleme Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Microsoft Azure yedekleme sunucusu kaydı başarıyla tamamlandığında, Genel Kurulum Sihirbazı'nı yükleme ve yapılandırma SQL Server ve Azure yedekleme sunucusu bileşenleri devam eder. SQL Server bileşen yüklemesi tamamlandıktan sonra Azure yedekleme sunucusu bileşenleri yüklenir.

    ![Azure Backup Sunucusu](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Yükleme adım tamamlandığında, ürünün masaüstü simgelerini de oluşturulmuş. Yalnızca ürün başlatmak için simgesini çift tıklatın.

### <a name="add-backup-storage"></a>Yedekleme depolama ekleme
İlk yedek kopyayı Azure yedekleme sunucusu makineye bağlı depolama üzerinde tutulur. Diskleri ekleme hakkında daha fazla bilgi için bkz: [depolama havuzlarını yapılandırın ve disk depolama](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Azure'a veri göndermeyi planladığınız olsa bile yedekleme depolama alanı eklemeniz gerekir. Azure yedekleme sunucusu geçerli mimarisinde Azure yedekleme kasası tutan *ikinci* yerel depolama sırasında verilerin kopyasını ilk (ve zorunlu) yedek kopyasını tutar.  
>
>

## <a name="4-network-connectivity"></a>4. Ağ bağlantısı
![step4](./media/backup-azure-microsoft-azure-backup/step4.png)

Azure yedekleme sunucusu Azure yedekleme hizmetine başarılı olarak çalışması ürün için bağlantısı gerektirir. Makine Azure bağlantısı olup olmadığını doğrulamak için kullanın ```Get-DPMCloudConnection``` komutunu Azure yedekleme sunucusu PowerShell konsolundaki. Bağlantı olduğundan, komutunu çıktısını TRUE ise başka bağlantısı yok.

Aynı anda Azure aboneliği sağlıklı bir durumda olması gerekir. Aboneliğinizin durumunu bulmak ve yönetmek için oturum [abonelik portal](https://account.windowsazure.com/Subscriptions).

Azure bağlantı ve Azure abonelik durumu öğrendikten sonra sunulan yedekleme/geri yükleme işlevselliği üzerindeki etkisini öğrenmek için aşağıdaki tabloyu kullanabilirsiniz.

| Bağlantı durumu | Azure Aboneliği | Azure'a yedekleme | Diske yedekleme | Azure'dan geri yükleme | Disk, geri yükleme |
| --- | --- | --- | --- | --- | --- |
| Bağlanıldı |Etkin |İzin verildi |İzin verildi |İzin verildi |İzin verildi |
| Bağlanıldı |Süresi Doldu |Durduruldu |Durduruldu |İzin verildi |İzin verildi |
| Bağlanıldı |Yetki Kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |
| Kayıp bağlantısı > 15 gün |Etkin |Durduruldu |Durduruldu |İzin verildi |İzin verildi |
| Kayıp bağlantısı > 15 gün |Süresi Doldu |Durduruldu |Durduruldu |İzin verildi |İzin verildi |
| Kayıp bağlantısı > 15 gün |Yetki Kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |

### <a name="recovering-from-loss-of-connectivity"></a>Bağlantı kaybına karşı kurtarma
Bir güvenlik duvarı veya Azure'a erişimi engelleyen bir proxy varsa, güvenlik duvarı/proxy profilinde aşağıdaki etki alanı adreslerini beyaz liste ile gerekir:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Azure bağlantı Azure yedekleme sunucusu makineye geri yüklendikten sonra gerçekleştirilebilir işlemleri Azure aboneliği durumuna göre belirlenir. Yukarıdaki tabloda makineye bağlı"sonra" izin verilen işlemleri hakkında ayrıntılar bulunur.

### <a name="handling-subscription-states"></a>Abonelik durumları işleme
Azure aboneliğinden olması mümkündür bir *süresi doldu* veya *Deprovisioned* durumunu *etkin* durumu. Ancak durumu ancak bu bazı ürün davranışı şifrelemelerini *etkin*:

* A *Deprovisioned* abonelik bu sağlaması kaldırılıyor. sağlaması dönem için işlevselliğini kaybeder. Dönüş üzerinde *etkin*, yedekleme/geri yükleme ürün işlevselliğini hale getirilir. Yerel disk üzerindeki yedekleme verileri de yeterli büyüklükte bekletme süresi ile bıraktıysanız alınabilir. Abonelik girdikten sonra ancak yedekleme verilerini Azure irretrievably kaybolur *Deprovisioned* durumu.
* Bir *süresi doldu* abonelik hale getirdiği kadar işlevselliği için yalnızca kaybederse *etkin* yeniden. Abonelik olan dönem için zamanlanan yedeklemeleri *süresi doldu* çalışmaz.

## <a name="troubleshooting"></a>Sorun giderme
Microsoft Azure Yedekleme Sunucusu Kurulum aşaması (veya yedekleme veya geri yükleme sırasında) hataları ile başarısız olursa, bu başvuru [hata kodları belge](https://support.microsoft.com/kb/3041338) daha fazla bilgi için.
Ayrıca başvurabilirsiniz [Azure Backup ile ilgili sık sorulan sorular](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı bilgi almak [DPM için ortamınızı hazırlama](https://technet.microsoft.com/library/hh758176.aspx) Microsoft TechNet sitesindeki. Ayrıca, üzerinde Azure yedekleme sunucusu dağıtılan ve kullanılabilecek desteklenen yapılandırmalar hakkında bilgi içerir.

Bu makaleler, Microsoft Azure yedekleme sunucusu kullanarak iş yükü koruması daha derin bir anlayış kazanmak için kullanabilirsiniz.

* [SQL Server Yedekleme](backup-azure-backup-sql.md)
* [SharePoint server yedekleme](backup-azure-backup-sharepoint.md)
* [Diğer sunucu yedekleme](backup-azure-alternate-dpm-server.md)
