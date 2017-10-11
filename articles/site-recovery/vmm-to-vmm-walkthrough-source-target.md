---
title: "Kaynak ve hedef Hyper-V çoğaltma Azure Site Recovery ile ikincil site için ayarlama | Microsoft Docs"
description: "Kaynak ve hedef Azure Site Recovery ile ikincil VMM sitesi için Hyper-V sanal makineleri çoğaltırken ayarlamak açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 07135e9b5e619971a59cc22ec68a0a4e8bcaabe1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-set-up-the-replication-source-and-target"></a>6. adım: çoğaltma kaynağı ve hedef ayarlama


Kurtarma Hizmetleri oluşturduktan sonra için Hyper-V çoğaltmasını içeren ikincil VMM sitesi için kasa [Azure Site Recovery](site-recovery-overview.md), kaynak ve hedef çoğaltma konumları ayarlamak için bu makaleyi kullanın. 

Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.




## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Azure Site kurtarma sağlayıcısı VMM sunucularında yükleme ve bulmak ve sunucuları kasaya kaydetmek.

1. Tıklatın **1. adım: altyapıyı hazırlama** > **kaynak**.

    ![Kaynağı ayarlama](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. **Kaynağı ayarla** kısmında, bir VMM sunucusu eklemek için **+ VMM**'ye tıklayın.

    ![Kaynağı ayarlama](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. İçinde **Sunucu Ekle**, denetleyin **System Center VMM sunucusunun** görünür **sunucu türü** ve VMM sunucusu karşılayan [Önkoşullar](#prerequisites).
4. Azure Site Recovery Sağlayıcısı yükleme dosyasını indirin.
5. Kayıt anahtarını indirin. Kurulumu çalıştırırken buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. VMM sunucusunda Azure Site Recovery Sağlayıcısı'nı yükleyin. Açıkça şey Hyper-V ana bilgisayar sunucuları üzerinde yüklemeniz gerekmez.


## <a name="install-the-azure-site-recovery-provider"></a>Azure Site kurtarma Sağlayıcısı'nı yüklemek

1. Sağlayıcı kurulum dosyasını her VMM sunucusunda çalıştırın. VMM bir kümede dağıtılmışsa, aşağıdaki ilk yüklediğinizde yapın:
    -  Etkin bir düğümde bir sağlayıcı yükleyin ve VMM sunucusunu kasaya kaydetmek için yüklemeyi sonlandırın.
    - Ardından, sağlayıcı diğer düğümlere yükleyin. Küme düğümlerini tüm sağlayıcının aynı sürümüne çalıştırmanız gerekir.
2. Kurulum, birkaç ön koşul denetimlerini çalıştırır ve VMM hizmetini durdurmak için izin ister. Kurulum bittiğinde VMM hizmeti otomatik olarak yeniden başlatılır. Bir VMM kümesinde yüklerseniz, küme rolünü durdurmanız istenir.
3. İçinde **Microsoft Update**, sağlayıcı güncelleştirmeleri Microsoft Update ilkenize uygun yüklü olduğunu belirtmek için de seçebilirsiniz.
4. İçinde **yükleme**kabul edin veya varsayılan yükleme konumunu değiştirmek ve tıklatın **yükleme**.

    ![Yükleme konumu](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. Yükleme tamamlandıktan sonra tıklayın **kaydetmek** sunucuyu kasaya kaydetmek için.

    ![Yükleme konumu](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. **Kasa adı** alanında, sunucunun kayıtlı olduğu kasanın adını doğrulayın. *İleri*’ye tıklayın.

    ![Sunucu kaydı](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. İçinde **Internet bağlantısı**, Azure'a VMM sunucusunda çalışan sağlayıcı nasıl bağlandığını belirtin.

    ![İnternet Ayarları](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - Sağlayıcı İnternete doğrudan ya da bir proxy üzerinden bağlanmalısınız belirtebilirsiniz.
   - Gerekiyorsa proxy ayarlarını belirtin.
   - Bir proxy kullanıyorsanız, belirtilen proxy kimlik bilgileri kullanılarak otomatik olarak bir VMM RunAs hesabı (DRAProxyAccount) oluşturulur. Bu hesabın kimlik doğrulamasını başarıyla gerçekleştirebilmesi için ara sunucuyu yapılandırın. RunAs hesabı ayarları VMM konsolundan değiştirilebilir > **ayarları** > **güvenlik** > **farklı çalıştır hesapları**. Değişiklikleri güncelleştirme için VMM hizmetini yeniden başlatın.
8. **Kayıt Anahtarı** kısmında, Azure Site Recovery'den indirdiğiniz anahtarı seçin ve VMM sunucusuna kopyalayın.
9. Şifreleme ayarı yalnızca Hyper-V sanal makinelerini Azure'daki VMM bulutlarına çoğaltırken kullanılır. İkincil bir siteye çoğaltma yapıyorsanız kullanılmaz.
10. **Sunucu adı** alanında, kasadaki VMM sunucusunu tanımlamak için bir kolay ad belirtin. Küme yapılandırmasında VMM kümesi rolü adını belirtin.
11. İçinde **Eşitle bulut meta verileri**VMM sunucusundaki tüm bulutların meta verilerini kasayla eşitlemek istediğinizi seçin. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Tüm Bulutları eşitlemek istemiyorsanız bu ayarı işaretsiz bırakın ve her Bulutu VMM konsolundaki bulut özelliklerinde tek tek eşitleyin.
12. İşlemi tamamlamak için **İleri**'ye tıklayın. Kayıttan sonra Azure Site Recovery tarafından VMM sunucusundan meta veriler alınır. Sunucu, kasadaki **Sunucular** sayfasında **VMM Sunucuları** sekmesinde görüntülenir.

    ![Sunucu](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. Sunucu Site Recovery konsolunda kullanılabilir duruma getirildikten sonra **kaynak** > **kaynağı** VMM sunucusunu seçin ve Hyper-V konağı bulunduğu Bulutu seçin. Daha sonra, **Tamam**'a tıklayın.

Sağlayıcı komut satırından da yükleyebilirsiniz:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef VMM sunucusunu ve Bulutu seçin:

1. Tıklatın **altyapıyı hazırlama** > **hedef**ve kullanmak istediğiniz hedef VMM sunucusunu seçin.
2. Site Recovery ile eşitlenmiş Bulutlar sunucuda görüntülenir. Hedef bulut seçin.

   ![Hedef](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a>Sonraki adımlar

Git [adım 7: ağ eşlemesini yapılandırma](vmm-to-vmm-walkthrough-network-mapping.md).
