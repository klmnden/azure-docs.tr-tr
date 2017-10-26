---
title: "Azure'da Linux SQL Server 2017 VM oluşturma | Microsoft Docs"
description: "Bu öğreticide Azure portalında Linux SQL Server 2017 sanal makinesi oluşturma adımları gösterilmiştir."
services: virtual-machines-linux
author: rothja
ms.author: jroth
manager: jhubbard
ms.date: 10/25/2017
ms.topic: hero-article
tags: azure-service-management
ms.devlang: na
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.technology: database-engine
ms.openlocfilehash: 54cb1e11e82c7a9be82252a0b93277353c5005e2
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="provision-a-linux-sql-server-virtual-machine-in-the-azure-portal"></a>Azure portalında bir Linux SQL Server sanal makinesi sağlama

> [!div class="op_single_selector"]
> * [Linux](provision-sql-server-linux-virtual-machine.md)
> * [Windows](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)

Bu hızlı başlangıç öğreticisinde Azure portalını kullanarak SQL Server 2017 yüklü bir Linux sanal makinesi oluşturacaksınız.

Bu öğreticide şunları yapacaksınız:

* [Galeriden bir Linux SQL VM oluşturma](#create)
* [ssh ile yeni VM'ye bağlanma](#connect)
* [SA parolasını değiştirme](#password)
* [Uzak bağlantılar için yapılandırma gerçekleştirme](#remote)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a id="create"></a> SQL Server yüklü bir Linux VM oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Sol bölmede **Yeni**'ye tıklayın.

1. **Yeni** bölmesinde **İşlem**'e tıklayın.

1. **Öne Çıkanlar** başlığının yanındaki **Tümünü gör**'e tıklayın.

   ![Tüm VM görüntülerini inceleme](./media/provision-sql-server-linux-virtual-machine/azure-compute-blade.png)

1. Arama kutusuna **SQL Server 2017** yazın ve **Enter** tuşuna basarak aramayı başlatın.

1. **Filtre** simgesine tıklayın, aramayı **Linux tabanlı**, **Microsoft** görüntüleri olarak sınırlayın ve **Bitti**'ye tıklayın.

    ![SQL Server 2017 VM görüntüleri için arama filtresi](./media/provision-sql-server-linux-virtual-machine/searchfilter.png)

1. Arama sonuçlarındaki bir SQL Server 2017 Linux görüntüsünü seçin. Bu öğreticide **Ücretsiz SQL Server Lisansı: Red Hat Enterprise Linux 7.4 üzerinde SQL Server 2017 Developer** kullanılmaktadır.

   > [!TIP]
   > Developer sürümü SQL Server lisanslama maliyeti olmadan Enterprise sürümü özellikleriyle test veya geliştirme yapmanızı sağlar. Yalnızca Linux VM çalıştırma maliyetleri için ödeme yaparsınız.

1. **Oluştur**'a tıklayın.

1. **Temel Bilgiler** penceresine Linux VM bilgilerini girin. 

    ![Temel bilgiler penceresi](./media/provision-sql-server-linux-virtual-machine/basics.png)

    > [!Note]
    > Kimlik doğrulaması için SSH ortak anahtarı veya Parola kullanabilirsiniz. SSH daha güvenlidir. SSH anahtarı oluşturma talimatları için bkz. [Azure'daki Linux VM için Linux ve Mac üzerinde SSH anahtarı oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys).

1. **Tamam** düğmesine tıklayın.

1. **Boyut** penceresinden bir makine boyutu seçin. Diğer boyutları görmek için **Tümünü görüntüle**'yi seçin. VM boyutları hakkında daha fazla bilgi için bkz. [Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes).

    ![VM boyutu seçme](./media/provision-sql-server-linux-virtual-machine/vmsizes.png)

   > [!TIP]
   > Geliştirme ve işlevsel test için **DS2** veya üzeri bir VM boyutu kullanmanızı öneririz. Performans testi için **DS13** veya üzeri kullanın.

1. **Seç**'e tıklayın.

1. **Ayarlar** penceresindeki ayarları değiştirebilir veya varsayılan ayarları tutabilirsiniz.

1. **Tamam** düğmesine tıklayın.

1. **Özet** sayfasında **Satın al**'a tıklayarak VM'yi oluşturun.

## <a id="connect"></a> Linux VM'ye bağlanma

BASH kabuğu kullanıyorsanız **ssh** komutuyla Azure VM'ye bağlanabilirsiniz. Aşağıdaki komutta yer alan VM kullanıcı adı ve IP adresini Linux VM bilgileriyle değiştirin.

```bash
ssh azureadmin@40.55.55.555
```

VM'nizin IP adresini Azure portalında bulabilirsiniz.

![Azure portalında IP adresi](./media/provision-sql-server-linux-virtual-machine/vmproperties.png)

Windows üzerinde çalışıyorsanız ve BASH kabuğunuz yoksa PuTTY gibi bir SSH istemcisi yükleyebilirsiniz.

1. [PuTTY'yi indirin ve yükleyin](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. PuTTY'yi çalıştırın.

1. PuTTY yapılandırma ekranına VM'nizin genel IP adresini girin.

1. Aç'a tıklayın ve sorulduğunda kullanıcı adınızla parolanızı girin.

Linux VM'lerinize bağlanma hakkında daha fazla bilgi için bkz. [Portal kullanarak Azure’da bir Linux VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-portal#ssh-to-the-vm).

## <a id="password"></a> SA parolasını değiştirme

Yeni sanal makine SQL Server'ı rastgele bir SA parolasıyla yükler. SQL Server'a SA kimlik bilgileriyle bağlanabilmek için bu parolayı sıfırlamanız gerekir.

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

1. Portalda **Sanal makineler**'i ve ardından SQL Server VM'nizi seçin.

1. Özellik listesinde **Ağ İletişimi**'ni seçin.

1. **Ağ İletişimi** penceresinde, **Gelen Bağlantı Noktası Kuralları** altındaki **Ekle** düğmesine tıklayın.

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
   sudo firewall-cmd –-reload
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure'da bir SQL Server 2017 sanal makinesi oluşturdunuz. Artık **sqlcmd** ile yerel olarak bağlanıp Transact-SQL sorguları çalıştırabilirsiniz.

Azure VM'sini SQL Server'a uzaktan bağlantı kuracak şekilde yapılandırdıysanız uzaktan bağlantı kurabilirsiniz. Windows'dan Linux üzerindeki SQL Server'a uzaktan bağlantı kurma örneği için bkz. [Linux üzerindeki SQL Server'a bağlanmak için Windows üzerinde SSMS kullanma](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-ssms). Visual Studio Code bağlantısı kurmak için bkz. [SQL Server için Transact-SQL betikleri oluşturma ve çalıştırma amacıyla Visual Studio Code'u kullanma](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode)

Linux üzerinde SQL Server hakkında daha fazla genel bilgi için bkz. [Linux üzerinde SQL Server 2017'ye genel bakış](https://docs.microsoft.com/sql/linux/sql-server-linux-overview). SQL Server 2017 Linux sanal makinelerini kullanma hakkında daha fazla bilgi için bkz. [Azure'daki SQL Server 2017 sanal makinelerine genel bakış](sql-server-linux-virtual-machines-overview.md).
