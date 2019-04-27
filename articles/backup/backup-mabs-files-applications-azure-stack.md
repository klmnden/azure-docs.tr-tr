---
title: Azure Stack Vm'leri dosyalarında yedekleme
description: Azure Backup, yedekleme ve Azure Stack dosyalarının ve uygulamalarının Azure Stack ortamınıza kurtarmak için kullanın.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/5/2018
ms.author: adigan
ms.openlocfilehash: 67d79f2aa41bab8a14d693098538d22ffeb05a4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60848805"
---
# <a name="back-up-files-on-azure-stack"></a>Azure Stack'te dosyaları yedekleme
Azure Backup, korumak (veya yedeklemek için) kullanabilirsiniz dosyaları ve Azure Stack'te uygulamaları. Dosya ve uygulamaları yedeklemek için Azure Stack üzerinde çalışan bir sanal makine olarak Microsoft Azure Backup sunucusu yükleyin. Aynı sanal ağdaki herhangi bir Azure Stack sunucuda dosyaları koruyabilirsiniz. Azure Backup sunucusu yükledikten sonra kısa vadeli yedekleme verileri için kullanılabilir yerel depolama alanını artırmak için Azure disk ekleyin. Azure Backup sunucusu, uzun süreli saklama için Azure depolama kullanır.

> [!NOTE]
> Azure Backup sunucusu ve System Center Data Protection Manager (DPM) benzer olsa da, DPM Azure Stack ile kullanmak için desteklenmiyor.
>

Bu makalede, Azure Backup sunucusu yükleme Azure Stack ortamında kapsamaz. Azure Stack'te Azure Backup sunucusu yüklemek için bkz [Azure Backup sunucusu yükleme](backup-mabs-install-azure-stack.md).


## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>Dosya ve klasörleri Azure Stack vm'lerinin Azure'a yedekleme

Azure Stack sanal makineler'de dosyaları korumak için Azure Backup sunucusu yapılandırmak için Azure Backup Sunucusu konsolunu açın. Konsolunda, koruma gruplarını yapılandırma ve sanal makinelerinizi şirket verilerini korumak için kullanacaksınız.

1. Azure Backup sunucusu konsolunda **koruma** ve araç çubuğunda **yeni** açmak için **yeni koruma grubu oluşturma** Sihirbazı.

   ![Azure Backup sunucusu konsolunda korumasını yapılandırma](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    Bu Sihirbazı'nı açmak için birkaç saniye sürebilir. Sihirbaz bitince **sonraki** ilerletmek için **koruma grubu türünü seçin** ekran.

   ![Yeni koruma grubu Sihirbazı'nı açar.](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. Üzerinde **koruma grubu türünü seçin** ekran öğesini **sunucuları** tıklatıp **sonraki**.

    ![Yeni koruma grubu Sihirbazı'nı açar.](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    **Grup üyelerini seçin** ekranı açılır. 

    ![Yeni koruma grubu Sihirbazı'nı açar.](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. İçinde **grup üyelerini seçin** ekranında **+** alt öğeleri listesini genişletin. Korumak istediğiniz tüm öğeleri için onay kutusunu işaretleyin. Tüm öğeleri seçtikten sonra tıklayın **sonraki**.

    ![Yeni koruma grubu Sihirbazı'nı açar.](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    Microsoft, bir koruma İlkesi bir koruma grubu paylaşacak tüm verilerden yararlanabiliyor önerir. System Center DPM makaleye bakın hakkında planlama ve koruma gruplarını dağıtma eksiksiz bilgi [koruma gruplarını dağıtma](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1801).

4. İçinde **veri koruma yöntemini seçin** ekranında, koruma grubu için bir ad yazın. Onay kutusunu seçip **vadeli koruma istiyorum:** ve **çevrimiçi koruma istiyorum**. **İleri**’ye tıklayın.

    ![Yeni koruma grubu Sihirbazı'nı açar.](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    Seçilecek **çevrimiçi koruma istiyorum**, önce seçmeniz gerekir. **vadeli koruma istiyorum:** Disk. Tek seçim kısa dönem koruma için disk, bu nedenle, azure Backup sunucusu bant olarak korumaz.

5. İçinde **kısa vadeli hedefleri belirtin** ekranında, disk ve artımlı yedeklemeleri kaydetmek ne zaman kaydedildi kurtarma noktalarını tutmak ne kadar süre seçin. **İleri**’ye tıklayın.

    > [!IMPORTANT]
    > Yapmanız gerekenler **değil** beş günden fazla bir süre için Azure Backup sunucusu bağlı disklerde operasyonel Kurtarma (Yedekleme) verileri korur.
    >

    ![Yeni koruma grubu Sihirbazı'nı açar.](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png) 

    Hızlı çalıştırmak için artımlı yedeklemeler için bir aralık seçmek yerine hemen her önce tam yedekleme zamanlanmış kurtarma noktası, tıklayın **bir kurtarma noktasından hemen önce**. Uygulama iş yüklerini koruyorsanız, (uygulamanın artımlı yedeklemeleri desteklediği koşuluyla) Azure Backup sunucusu başına eşitlemesinin sıklığı kurtarma noktası oluşturur. Uygulama artımlı yedeklemeleri desteklemiyorsa, Azure Backup sunucusu hızlı çalışan tam yedekleme.

    İçin **dosya kurtarma noktaları**, Kurtarma noktaları oluşturmak ne zaman belirtin. Tıklayın **Değiştir** kurtarma noktalarının oluşturulma haftanın günleri ve saatler ayarlamak için.

6. İçinde **disk ayırmasını gözden geçirin** ekranında, koruma grubu için ayrılmış depolama havuzu disk alanını inceleyin.

    **Toplam veri boyutu** yedeklemek istediğiniz veri boyutu ve **sağlanacak alan Disk** üzerinde Azure Backup sunucusu koruma grubu için önerilen alandır. Azure Backup sunucusu ayarları temel alarak ideal yedekleme birimini seçer. Ancak, Disk ayırma ayrıntıları yedekleme birimi seçeneklerini düzenleyebilirsiniz. İş yükleri için açılan menüden tercih edilen depolamayı seçin. Düzenlemeleriniz kullanılabilir Disk depolama alanı bölmesinde toplam depolama alanı ve boş depolama alanı için değerleri değiştirin. Yetersiz sağlanmış alan Azure Backup sunucusu, yedeklemeler sayesinde gelecekte sorunsuz bir şekilde devam etmek için birime eklemenizi önerdiği depolama miktarıdır.

7. İçinde **çoğaltma oluşturma yöntemini seçin**nasıl ilk tam veri çoğaltmasını istediğinizi seçin. Ağ üzerinden çoğaltmasına karar verirseniz, yoğun olmayan bir saat seçtiğiniz Azure önerir. Büyük miktarda veri ve en iyi durumda olmayan ağ koşulları için verileri çıkarılabilir medya kullanarak çoğaltmayı göz önünde bulundurun.

8. İçinde **tutarlılık denetimi seçenekleri**nasıl tutarlılık denetimleri otomatikleştirilmesini istediğinizi seçin. Veri çoğaltma tutarsız hale geldiğinde veya bir zamanlamaya göre çalıştırmak tutarlılık denetimlerini etkinleştir. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, istediğiniz zaman el ile denetim çalıştırın:
    * İçinde **koruma** Azure Backup sunucusu konsolunun alana sağ tıklayın ve koruma grubunu seçin **tutarlılık denetimi gerçekleştir**.

9. Üzerinde Azure'a yedeklemek isterseniz **çevrimiçi koruma verilerini belirtin** sayfasında Azure'a yedeklemek istediğiniz iş yüklerinin seçili olduğundan emin olun.

10. İçinde **çevrimiçi yedekleme zamanlamasını**, azure'a artımlı yedeklemelerin ne zaman gerçekleşmesi gerektiğini belirtin. 

    Her gün/hafta/ay/yıl ve hangi çalışacakları saat/tarih çalışacak yedeklemeler zamanlayabilirsiniz. Yedeklemeler günde ortaya çıkabilir. Bir yedekleme işinin çalışma, her zaman Azure Backup sunucusu diskte depolanan yedeklenmiş verilerin kopyasından Azure üzerinde bir kurtarma noktası oluşturulur.

11. İçinde **çevrimiçi bekletme ilkesini belirtin**, günlük/Haftalık/Aylık/yıllık yedeklerden oluşturulan kurtarma noktalarının Azure'da nasıl bekletileceğini belirtin.

12. İçinde **çevrimiçi çoğaltma seçin**, ilk tam veri çoğaltmanın nasıl gerçekleştirildiğini belirtin. 

13. Üzerinde **özeti**, ayarlarınızı gözden geçirin. Tıkladığınızda **Grup Oluştur**, ilk veri çoğaltma gerçekleşir. Veri kopyalama tamamlandığında, üzerinde **durumu** sayfasında, koruma grubunun durumu gösteren olarak **Tamam**. İlk yedekleme işini ayarlarına uygun olarak bir koruma grubu gerçekleşir.

## <a name="recover-file-data"></a>Dosya verilerini kurtarma

Sanal makinenize verileri kurtarmak için Azure Backup Sunucusu konsolunu kullanın.

1. Azure Backup sunucusu konsolunda, gezinti çubuğunda tıklatın **kurtarma** kurtarmak istediğiniz verilerin göz atın. Sonuçlar bölmesinde verileri seçin.

2. Kurtarma noktaları bölümündeki takvimde Kalın tarihleri kullanılabilir kurtarma noktalarını belirtin. Kurtarma tarihi seçin.

3. İçinde **kurtarılabilir öğe** bölmesinde, kurtarmak istediğiniz öğeyi seçin.

4. İçinde **eylemleri** bölmesinde tıklayın **kurtarmak** Kurtarma Sihirbazı'nı açın.

5. Veriler aşağıdaki gibi kurtarabilirsiniz:

    * **Özgün konumuna kurtarma** -istemci bilgisayar VPN üzerinden bağlı ise, bu seçeneği çalışmıyor. Bunun yerine alternatif bir konum kullanın ve verileri o konumdan kopyalayın.
    * **Alternatif bir konuma Kurtar**

6. Kurtarma seçeneklerini belirtin:

    * İçin **var olan sürüm kurtarma davranışı**seçin **kopya oluştur**, **atla**, veya **üzerine yaz**. Üzerine yalnızca özgün konuma kurtarma yaptığınızda kullanılabilir.
    * İçin **geri güvenlik**, seçin **ayarlarını hedef bilgisayara Uygula** veya **kurtarma noktası sürümünün güvenlik ayarlarını uygula**.
    * İçin **ağ bant genişliği kullanımını azaltma**, tıklayın **Değiştir** ağ bant genişliği kullanımını azaltmayı etkinleştirmek için.
    * **Bildirim** tıklayın **kurtarma tamamlandığında e-posta Gönder**, ve bildirimi alacak olan alıcıları belirtin. E-posta adreslerini virgülle ayırın.
    * Seçimleri yaptıktan sonra tıklayın **İleri**

7. Kurtarma ayarlarınızı gözden geçirin ve tıklayın **kurtarmak**. 

    > [!Note] 
    > Kurtarma işi devam ederken, seçilen kurtarma öğeleri için tüm eşitleme işleri iptal edilir.
    >

Modern yedekleme depolama alanı (MBS) kullanıyorsanız, dosya sunucusu son kullanıcı Kurtarma (EUR) desteklenmez. Dosya sunucusu EUR Birim Gölge Kopyası Hizmeti (Modern yedekleme depolama alanı kullanmaz, VSS üzerinde), bir bağımlılığı vardır. EUR etkinleştirilirse, verileri kurtarmak için aşağıdaki adımları kullanın:

1. Korumalı dosyalara gidin ve dosya adını sağ tıklatın ve seçin **özellikleri**.

2. Üzerinde **özellikleri** menüsünde tıklatın **önceki sürümler** ve kurtarmak istediğiniz sürümü seçin.

## <a name="view-azure-backup-server-with-a-vault"></a>Bir kasa ile görünümü Azure Backup sunucusu
Azure portalında Azure Backup sunucusu olan varlıkları görüntülemek için aşağıdaki adımları izleyin:
1. Kurtarma Hizmetleri kasasını açın.
2. Yedekleme Altyapısı'nı tıklatın.
3. Yedekleme Yönetimi sunucularının görünümü.

## <a name="see-also"></a>Ayrıca bkz.
Diğer iş yüklerini korumak için Azure Backup sunucusu kullanma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:
- [SharePoint grubunu yedekleme](https://docs.microsoft.com/azure/backup/backup-mabs-sharepoint-azure-stack)
- [SQL server'ı yedekleme](https://docs.microsoft.com/azure/backup/backup-mabs-sql-azure-stack)
