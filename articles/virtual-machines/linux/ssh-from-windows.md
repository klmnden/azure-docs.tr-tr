---
title: "Linux VM'ler için SSH anahtarları kullanma | Microsoft Docs"
description: "Oluştur ve Azure Linux sanal makineye bağlanmak için bir Windows bilgisayarda SSH anahtarları kullanma hakkında bilgi edinin."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: danlep
ms.openlocfilehash: 66837a3a153cda041f5351c52c8ccb1f8ccfea50
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Windows Azure üzerinde ile SSH kullanma anahtarları nasıl
> [!div class="op_single_selector"]
> * [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

Azure'daki Linux sanal makineleri (VM'ler) bağlandığınızda, kullanması gereken [ortak anahtar şifrelemesini](https://wikipedia.org/wiki/Public-key_cryptography) Linux VM'NİZDE oturum açmak için daha güvenli bir şekilde sağlamak için. Bu işlem kendiniz yerine bir kullanıcı adı ve parola kimlik doğrulaması için güvenli Kabuk (SSH) komutunu kullanarak ortak ve özel bir anahtar değişimi içerir. Parolalar yanılma saldırılarına, özellikle web sunucuları gibi İnternete VM'ler için etkilenir. Bu makalede SSH anahtarları ve bir Windows bilgisayarda uygun anahtarları oluşturmak nasıl genel bir bakış sağlar.

## <a name="overview-of-ssh-and-keys"></a>SSH ve anahtarları genel bakış
Güvenli bir şekilde, Linux VM'ye ortak ve özel anahtarları kullanarak oturum:

* **Ortak anahtar** Linux VM veya ortak anahtar şifrelemesi ile kullanmak istediğiniz başka bir hizmet yerleştirilir.
* **Özel anahtarı** kimliğinizi doğrulamak için oturum açtığınızda ne, Linux VM'NİZDE mevcut değil. Bu özel anahtarı koruyun. Özel anahtarı paylaşmayın.

Bu ortak ve özel anahtarlar, birden çok sanal makineleri ve hizmetleri üzerinde kullanılabilir. Her VM veya erişmek istediğiniz hizmet için bir çift anahtar gerekmez. Daha ayrıntılı bir bakış için bkz: [ortak anahtar şifrelemesini](https://wikipedia.org/wiki/Public-key_cryptography).

SSH güvenli olmayan bağlantıları üzerinden güvenli oturum açma bilgileri veren bir şifreli bir bağlantı protokolüdür. Bu Azure'da barındırılan Linux VM'ler için varsayılan bağlantı protokolüdür. SSH kendisini şifreli bir bağlantı sağlasa da, SSH bağlantılarla parolalarla hala VM yanılma saldırılarını veya parolalarını tahmin açık halde kalır. SSH kullanarak bir VM bağlayan bir daha güvenli ve tercih edilen yöntem, bu ortak ve özel anahtarlar olarak da bilinen SSH anahtarları kullanmaktır.

SSH anahtarları kullanmayı istemiyorsanız, hala bir parola kullanarak, Linux VM'ler için oturum açabilir. VM Internet'e açık değilse, parolaları kullanarak yeterli olabilir. Ancak, yine için her bir Linux VM Parolalarınızı yönetin ve Sağlıklı parola ilkeleri ve minimum parola uzunluğu ve bunları düzenli olarak güncelleştirilmesi gibi uygulamalarını korumak gerekir. SSH anahtarları kullanımını arasında birden çok sanal makineleri tek tek kimlik bilgilerini yönetme karmaşıklığını azaltır.

## <a name="windows-packages-and-ssh-clients"></a>Windows paketleri ve SSH istemcileri
Bağlanmak ve Azure kullanarak Linux VM'ler yönetmek bir **SSH istemcisi**. Windows bilgisayarlarda yüklü olan bir SSH istemcisi genellikle yok. Windows 10 Anniversary Update Windows için Bash eklenir ve en son Windows 10 oluşturucuları güncelleştirmesi ek güncelleştirmeler sağlar. Linux için bu Windows alt çalıştırın ve yardımcı programlar bir Bash kabuğunda içinde yerel olarak bir SSH istemcisi gibi erişim sağlar. Ardından, Linux belgeleri gibi izleyebilirsiniz [Linux için SSH anahtar çiftleri oluşturmak nasıl](mac-create-ssh-keys.md). Bash Windows halen geliştirilme aşamasındadır ve beta sürümü olarak kabul edilir. Windows için Bash hakkında daha fazla bilgi için bkz: [Bash Ubuntu Windows üzerinde](https://msdn.microsoft.com/commandline/wsl/about).

Bir şey Windows dışındaki Bash kullanmak istiyorsanız, yükleyebilmek için ortak Windows SSH istemcilerini aşağıdaki paketlerinde bulunur:

* [Windows için Git](https://git-for-windows.github.io/)
* [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)


## <a name="which-key-files-do-you-need-to-create"></a>Anahtar dosyaları oluşturmanız gerekiyor mu?
Azure en az 2048 bit gerektirir **ssh-rsa** ortak ve özel anahtarlar biçimlendirilmiş. Klasik dağıtım modeli kullanarak Azure kaynaklarına yönetiyorsanız, ayrıca bir PEM oluşturmak gerekir (`.pem` dosyası).

Dağıtım senaryoları ve her kullandığınız dosya türleri şunlardır:

1. **SSH-rsa** anahtarları kullanarak herhangi bir dağıtım için gerekli [Azure portal](https://portal.azure.com)ve Resource Manager dağıtımları kullanarak [Azure CLI](../../cli-install-nodejs.md).
   * Bu anahtarları, genellikle tüm çoğu kişi ihtiyacınız olan.
2. A `.pem` dosya VM'ler kullanarak Klasik dağıtımı oluşturmak için gereklidir. Bu anahtarları Klasik dağıtımlarda kullanılırken desteklenen [Azure portal](https://portal.azure.com) veya [Azure CLI](../../cli-install-nodejs.md).
   * Yalnızca klasik dağıtım modeli kullanılarak oluşturulan kaynakları yönetiyorsanız, bu ek anahtar ve sertifikalar oluşturmanız gerekir.

## <a name="install-git-for-windows"></a>Windows için Git yükleme
Önceki bölümde listelenen dahil çeşitli paketler `openssl` Windows için aracı. Bu araç, ortak ve özel anahtarlar oluşturmak için gereklidir. Aşağıdaki örnekler yüklemek ve kullanmak nasıl ayrıntı **Windows için Git**tercih ettiğiniz hangi paket seçebilirsiniz ancak. **Windows için Git** , bazı ek açık kaynaklı yazılım erişmenizi ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) araçları ve yardımcı programları Linux VM'ler ile çalışırken, yararlı olabilir.

1. İndirme ve yükleme **Windows için Git** aşağıdaki konumdan: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).
2. Özellikle bunları değiştirmek gerekli olmadıkça varsayılan seçenekleri yükleme işlemi sırasında kabul edin.
3. Çalıştırma **Git Bash** gelen **Başlat menüsü** > **Git** > **Git Bash**. Konsolunda, aşağıdaki örneğe benzer:

    ![Windows için Git Bash Kabuk](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a>Özel bir anahtar oluşturun
1. İçinde **Git Bash** penceresi, kullanım `openssl.exe` özel bir anahtar oluşturmak için. Aşağıdaki örnek adlı bir anahtar oluşturur `myPrivateKey` ve adlı sertifika `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   Bash hata bildirirse, yeni bir açmayı deneyin **Git Bash** yükseltilmiş ayrıcalıklarla penceresi. Ardından, yeniden `openssl` komutu.

2. Komut istemlerini ülke adı, konumu, kuruluşunuzun adı, vb. için yanıtlayın.
3. Yeni özel anahtar ve sertifika, geçerli çalışma dizini içinde oluşturulur. Bir güvenlik önlemi olarak, böylece yalnızca erişebilir özel anahtarınızı izinleri ayarlamalısınız:

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. [Sonraki bölümde](#create-a-private-key-for-putty) hem görüntülemek ve ortak anahtarı kullanır ve bir özel anahtar SSH Linux VM'ler için PuTTY kullanma belirli oluşturmak üzere PuTTYgen kullanarak ayrıntıları. Aşağıdaki komutu adlı ortak bir anahtar dosyası oluşturur `myPublicKey.key` hemen kullanabileceğiniz:

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. Ayrıca Klasik kaynakları yönetmek gerekiyorsa, dönüştürmek `myCert.pem` için `myCert.cer` (DER ile kodlanmış X509 sertifika). Bu isteğe bağlı adım, yalnızca özellikle eski Klasik kaynakları yönetmek gerekiyorsa gerçekleştirin.

    Aşağıdaki komutu kullanarak sertifikayı Dönüştür:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>PuTTY için özel bir anahtar oluşturun
Putty'yi Windows için ortak bir SSH İstemcisi ' dir. İstediğiniz herhangi bir SSH istemcisi kullanmakta serbestsiniz. PuTTY kullanmak için ek bir anahtar - PuTTY özel anahtar (PPK) türü oluşturmanız gerekir. PuTTY kullanmak istemiyorsanız bu bölümü atlayabilirsiniz.

Aşağıdaki örnek, bu ek özel anahtarı kullanmak özellikle PuTTY için oluşturur:

1. Kullanım **Git Bash** PuTTYgen anlayabileceği bir RSA özel anahtarı özel anahtarınızı dönüştürülemiyor. Aşağıdaki örnek adlı bir anahtar oluşturur `myPrivateKey_rsa` adlı varolan anahtarından `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Bir güvenlik önlemi olarak, böylece yalnızca erişebilir özel anahtarınızı izinleri ayarlamalısınız:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. Yükleyin ve aşağıdaki konumdan PuTTYgen çalıştırın: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
3. Menüsünü tıklatın: **dosya** > **yük özel anahtar**
4. Özel anahtarınızı bulun (`myPrivateKey_rsa` önceki örnekte). Varsayılan dizin başlattığınızda **Git Bash** olan `C:\Users\%username%`. Dosya Filtresi göstermek için değiştirme **tüm dosyalar (\*.\*)** :

    ![Var olan özel anahtarı PuTTYgen yükleme](./media/ssh-from-windows/load-private-key.png)
5. Tıklatın **açık**. Bir istem anahtarının başarıyla içe aktarıldığından gösterir:

    ![PuTTYgen başarıyla içeri aktarıldı anahtarına](./media/ssh-from-windows/successfully-imported-key.png)
6. Tıklatın **Tamam** istemini kapatın.
7. Ortak anahtarını üst kısmında görüntülenir **PuTTYgen** penceresi. Kopyalayın ve bir Linux VM oluşturduğunuzda, bu ortak anahtar Azure portalında veya Azure Resource Manager şablonu yapıştırın. Tıklatarak **ortak anahtarı Kaydet** bilgisayarınıza bir kopyasını kaydetmek için:

    ![PuTTY ortak anahtar dosyasını kaydedin](./media/ssh-from-windows/save-public-key.png)

    Aşağıdaki örnekte nasıl kopyalayıp bir Linux VM oluşturduğunuzda, bu ortak anahtar Azure portalına gösterir. Ortak anahtar genellikle sonra depolanır `~/.ssh/authorized_keys` , yeni bir VM üzerinde.

    ![Azure portalında bir VM oluşturduğunuzda ortak anahtarı kullan](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. Geri **PuTTYgen**, tıklatın **özel anahtarı Kaydet**:

    ![PuTTY özel anahtar dosyasını kaydedin](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > Bir istem anahtarınız için bir parola girmeden devam etmek isteyip istemediğinizi sorar. Bir parola özel anahtarınızı bağlı bir parola gibidir. Birisi olsa özel anahtarınızı almak için bunlar hala yeni anahtarı kullanarak kimlik doğrulaması için olmaz. Bunlar ayrıca parola gerekir. Birisi özel anahtarınızı elde ederse bir parola, bunlar herhangi bir VM veya bu anahtarı kullanan hizmet oturum açabilir. Bir parola oluşturmanız önerilir. Ancak, parolayı unutursanız, bunu kurtarma yolu yoktur.
   >
   >

    Bir parola girmek ister tıklatmak **Hayır**, ana PuTTYgen penceresinde bir parola girin ve ardından **özel anahtarı Kaydet** yeniden. Aksi takdirde tıklatın **Evet** isteğe bağlı bir parola sağlamadan devam etmek için.
9. Bir ad ve PPK dosyanızı kaydetmek için konum girin.

## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Bir Linux makineye SSH için Putty kullanın
Yeniden PuTTY Windows için ortak bir SSH İstemcisi ' dir. İstediğiniz herhangi bir SSH istemcisi kullanmakta serbestsiniz. Aşağıdaki adımlar, Azure SSH kullanarak VM ile kimlik doğrulaması için özel anahtarınızı kullanmayı ayrıntılı olarak açıklanmaktadır. SSH bağlantı kimliğini doğrulamak için özel anahtarınızı yüklemeye gerek bakımından diğer SSH anahtar istemcilere adımları benzerdir.

1. İndirme ve çalıştırma putty aşağıdaki konumdan: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Ana bilgisayar adı veya IP adresi, VM'nin Azure portalından doldurun:

    ![PuTTY yeni bağlantı Aç](./media/ssh-from-windows/putty-new-connection.png)
3. Seçmeden önce **açık**, tıklatın **bağlantı** > **SSH** > **Auth** sekmesi. Göz atın ve özel anahtarınızı seçin:

    ![Kimlik doğrulaması için PuTTY özel anahtarınızı seçin](./media/ssh-from-windows/putty-auth-dialog.png)
4. Tıklatın **açık** , sanal makineye bağlanmak için

## <a name="next-steps"></a>Sonraki adımlar
Ortak ve özel anahtarlar da oluşturabileceğiniz [OS X ile Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Windows için Bash ve Windows bilgisayarınızda OSS araçları kullanıma hazır olması avantajları hakkında daha fazla bilgi için bkz: [Bash Ubuntu Windows üzerinde](https://msdn.microsoft.com/commandline/wsl/about).

SSH kullanarak Linux Vm'lerinizi bağlanmak için bkz: konusunda sorun yaşıyorsanız [sorun giderme SSH bağlantı Azure Linux VM'de](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
