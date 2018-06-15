---
title: Windows Azure VM MongoDB yükleyin. | Microsoft Docs
description: Resource Manager dağıtım modeli kullanılarak oluşturulmuş Windows Server 2012 R2 çalıştıran bir Azure VM MongoDB yüklemeyi öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: f3fe9751467a1fc34f4e9d02855c4aff307424a3
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
ms.locfileid: "26745988"
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Yükleme ve Azure Windows VM üzerinde MongoDB yapılandırma
[MongoDB](http://www.mongodb.org) bir popüler açık kaynak, yüksek performanslı NoSQL veritabanıdır. Bu makalede yükleme ve bir Windows Server 2016 sanal makinede (VM) Azure MongoDB yapılandırma sırasında size kılavuzluk eder. Ayrıca [MongoDB azure'da bir Linux VM yüklemek](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Ön koşullar
Yükleyip MongoDB yapılandırmadan önce bir VM oluşturun ve ideal olarak, bir veri diski ekleyin, gerekir. Bir VM oluşturun ve bir veri diski eklemek için aşağıdaki makalelere bakın:

* Kullanarak bir Windows Server VM oluşturma [Azure portalı](quick-create-portal.md) veya [Azure PowerShell](quick-create-powershell.md).
* Bir veri diski kullanarak bir Windows Server VM ekleme [Azure portalı](attach-managed-disk-portal.md) veya [Azure PowerShell](attach-disk-ps.md).

Yükleme ve yapılandırma MongoDB başlamaya [oturum açın, Windows Server VM](connect-logon.md) Uzak Masaüstü kullanarak.

## <a name="install-mongodb"></a>MongoDB'yi yükleme
> [!IMPORTANT]
> Kimlik doğrulaması ve IP adresi bağlama gibi MongoDB güvenlik özellikleri varsayılan olarak etkin değildir. Güvenlik özellikleri, MongoDB üretim ortamına dağıtmadan önce etkinleştirilmelidir. Daha fazla bilgi için bkz: [MongoDB güvenlik ve kimlik doğrulama](http://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. Uzak Masaüstü kullanarak VM'nizi bağlandıktan sonra Internet Explorer görev çubuğundan açın.
2. Seçin **kullanım önerilen güvenlik, gizlilik ve uyumluluk ayarları** zaman Internet Explorer ilk açar ve tıklatın **Tamam**.
3. Internet Explorer Artırılmış Güvenlik Yapılandırması, varsayılan olarak etkindir. MongoDB Web sitesi, izin verilen siteler listesine ekleyin:
   
   * Seçin **Araçları** sağ üst köşesinde simgesi.
   * İçinde **Internet Seçenekleri**seçin **güvenlik** sekmesini tıklatın ve ardından **Güvenilen siteler** simgesi.
   * Tıklatın **siteleri** düğmesi. Ekleme *https://\*. mongodb.com* listesine Güvenilen siteler ve ardından iletişim kutusunu kapatın.
     
     ![Internet Explorer güvenlik ayarlarını yapılandırın](./media/install-mongodb/configure-internet-explorer-security.png)
4. Gözat [MongoDB - indirmeleri](http://www.mongodb.com/downloads) sayfa (http://www.mongodb.com/downloads).
5. Gerekirse, seçin **Community Server** edition ve ardından geçerli en yeni kararlı sürüm için*Windows Server 2008 R2 64-bit ve sonraki*. Yükleyici indirmek için tıklayın **yükleme (MSI)**.
   
    ![MongoDB yükleyici indirin](./media/install-mongodb/download-mongodb.png)
   
    İndirme tamamlandıktan sonra yükleyiciyi çalıştırın.
6. Okuyun ve Lisans Sözleşmesi'ni kabul edin. İstendiğinde, seçin **tam** yükleyin.
7. İsterseniz, ayrıca pusula, MongoDB için bir grafik arabirim yüklemek isteyebilirsiniz.
8. Son ekranında tıklatın **yükleme**.

## <a name="configure-the-vm-and-mongodb"></a>MongoDB ve VM yapılandırma
1. Yol değişkenleri MongoDB yükleyici tarafından güncelleştirilmez. MongoDB olmadan `bin` path değişkeni konumunda, tam yolu MongoDB yürütülebilir her kullanışınızda belirtmeniz gerekir. Path değişkenine konumu eklemek için:
   
   * Sağ **Başlat** menü ve select **sistem**.
   * Tıklatın **Gelişmiş Sistem ayarları**ve ardından **ortam değişkenleri**.
   * Altında **sistem değişkenleri**seçin **yolu**ve ardından **Düzenle**.
     
     ![Yol değişkenleri yapılandırın](./media/install-mongodb/configure-path-variables.png)
     
     Yolu, MongoDB eklemek `bin` klasör. MongoDB yüklü genellikle *C:\Program Files\MongoDB*. VM üzerinde yükleme yolunu doğrulayın. Aşağıdaki örnek, MongoDB yükleme konumu için varsayılan `PATH` değişkeni:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.6\bin
     ```
     
     > [!NOTE]
     > Başında noktalı virgül eklediğinizden emin olun (`;`) bir konuma eklediğiniz belirtmek için `PATH` değişkeni.

2. MongoDB veri ve günlük dizinleri veri diskinizde oluşturun. Gelen **Başlat** menüsünde, select **komut istemi**. Aşağıdaki örnekler dizinler F: sürücüde oluşturun.
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. Verilerinizin yolunu ayarlama aşağıdaki komutla bir MongoDB örneği başlatın ve dizinleri uygun şekilde oturum:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Günlük dosyaları ayırmak ve bağlantıları dinlemeyi başlatmak MongoDB için birkaç dakika sürebilir. Tüm günlük iletilerini yönlendirilir *F:\MongoLogs\mongolog.log* olarak dosya `mongod.exe` sunucu başlar ve günlük dosyalarını ayırır.
   
   > [!NOTE]
   > MongoDB örneğinizin çalışırken komut istemini bu görevde odaklanmış kalır. Komut İstemi penceresini MongoDB çalıştırmaya devam etmek için açık bırakın. Veya sonraki adımda ayrıntılı olarak hizmet olarak Mongodb'yi yükleyin.

4. Daha güçlü bir MongoDB deneyimi için yükleme `mongod.exe` bir hizmet olarak. Bir hizmet oluşturma MongoDB kullanmak istediğiniz her zaman çalışan bir komut istemi bırakın gerekmez anlamına gelir. Hizmet, veri ve günlük dizinlerinizi yolunu uygun şekilde ayarlama aşağıdaki gibi oluşturun:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install
    ```
   
    Yukarıdaki komut, MongoDB, "Mongo VT" açıklaması ile adlı bir hizmette oluşturur. Aşağıdaki parametreleri de belirtilir:
   
   * `--dbpath` Seçeneği veri dizininin konumunu belirtir.
   * `--logpath` Seçeneği bir günlük dosyası belirtmek için kullanılmalıdır çünkü çalışan hizmetin çıktı görüntülemek için bir komut penceresi yok.
   * `--logappend` Seçeneği, hizmet yeniden varolan günlük dosyasına eklenecek çıkışına neden belirtir.
   
   MongoDB hizmetini başlatmak için aşağıdaki komutu çalıştırın:
   
    ```
    net start MongoDB
    ```
   
    MongoDB hizmet oluşturma hakkında daha fazla bilgi için bkz: [MongoDB için bir Windows hizmeti yapılandırma](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>MongoDB örneği test
Tek bir örneği olarak çalışan veya bir hizmet olarak yüklü MongoDB ile oluşturma ve kullanma, veritabanlarınızı şimdi başlayabilirsiniz. MongoDB Yönetim Kabuğu'nu başlatmak için başka bir komut istemi penceresinden açın **Başlat** menüsünde ve aşağıdaki komutu girin:

```
mongo  
```

Veritabanlarıyla listeleyebilirsiniz `db` komutu. Bazı verileri aşağıdaki şekilde ekleyin:

```
db.foo.insert( { a : 1 } )
```

Verileri gibi arayabilirsiniz:

```
db.foo.find()
```

Çıktı aşağıdaki örneğe benzer:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Çıkış `mongo` gibi konsolu:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Güvenlik Duvarı ve ağ güvenlik grubu kurallarını yapılandırma
MongoDB yüklü ve çalışır durumda, MongoDB için uzaktan bağlanabilmesi için bir bağlantı noktası Windows Güvenlik Duvarı'nı açın. TCP bağlantı noktası 27017 izin vermek için yeni bir gelen kuralı oluşturmak için yönetici bir PowerShell komut istemini açın ve aşağıdaki komutu girin:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

Kullanarak kural oluşturabilirsiniz **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** grafik yönetim aracıdır. TCP bağlantı noktası 27017 izin vermek için yeni bir gelen kuralı oluşturun.

Gerekirse, varolan bir Azure sanal ağ alt ağı dışında adresinden erişime izin vermek için bir ağ güvenlik grubu kural oluşturun. Ağ güvenlik grubu kurallarını kullanarak oluşturabileceğiniz [Azure portal](nsg-quickstart-portal.md) veya [Azure PowerShell](nsg-quickstart-powershell.md). Windows Güvenlik duvarı kurallarıyla olduğu gibi MongoDB VM sanal ağ arabirimi için TCP bağlantı noktası 27017 izin verin.

> [!NOTE]
> TCP bağlantı noktası 27017 MongoDB tarafından kullanılan varsayılan bağlantı noktasıdır. Bu bağlantı noktasını kullanarak değiştirebilirsiniz `--port` başlatırken parametresi `mongod.exe` el ile veya bir hizmet. Bağlantı noktasını değiştirirseniz, önceki adımlarda Windows Güvenlik Duvarı ve ağ güvenlik grubu kuralları güncelleştirmek emin olun.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, MongoDB, Windows VM yükleyip öğrendiniz. Gelişmiş konular, izleyerek MongoDB, Windows VM artık erişebilir [MongoDB belgelerine](https://docs.mongodb.com/manual/).

