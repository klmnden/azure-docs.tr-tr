---
title: Bir Windows azure'da sanal makinesi üzerinde MongoDB yükleme | Microsoft Docs
description: Azure Resource Manager dağıtım modeli kullanılarak oluşturulmuş Windows Server 2012 R2 çalıştıran bir VM'de MongoDB yüklemeyi öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: afd8e6b47fb86985acde062af1fb38ec3af4e902
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60711445"
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Yükleme ve azure'da Windows sanal makinesi üzerinde MongoDB yapılandırma
[MongoDB](https://www.mongodb.org) popüler açık kaynaklı, yüksek performanslı NoSQL veritabanıdır. Bu makalede yükleme ve azure'da bir Windows Server 2016 sanal makine (VM) üzerinde MongoDB yapılandırma sırasında size kılavuzluk eder. Ayrıca [azure'da bir Linux sanal makinesi üzerinde MongoDB yükleme](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Önkoşullar
Yükleyip MongoDB yapılandırmadan önce bir VM oluşturun ve ideal olarak, bir veri diski eklemek için gerekir. VM oluşturma ve bir veri diski eklemek için aşağıdaki makalelere bakın:

* Kullanarak bir Windows Server VM oluşturma [Azure portalında](quick-create-portal.md) veya [Azure PowerShell](quick-create-powershell.md).
* Kullanarak bir Windows Server VM veri diski ekleme [Azure portalında](attach-managed-disk-portal.md) veya [Azure PowerShell](attach-disk-ps.md).

MongoDB yükleme ve yapılandırma, başlamak için [, Windows Server VM'de oturum](connect-logon.md) Uzak Masaüstü kullanarak.

## <a name="install-mongodb"></a>MongoDB'yi yükleme
> [!IMPORTANT]
> Kimlik doğrulaması ve IP adresi bağlama gibi MongoDB güvenlik özellikleri varsayılan olarak etkin değildir. Güvenlik özellikleri, MongoDB, bir üretim ortamına dağıtmadan önce etkinleştirilmesi gerekir. Daha fazla bilgi için [MongoDB güvenlik ve kimlik doğrulama](https://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. Uzak Masaüstü kullanarak VM'nize bağlandıktan sonra görev çubuğundan Internet Explorer açın.
2. Seçin **önerilen güvenlik, gizlilik ve uyumluluk ayarlarını kullan** zaman Internet Explorer ilk açılır ve tıklayın **Tamam**.
3. Internet Explorer Artırılmış Güvenlik Yapılandırması, varsayılan olarak etkindir. MongoDB Web sitesi, izin verilen siteler listesine ekleyin:
   
   * Seçin **Araçları** simgesine dokunun.
   * İçinde **Internet Seçenekleri**seçin **güvenlik** sekmesine tıklayın ve ardından **Güvenilen siteler** simgesi.
   * Tıklayın **siteleri** düğmesi. Ekleme *https://\*. mongodb.com* listesine Güvenilen siteler ve ardından iletişim kutusunu kapatın.
     
     ![Internet Explorer güvenlik ayarlarını yapılandırın](./media/install-mongodb/configure-internet-explorer-security.png)
4. Gözat [MongoDB - indirir](https://www.mongodb.com/downloads) sayfa (https://www.mongodb.com/downloads).
5. Gerekirse, seçin **Community Server** edition'ı ve ardından geçerli en son kararlı sürüm için*Windows Server 2008 R2 64 bit ve üzeri*. Yükleyiciyi indirmek için tıklayın **İNDİRME (MSI)** .
   
    ![MongoDB yükleyicisini indirin](./media/install-mongodb/download-mongodb.png)
   
    İndirme tamamlandıktan sonra yükleyiciyi çalıştırın.
6. Okuyun ve lisans sözleşmesini kabul edin. İstendiğinde, seçin **tam** yükleyin.
7. İsterseniz, Compass, MongoDB için bir grafik arabirim de yüklemeyi seçebilirsiniz.
8. Son ekrana tıklayın **yükleme**.

## <a name="configure-the-vm-and-mongodb"></a>MongoDB ve VM yapılandırma
1. Yol değişkenleri MongoDB yükleyicisi tarafından güncelleştirilir. MongoDB olmadan `bin` yol değişkeninize konumunda, tam yolu MongoDB yürütülebilir her kullanışınızda belirtmeniz gerekir. Konumun yol değişkeninize eklemek için:
   
   * Sağ **Başlat** seçin ve menü **sistem**.
   * Tıklayın **Gelişmiş Sistem ayarları**ve ardından **ortam değişkenlerini**.
   * Altında **sistem değişkenlerini**seçin **yolu**ve ardından **Düzenle**.
     
     ![Yol değişkenleri yapılandırın](./media/install-mongodb/configure-path-variables.png)
     
     Yolu, Mongodb'ye eklemek `bin` klasör. MongoDB yüklenmiş genellikle *C:\Program Files\MongoDB*. Vm'nizde yükleme yolunu doğrulayın. Aşağıdaki örnek, MongoDB yükleme konumuna varsayılan ekler `PATH` değişkeni:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.6\bin
     ```
     
     > [!NOTE]
     > Başında noktalı virgül eklediğinizden emin olun (`;`) bir yere eklediğiniz belirtmek için `PATH` değişkeni.

2. MongoDB veri ve günlük dizinleri, verileri disk üzerinde oluşturun. Gelen **Başlat** menüsünde **komut istemi**. Aşağıdaki örnekler, F: sürücüde dizinleri oluşturma
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. Verilerinizin yolunu değiştirerek aşağıdaki komutu, bir MongoDB örneğine başlayın ve dizinleri buna uygun olarak oturum açın:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Bu günlük dosyaları ayırmak ve bağlantıları dinlemeyi başlatmak MongoDB için birkaç dakika sürebilir. Tüm günlük iletilerini yönlendirilmiş *F:\MongoLogs\mongolog.log* Dosyala `mongod.exe` sunucusu başlatır ve günlük dosyaları ayırır.
   
   > [!NOTE]
   > MongoDB örneğinizin çalışırken komut isteminde bu görevde odaklanmış kalır. Komut İstemi penceresini MongoDB çalıştırmaya devam etmek için açık bırakın. Veya sonraki adımda açıklandığı bir hizmet olarak MongoDB yükleme.

4. Daha güçlü bir MongoDB deneyimi için yükleme `mongod.exe` hizmet olarak. Bir hizmet oluşturma, MongoDB kullanmak istediğiniz her saat çalışan bir komut istemi bırakın gerekmez anlamına gelir. Hizmet, veri ve günlük dizinlerinizi yolunu uygun şekilde ayarlama şu şekilde oluşturun:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install
    ```
   
    Yukarıdaki komut, "Mongo DB" açıklaması ile MongoDB, adlandırılmış bir hizmet oluşturur. Aşağıdaki parametreleri de belirtilir:
   
   * `--dbpath` Seçeneği veri dizininin konumunu belirtir.
   * `--logpath` Seçeneği kullanılmalıdır bir günlük dosyası belirtmek için bir komut penceresi çıkışını görüntülemek için çalışan hizmetin sahip olmadığından.
   * `--logappend` Seçeneği, hizmetin yeniden başlatılması mevcut günlük dosyasına eklenecek çıkış neden olduğunu belirtir.
   
   MongoDB hizmetini başlatmak için aşağıdaki komutu çalıştırın:
   
    ```
    net start MongoDB
    ```
   
    Hizmet MongoDB oluşturma hakkında daha fazla bilgi için bkz. [MongoDB için bir Windows hizmetini yapılandırma](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>MongoDB örneğine test
Tek bir örnek olarak çalışan veya yüklü bir hizmet olarak MongoDB ile oluşturma ve veritabanlarınızı kullanma şimdi başlayabilirsiniz. MongoDB Yönetim Kabuğu'nu başlatmak için başka bir komut istemi penceresinden açın **Başlat** menüsünde ve aşağıdaki komutu girin:

```
mongo  
```

Veritabanlarıyla listeleyebilirsiniz `db` komutu. Bazı verileri aşağıdaki şekilde ekleyin:

```
db.foo.insert( { a : 1 } )
```

Şu şekilde verileri için arama:

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
MongoDB yüklü ve çalışır durumda, MongoDB için uzaktan bağlanabilmesi için bir bağlantı noktası Windows Güvenlik Duvarı'nı açın. TCP bağlantı noktası 27017 izin vermek için yeni bir gelen kuralı oluşturmak için bir yönetici bir PowerShell istemi açın ve aşağıdaki komutu girin:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

Kullanarak kural oluşturabilirsiniz **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** grafik yönetim aracıdır. TCP bağlantı noktası 27017 izin vermek için yeni bir gelen kuralı oluşturun.

Gerekirse, mevcut Azure sanal ağ alt ağı dışında adresinden erişime izin vermek için bir ağ güvenlik grubu kuralı oluşturun. Ağ güvenlik grubu kurallarını kullanarak oluşturabileceğiniz [Azure portalında](nsg-quickstart-portal.md) veya [Azure PowerShell](nsg-quickstart-powershell.md). Windows Güvenlik duvarı kurallarıyla olduğu gibi TCP bağlantı noktası 27017 MongoDB sanal makinenizin sanal ağ arabirimine izin verin.

> [!NOTE]
> TCP bağlantı noktası 27017 MongoDB tarafından kullanılan varsayılan bağlantı noktasıdır. Bu bağlantı noktasını kullanarak değiştirebileceğiniz `--port` başlatma sırasında parametre `mongod.exe` el ile veya bir hizmet. Bağlantı noktasını değiştirirseniz, önceki adımda Windows Güvenlik Duvarı ve ağ güvenlik grubu kuralları güncelleştirdiğinizden emin olun.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, yükleme ve Windows sanal makinenizde MongoDB yapılandırma öğrendiniz. Gelişmiş konular, izleyerek MongoDB Windows sanal makinenizde, artık erişebilirsiniz [MongoDB belgelerine](https://docs.mongodb.com/manual/).

