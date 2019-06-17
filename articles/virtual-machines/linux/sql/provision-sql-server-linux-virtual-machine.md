---
title: Azure'da Linux SQL Server 2017 VM oluşturma | Microsoft Docs
description: Bu öğreticide Azure portalında Linux SQL Server 2017 sanal makinesi oluşturma adımları gösterilmiştir.
services: virtual-machines-linux
author: MashaMSFT
manager: craigg
ms.date: 12/5/2018
ms.topic: conceptual
tags: azure-service-management
ms.devlang: na
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 468db9d62a98e079fbe6954843e23a518eaabd0a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60478607"
---
# <a name="provision-a-linux-sql-server-virtual-machine-in-the-azure-portal"></a>Azure portalında bir Linux SQL Server sanal makinesi sağlama

> [!div class="op_single_selector"]
> * [Linux](provision-sql-server-linux-virtual-machine.md)
> * [Windows](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)

Bu hızlı başlangıç öğreticisinde SQL Server 2017 yüklü bir Linux sanal makine oluşturmak için Azure portalını kullanın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* [Galeriden bir Linux SQL VM oluşturma](#create)
* [ssh ile yeni VM'ye bağlanma](#connect)
* [SA parolasını değiştirme](#password)
* [Uzak bağlantılar için yapılandırma gerçekleştirme](#remote)

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a id="create"></a> SQL Server yüklü bir Linux VM oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**.

1. İçinde **kaynak Oluştur** bölmesinde **işlem**.

1. Seçin **tümünü gör** yanındaki **öne çıkan** başlığı.

   ![Tüm VM görüntülerini inceleme](./media/provision-sql-server-linux-virtual-machine/azure-compute-blade.png)

1. Arama kutusuna **SQL Server 2017**seçip **Enter** aramayı başlatmak için.

1. Arama sonuçlarını seçerek sınırlandırmak **işletim sistemi** > **Redhat**. Ardından, altında **yayımcı**, seçin **Microsoft**.

    ![SQL Server 2017 VM görüntüleri için arama filtresi](./media/provision-sql-server-linux-virtual-machine/searchfilter.png)

1. Arama sonuçlarındaki bir SQL Server 2017 Linux görüntüsünü seçin. Bu öğreticide **ücretsiz SQL Server Lisansı: SQL Server 2017 Geliştirici Red Hat Enterprise Linux 7.4**.

   > [!TIP]
   > Developer sürümü, özellikleri, ancak SQL Server lisanslama maliyeti olmadan Enterprise edition ile geliştirme veya test olanak tanır. Yalnızca Linux VM çalıştırma maliyetleri için ödeme yaparsınız.

1. Altında **dağıtım modeli seçin**, iş yükü gereksinimlerinize uyan bir dağıtım modelini seçin.

    > [!Note]
    > Yeni iş yükleri için kullanın **Resource Manager**. Mevcut bir sanal ağa bağlanmak için iş yükünüz için sanal ağın dağıtım yöntemini seçin. Dağıtım modelleri hakkında daha fazla bilgi için bkz. [Azure Resource Manager ve klasik dağıtım modellerini](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

1. **Oluştur**’u seçin.

### <a name="set-up-your-linux-vm"></a>Linux VM'nizi ayarlayın

1. İçinde **Temelleri** sekmesinde, **abonelik** ve **kaynak grubu**. 

    ![Temel bilgiler penceresi](./media/provision-sql-server-linux-virtual-machine/basics.png)

1. İçinde **sanal makine adı**, yeni Linux VM'NİZDE için bir ad girin.
1. Ardından, yazın veya aşağıdaki değerleri seçin:
   * **Bölge**: Sizin için en uygun Azure bölgesi seçin.
   * **Kullanılabilirlik seçeneklerini**: Uygulama ve verileriniz için en iyi kullanılabilirlik ve yedekliliği seçeneği seçin.
   * **Değiştirme boyutu**: İşiniz bittiğinde, bir makine boyutu seçin ve bu seçeneği **seçin**. VM boyutları hakkında daha fazla bilgi için bkz. [Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes).

     ![VM boyutu seçme](./media/provision-sql-server-linux-virtual-machine/vmsizes.png)

   > [!TIP]
   > Geliştirme ve işlevsel test için bir VM boyutu kullanın **DS2** veya üzeri. Performans testi için **DS13** veya üzeri kullanın.

   * **Kimlik doğrulama türü**: Seçin **SSH ortak anahtarı**.

     > [!Note]
     > Kimlik doğrulaması için SSH ortak anahtarı veya Parola kullanabilirsiniz. SSH daha güvenlidir. SSH anahtarı oluşturma talimatları için bkz. [Azure'daki Linux VM için Linux ve Mac üzerinde SSH anahtarı oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys).

   * **Kullanıcı adı**: VM için yönetici adı girin.
   * **SSH ortak anahtarı**: RSA ortak anahtarınızı girin.
   * **Ortak gelen bağlantı noktası**: Seçin **Seçili bağlantı noktalarına izin** ve çekme **SSH (22)** içinde bağlantı noktası **seçin ortak gelen bağlantı noktası** listesi. Bu hızlı başlangıçta, bağlanmak ve SQL Server yapılandırmasını tamamlamak Bu adım gereklidir. SQL Server’a uzaktan bağlanmak istiyorsanız, İnternet üzerinden bağlantılar için **MS SQL (1433)** seçeneğini de belirleyerek 1433 numaralı bağlantı noktasını açın.

   ![Gelen bağlantı noktaları](./media/provision-sql-server-linux-virtual-machine/port-settings.png)

1. Aşağıdaki ek Sekmelerdeki ayarlar istediğinizden veya varsayılan ayarları tutun değişiklikleri yapın.
    * **Diskler**
    * **Ağ**
    * **Yönetim**
    * **Konuk yapılandırma**
    * **Etiketler**

1. **İncele ve oluştur**’u seçin.
1. İçinde **gözden geçir + Oluştur** bölmesinde **Oluştur**.

## <a id="connect"></a> Linux VM'ye bağlanma

BASH kabuğu kullanıyorsanız **ssh** komutuyla Azure VM'ye bağlanabilirsiniz. Aşağıdaki komutta yer alan VM kullanıcı adı ve IP adresini Linux VM bilgileriyle değiştirin.

```bash
ssh azureadmin@40.55.55.555
```

VM'nizin IP adresini Azure portalında bulabilirsiniz.

![Azure portalında IP adresi](./media/provision-sql-server-linux-virtual-machine/vmproperties.png)

Windows üzerinde çalıştırıyorsanız ve bir BASH kabuğunda yoksa PuTTY gibi bir SSH İstemcisi'ni yükleyin.

1. [PuTTY'yi indirin ve yükleyin](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. PuTTY'yi çalıştırın.

1. PuTTY yapılandırma ekranına VM'nizin genel IP adresini girin.

1. Seçin **açık** ve kullanıcı adınızı ve parolanızı girin.

Linux VM'lerinize bağlanma hakkında daha fazla bilgi için bkz. [Portal kullanarak Azure’da bir Linux VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-portal).

> [!Note]
> Kayıt defterinde önbelleğe alınmamış sunucunun ana makine anahtarı hakkında bir PuTTY güvenlik uyarısı görürseniz, aşağıdaki seçeneklerden birini seçin. Bu konak güveniyorsanız seçin **Evet** anahtar PuTTy'nın önbelleğine eklemek ve bağlanmaya devam etmek için. Yalnızca bir kez bağlanmaya ilişkin gerçekleştirmek istiyorsanız, eklemeden önbellek anahtarı seçin **Hayır**. Bu konak güvenmiyorsanız seçin **iptal** bağlantı bırakmaya karar.

## <a id="password"></a> SA parolasını değiştirme

Yeni sanal makine SQL Server'ı rastgele bir SA parolasıyla yükler. SQL Server'a SA kimlik bilgileriyle bağlanmadan önce bu parolayı sıfırlayın.

1. Linux VM'nize bağlandıktan sonra yeni bir komut terminali açın.

1. Aşağıdaki komutları kullanarak SA parolasını değiştirin:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```

   Sorulduğunda yeni bir SA parolası girin ve tekrar girerek onaylayın.

1. SQL Server hizmetini yeniden başlatın.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="add-the-tools-to-your-path-optional"></a>Yolunuza araçları ekleme (isteğe bağlı)

Varsayılan olarak SQL Server komut satırı araçları paketi dahil olmak üzere birkaç SQL Server [paketi](sql-server-linux-virtual-machines-overview.md#packages) yüklenmiştir. Araçlar paketi **sqlcmd** ve **bcp** araçlarını içerir. Kolaylık sağlaması amacıyla araçlar yolu olan `/opt/mssql-tools/bin/` girişini **PATH** ortam değişkeninize ekleyebilirsiniz.

1. **PATH** ortam değişkenini hem oturum açma bilgileriyle başlatılan oturumları hem de etkileşimli/oturum açma bilgisi olmadan başlatılan oturumları için değiştirmek üzere aşağıdaki komutları çalıştırın:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a id="remote"></a> Uzak bağlantılar için yapılandırma gerçekleştirme

Azure VM üzerindeki SQL Server'a uzaktan bağlanmanız gerekirse ağ güvenlik grubu üzerinde bir gelen kuralı yapılandırmanız gerekir. Kural SQL Server'ın dinlediği bağlantı noktasından (varsayılan olarak 1433) gelen trafiğe izin verir. Aşağıdaki adımlar, bu işlemi Azure portalından nasıl yapacağınızı göstermektedir.

> [!TIP]
> Sağlama sırasında ayarlarda **MS SQL (1433)** gelen bağlantı noktasını seçtiyseniz, bu değişiklikler sizin için yapılmış olur. Güvenlik duvarını yapılandırmayla ilgili sonraki bölme geçebilirsiniz.

1. Portalda **Sanal makineler**'i ve ardından SQL Server VM'nizi seçin.
1. Sol gezinti bölmesindeki altında **ayarları**seçin **ağ**.
1. Ağ penceresinde **gelen bağlantı noktası Ekle** altında **gelen bağlantı noktası kuralları**.

   ![Gelen bağlantı noktası kuralları](./media/provision-sql-server-linux-virtual-machine/networking.png)

1. **Hizmet** listesinde **MS SQL** girişini seçin.

    ![MS SQL güvenlik grubu kuralı](./media/provision-sql-server-linux-virtual-machine/sqlnsgrule.png)

1. VM kuralını kaydetmek için **Tamam**'a tıklayın.

### <a name="open-the-firewall-on-rhel"></a>RHEL güvenlik duvarını açma

Bu öğreticide nasıl Red Hat Enterprise Linux (RHEL) VM oluşturacağınız gösterilmiştir. RHEL VM'lerine uzaktan bağlanmak isterseniz Linux güvenlik duvarı üzerindeki 1433 numaralı bağlantı noktasını da açmanız gerekir.

1. RHEL VM'nize [bağlanın](#connect).

1. BASH kabuğunda aşağıdaki komutları çalıştırın:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure'da bir SQL Server 2017 sanal makinesi oluşturdunuz. Artık **sqlcmd** ile yerel olarak bağlanıp Transact-SQL sorguları çalıştırabilirsiniz.

Azure VM uzak SQL Server bağlantıları için yapılandırılmışsa, uzak bağlantı olmalıdır. Windows'dan Linux üzerindeki SQL Server'a uzaktan bağlantı kurma örneği için bkz. [Linux üzerindeki SQL Server'a bağlanmak için Windows üzerinde SSMS kullanma](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-ssms). Visual Studio Code bağlantısı kurmak için bkz. [SQL Server için Transact-SQL betikleri oluşturma ve çalıştırma amacıyla Visual Studio Code'u kullanma](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode)

Linux üzerinde SQL Server hakkında daha fazla genel bilgi için bkz. [genel bakış, SQL Server 2017 Linux'ta](https://docs.microsoft.com/sql/linux/sql-server-linux-overview). SQL Server 2017 Linux sanal makinelerini kullanma hakkında daha fazla bilgi için bkz. [Azure'daki SQL Server 2017 sanal makinelerine genel bakış](sql-server-linux-virtual-machines-overview.md).
