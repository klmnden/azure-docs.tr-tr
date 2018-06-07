---
title: Azure kurtarma Hizmetleri kasasını Sil '
description: Bu makalede, bir kurtarma Hizmetleri kasasını Sil açıklanmaktadır. Makale bir kasa silmeyi deneyin, ancak olamaz sorun giderme adımlarını içerir.
services: service-name
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: markgal
ms.openlocfilehash: 844a70aa6fe003c6ad5816aaec9c32db9104c620
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605349"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme
Bu makalede, Azure portalında bir kurtarma Hizmetleri kasasını Sil açıklanmaktadır. Yedekleme kasaları olsaydı, Kurtarma Hizmetleri kasalarının dönüştürüldü.   

Kurtarma Hizmetleri kasası silinmesi olduğundan, bir tek adımlı işlem - *tüm kaynakları içermiyor. kasaya sağlanan*. Kurtarma Hizmetleri kasası silmeden önce kaldırın veya tüm kaynakları kasadaki silmeniz gerekir. Kaynakları içeren bir kasa silme denerseniz, aşağıdaki görüntü gibi bir hata alıyorsunuz:

![Kasa silme hatası](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Kasa kaynaklardan temizlediniz kadar tıklatarak **yeniden deneme** aynı hata üretir. Bu hata iletisini takılmış tıklatmak **iptal** ve kasadaki kaynaklarını silmek için aşağıdaki adımları kullanın.

## <a name="removing-items-from-a-vault-protecting-a-vm"></a>Bir VM koruma kasadan öğeleri kaldırma
Açık kurtarma Hizmetleri kasası zaten varsa, ikinci adıma atlayın.

1. Azure Portalı'nı açın ve Panodan silmek istediğiniz kasaya açın.

   Hub menüsünde panoya sabitlenmiş kurtarma Hizmetleri kasası yoksa tıklatın **daha Hizmetleri** ve kaynak listesinde yazın **kurtarma Hizmetleri**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasaları**’na tıklayın.

   ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Kurtarma Hizmetleri kasalarının listesi görüntülenir. Listeden silmek istediğiniz kasayı seçin.

   ![Kasa listesinden seçin](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. Kasa Görünümü'nde bakmak **Essentials** bölmesi. Bir kasa silmek için korumalı öğeler olamaz. Varsa **yedekleme öğeleri** veya **yönetim sunucularını yedekleme** sıfır gösterme öğelerden kaldırmanız gerekir. Veri içeriyorsa, kasaya silemezsiniz.

    ![Korumalı öğeler için Essentials bölmesi bakın](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    VM'ler ve dosyaların/klasörlerin yedekleme öğeleri olarak kabul edilir ve listelenen **yedekleme öğeleri** Essentials bölmesinin alanını. Bir DPM sunucusu listelenen **Yedekleme Yönetimi Sunucusu** Essentials bölmesinin alanını. **Öğeleri çoğaltılan** Azure Site Recovery hizmetine ilgilidir.
3. Korumalı öğeleri kasadan kaldırma başlamak için kasaya öğeleri bulur. Kasa panosunda tıklatın **ayarları**ve ardından **yedekleme öğeleri** bu menüsünü açın.

    ![Kasa listesinden seçin](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    **Yedekleme öğeleri** menü öğesi türüne göre ayrı listeleri vardır: Azure sanal makine ya da dosya klasörleri (görüntü bakın). Gösterilen varsayılan öğesi türü Azure sanal makineleri listesidir. Kasaya dosya-klasör öğeleri listesini görüntülemek için seçin **dosya klasörleri** açılır menüsünden.
4. Bir VM koruma kasadan bir öğesini silmeden önce öğesi'nin yedekleme işini durdurmak ve kurtarma noktası verilerini silmeniz gerekir. Kasadaki her öğe için şu adımları izleyin:

    a. Üzerinde **yedekleme öğeleri** menüsünde öğeyi sağ tıklatın ve bağlam menüsünden seçin **Dur yedekleme**.

    ![yedekleme işini durdurma](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Durdur yedekleme menüsü açılır.

    b. Üzerinde **durdurmak yedekleme** menüsünde gelen **bir seçenek belirleyin** menüsünde, select **yedekleme verilerini Sil** > öğesinin adını yazın > tıklatıp **Dur yedekleme**.

    Bunu silmek istediğinizde doğrulamak için öğesinin adı yazın. **Durdurmak yedekleme** öğeyi doğrulayın sonra düğmesini etkinleştirir. Yedekleme öğesinin adını yazmanız için iletişim kutusu görmüyorsanız, seçtiğiniz **yedekleme verileri koru** seçeneği.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    İsteğe bağlı olarak, bir nedeni neden, verileri siliniyor ve açıklamaları ekleme sağlayabilir. Tıklattıktan sonra **durdurmak yedekleme**, kasaya silmeye çalışmadan önce tamamlamak silme işlemi izin. İş tamamlandığını doğrulamak için Azure iletilerini denetleyin ![yedekleme verilerini silmek](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra hizmeti bir ileti gönderir: yedekleme işlemi durduruldu ve yedekleme verileri silindi.

    c. Listeden bir öğe üzerinde sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **yenileme** kasadaki kalan öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede hiç öğe olduğunda kaydırın **Essentials** kurtarma Hizmetleri kasası menü bölmesinde. Döndürmemelidir olması herhangi **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan** listelenir. Öğeleri kasaya görüntülenmeye devam ederse, üç adıma geri dönün ve farklı öğe türü listesini seçin.  
5. Kasa araç çubuğunda başka öğe yok olduğunda tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-vault.png)
6. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.

## <a name="what-if-i-stopped-the-backup-process-but-retained-the-data"></a>Ne yedekleme işlemi durduruldu ancak veri tutulur?
Yedekleme işlemi ancak yanlışlıkla durdurduysanız *korunur* verileri kasa silmeden önce yedekleme verilerini silmeniz gerekir. Yedekleme verilerini silmek için:

1. Üzerinde **yedekleme öğeleri** menüsünde öğeyi sağ tıklatın ve bağlam menüsünde **yedekleme verileri Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    **Yedekleme verilerini Sil** menüsü açılır.
2. Üzerinde **yedekleme verilerini Sil** menüsünde öğesinin adını yazın ve tıklatın **silmek**.

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

    Kasayı silme için işaretini kaldırın veya silin, korunan verilerin kasası. Birçok kurtarma noktaları ve koruma grubundaki veri varsa, verileri silmek için birkaç dakika sürebilir. **Korumayı Durdur** iletişim kutusunu gösterir zaman işi tamamlandı.

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

    ![Üretim sunucularında menüsünü açın](./media/backup-azure-delete-vault/delete-production-server.png)

    **Üretim sunucuları** menüsü açılır ve kasadaki tüm üretim sunucuları listeler.

    ![Üretim sunucuları listesi](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. Üzerinde **üretim sunucuları** menüsünde, sunucu üzerinde sağ tıklayın ve **silmek**.

    ![Üretim sunucusunu Sil ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    **Silmek** menüsü açılır.

    ![Üretim sunucusunu Sil ](./media/backup-azure-delete-vault/delete-blade.png)
3. Üzerinde **silmek** menüsünde, sunucu adını doğrulayın ve tıklatın **silmek**. Doğru etkinleştirmek için sunucunun adı olmalıdır **silmek** düğmesi.

    Kasa silindikten sonra kasa silinmiş bildiren bir ileti alırsınız. Tüm sunucuları kasaya sildikten sonra geri kasa Panosu Essentials bölmesinde kaydırın.
4. Kasa panosunda olmadığından emin olun hiçbir **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan**. Kasa araç çubuğunda tıklatın **silmek**.
5. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.

## <a name="find-the-backup-management-servers-registered-to-the-vault"></a>Yedekleme Yönetimi sunucuları kasaya kayıtlı Bul
Bir kasaya kayıtlı birden çok sunucu varsa, bunları unutmayın zor olabilir. Kasaya kayıtlı sunucuları görmek ve bunları silmek için:

1. Kasa panosunu açın.
2. İçinde **Essentials** bölmesinde tıklatın **ayarları** menüyü açmak için.

    ![Ayarlar menüsünü açın](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. Üzerinde **ayarları** menüsünde tıklatın **Yedekleme Altyapısı**.
4. Üzerinde **Yedekleme Altyapısı** menüsünde tıklatın **yedekleme yönetim sunucuları**. Yedekleme Yönetimi sunucuları menüsü açılır.

    ![Yedekleme yönetim sunucuları listesi](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. Listeden bir sunucu silmek için sunucu adına sağ tıklayın ve ardından **silmek**.
    **Silmek** menüsü açılır.
6. Üzerinde **silmek** menüsünde sunucunun adını belirtin. Uzun bir adı olması durumunda kopyalayın ve yedekleme yönetim sunucuların listesinden yapıştırın. Sonra **Sil**’e tıklayın.  
