---
title: Azure yedekleme ile azure'a bir SAP HANA veritabanı yedekleme | Microsoft Docs
description: Bu öğretici, Azure'da Azure Backup hizmeti ile bir SAP HANA veritabanını yedeklemek açıklanmaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: raynew
ms.openlocfilehash: a16ed7134fc9f3c159715f58f116de3fb30e8aca
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481133"
---
# <a name="back-up-an-sap-hana-database"></a>Bir SAP HANA veritabanını yedekleme

[Azure yedekleme](backup-overview.md) azure'da SAP HANA veritabanlarını yedeklemeyi destekler.

> [!NOTE]
> Bu özellik şu anda genel Önizleme aşamasındadır. Üretime hazır değil ve garantili bir SLA yoktur. 

## <a name="scenario-support"></a>Senaryo desteği

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen coğrafi bölgeler** | Avustralya Güneydoğu, Doğu Avustralya <br> Güney Brezilya <br> Kanada Orta, Kanada Doğu <br> Güney Doğu Asya, Doğu Asya <br> Doğu ABD, Doğu ABD 2, Batı Orta ABD, Batı ABD, Batı ABD 2, Kuzey Orta ABD, Orta ABD, Güney Orta ABD<br> Hindistan Orta Hindistan, Güney Hindistan <br> Doğu Japonya, Batı Japonya<br> Kore Orta, Kore Güney <br> Kuzey Avrupa, Batı Avrupa <br> UK Güney, UK Batı
**Desteklenen VM işletim sistemleri** | SLES 12 SP2 veya SP3.
**Desteklenen HANA sürümleri** | SDC on HANA 1.x, MDC on HANA 2.x <= SPS03

### <a name="current-limitations"></a>Geçerli sınırlamalar

- Yalnızca, Azure Vm'lerinde çalışan SAP HANA veritabanlarını da yedekleyebilirsiniz.
- Bu gibi durumlarda, SAP HANA yedeklemesi yalnızca Azure portalında yapılandırabilirsiniz. Bu özellik, PowerShell, CLI veya REST API ile yapılandırılamaz.
- Yalnızca, ölçek büyütme modunda veritabanlarını da yedekleyebilirsiniz.
- 15 dakikada bir veritabanı günlüklerini yedekleyebilirsiniz. Günlük yedeklerinin, yalnızca veritabanı için başarılı bir tam yedekleme tamamlandıktan sonra akışı başlar.
- Tam ve farklı yedeklemelerini alabilir. Artımlı yedekleme şu anda desteklenmemektedir.
- SAP HANA yedeklemeler için uygulandıktan sonra yedekleme ilkesini değiştiremezsiniz. Farklı ayarlarla yedeklemek istiyorsanız, yeni bir ilke oluşturun veya farklı bir ilke atayabilirsiniz.
  - Yeni bir ilke oluşturmak için kasaya tıklayın **ilkeleri** > **yedekleme ilkeleri** >  **+ Ekle** > **SAP HANA'da Azure VM**, ilke ayarlarını belirtin.
  - Veritabanı çalıştıran sanal Makinenin özelliklerinde farklı bir ilke atamak için geçerli ilke adına tıklayın. Ardından **yedekleme İlkesi** sayfa yedekleme için kullanılacak farklı bir ilke seçin.

## <a name="prerequisites"></a>Önkoşullar

Yedeklemeleri yapılandırmadan önce aşağıdakileri yaptığınızdan emin olun:

1. VM üzerinde çalışan SAP HANA veritabanı resmi Microsoft Yükleme [.NET Core çalışma zamanı 2.1](https://dotnet.microsoft.com/download/linux-package-manager/sles/runtime-current) paket. Aşağıdakilere dikkat edin:
    - Yalnızca gereksinim duyduğunuz **dotnet çalışma zamanı 2.1** paket. İhtiyacınız olmayan **aspnetcore çalışma zamanı 2.1**.
    - VM İnternet'e erişmek için yansıtma veya dotnet çalışma zamanı 2.1 (ve tüm bağımlı RPMs) akışı Microsoft pakette belirtilen bir çevrimdışı-önbelleğe belirtin sayfasında yoksa.
    - Paket yüklemesi sırasında bir seçenek belirtmek için istenebilir. Bu durumda belirtin **Çözüm 2**.

        ![Paket yükleme seçeneği](./media/backup-azure-sap-hana-database/hana-package.png)

2. VM üzerinde yüklemek ve ODBC sürücü paketleri ortamından resmi SLES paket/zypper, kullanarak aşağıdaki gibi etkinleştirin:

    ```unix
    sudo zypper update
    sudo zypper install unixODBC
    ```

3. Azure, yordamda anlatıldığı gibi ulaşabilmeleri VM'den internet'e bağlanmaya izin [aşağıda](#set-up-network-connectivity).

4. Ön kayıt betiği, HANA kök kullanıcı olarak, yüklü olduğu sanal makinede çalıştırın. Betik [portalında](#discover-the-databases) akış ve ayarlamak için gerekli olan [sağ izinleri](backup-azure-sap-hana-database-troubleshoot.md#setting-up-permissions).

### <a name="set-up-network-connectivity"></a>Ağ bağlantısı ayarlama

Tüm işlemler için SAP HANA VM'nin Azure genel IP adreslerine bağlantısı gerekir. (Veritabanı keşfi, yedeklemeleri yapılandırma, yedeklemeler zamanlamak, Kurtarma noktalarını geri ve benzeri) VM operations bağlantısı olmadan çalışamaz. Azure veri merkezi IP aralıkları erişimine izin vererek, bağlantı kurar: 

- İndirebilirsiniz [IP adresi aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653) Azure veri merkezleri için ve ardından bu IP adreslerine erişim izni vermek.
- Ağ güvenlik grupları (Nsg'ler) kullanıyorsanız, AzureCloud kullanabileceğiniz [hizmet etiketi](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) izin tüm Azure genel IP adresleri. Kullanabileceğiniz [kümesi AzureNetworkSecurityRule cmdlet'i](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) NSG kurallarını değiştirmek için.

## <a name="onboard-to-the-public-preview"></a>Genel Önizleme ekleme

Genel Önizleme aşamasında aşağıdaki gibi ekleme:

- Portalda, abonelik Kimliğiniz için kurtarma Hizmetleri hizmet sağlayıcısı tarafından kaydetme [şu makaleyi](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal). 
- PowerShell için bu cmdlet'i çalıştırın. Bunu, "Kaydedildi" doldurmanız gerekir.

    ```powershell
    PS C:>  Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```



[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Veritabanlarını Bul

1. Bir kasadaki içinde **Başlarken**, tıklayın **yedekleme**. İçinde **iş yükünüz çalıştığı?** seçin **Azure VM'de SAP HANA**.
2. Tıklayın **Başlat bulma**. Bu kasa bölgesindeki korumasız sanal makineleri bulma başlatır.

   - Bulunduktan sonra listelenen adı ve kaynak grubu tarafından korunmayan VM'ler Portalı'nda görünür.
   - Bir VM beklendiği gibi listede yoksa, zaten bir kasada yedekleneceğini olup olmadığını denetleyin.
   - Birden çok VM aynı ada sahip olabilir, ancak farklı kaynak gruplarına ait oldukları.

3. İçinde **sanal makineleri Seç**, veritabanı bulma işlemi için SAP HANA Vm'leri erişmek Azure Backup hizmeti için izinleri sağlayan komut dosyasını indirmek için bağlantıya tıklayın
4. Yedeklemek istediğiniz SAP HANA veritabanlarını barındıran her VM'de betiği çalıştırın.
5. Komut içinde Vm'lerde çalıştırdıktan sonra **sanal makineleri Seç**, sanal makineleri seçin. Ardından **Bul DBs**.
6. Azure yedekleme, VM üzerindeki tüm SAP HANA veritabanlarını bulur. Azure Backup, bulma sırasında VM'yi kasa ile kaydeder ve sanal makinede bir uzantı yükler. Aracı veritabanı üzerinde yüklü.

    ![SAP HANA veritabanlarını bulmak](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

Şimdi yedeklemeyi etkinleştir.

1. Adım 2'de tıklayın **yedeklemeyi Yapılandır**.
2. İçinde **yedeklenecek öğeleri seçin**, korumak istediğiniz tüm veritabanlarını seçin > **Tamam**.
3. İçinde **yedekleme İlkesi** > **yedekleme ilkesi seçmek**, aşağıdaki yönergelere uygun olarak veritabanları için yeni bir yedekleme ilkesi oluşturun.
4. Şirket ilkesi oluşturduktan sonra **yedekleme** menüsünde tıklatın **yedeklemeyi etkinleştir**.
5. Yedekleme yapılandırması ilerlemeyi **bildirimleri** portalının alan.

### <a name="create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturma

Bir yedekleme İlkesi yedeklemeleri ne zaman alınacağının ve ne kadar süreyle tutulur tanımlar.

- Kasa düzeyinde bir ilke oluşturulur.
- Birden çok kasa ve aynı yedekleme ilkesine kullanabilirsiniz, ancak her kasa için yedekleme ilkesini uygulama.

İlke ayarları aşağıdaki gibi belirtin:

1. İçinde **ilke adı**, yeni ilke için bir ad girin.
2. İçinde **tam yedekleme İlkesi**seçin bir **yedekleme sıklığı**, seçin **günlük** veya **haftalık**.
   - **Günlük**: Saat dilimi yedekleme işi başlar ve saat seçin.
   
       - Tam yedekleme çalıştırmanız gerekir. Bu seçeneği devre dışı olamaz.
       - Tıklayın **tam yedekleme** ilkesini görüntülemek için.
       - Günlük tam yedekleme için fark yedeklemelerinin oluşturulamıyor.
       
   - **Haftalık**: Haftanın günü, saat ve saat dilimi yedekleme işi çalıştığı günü seçin.
3. İçinde **bekletme aralığı**, tam yedekleme için koruma ayarlarını yapılandırın.
    - Tüm seçenekleri varsayılan olarak seçilidir. Kullanarak ve bunu o istemediğiniz herhangi bir bekletme aralığı sınırları temizleyin.
    - Yedekleme (tam/değişiklik/günlüğü) herhangi bir türü için en düşük bekletme süresi yedi gündür.
    - Kurtarma noktalarının bekletme, bekletme aralığına göre etiketlenir. Örneğin, bir günlük tam yedekleme öğesini seçerseniz, yalnızca bir tam yedekleme her gün tetiklenir.
    - Yedekleme haftalık bekletme aralığı ve ayarı bağlı olarak belirli bir günde etiketlenmiş ve korunur.
    - Aylık ve yıllık bekletme aralıkları benzer şekilde davranır.

4. İçinde **tam yedekleme İlkesi** menüsünde tıklatın **Tamam** ayarları kabul etmek için.
5. Seçin **değişiklik yedeği** fark ilkesi eklemek için.
6. İçinde **fark yedekleme İlkesi**seçin **etkinleştirme** sıklığı ve bekletme denetimleri açın.
    - En fazla günde bir fark yedekleme tetikleyebilirsiniz.
    - Değişiklik yedekleri, en fazla 180 gün için saklanabilir. Daha uzun bekletme süresi gerekiyorsa, tam yedeklemeler kullanmanız gerekir.

    > [!NOTE]
    > Artımlı yedeklemeler şu anda desteklenmemektedir. 

7. Tıklayın **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.
8. Seçin **günlük yedekleme** bir işlem günlüğü yedekleme ilkesi eklemek için
    - İçinde **günlük yedeği**seçin **etkinleştirme**.
    - Sıklığı ve bekletme denetimleri ayarlayın.

    > [!NOTE]
    > Yalnızca günlük yedeklemeler başarılı bir tam yedekleme tamamlandıktan sonra akışı başlar.

9. Tıklayın **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.
10. Yedekleme ilkesi tanımlama tamamladıktan sonra tıklayın **Tamam**.


## <a name="run-an-on-demand-backup"></a>Bir talep üzerine yedekleme gerçekleştirin

Yedekleme İlkesi zamanlamaya uygun olarak çalıştırın. Bir talep üzerine yedekleme gibi çalıştırabilirsiniz:


1. Kasa menüden **yedekleme öğeleri**.
2. İçinde **yedekleme öğeleri**SAP HANA veritabanı çalıştıran sanal Makineyi seçin ve ardından **Şimdi Yedekle**.
3. İçinde **Şimdi Yedekle**, kurtarma noktası korunması gereken son günü seçmek için takvim denetimini kullanın. Daha sonra, **Tamam**'a tıklayın.
4. Portal bildirimlerini izleyin. Kasa panosunda iş ilerleme durumunu izleyebilirsiniz > **yedekleme işleri** > sürüyor. Veritabanınızın boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Bir veritabanı üzerinde SAP HANA Studio yedekleme ile Azure etkin yedekleme Çalıştır

(HANA Studio kullanarak) yerel yedeklenen bir veritabanını Azure Backup ile yedekleyin istiyorsanız, aşağıdakileri yapın:

1. Herhangi bir tam tamamlanmasını bekleyin veya son veritabanı için günlük yedeklemeler. SAP HANA Studio durumunu kontrol edin.
2. Günlük yedeklemeleri devre dışı bırakın ve ilgili veritabanı için dosya sistemini yedekleme kataloğunu ayarlayın.
3. Bunu yapmak için çift **systemdb** > **yapılandırma** > **Veritabanı Seç** > **filtre (günlük)** .
4. Ayarlama **enable_auto_log_backup** için **Hayır**.
5. Ayarlama **log_backup_using_backint** için **False**.
6. Geçici bir tam veritabanı yedekleme yararlanın.
7. Bitirmek için katalog yedekleme ve tam yedekleme için bekleyin.
8. Önceki ayarlara geri Azure için geri döndür:
    - Ayarlama **enable_auto_log_backup** için **Evet**.
    - Ayarlama **log_backup_using_backint** için **True**.



## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-sap-hana-database-troubleshoot.md) nasıl Azure Vm'lerinde SAP HANA yedekleme kullanırken sık karşılaşılan hataları giderme.
[Hakkında bilgi edinin](backup-azure-arm-vms-prepare.md) Azure VM'lerin yedeklenmesi.
