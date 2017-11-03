---
title: "Azure sanal makinelerde Değişiklikleri İzle | Microsoft Docs"
description: "Değişiklik izleme, sanal makinelere dosyaları ve kayıt defteri değişiklikleri izlemek için kullanın."
services: automation
documentationcenter: automation
author: eslesar
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: eslesar
ms.custom: 
ms.openlocfilehash: 5c6e8390ec8533fc7ab281c212e47a6982b30f1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="track-changes-in-your-azure-virtual-machines"></a>Azure sanal makinelerde Değişiklikleri İzle

Değişiklik izleme etkinleştirerek, değişiklikleri dosyaları ve Windows kayıt defteri anahtarları için sanal makinelerde izleyebilirsiniz. Yapılandırma değişiklikleri tanımlayan işletim sorunları belirlemenize yardımcı olabilir.

Değişiklik izleme, Azure sanal makineden bir doğrudan etkinleştirebilirsiniz.

Bir Azure sanal makinesi yoksa, bir'ndaki yönergeleri izleyerek oluşturabilirsiniz [Windows Hızlı Başlangıç](../virtual-machines/windows/quick-create-portal.md) veya [Linux quickstart](../virtual-machines/linux/quick-create-portal.md) makalesi.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="enable-change-tracking-for-an-azure-virtual-machine"></a>Değişiklik bir Azure sanal makinesi için izlemeyi etkinleştir

1. Azure portalının sol tarafında **Sanal makineler**'i seçin.
2. Listede, bir sanal makineyi seçin.
3. Sanal makine penceresinde altında **Operations**seçin **değişiklik izleme**. 

   ![Değişiklik yerleşik vm izleme](./media/automation-vm-change-tracking/change-onboard-vm-blade.png)  
    **Güncelleştirme yönetimini etkinleştirme** penceresi açılır.

    Bu sanal makine için değişiklik izleme etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir. Değişiklik izleme etkin değilse, çözüm etkinleştirme seçeneği vermiş bir başlığı görüntülenir.

   ![Değişiklik izleme yerleşik yapılandırma başlığı](./media/automation-vm-change-tracking/change-onboard-banner.png)

4. Çözümü etkinleştirmek için başlığı seçin. Aşağıdaki öğeler yoksa, bunlar otomatik olarak eklenir:

   * [Günlük analizi](../log-analytics/log-analytics-overview.md) çalışma
   * [Otomasyon](../automation/automation-offering-get-started.md) hesabı

5. Değişiklikleri izlemek ve ardından bir Otomasyon hesabı seçin değişiklik izleme verileri günlükleri depolamak için bir günlük analizi çalışma alanı seçin **etkinleştirmek**.  
    Çözümün etkinleştirildiği durum çubuğunda bildirilir. Bu işlemin tamamlanması 15 dakika sürebilir.

## <a name="configure-change-tracking"></a>Değişiklik izlemeyi Yapılandır

Değişiklik izleme etkinleştirildikten sonra **değişiklik izleme** penceresi görüntülenir. 

Hangi dosya ve kayıt defteri anahtarlarını izlemek üzere seçmek için Seç **ayarlarını düzenleme**.

   ![Değişiklik izleme ayarlarını Düzenle](./media/automation-vm-change-tracking/change-edit-settings.png)

   **Çalışma alanı yapılandırmasını** penceresi açılır. 

İçinde **çalışma alanı yapılandırmasını** penceresinde, Windows kayıt defteri anahtarları, Windows dosyaları ya da Linux dosyaları izlenmesi gereken sonraki üç bölümde özetlendiği gibi ekleyin.

### <a name="add-a-windows-registry-key"></a>Windows kayıt defteri anahtarı ekleme

1. Üzerinde **Windows kayıt defteri** sekmesine **Ekle**.  
    **Değişiklik izleme için Windows kayıt defteri eklemek** penceresi açılır.

   ![Değişiklik izleme kayıt ekleyin](./media/automation-vm-change-tracking/change-add-registry.png)

2. Altında **etkin**seçin **doğru**.
3. İçinde **öğe adı** kutusuna, kolay bir ad girin.
4. (İsteğe bağlı) İçinde **grup** kutusunda, bir grup adı girin.
5. İçinde **Windows kayıt defteri anahtarı** kutusunda, izlemek istediğiniz kayıt defteri anahtarı adını girin.
6. **Kaydet**’i seçin.

### <a name="add-a-windows-file"></a>Bir Windows dosya ekleme

1. Üzerinde **Windows dosyalarını** sekmesine **Ekle**.  
    **Değişiklik izleme için Windows Dosya Ekle** penceresi açılır.

   ![Değişiklik izleme Windows dosyası ekleme](./media/automation-vm-change-tracking/change-add-win-file.png)

2. Altında **etkin**seçin **doğru**.
3. İçinde **öğe adı** kutusuna, kolay bir ad girin.
4. (İsteğe bağlı) İçinde **grup** kutusunda, bir grup adı girin.
5. İçinde **yolu girin** kutusunda, izlemek istediğiniz dosyanın tam yolunu ve dosya adını girin.
6. **Kaydet**’i seçin.

### <a name="add-a-linux-file"></a>Bir Linux dosyası ekleme

1. Üzerinde **Linux dosyaları** sekmesine **Ekle**.  
    **Değişiklik izleme için Linux Dosya Ekle** penceresi açılır.

   ![Değişiklik izleme Linux dosya ekleme](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. Altında **etkin**seçin **doğru**.
3. İçinde **öğe adı** kutusuna, kolay bir ad girin.
4. (İsteğe bağlı) İçinde **grup** kutusunda, bir grup adı girin.
5. İçinde **yolu girin** kutusunda, izlemek istediğiniz dosyanın tam yolunu ve dosya adını girin.
6. İçinde **yola** kutusunda, şunlardan birini seçin **dosya** veya **Directory**.
7. Altında **özyineleme**, belirtilen yol ve tüm dosyaları ve yolları altındaki değişiklikleri izlemek, seçmek için **üzerinde**. Yalnızca seçili yolu veya dosya izlemek için **devre dışı**.
8. Altında **kullanım Sudo**, gerekli dosyaları izlemek için `sudo` erişimi, select komutu **üzerinde**. Aksi takdirde seçin **devre dışı**.
9. **Kaydet**’i seçin.

## <a name="view-changes"></a>Değişiklikleri görüntüleme

İçinde **değişiklik izleme** penceresinde görüntüleyebilirsiniz değişiklikleri her çeşitli kategorileri, sanal makine için zaman içinde.

   ![Değişiklik izleme görünümü değişiklikleri Ekle](./media/automation-vm-change-tracking/change-view-changes.png)

Kategoriler ve görüntülemek için değişiklikleri zaman aralığını seçebilirsiniz. Pencerenin üst kısmında, grafik gösterimi değişiklikleri zaman içinde görüntüleyebilirsiniz. Pencerenin alt kısmındaki yapılan son değişikliklerin bir listesi görüntüleyebilirsiniz.

## <a name="view-change-tracking-log-data"></a>Değişiklik izleme günlüğü verilerini görüntüleme

Değişiklik izleme için günlük analizi gönderilen günlük verileri oluşturur. Sorguları çalıştırarak günlüklerini aramak için seçin **günlük analizi** en üstündeki **değişiklik izleme** penceresi.

   ![Değişiklik izleme günlük analizi](./media/automation-vm-change-tracking/change-log-analytics.png)

Çalıştıran ve günlük dosyalarını günlük analizi arama hakkında daha fazla bilgi için bkz: [günlük analizi](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* Değişiklik izleme hakkında daha fazla bilgi için bkz: [değişiklik izleme çözümü ile ortamınızdaki yazılım değişiklikleri izle](../log-analytics/log-analytics-change-tracking.md).
* Sanal makineler için güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz: [OMS güncelleştirme yönetimi çözümünde](../operations-management-suite/oms-solution-update-management.md).
