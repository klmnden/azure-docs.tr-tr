---
title: "MySQL çalıştıran klasik bir Azure VM oluşturma | Microsoft Docs"
description: "Windows Server 2012 R2 ve klasik dağıtım modeli kullanılarak MySQL veritabanı çalıştıran bir Azure sanal makine oluşturun."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 98fa06d2-9b92-4d05-ac16-3f8e9fd4feaa
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: cynthn
ms.openlocfilehash: 11850e5ce20efae88a7af9c1d2e4761ed2b70cd7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2016"></a>Windows Server 2016 çalıştıran Klasik dağıtım modeli kullanılarak oluşturulmuş bir sanal makinede MySQL yükleme
[MySQL](https://www.mysql.com) bir popüler açık kaynak, SQL veritabanı. Bu öğreticide, yüklemek ve çalıştırmak nasıl gösterilir **5.7.18 MySQL community sürümü** MySQL sunucusu çalıştıran bir sanal makine olarak **Windows Server 2016**. Deneyiminizi MySQL veya Windows Server'ın diğer sürümleri için biraz farklı olabilir.

Linux'ta MySQL yükleme ile ilgili yönergeler için bkz: [Azure üzerinde MySQL yükleme](../../linux/mysql-install.md).

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## <a name="create-a-virtual-machine-running-windows-server-2016"></a>Windows Server 2016 çalıştıran bir sanal makine oluşturma
Windows Server 2016 çalıştıran bir VM zaten sahip değilseniz, bu kullanabilirsiniz [öğretici](./tutorial.md) sanal makine oluşturulamıyor.

## <a name="attach-a-data-disk"></a>Veri diski ekleme
Sanal makine oluşturulduktan sonra isteğe bağlı olarak bir veri diski ekleyebilirsiniz. Bir veri diski işletim sistemi sürücüsünde (C:) alanı azalmasını önlemek için ve üretim iş yükleri için önerilir ekleme, işletim sistemi içerir.

Bkz: [bir Windows sanal makineye bir veri diski Ekle nasıl](../attach-managed-disk-portal.md) ve boş bir disk eklemek için yönergeleri izleyin. Konak önbellek ayarını **hiçbiri** veya **salt okunur**.

## <a name="log-on-to-the-virtual-machine"></a>Sanal makinede oturum açma
Ardından, artıracaksınız [sanal makinede oturum açın](./connect-logon.md) MySQL yükleyebilmek için.

## <a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Yükleme ve sanal makinede MySQL Community Server çalıştırma
MySQL Server topluluk sürümünü çalıştıran yüklemek ve yapılandırmak için aşağıdaki adımları izleyin:

> [!NOTE]
> Internet Explorer kullanarak öğeleri indirirken IE ayarlayabilirsiniz **Artırılmış Güvenlik Yapılandırması** için kapalı ve yükleme işlemini basitleştirir. Başlat menüsünden Yönetimsel Araçlar/Sunucu Yöneticisi'ni / yerel sunucuya tıklayın ve ardından IE **Artırılmış Güvenlik Yapılandırması** ve yapılandırmayı kapalı olarak ayarlayın).
>
>

1. Uzak Masaüstü kullanarak sanal makineye bağlandıktan sonra tıklatın **Internet Explorer** başlangıç ekranından.
2. Seçin **Araçları** sağ üst köşesinde (cogged tekerleği simgesi) düğmesine tıklayın ve ardından **Internet Seçenekleri**. Tıklatın **güvenlik** sekmesini tıklatın, **Güvenilen siteler** simgesine ve ardından **siteleri** düğmesi. Http://*.MySQL.com Güvenilen siteler listesine ekleyin. Tıklatın **Kapat**ve ardından **Tamam**.
3. Adres çubuğu, Internet Explorer'ın, https://dev.mysql.com/downloads/mysql/ yazın.
4. Bulun ve Windows için MySQL yükleyicisi en son sürümünü indirmek için MySQL sitesini kullanın. MySQL yükleyici seçerken, tam kümesi (örneğin, mysql-yükleyici-topluluk-5.7.18.0.msi dosya boyutuna 352.8 MB) dosya ve yükleyici kaydedin sürümü yükleyin.
5. Yükleyici, indirme tamamlandığında tıklatın **çalıştırmak** Kurulum başlatmak için.
6. Üzerinde **Lisans Sözleşmesi'ni** sayfasında lisans anlaşmasını kabul etmeniz ve tıklayın **sonraki**.
7. Üzerinde **kurulum türünü seçme** sayfasında, tıklatın ve ardından Kurulum türünü **sonraki**. Aşağıdaki adımlarda seçimini varsayılmaktadır **yalnızca sunucu** kurulum türü.
8. Varsa **denetleyin gereksinimleri** sayfasında görüntüler, **yürütme** eksik bileşenleri yükleyin yükleyici izin vermek için. Görüntüleyen, C++ yeniden dağıtılabilir çalışma zamanı gibi yönergeleri izleyin.
9. Üzerinde **yükleme** sayfasında, **yürütme**. Yükleme tamamlandığında tıklatın **sonraki**.

10. Üzerinde **ürün yapılandırma** sayfasında, **sonraki**.

11. Üzerinde **türü ve ağ** sayfasında, gerekirse TCP bağlantı noktasını da dahil olmak üzere, istenen yapılandırma türü ve bağlantı seçenekleri belirtin. Seçin **Gelişmiş Seçenekleri Göster**ve ardından **sonraki**.
    ![](./media/mysql-2008r2/MySQL_TypeNetworking.png)

12. Üzerinde **hesapları ve rolleri** sayfasında, güçlü bir MySQL kök parola belirtin. Gerektiğinde ek MySQL kullanıcı hesaplarını ekleyin ve ardından **sonraki**.

    ![](./media/mysql-2008r2/MySQL_AccountsRoles_Filled.png)
13. Üzerinde **Windows hizmeti** sayfasında, MySQL sunucusu Windows hizmeti olarak gerektiği şekilde çalıştırmak için varsayılan ayarlarında yapılan değişiklikler belirtin ve ardından **sonraki**.

    ![](./media/mysql-2008r2/MySQL_WindowsService.png)
14. Seçimlerine **eklentileri ve uzantıları** sayfasında isteğe bağlı. Tıklatın **sonraki** devam etmek için.
15. Üzerinde **Gelişmiş Seçenekler** sayfasında, gerektiğinde değişiklikleri günlüğe kaydetme seçeneklerini belirtin ve ardından **sonraki**.

    ![](./media/mysql-2008r2/MySQL_AdvOptions.png)
16. Üzerinde **geçerli sunucu yapılandırmasını** sayfasında, **yürütme**. Yapılandırma adımları tamamlandığında tıklatın **son**.
17. Üzerinde **ürün yapılandırma** sayfasında, **sonraki**.
18. Üzerinde **yükleme tamamlandı** sayfasında, **Panoya Kopyala günlük** daha sonra inceleyin ve ardından istiyorsanız **son**.
19. Başlangıç ekranından yazın **mysql**ve ardından **MySQL 5.7 komut satırı istemci**.
20. Adım 12'de belirttiğiniz bir kök parola girin ve MySQL yapılandırmak için komutları burada verebileceği İstemi ile sunulur. Komutlar ve söz dizimi ayrıntıları için bkz: [MySQL başvuru kılavuzlarına](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html).

    ![](./media/mysql-2008r2/MySQL_CommandPrompt.png)
21. Temel ve veri dizinleri ve sürücüler gibi sunucu yapılandırmasını varsayılan ayarları da yapılandırabilirsiniz. Daha fazla bilgi için bkz: [6.1.2 yapılandırma sunucusu varsayılan olarak](https://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Uç noktaları yapılandırma

MySQL hizmeti Internet üzerindeki istemci bilgisayarlar için kullanılabilir olması TCP bağlantı noktası için bir uç nokta yapılandırmak ve bir Windows Güvenlik duvarı kuralı oluşturmanız gerekir. MySQL Server hizmeti MySQL istemcileri için dinlediği varsayılan bağlantı noktası 3306 değerdir. Bağlantı noktası üzerinde sağlanan değer ile tutarlı olduğu sürece, başka bir bağlantı noktası belirtebilirsiniz **türü ve ağ** sayfa (11. adım önceki yordamın).

> [!NOTE]
> Üretim kullanımı için MySQL Server hizmeti Internet üzerindeki tüm bilgisayarlar için kullanılabilir hale güvenlik etkilerini göz önünde bulundurun. Uç nokta bir erişim denetimi listesi (ACL) ile kullanmasına izin verilen kaynak IP adresleri kümesini tanımlayabilir. Daha fazla bilgi için bkz: [ayarlamak yukarı uç noktaları için bir sanal makine nasıl](setup-endpoints.md).
>
>

MySQL Server hizmeti için bir uç nokta yapılandırmak için:

1. Azure portalında tıklatın **sanal makineleri (Klasik)**, MySQL sanal makine adına tıklayın ve ardından **uç noktaları**.
2. Komut çubuğunda, **Ekle**.
3. Üzerinde **uç nokta ekleme** sayfasında, için benzersiz bir ad yazın **adı**.
4. Seçin **TCP** protokol olarak.
5. Bağlantı noktası numarasını yazın **3306**, hem de **genel bağlantı noktası** ve **özel bağlantı noktası**ve ardından **Tamam**.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>MySQL trafiğine izin vermek için bir Windows Güvenlik duvarı kuralı ekleyin
Internet'ten MySQL trafiğe izin veren bir Windows Güvenlik duvarı kuralı eklemek için aşağıdaki komutu çalıştırın bir _yükseltilmiş Windows PowerShell komut isteminde_ MySQL sunucusu sanal makinesinde.

    New-NetFirewallRule -DisplayName "MySQL57" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public

## <a name="test-your-remote-connection"></a>Uzak bağlantıyı sınayın
MySQL sunucusu hizmetini çalıştıran Azure VM'ye uzak bağlantınızı sınamak için VN içeren bulut hizmeti DNS adı sağlamanız gerekir.

1. Azure portalında tıklatın **sanal makineleri (Klasik)**, MySQL server sanal makine adına tıklayın ve ardından **genel bakış**.
2. Sanal makine panodan Not **DNS adı** değeri. Örnek aşağıda verilmiştir:

   ![](media/mysql-2008r2/MySQL_DNSName.png)
3. MySQL veya MySQL istemcisini çalıştıran bir yerel bilgisayardan MySQL kullanıcı olarak oturum açmak için aşağıdaki komutu çalıştırın.

     MySQL -u <yourMysqlUsername> - p -h<yourDNSname>

   Örneğin, MySQL kullanıcı adı kullanarak _dbadmin3_ ve _testmysql.cloudapp.net_ DNS adı sanal makine için aşağıdaki komutu kullanarak MySQL başlayabilirsiniz:

     MySQL -u dbadmin3 -p -h testmysql.cloudapp.net

## <a name="next-steps"></a>Sonraki adımlar
MySQL çalıştırma hakkında daha fazla bilgi için bkz: [MySQL belgeleri](http://dev.mysql.com/doc/).
