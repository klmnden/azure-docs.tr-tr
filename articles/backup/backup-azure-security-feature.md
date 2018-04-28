---
title: Azure Yedekleme'yi karma yedeklemeleri korunmasına yardımcı olmak için güvenlik özellikleri | Microsoft Docs
description: Güvenlik özellikleri yedeklemeleri daha güvenli hale getirmek için Azure Backup ile kullanmayı öğrenin
services: backup
documentationcenter: ''
author: JPallavi
manager: vijayts
editor: ''
ms.assetid: 47bc8423-0a08-4191-826d-3f52de0b4cb8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pajosh
ms.openlocfilehash: 2529d19dbf0ca0fb59f5abe48be3e8b14e862e29
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="security-features-to-help-protect-hybrid-backups-that-use-azure-backup"></a>Azure Yedekleme'yi karma yedeklemeleri korunmasına yardımcı olmak için güvenlik özellikleri
Kötü amaçlı yazılım, yazılımı ve yetkisiz erişim, gibi güvenlik sorunları hakkında endişeleriniz artmaktadır. Bu güvenlik sorunları para ve veri açısından pahalı olabilir. Bu tür saldırılara karşı koruma sağlamak için Azure yedekleme karma yedeklemeleri korunmasına yardımcı olmak için güvenlik özellikleri sağlar. Bu makalede, etkinleştirme ve Azure kurtarma Hizmetleri aracısını ve Azure yedekleme Sunucusu'nu kullanarak bu özellikleri kullanan alınmaktadır. Bu özellikler şunlardır:

- **Önleme**. Bir parola değiştirme gibi kritik bir işlem gerçekleştirildiğinde ek bir kimlik doğrulama katmanı eklenir. Bu doğrulama gibi işlemleri yalnızca geçerli Azure kimlik bilgilerine sahip kullanıcılar tarafından gerçekleştirilebilir sağlamaktır.
- **Uyarı**. Yedekleme verileri silme gibi kritik bir işlem gerçekleştirildiğinde abonelik Yönetici'ye bir e-posta bildirimi gönderilir. Bu e-posta kullanıcıya gibi eylemleri hakkında hızlı bir şekilde bildirilir sağlar.
- **Kurtarma**. Silinen yedekleme verileri silme tarihten itibaren 14 gün için ek bir korunur. Bu yüzden hiçbir veri kaybı saldırının olsa bile bu belirli bir süre içinde verilerin kurtarılabilirliği sağlar. Ayrıca, büyük sayıda en az kurtarma noktası bozuk veri karşı koruma sağlamak için korunur.

> [!NOTE]
> Altyapı (ıaas) VM yedekleme olarak kullanıyorsanız, güvenlik özellikleri etkinleştirilmemelidir. Bu özellikler etkinleştirmeden etkilerini kalmaması henüz Iaas VM yedekleme için kullanılabilir değildir. Yalnızca kullanıyorsanız, güvenlik özellikleri etkinleştirilmiş olmalıdır: <br/>
>  * **Azure Backup Aracısı**. En düşük aracı sürümü 2.0.9052. Bu özellikler etkinleştirdikten sonra kritik işlemleri gerçekleştirmek için bu aracı sürümüne yükseltmeniz gerekir. <br/>
>  * **Azure Backup sunucusu**. En düşük Azure Yedekleme aracısı sürümü 2.0.9052 Azure yedekleme sunucusu ile güncelleştirme 1. <br/>
>  * **System Center Data Protection Manager**. En düşük Azure Yedekleme aracısı sürümü 2.0.9052 Data Protection Manager 2012 R2 UR12 veya Data Protection Manager 2016 UR2. <br/> 


> [!NOTE]
> Bu özellikler, yalnızca kurtarma Hizmetleri kasası için kullanılabilir. Yeni oluşturulan tüm kurtarma Hizmetleri kasalarının varsayılan olarak etkinleştirilen bu özellikler vardır. Var olan kurtarma Hizmetleri kasalarının kullanıcılar, bu özellikler aşağıdaki bölümde belirtilen adımlar kullanarak etkinleştirin. Sonra özelliklerinin etkin olduğu, tüm kurtarma Hizmetleri Aracısı Bilgisayarları için Azure yedekleme sunucu örnekleri, uygulamak ve kayıtlı Data Protection Manager sunucuları kasaya. Bu ayarın etkinleştirilmesi tek seferlik bir işlemdir ve etkinleştirdikten sonra bu özellikleri devre dışı bırakılamıyor.
>

## <a name="enable-security-features"></a>Güvenlik özellikleri sağlar
Kurtarma Hizmetleri kasası oluşturuyorsanız, tüm güvenlik özelliklerini kullanabilirsiniz. Varolan bir kasa ile çalışıyorsanız, aşağıdaki adımları izleyerek güvenlik özellikleri sağlar:

1. Azure portalında Azure kimlik bilgilerinizi kullanarak oturum açın.
2. Seçin **Gözat**ve türü **kurtarma Hizmetleri**.

    ![Ekran görüntüsü, Azure portal gözatma seçeneği](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    Kurtarma hizmetleri kasalarının listesi görünür. Bu listeden bir kasa seçin. Seçilen kasa panosu açılır.
3. Listeden kasasında altında görüntülenen öğelerin **ayarları**, tıklatın **özellikleri**.

    ![Ekran görüntüsü, Kurtarma Hizmetleri kasası seçenekleri](./media/backup-azure-security-feature/vault-list-properties.png)
4. Altında **güvenlik ayarları**, tıklatın **güncelleştirme**.

    ![Ekran görüntüsü, Kurtarma Hizmetleri kasası özellikleri](./media/backup-azure-security-feature/security-settings-update.png)

    Güncelleştirme bağlantı açar **güvenlik ayarları** özelliklerinin bir özetini sağlar ve bunları sağlamanıza olanak tanır, dikey.
5. Aşağı açılan listeden **Azure multi-Factor Authentication yapılandırdığınız?**, etkinleştirdiyseniz, onaylamak için bir değer seçin [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md). Etkinleştirilirse, Azure portalında oturum açma sırasında başka bir CİHAZDAN (örneğin, cep telefonu) kimlik doğrulaması için istenir.

   Yedekleme kritik işlemler gerçekleştirdiğinizde, PIN, Azure portalında kullanılabilir güvenlik girmeniz gerekir. Azure multi-Factor Authentication etkinleştirilmesi bir güvenlik katmanı ekler. Yalnızca yetkili kullanıcıların geçerli Azure kimlik bilgileriyle ve ikinci bir CİHAZDAN kimlik doğrulaması, Azure portalına erişebilir.
6. Güvenlik ayarları kaydetmek için seçin **etkinleştirmek** tıklatıp **kaydetmek**. Seçebileceğiniz **etkinleştirmek** yalnızca arasında bir değer seçtikten sonra **Azure multi-Factor Authentication yapılandırdığınız?** önceki adımda listesi.

    ![Güvenlik ayarları ekran görüntüsü](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>Yedekleme verileri Kurtar silindi
Yedekleme ek 14 gün boyunca silinen yedekleme verilerini korur ve onu silmez varsa, hemen **Dur yedekleme yedekleme verileri silmek ile** işlemi gerçekleştirilir. 14 günlük süre içinde bu verileri geri yüklemek için kullanmakta olduğunuz bağlı olarak aşağıdaki adımları uygulayın:

İçin **Azure kurtarma Hizmetleri aracısını** kullanıcılar:

1. Yedekler nerede gerçekleştiği bilgisayar hala kullanılabilir durumdaysa kullanmak [aynı makineye veri Kurtar](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) tüm eski kurtarma noktalarından kurtarmak için Azure kurtarma Hizmetleri.
2. Bu bilgisayarda kullanılabilir durumda değilse, kullanın [alternatif bir makine kurtarması](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) bu verileri almak üzere başka bir Azure kurtarma hizmetleri bilgisayar kullanmak için.

İçin **Azure yedekleme sunucusu** kullanıcılar:

1. Sunucunun yedekler nerede gerçekleştiği hala kullanılabilir ise, silinen veri kaynaklarını yeniden korumak ve kullanmak **verileri kurtarabilirsiniz** tüm eski kurtarma noktalarından Kurtarma özelliği.
2. Bu sunucu kullanılabilir değilse, [başka bir Azure yedekleme sunucusundan veri Kurtar](backup-azure-alternate-dpm-server.md) bu verileri almak için başka bir Azure yedekleme sunucusu örneği kullanmak için.

İçin **Data Protection Manager** kullanıcılar:

1. Sunucunun yedekler nerede gerçekleştiği hala kullanılabilir ise, silinen veri kaynaklarını yeniden korumak ve kullanmak **verileri kurtarabilirsiniz** tüm eski kurtarma noktalarından Kurtarma özelliği.
2. Bu sunucu kullanılabilir değilse, [dış DPM Ekle](backup-azure-alternate-dpm-server.md) bu verileri almak için başka bir Data Protection Manager sunucusu kullanmak üzere.

## <a name="prevent-attacks"></a>Saldırıları önlemek
Denetimleri yalnızca geçerli kullanıcıların çeşitli işlemler gerçekleştirebilirsiniz emin olmak için eklenmiştir. Bunlar, fazladan bir kimlik doğrulama katmanı ekleme ve kurtarma amacıyla bir minimum bekletme aralığı koruyarak içerir.

### <a name="authentication-to-perform-critical-operations"></a>Kritik işlemleri gerçekleştirmek için kimlik doğrulaması
Kimlik doğrulaması kritik işlemler için fazladan bir katmanı eklemenin bir parçası olarak, gerçekleştirdiğinizde güvenlik PIN girmeniz istenir **Delete verilerle korumayı Durdur** ve **değişiklik parola** işlemleri.

Bu PIN almak için:

1. Azure Portal’da oturum açın.
2. Gözat **kurtarma Hizmetleri kasası** > **ayarları** > **özellikleri**.
3. Altında **güvenlik PIN**, tıklatın **Generate**. Bu Azure kurtarma Hizmetleri Aracısı kullanıcı arabiriminde girilmesi için PIN kodunu içeren bir dikey pencere açılır.
    Bu PIN yalnızca beş dakika için geçerlidir ve bu süreden sonra otomatik olarak oluşturulan.

### <a name="maintain-a-minimum-retention-range"></a>Minimum bekletme aralığı koru
Her zaman geçerli bir kurtarma noktası sayısı kullanılabilir olduğundan emin olmak için aşağıdaki denetimleri eklenmiştir:

- Günlük bekletme, en az **yedi** gün bekletme yapılması.
- Haftalık bekletme, en az **dört** hafta bekletme yapılması.
- Aylık bekletme, en az **üç** aylık saklamayla yapılması.
- Yıllık bekletme, en az **bir** bekletme yılın yapılması.

## <a name="notifications-for-critical-operations"></a>Bildirimleri kritik işlemleri için
Genellikle, kritik bir işlem gerçekleştirilirken Abonelik Yöneticisi işleminin ayrıntılarını içeren bir e-posta bildirimi gönderilir. Bu bildirimler için ek e-posta alıcıları Azure portalını kullanarak yapılandırabilirsiniz.

Bu makalede açıklanan güvenlik özellikleri hedeflenen saldırılara karşı savunma mekanizmaları sağlar. Daha da önemlisi, saldırının olursa, bu özellikler, verilerinizi kurtarmak için kabiliyeti sağlar.

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
| İşlem | Hata ayrıntıları | Çözüm |
| --- | --- | --- |
| İlke değişikliği |Yedekleme İlkesi değiştirilemedi. Hata: Bir iç hata nedeniyle [0x29834] geçerli işlem başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse lütfen Microsoft desteğine başvurun. |**Neden:**<br/>Güvenlik ayarları etkin, desteklenmeyen bir sürümünde olan ve, yukarıda belirtilen minimum değerler aşağıda saklama aralığını çalıştığınızda bu hatayı gelir (desteklenen sürümleri, bu makalenin ilk notta belirtilir). <br/>**Önerilen eylem:**<br/> Bu durumda, yukarıda belirtilen minimum bekletme (günlük, haftalık, üç hafta aylık veya yıllık yedekleme için bir yıl için dört hafta için yedi gün) saklama dönemi ayarlamalısınız İlkesi ile devam etmek için güncelleştirmeleri ilgili. İsteğe bağlı olarak, tercih edilen yaklaşım Azure yedekleme sunucusu ve/veya DPM UR tüm güvenlik güncelleştirmelerini yararlanmak için yedekleme aracısını güncelleştirmek olur. |
| Parola değiştirme |Güvenlik girdiğiniz PIN yanlış. (KİMLİK: 100130) Bu işlemi tamamlamak için doğru güvenlik PIN sağlar. |**Neden:**<br/> (Parola değiştirme gibi) kritik işlemi gerçekleştirirken geçersiz veya süresi dolmuş güvenlik PIN girdiğinizde, bu hata verilir. <br/>**Önerilen eylem:**<br/> İşlemi tamamlamak için geçerli güvenlik PIN girmeniz gerekir. PIN almak için Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na gidin > Ayarlar > Özellikler > Güvenlik PIN oluşturun. Bu PIN, parolayı değiştirmek için kullanın. |
| Parola değiştirme |İşlem başarısız oldu. KİMLİĞİ: 120002 |**Neden:**<br/>Bu hata, güvenlik ayarları etkin, parola değiştirmeye ve desteklenmeyen sürümüne (geçerli sürümleri, bu makalenin ilk notta belirtilen) gelir.<br/>**Önerilen eylem:**<br/> Parolayı değiştirmek için ilk yedekleme aracı için en düşük sürüm en düşük 2.0.9052, en düşük güncelleştirme 1, Azure yedekleme sunucusuna güncelleştirmeniz gerekir ve/veya en az DPM 2012 R2 UR12 için DPM'yi veya DPM 2016 UR2 (indirme aşağıdaki bağlantıları), ardından geçerli güvenlik PIN girin. PIN almak için Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na gidin > Ayarlar > Özellikler > Güvenlik PIN oluşturun. Bu PIN, parolayı değiştirmek için kullanın. |

## <a name="next-steps"></a>Sonraki adımlar
* [Azure kurtarma Hizmetleri kasası ile çalışmaya başlama](backup-azure-vms-first-look-arm.md) bu özellikleri etkinleştirmek için.
* [En son Azure kurtarma Hizmetleri Aracısı'nı indirme](http://aka.ms/azurebackup_agent) , yedekleme verilerinizi saldırılara karşı koruma ve Windows bilgisayarların korunmasına yardımcı olmak için.
* [En son Azure yedekleme Sunucusu'nu Yükle](https://aka.ms/latest_azurebackupserver) iş yüklerini korumak ve yedekleme verilerinizi saldırılara karşı koruma sağlamak için.
* [System Center 2012 R2 Data Protection Manager için UR12 karşıdan](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) veya [UR2 System Center 2016 Data Protection Manager için karşıdan](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager) iş yüklerini korumak ve yedekleme verilerinizi saldırılara karşı koruma sağlamak için.
