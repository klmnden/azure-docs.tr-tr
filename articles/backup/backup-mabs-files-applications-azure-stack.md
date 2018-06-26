---
title: Azure yığın VM'ler dosyalarında yedeklemek
description: Azure Backup, yedekleme ve Azure yığın dosyaları ve uygulamaları Azure yığın ortamınıza kurtarmak için kullanın.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/5/2018
ms.author: adigan
ms.openlocfilehash: 2fb3bad56de781dd81d4c5f82b734c9420c75dee
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751713"
---
# <a name="back-up-files-on-azure-stack"></a>Azure yığında dosyalarını yedekleyin
Koruma (veya yedeklemek için) Azure Yedekleme'yi kullanabilirsiniz dosyalar ve uygulamalar Azure yığında. Dosya ve uygulamaların yedeklemek için Azure yığın üzerinde çalışan bir sanal makine olarak Microsoft Azure yedekleme sunucusu yükleyin. Aynı sanal ağda herhangi bir Azure yığın sunucuda dosyaları koruyabilirsiniz. Azure yedekleme sunucusu kez yüklediğiniz, kısa vadeli yedekleme verileri için kullanılabilir yerel depolama artırmak için Azure disk ekleyin. Azure yedekleme sunucusu uzun vadeli bekletme için Azure storage kullanır.

> [!NOTE]
> Azure yedekleme sunucusu ve System Center Data Protection Manager (DPM) benzer olmakla birlikte, DPM Azure yığını ile kullanım için desteklenmez.
>

Bu makalede Azure yığın ortamında Azure yedekleme sunucusu yükleme kapsamaz. Azure yığında Azure yedekleme sunucusu yüklemek için bkz [Azure yedekleme sunucusu yükleme](backup-mabs-install-azure-stack.md).


## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>Dosya ve klasörleri Azure yığın VM'de Azure'a yedekleyin

Azure yığın VM'ler sanal makinelerinizde dosyaları korumak için Azure yedekleme Sunucusu'nu yapılandırmak için Azure Yedekleme Sunucusu konsolunu açın. Koruma gruplarını yapılandırma ve sanal makinelerinizi üzerindeki verileri korumak için konsol kullanırsınız.

1. Azure yedekleme sunucusu konsolunda **koruma** ve araç çubuğunda **yeni** açmak için **yeni koruma grubu oluşturma** Sihirbazı.

   ![Azure yedekleme sunucusu konsolunda korumasını yapılandırma](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    Sihirbazı'nı açmak için birkaç saniye sürebilir. Sihirbazı açılır sonra tıklayın **sonraki** ilerletmek için **koruma grubu türünü seçin** ekran.

   ![Yeni koruma grubu Sihirbazı'nı açar](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. Üzerinde **koruma grubu türünü seçin** ekranında, seçin **sunucuları** tıklatıp **sonraki**.

    ![Yeni koruma grubu Sihirbazı'nı açar](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    **Grup üyelerini seçin** ekranı açılır. 

    ![Yeni koruma grubu Sihirbazı'nı açar](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. İçinde **grup üyelerini seçin** ekranında **+** alt öğeler listesini genişletin. Korumak istediğiniz tüm öğeleri için onay kutusunu seçin. Tüm öğeleri seçildikten sonra tıklatın **sonraki**.

    ![Yeni koruma grubu Sihirbazı'nı açar](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    Microsoft, bir koruma İlkesi, bir koruma grubuna paylaşacak tüm veri yerleştirme önerir. Tam bilgi hakkında planlama ve koruma grupları, dağıtma System Center DPM makalesine bakın [dağıtmak koruma grupları](https://docs.microsoft.com/en-us/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1801).

4. İçinde **veri koruma yöntemini seçin** ekranında, koruma grubu için bir ad yazın. Onay kutusunu seçip **kullanarak kısa vadeli koruma istiyorum:** ve **çevrimiçi koruma istiyorum**. **İleri**’ye tıklayın.

    ![Yeni koruma grubu Sihirbazı'nı açar](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    Seçilecek **çevrimiçi koruma istiyorum**, önce seçmeniz gerekir. **kullanarak kısa vadeli koruma istiyorum:** Disk. Diske kısa süreli koruma için yalnızca bir seçimdir şekilde azure yedekleme sunucusu bant olarak korumaz.

5. İçinde **kısa vadeli hedefleri belirtin** ekranında, disk ve artımlı yedeklemeler kaydetmek ne zaman için kaydedilmiş kurtarma noktalarını korumak için ne kadar süreyle seçin. **İleri**’ye tıklayın.

    > [!IMPORTANT]
    > Yapmanız gerekenler **değil** beş günden fazla bir süre için Azure yedekleme sunucusuna bağlı disklerde işletimsel Kurtarma (Yedekleme) verileri korur.
    >

    ![Yeni koruma grubu Sihirbazı'nı açar](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png) 

    Hızlı çalıştırmak için artımlı yedeklemeler için bir aralık seçmek yerine hemen her önce tam yedekleme zamanlanmış kurtarma noktası, tıklatın **bir kurtarma noktasından hemen önce**. Uygulama iş yüklerini koruyorsanız (uygulamanın artımlı yedeklemeleri desteklediği sağlanan) Azure yedekleme sunucusu eşitleme sıklığı zamanlaması başına kurtarma noktası oluşturur. Uygulama artımlı yedeklemeleri desteklemiyorsa, Azure yedekleme sunucusu hızlı çalışan tam yedekleme.

    İçin **dosya kurtarma noktaları**, Kurtarma noktaları oluşturma zamanı belirtin. Tıklatın **Değiştir** kurtarma noktaları oluşturduğunuzda haftanın günleri ve saatleri ayarlamak için.

6. İçinde **disk ayırmasını gözden** ekranında, koruma grubu için ayrılmış depolama havuzu disk alanını inceleyin.

    **Toplam veri boyutu** yedeklemek istediğiniz veri boyutu ve **Disk sağlanacak alan** Azure yedekleme sunucusuna koruma grubu için önerilen bir alandır. Azure Backup sunucusu ayarlarınızı temel alan ideal bir yedekleme birim seçer. Ancak, Disk ayırma ayrıntıları yedekleme birimi seçimleri düzenleyebilirsiniz. İş yükleri için tercih edilen depolama açılır menüde seçin. Düzenlemeleriniz değerleri kullanılabilir Disk depolama alanı bölmesinde toplam depolama alanı ve boş depolama için değiştirin. Azure yedekleme sunucusu yedeklemelerde sorunsuz gelecekte devam etmek için birime eklediğiniz öneren depolama miktarını underprovisioned alanıdır.

7. İçinde **çoğaltma oluşturma yöntemini seçmek**, ilk tam veri çoğaltmayı düzenlemek istediğiniz yöntemini seçin. Ağ üzerinden çoğaltmasına karar verirseniz, yoğun olmayan bir zamanı seçin Azure önerir. Büyük miktarlarda veri veya en iyi durumda olmayan ağ koşulları için verileri çıkarılabilir medya kullanarak çoğaltmayı düşünebilirsiniz.

8. İçinde **tutarlılık denetimi seçeneklerini belirleyin**, tutarlılık denetimleri otomatikleştirmek istediğiniz yöntemini seçin. Yalnızca veri çoğaltma tutarsız hale geldiğinde veya bir zamanlamaya göre çalıştırmak tutarlılık denetimleri etkinleştirin. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, el ile denetim tarafından herhangi bir zamanda çalıştırın:
    * İçinde **koruma** Azure yedekleme sunucusu Konsolu'nun alanı seçin ve koruma grubunu sağ tıklatın **tutarlılık denetimi gerçekleştir**.

9. Üzerinde Azure'a yedeklemek isterseniz **çevrimiçi koruma verilerini belirtin** sayfası Azure'a yedeklemek istediğiniz iş yüklerini seçili olduğundan emin olun.

10. İçinde **çevrimiçi yedekleme zamanlamasını**, Azure için artımlı yedeklemeleri ne zaman gerçekleşeceğini belirtin. 

    Her gün/hafta/ay/yıl ve hangi çalışacakları saat/tarih çalışacak yedeklemeler zamanlayabilirsiniz. Yedekleme günde en fazla iki kez ortaya çıkabilir. Bir yedekleme işi her çalıştığında, Azure yedekleme sunucusu diskte depolanan yedeklenmiş verileri kopyasından Azure'da bir veri kurtarma noktası oluşturulur.

11. İçinde **çevrimiçi bekletme ilkesini belirtin**, günlük/Haftalık/Aylık/yıllık yedeklemelerden oluşturulan kurtarma noktalarının Azure'da nasıl korunur belirtin.

12. İçinde **çevrimiçi çoğaltma seçin**, ilk tam veri çoğaltmasının nasıl gerçekleştiğini belirtin. 

13. Üzerinde **Özet**, ayarlarınızı gözden geçirin. Tıkladığınızda **Grup Oluştur**, ilk veri çoğaltma oluşur. Veri çoğaltma tamamlandığında, üzerinde **durum** sayfasında, koruma grubunun durumu gösterir olarak **Tamam**. İlk yedekleme işini ayarlarına uygun olarak koruma grubu gerçekleşir.

Yanıtlanması gereken Soru: nasıl, genişletin Azure yığın kısa vadeli disk depolama için disk depolama alanı. Kısa vadeli disk depolama açıklayan çıkışı çağrılacak gereksinim kuralları nelerdir?

## <a name="recover-file-data"></a>Dosya verilerini kurtarma

Sanal makinenize verileri kurtarmak için Azure Yedekleme Sunucusu konsolunu kullanın.

1. Azure yedekleme sunucusu konsolunda, gezinti çubuğunda tıklatın **kurtarma** ve kurtarmak istediğiniz verileri gözatın. Sonuçlar bölmesinde verileri seçin.

2. Kurtarma noktaları bölümündeki takvimde Kalın tarihleri kurtarma noktası kullanılabilir gösterir. Bir kurtarma noktası kurtarmak için tarihi seçin.

3. İçinde **kurtarılabilir öğe** bölmesinde, kurtarmak istediğiniz öğeyi seçin.

4. İçinde **Eylemler** bölmesinde tıklatın **kurtarmak** Kurtarma Sihirbazı'nı açmak için.

5. Verileri aşağıdaki şekilde kurtarabilirsiniz:

    * **Özgün konuma Kurtar** -istemci bilgisayar VPN üzerinden bağlı ise, bu seçeneği çalışmıyor. Bunun yerine alternatif bir konum kullanın ve ardından verileri o konumdan kopyalayabilirsiniz.
    * **Alternatif konuma Kurtar**

6. Kurtarma seçeneklerini belirtin:

    * İçin **var olan sürüm kurtarma davranışı**seçin **kopya oluştur**, **atla**, veya **üzerine yaz**. Üzerine yalnızca özgün konuma kurtarma olduğunda kullanılabilir.
    * İçin **geri güvenlik**, seçin **hedef bilgisayarın ayarlarını uygula** veya **kurtarma noktası sürümünün güvenlik ayarlarını uygula**.
    * İçin **ağ bant genişliği kullanımını azaltma**, tıklatın **Değiştir** ağ bant genişliği kullanımını azaltmayı etkinleştirmek için.
    * **Bildirim** tıklatın **kurtarma tamamlandığında bir e-posta Gönder**, ve bildirimi alacak alıcıları belirtin. E-posta adreslerini virgülle ayırın.
    * Seçimlerinizi yaptıktan sonra tıklatın **sonraki**

7. Kurtarma ayarlarınızı gözden geçirin ve tıklayın **kurtarmak**. 

    > [!Note] 
    > Kurtarma işi devam ederken, seçilen kurtarma öğelerini tüm eşitleme işleri iptal edilir.
    >

Modern yedekleme depolama alanı (MB) kullanıyorsanız, dosya sunucusu son kullanıcı Kurtarma (EUR) desteklenmiyor. Dosya sunucusu EUR Birim Gölge Kopyası Hizmeti (Modern yedekleme depolama kullanan değil VSS üzerinde), bir bağımlılığa sahiptir. EUR etkinleştirilirse, verileri kurtarmak için aşağıdaki adımları kullanın:

1. Korumalı dosyalara gidin ve dosya adını sağ tıklatın ve seçin **özellikleri**.

2. Üzerinde **özellikleri** menüsünde tıklatın **önceki sürümler** ve kurtarmak istediğiniz sürümü seçin.

## <a name="view-azure-backup-server-with-a-vault"></a>Görünüm Azure yedekleme sunucusu bir kasa ile
Azure Portal'da Azure yedekleme sunucusu varlıkları görüntülemek için aşağıdaki adımları izleyebilirsiniz:
1. Kurtarma Hizmetleri kasası açın.
2. Yedekleme Altyapısı'ı tıklatın.
3. Görünüm yedek yönetim sunucusu.

## <a name="see-also"></a>Ayrıca bkz.
Diğer iş yüklerini korumak için Azure Backup'ı kullanma hakkında daha fazla bilgi için aşağıdaki makalelere birine bakın:
- [SharePoint grubunun kurulumu yedekleyin](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sharepoint-azure-stack)
- [SQL server'ı Yedekle](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sql-azure-stack)
