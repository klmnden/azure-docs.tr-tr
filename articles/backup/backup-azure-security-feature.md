---
title: Azure Backup kullanan karma yedeklemeler korumaya yardımcı olmak için güvenlik özellikleri
description: Güvenlik özellikleri Azure Yedekleme'de yedeklemeleri daha güvenli hale getirmek için kullanmayı öğrenin
services: backup
author: utraghuv
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/08/2017
ms.author: utraghuv
ms.openlocfilehash: eaa0c0dc45b37491cd55033b49e2f78d219d416b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565710"
---
# <a name="security-features-to-help-protect-hybrid-backups-that-use-azure-backup"></a>Azure Backup kullanan karma yedeklemeler korumaya yardımcı olmak için güvenlik özellikleri
Yetkisiz erişim, kötü amaçlı yazılım ve fidye yazılımı gibi güvenlik konularıyla ilgili endişelerini artmaktadır. Bu güvenlik sorunları para ve veri açısından pahalı olabilir. Bu tür saldırılara karşı korunmak için Azure Backup artık karma yedeklemeler korumaya yardımcı olmak için güvenlik özellikleri sağlar. Bu makalede bir Azure kurtarma Hizmetleri aracısını ve Azure Backup sunucusu kullanarak bu özelliklerin nasıl etkinleştirileceği anlatılmaktadır. Bu özellikler şunlardır:

- **Önleme**. Bir parola değiştirme gibi kritik bir işlemin gerçekleştirildiğinde, ek bir kimlik doğrulama katmanı eklenir. Bu doğrulama gibi işlemleri yalnızca geçerli Azure kimlik bilgilerine sahip kullanıcılar tarafından gerçekleştirilebilir sağlamaktır.
- **Uyarı**. Yedekleme verilerini silme gibi kritik bir işlemin gerçekleştirildiğinde Abonelik Yöneticisi için bir e-posta bildirimi gönderilir. Bu e-posta, kullanıcının bu tür eylemleri hakkında hızlı bir şekilde bildirilir sağlar.
- **Kurtarma**. Silinmiş yedekleme verilerini silme tarihinden itibaren 14 gün için ek bir korunur. Bu yüzden veri kaybı olmadan bir saldırı olsa bile bu belirli bir süre içinde verilerin kurtarılabilirliğini sağlar. Ayrıca, büyük bir sayı en düşük kurtarma noktası bozuk verileri karşı koruma sağlamak için tutulur.

> [!NOTE]
> Güvenlik özellikleri olarak hizmet (Iaas) sanal makine yedekleme altyapınızı kullanıyorsanız etkinleştirilmemelidir. Bu özellikler için bunları etkinleştirerek hiçbir etkisi olmaz henüz Iaas VM yedekleme için kullanılabilir değildir. Yalnızca kullanıyorsanız güvenlik özelliklerinin etkinleştirilmesi gerekir: <br/>
>  * **Azure Backup Aracısı**. En düşük aracı sürümü olarak 2.0.9052. Bu özellikleri etkinleştirdikten sonra kritik işlemler gerçekleştirmek için bu aracı sürümüne yükseltmeniz gerekir. <br/>
>  * **Azure Backup sunucusu**. En düşük Azure Yedekleme aracısı sürümü 2.0.9052'yi Azure Backup sunucusu ile güncelleştirme 1. <br/>
>  * **System Center Data Protection Manager**. En düşük Azure Yedekleme aracısı sürümü 2.0.9052'yi Data Protection Manager 2012 R2 UR12 veya Data Protection Manager 2016 UR2 ile. <br/>


> [!NOTE]
> Bu özellikler, yalnızca kurtarma Hizmetleri kasası için kullanılabilir. Yeni oluşturulan tüm kurtarma Hizmetleri kasaları, varsayılan olarak etkin bu özelliklere sahip. Var olan kurtarma Hizmetleri kasaları için kullanıcılar, bu özellikler aşağıdaki bölümde anlatılan adımları kullanarak etkinleştirin. Özelliklerinin etkin olduğu, bunlar tüm kurtarma Hizmetleri Aracısı Bilgisayarları için Azure Backup sunucusu örnekleri, uygulayın ve Data Protection Manager sunucuları kaydedildi sonra kasayı. Bu ayarın etkinleştirilmesi tek seferlik bir işlemdir ve etkinleştirdikten sonra bu özellikleri devre dışı bırakılamıyor.
>

## <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştirme
Kurtarma Hizmetleri kasası oluşturuyorsanız, tüm güvenlik özelliklerini kullanabilirsiniz. Var olan bir kasa ile çalışıyorsanız, aşağıdaki adımları izleyerek güvenlik özellikleri sağlar:

1. Azure portalında Azure kimlik bilgilerinizi kullanarak oturum açın.
2. Seçin **Gözat**ve türü **kurtarma Hizmetleri**.

    ![Ekran görüntüsü, Azure portal gözatma seçeneği](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    Kurtarma hizmetleri kasalarının listesi görünür. Bu listeden bir kasa seçin. Seçilen kasa panosu açılır.
3. Kasa altında altında görünür öğeler listeden **ayarları**, tıklayın **özellikleri**.

    ![Ekran görüntüsü, Kurtarma Hizmetleri kasası seçenekleri](./media/backup-azure-security-feature/vault-list-properties.png)
4. Altında **güvenlik ayarları**, tıklayın **güncelleştirme**.

    ![Ekran görüntüsü, Kurtarma Hizmetleri kasası özellikleri](./media/backup-azure-security-feature/security-settings-update.png)

    Güncelleştirme bağlantı açar **güvenlik ayarları** dikey penceresinde hangi özelliklerin özeti sağlar ve bunları etkinleştirmenize imkan tanır.
5. Aşağı açılan listeden **Azure multi-Factor Authentication yapılandırdığınız?** , etkinleştirdiğiniz olmadığını onaylamak için bir değer seçin [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md). Etkinleştirilirse, Azure portalında oturum açma sırasında başka bir CİHAZDAN (örneğin, bir cep telefonu) kimlik doğrulaması istenir.

   Yedekleme kritik işlemler gerçekleştirdiğinizde, bir güvenlik PIN'i, Azure portalında kullanılabilir girmek zorunda. Azure çok faktörlü kimlik doğrulamasının etkinleştirilmesi bir güvenlik katmanı ekler. Yalnızca yetkili kullanıcıların geçerli Azure kimlik bilgileri ile ve ikinci bir CİHAZDAN kimlik doğrulaması, Azure portalına erişebilir.
6. Güvenlik ayarları kaydetmek için seçmeniz **etkinleştirmek** tıklatıp **Kaydet**. Seçebileceğiniz **etkinleştirmek** yalnızca bir değer seçtikten sonra **Azure multi-Factor Authentication yapılandırdığınız?** listesi önceki adımda.

    ![Güvenlik ayarları görüntüsü](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>Silinen yedekleme verileri kurtarma
Yedekleme silinen yedekleme verileri ek 14 gün boyunca saklar ve bu silmez varsa hemen **yedekleme verileri içeren yedeklemeyi durdurma** işlemi gerçekleştirildi. Bu veriler 14 günlük dönemin geri yüklemek için kullanmakta olduğunuz bağlı olarak aşağıdaki adımları uygulayın:

İçin **Azure kurtarma Hizmetleri aracısını** kullanıcılar:

1. Yedekleme işleminin gerçekleştiği bilgisayar hala kullanılabilir haldeyse, silinen veri kaynaklarını yeniden korumak ve kullanmak [aynı makineye veri Kurtar](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) tüm eski kurtarma noktalarından kurtarmak için Azure kurtarma Hizmetleri.
2. Bu bilgisayarın kullanılabilir değilse, [kurtarmak için başka bir makineyi](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) başka bir Azure kurtarma hizmetleri bilgisayar bu verileri almak için kullanılacak.

İçin **Azure Backup sunucusu** kullanıcılar:

1. Sunucunun yedekleme işleminin gerçekleştiği hala kullanılabilir haldeyse, silinen veri kaynaklarını yeniden korumak ve kullanmak **veri kurtarma** tüm eski kurtarma noktalarından kurtarmak için özellik.
2. Bu sunucu kullanılabilir değilse, [başka bir Azure Backup Sunucusu'ndan veri kurtarma](backup-azure-alternate-dpm-server.md) bu verileri almak için başka bir Azure Backup sunucusu örneği kullanmak için.

İçin **Data Protection Manager** kullanıcılar:

1. Sunucunun yedekleme işleminin gerçekleştiği hala kullanılabilir haldeyse, silinen veri kaynaklarını yeniden korumak ve kullanmak **veri kurtarma** tüm eski kurtarma noktalarından kurtarmak için özellik.
2. Bu sunucu kullanılabilir değilse, [dış DPM Ekle](backup-azure-alternate-dpm-server.md) başka bir Data Protection Manager sunucusunun bu verileri almak için kullanılacak.

## <a name="prevent-attacks"></a>Saldırılarını önleme
Denetimleri yalnızca geçerli kullanıcı çeşitli işlemler gerçekleştirebilirsiniz emin olmak için eklendi. Bunlar, ek bir kimlik doğrulama katmanı ekleyerek ve kurtarma amacıyla en düşük bekletme aralığı koruma içerir.

### <a name="authentication-to-perform-critical-operations"></a>Kritik işlemler gerçekleştirmek için kimlik doğrulaması
Kritik işlemler için kimlik doğrulaması ek bir koruma katmanı ekleme bir parçası olarak, gerçekleştirdiğinizde güvenlik PIN'i girmeleri istenir **silme verilerle korumayı Durdur** ve **değişiklik parola** operations.

> [!NOTE]
> 
> Şu anda güvenlik PIN'i için desteklenmiyor **silme verilerle korumayı Durdur** DPM ve MABS için.

Bu PIN'i almak için:

1. Azure Portal’da oturum açın.
2. Gözat **kurtarma Hizmetleri kasası** > **ayarları** > **özellikleri**.
3. Altında **güvenlik PIN'i**, tıklayın **Oluştur**. Bu, Azure kurtarma Hizmetleri Aracısı kullanıcı arabiriminde girilmesi için PIN kodunu içeren bir dikey pencere açılır.
    Yalnızca beş dakika boyunca bu PIN'in geçerli olduğu ve bu süreden sonra otomatik olarak oluşturulan.

### <a name="maintain-a-minimum-retention-range"></a>En düşük bekletme aralığı koru
Kullanılabilir olduğundan emin her zaman geçerli bir kurtarma noktası sayısını sağlamak için aşağıdaki denetimleri eklenmiştir:

- Günlük bekletme, en az **yedi** gün saklama yapılmalıdır.
- Haftalık bekletme, en az **dört** haftalık bekletme yapılmalıdır.
- Aylık bekletme, en az **üç** aylık saklamayla yapılmalıdır.
- Yıllık bekletme, en az **bir** yıllık bekletme süresi yapılmalıdır.

## <a name="notifications-for-critical-operations"></a>Kritik işlemler için bildirimleri
Genellikle, kritik bir işlemin gerçekleştirildiğinde, abonelik yöneticisinin işlemiyle ilgili ayrıntıları içeren bir e-posta bildirimi gönderilir. Azure portalını kullanarak ek e-posta alıcılarını bu bildirimleri yapılandırabilirsiniz.

Bu makalede bahsedilen güvenlik özellikleri hedeflenmiş saldırılara karşı savunma mekanizmaları sağlar. Daha da önemlisi, saldırının olursa, bu özellikler, veri kurtarma olanağı sağlar.

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
| İşlem | Hata Ayrıntıları | Çözüm |
| --- | --- | --- |
| İlke değişikliği |Yedekleme İlkesi değiştirilemedi. Hata: Geçerli işlem bir [0x29834] iç hizmet hatası nedeniyle başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse lütfen Microsoft desteğine başvurun. |**Neden:**<br/>Bu hata güvenlik ayarı etkinleştirilirse, desteklenmeyen sürümü kullandığınızdan ve, yukarıda belirtilen minimum değerleri aşağıda bekletme aralığını azaltabilir çalıştığınızda gelir (desteklenen sürümleri, bu makalenin ilk Not belirtilir). <br/>**Önerilen eylem:**<br/> Bu durumda, yukarıda belirtilen en düşük bekletme (günlük, haftalık, üç hafta aylık veya yıllık yedekleme için bir yıl için dört hafta için yedi gün) Bekletme dönemi ayarlamalısınız ilgili güncelleştirmeleri İlkesi ile devam edin. İsteğe bağlı olarak, Azure Backup sunucusu ve/veya DPM UR tüm güvenlik güncelleştirmelerini yararlanmak için yedekleme aracısını güncelleştirmek için tercih edilen yaklaşım olacaktır. |
| Parola değiştirme |Girilen güvenlik PIN'i hatalı. (Kimlik: 100130) bu işlemi tamamlamak için doğru güvenlik PIN'ini girin. |**Neden:**<br/> (Parola değişikliği gibi) kritik işlem gerçekleştirilirken güvenlik PIN'ini geçersiz veya süresi dolmuş girdiğinizde bu hatayı gelir. <br/>**Önerilen eylem:**<br/> İşlemi tamamlamak için geçerli güvenlik PIN'i girmeniz gerekir. PIN almak için Azure Portal'da oturum açın ve kurtarma Hizmetleri kasası > Ayarlar > Özellikler > Güvenlik PIN'i oluştur. Bu PIN, parolayı değiştirmek için kullanın. |
| Parola değiştirme |İşlem başarısız oldu. KİMLİĞİ: 120002 |**Neden:**<br/>Bu hata, güvenlik ayarları etkinleştirildiğinden, parola değiştirmeye ve desteklenmeyen bir sürümü (Bu makalenin ilk Not belirtilen geçerli sürümler) olan gelir.<br/>**Önerilen eylem:**<br/> Parolayı değiştirmek için ilk yedekleme aracı için en düşük sürümü olarak 2.0.9052 en düşük, en düşük güncelleştirme 1'için Azure Backup sunucusu güncelleştirmeniz gerekir ve/veya DPM sunucusuna en az DPM 2012 R2 UR12 veya DPM 2016 UR2 (indirme bağlantıları aşağıdaki), ardından geçerli güvenlik PIN'ini girin. PIN almak için Azure Portal'da oturum açın ve kurtarma Hizmetleri kasası > Ayarlar > Özellikler > Güvenlik PIN'i oluştur. Bu PIN, parolayı değiştirmek için kullanın. |

## <a name="next-steps"></a>Sonraki adımlar
* [Azure kurtarma Hizmetleri kasası ile çalışmaya başlama](backup-azure-vms-first-look-arm.md) bu özellikleri etkinleştirmek için.
* [En son Azure kurtarma Hizmetleri aracısını indirme](https://aka.ms/azurebackup_agent) yedekleme verilerinizi saldırılarına karşı koruma ve Windows bilgisayarların korunmasına yardımcı olmak için.
* [En son Azure Backup sunucusu indirme](https://aka.ms/latest_azurebackupserver) iş yüklerini korumak ve yedekleme verilerinizi saldırılarına karşı koruma sağlamak için.
* [System Center 2012 R2 Data Protection Manager için UR12 indirme](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) veya [UR2 System Center 2016 Data Protection Manager için indirme](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager) iş yüklerini korumak ve yedekleme verilerinizi saldırılarına karşı koruma sağlamak için.
