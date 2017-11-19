---
title: " Azure kurtarma Hizmetleri kasasını Sil | Microsoft Docs "
description: "Nasıl bir Azure yedekleme ve kurtarma Hizmetleri kasası silinir. Bir yedekleme kasası, bir Azure bulut kasası ya da Azure recovery kasası çağrılabilir. Klasik portalında veya Azure portalında bir yedekleme kasası silinemiyor zaman sorunlarını giderme."
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/10/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: b07b9e01a5a8d8a5189b130fb5a9baeef7a43f4f
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme
Azure Backup hizmetinde iki tür kasa bulunur: Backup kasası ve Kurtarma Hizmetleri kasası. Backup kasası ilk önce gelmiştir. Ardından genişletilmiş Resource Manager dağıtımlarını desteklemek üzere Kurtarma Hizmetleri kasası kullanıma sunulmuştur. Genişletilmiş özellikler ve kasasında depolanan bilgileri bağımlılıkları nedeniyle, bir yedekleme veya kurtarma Hizmetleri kasası silme kafa karıştırıcı olabilir. Bu makale, Klasik Portalı'nı ve Azure portalını kasalarında silmek açıklanmaktadır.  

| **Dağıtım türü** | **Portal** | **Kasa adı** |
| --- | --- | --- |
| Klasik |Klasik |Yedekleme kasası |
| Resource Manager |Azure |Kurtarma Hizmetleri kasası |

> [!NOTE]
> Yedekleme kasaları, Resource Manager ile dağıtılan çözümleri koruyamaz. Ancak, bir kurtarma Hizmetleri kasası classically dağıtılan sunucularını ve Vm'leri korumak için kullanabilirsiniz.  
>

> [!IMPORTANT]
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> Sonra **30 Kasım 2017**, yedekleme kasaları oluşturmak için PowerShell kullanmanız mümkün olmaz. <br/> **30 Kasım 2017 tarafından**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

Bu makalede, kullandığımız terim yedekleme kasası ya da kurtarma Hizmetleri kasası genel forma başvurmak için Kasa. Kasa arasında ayrım yapmak gerekli olduğunda resmi adı, yedekleme kasası ya da kurtarma Hizmetleri kasası kullanın.

## <a name="deleting-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası silme
Kurtarma Hizmetleri kasası silinmesi olduğundan, bir tek adımlı işlem - *tüm kaynakları içermiyor. kasaya sağlanan*. Kurtarma Hizmetleri kasası silmeden önce kaldırın veya tüm kaynakları kasadaki silmeniz gerekir. Kaynakları içeren bir kasa silme denerseniz, aşağıdaki görüntü gibi bir hata alıyorsunuz:

![Kasa silme hatası](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Kasa kaynaklardan temizlediniz kadar tıklatarak **yeniden deneme** aynı hata üretir. Bu hata iletisini takılmış tıklatmak **iptal** ve kasadaki kaynaklarını silmek için aşağıdaki adımları kullanın.

### <a name="removing-the-items-from-a-vault-protecting-a-vm"></a>Bir VM koruma kasadan öğeleri kaldırma
Açık kurtarma Hizmetleri kasası zaten varsa, ikinci adıma atlayın.

1. Azure Portalı'nı açın ve Panodan silmek istediğiniz kasaya açın.

   Hub menüsünde panoya sabitlenmiş kurtarma Hizmetleri kasası yoksa tıklatın **daha Hizmetleri** ve kaynak listesinde yazın **kurtarma Hizmetleri**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Tıklatın **kurtarma Hizmetleri kasaları**.

   ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Kurtarma Hizmetleri kasalarının listesi görüntülenir. Listeden silmek istediğiniz kasayı seçin.

   ![Kasa listesinden seçin](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. Kasa Görünümü'nde bakmak **Essentials** bölmesi. Bir kasa silmek için korumalı öğeler olamaz. Bir sayı ya da altında sıfır dışındaki görürseniz **yedekleme öğeleri** veya **yönetim sunucularını yedekleme**, kasaya silmeden önce bu öğeleri kaldırmanız gerekir.

    ![Korumalı öğeler için Essentials bölmesi bakın](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    VM'ler ve dosyaların/klasörlerin yedekleme öğeleri olarak kabul edilir ve listelenen **yedekleme öğeleri** Essentials bölmesinin alanını. Bir DPM sunucusu listelenen **Yedekleme Yönetimi Sunucusu** Essentials bölmesinin alanını. **Öğeleri çoğaltılan** Azure Site Recovery hizmetine ilgilidir.
3. Korumalı öğeleri kasadan kaldırma başlamak için kasaya öğeleri bulur. Kasa panosunda tıklatın **ayarları**ve ardından **yedekleme öğeleri** bu dikey penceresini açın.

    ![Kasa listesinden seçin](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    **Yedekleme öğeleri** dikey sahip öğe türüne göre ayrı listeler: Azure sanal makine ya da dosya klasörleri (görüntü bakın). Gösterilen varsayılan öğesi türü Azure sanal makineleri listesidir. Kasaya dosya-klasör öğeleri listesini görüntülemek için seçin **dosya klasörleri** açılır menüsünden.
4. Bir VM koruma kasadan bir öğesini silmeden önce öğesi'nin yedekleme işini durdurmak ve kurtarma noktası verilerini silmeniz gerekir. Kasadaki her öğe için şu adımları izleyin:

    a. Üzerinde **yedekleme öğeleri** dikey penceresinde, öğeyi sağ tıklatın ve bağlam menüsünden seçin **Dur yedekleme**.

    ![yedekleme işini durdurma](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Durdur yedekleme dikey pencere açılır.

    b. Üzerinde **durdurmak yedekleme** dikey penceresinde, gelen **bir seçenek belirleyin** menüsünde, select **yedekleme verilerini Sil** > öğesinin adını yazın > tıklatıp **Dur yedekleme**.

    Bunu silmek istediğinizde doğrulamak için öğesinin adı yazın. **Durdurmak yedekleme** öğeyi doğrulayın sonra düğmesini etkinleştirir. Yedekleme öğesinin adını yazmanız için iletişim kutusu görmüyorsanız, seçtiğiniz **yedekleme verileri koru** seçeneği.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    İsteğe bağlı olarak, bir nedeni neden, verileri siliniyor ve açıklamaları ekleme sağlayabilir. Tıklattıktan sonra **durdurmak yedekleme**, kasaya silmeye çalışmadan önce tamamlamak silme işlemi izin. İş tamamlandığını doğrulamak için Azure iletilerini denetleyin ![yedekleme verilerini silmek](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra yedekleme işlemi durduruldu ve bu öğe için yedekleme verileri silindi bildiren bir ileti alırsınız.

    c. Listeden bir öğe üzerinde sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **yenileme** kasadaki kalan öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede hiç öğe olduğunda kaydırın **Essentials** yedekleme kasası dikey bölmesinde. Döndürmemelidir olması herhangi **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan** listelenir. Öğeleri kasaya görüntülenmeye devam ederse, üç adıma geri dönün ve farklı öğe türü listesini seçin.  
5. Kasa araç çubuğunda başka öğe yok olduğunda tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-vault.png)
6. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.

## <a name="what-if-i-stopped-the-backup-process-but-retained-the-data"></a>Ne yedekleme işlemi durduruldu ancak veri tutulur?
Yedekleme işlemi ancak yanlışlıkla durdurduysanız *korunur* verileri kasa silmeden önce yedekleme verilerini silmeniz gerekir. Yedekleme verilerini silmek için:

1. Üzerinde **yedekleme öğeleri** dikey penceresinde, öğeyi sağ tıklatın ve bağlam menüsünde **yedekleme verileri Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    **Yedekleme verilerini Sil** dikey pencere açılır.
2. Üzerinde **yedekleme verilerini Sil** dikey penceresinde, öğenin adını yazın ve tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Veri sildikten sonra adım 4 c dönün ve işlemine devam edin.

## <a name="delete-a-vault-used-to-protect-a-dpm-server"></a>Bir DPM sunucusu korumak için kullanılan bir kasa silme
Bir DPM sunucusu korumak için kullanılan bir kasa silmeden önce oluşturulmuş bir kurtarma noktası temizleyin ve kasa sunucusundan kaydını gerekir.

Bir koruma grubuyla ilişkili verileri silmek için:

1. DPM Yönetici Konsolu'nda **koruma** > bir koruma grubu seçin > koruma grubu üyesini seçin > araç şeridinde tıklatıp **kaldırmak**.

  Etkinleştirmek için koruma grubu üyesini seçin **kaldırmak** araç şeridinde düğmesi. Örnekte, üyesidir **dummyvm9**. Birden çok üyeleri koruma grubuna seçmek için üye a tıklayarak Ctrl tuşunu basılı tutun.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    **Korumayı Durdur** iletişim kutusunu açar.
2. İçinde **korumayı Durdur** iletişim kutusunda **korumalı verileri Sil**, tıklatıp **korumayı Durdur**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    Kasayı silme için işaretini kaldırın veya silin, korunan verilerin kasası. Kurtarma noktaları ve veri koruma grubundaki sayısına bağlı olarak, herhangi bir yere birkaç saniyeden birkaç dakika verileri silmek için sürebilir. **Korumayı Durdur** iş tamamlandığında durumu iletişim kutusunu gösterir.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Bu işlem, tüm koruma gruplarındaki tüm üyeleri için devam edin.

    Tüm korumalı veriler ve koruma grupları kaldırın.
4. Tüm üyeleri koruma grubundan sildikten sonra Azure portalına geçin. Kasa panosunu açın ve olmadığından emin olun hiçbir **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan**. Kasa araç çubuğunda tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-vault.png)

    Yedekleme Yönetimi sunucuları kasaya kayıtlı varsa kasaya hiçbir veri olsa, kasaya silemezsiniz. Kasayla ilişkili yedekleme yönetim sunucuları silindi ancak listelenen sunucular **Essentials** bölmesinde bkz [kasaya kayıtlı yedekleme yönetim sunucularını bulmak](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault).
5. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.

## <a name="delete-a-vault-used-to-protect-a-production-server"></a>Bir üretim sunucusu korumak için kullanılan bir kasa silme
Bir üretim sunucusu korumak için kullanılan bir kasa silmeden önce silmek veya sunucu kasası'ndan kaydı silinemedi.

Kasayla ilişkili üretim sunucusunu silmek için:

1. Azure portalında kasa panosunu açın ve tıklatın **ayarları** > **Yedekleme Altyapısı** > **üretim sunucuları**.

    ![Üretim sunucularında dikey penceresini açın](./media/backup-azure-delete-vault/delete-production-server.png)

    **Üretim sunucuları** dikey penceresi açılır ve kasadaki tüm üretim sunucuları listeler.

    ![Üretim sunucuları listesi](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. Üzerinde **üretim sunucuları** dikey penceresinde, sunucu üzerinde sağ tıklayın ve **silmek**.

    ![Üretim sunucusunu Sil ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    **Silmek** dikey pencere açılır.

    ![Üretim sunucusunu Sil ](./media/backup-azure-delete-vault/delete-blade.png)
3. Üzerinde **silmek** dikey penceresinde, sunucu adını doğrulayın ve tıklatın **silmek**. Doğru etkinleştirmek için sunucunun adı olmalıdır **silmek** düğmesi.

    Kasa silindikten sonra kasa silinmiş bildiren bir ileti alırsınız. Tüm sunucuları kasaya sildikten sonra geri kasa Panosu Essentials bölmesinde kaydırın.
4. Kasa panosunda olmadığından emin olun hiçbir **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan**. Kasa araç çubuğunda tıklatın **silmek**.
5. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.

## <a name="delete-a-backup-vault-in-classic-portal"></a>Klasik Portalı'ndaki yedekleme kasalarının Sil
Klasik Portalı'ndaki bir yedekleme kasası silmek için aşağıdaki yönergeleri verilmiştir. Yedekleme kasası silmeden önce öğeleri yedeklenmiş kurtarma noktalarını silmeniz gerekir ve kayıtlı sunucuları kaldırın. Kayıtlı sunucuları, Windows Server, iş istasyonu veya kasaya kayıtlı olan sanal makineler olabilir.

1. Açık [Klasik portal](https://manage.windowsazure.com).

2. Yedekleme kasalarının listesinden silmek istediğiniz kasayı seçin.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    Kasa panosu açılır. Windows sunucuları ve/veya Azure kasa ile ilişkilendirilmiş sanal makine sayısı bakın. Ayrıca, Azure'da tüketilen toplam depolama bakın. Tüm yedekleme işleri durdurur ve kasa silmeden önce tüm veri.

3. Tıklatın **korunan öğeler** sekmesini ve ardından **korumayı Durdur**

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    **'Kasanızı' korumasını Durdur** iletişim kutusu görüntülenir.
4. İçinde **'kasanızı' korumasını Durdur** iletişim kutusunda, onay **Delete ilişkilendirilmiş yedekleme verileri** tıklatıp ![onay işareti](./media/backup-azure-delete-vault/checkmark.png). <br/>
    İsteğe bağlı olarak, korumayı durdurma nedeni seçin ve bir açıklama sağlayın.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    Kasadaki öğeleri sildikten sonra kasa boş olur.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)
5. Sekmeleri listesinde tıklatın **kayıtlı öğeler**. **Türü** açılır menüsünde, kasaya kayıtlı sunucu türünü seçmenize olanak sağlar. Türü, Windows Server ya da Azure sanal makine olabilir. Aşağıdaki örnekte, kasaya kayıtlı sanal makineyi seçin ve tıklatın **Unregister**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/classic-portal-unregister.png)

  Windows Server için kaydını silmek istediğiniz **türü** açılır menüsünde, select **Windows Server**, tıklatın ![onay işareti](./media/backup-azure-delete-vault/checkmark.png) ekranı yenilemek için ve ardından tıklatın **silmek**. <br/>

  ![Windows Server'ı seçin](./media/backup-azure-delete-vault/select-windows-server.png)

6. Sekmeleri listesinde tıklatın **Pano** Bu sekme açın. Kayıtlı sunucu ya da Azure sanal makineleri bulutta korumalı olmadığını doğrulayın. Ayrıca, depolama alanına veri olduğunu doğrulayın. Tıklatın **silmek** kasası silinemiyor.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    Silme yedekleme kasası onay ekranı açılır. Neden, kasaya silmekte olduğunuz öğesini tıklatıp bir seçenek belirleyin ![onay işareti](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    Kasa silinir ve klasik portal panosuna geri dönersiniz.

### <a name="find-the-backup-management-servers-registered-to-the-vault"></a>Yedekleme Yönetimi sunucuları kasaya kayıtlı Bul
Bir kasaya kayıtlı birden çok sunucu varsa, bunları unutmayın zor olabilir. Kasaya kayıtlı sunucuları görmek ve bunları silmek için:

1. Kasa panosunu açın.
2. İçinde **Essentials** bölmesinde, tıklatın **ayarları** bu dikey penceresini açın.

    ![ayarları dikey penceresini açın](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. Üzerinde **ayarlar dikey**, tıklatın **Yedekleme Altyapısı**.
4. Üzerinde **Yedekleme Altyapısı** dikey penceresinde tıklatın **yedekleme yönetim sunucuları**. Yedekleme Yönetimi sunucuları dikey pencere açılır.

    ![Yedekleme yönetim sunucuları listesi](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. Listeden bir sunucu silmek için sunucu adına sağ tıklayın ve ardından **silmek**.
    **Silmek** dikey pencere açılır.
6. Üzerinde **silmek** dikey penceresinde, sunucunun adını belirtin. Uzun bir adı olması durumunda kopyalayın ve yedekleme yönetim sunucuların listesinden yapıştırın. Ardından **silmek**.  
