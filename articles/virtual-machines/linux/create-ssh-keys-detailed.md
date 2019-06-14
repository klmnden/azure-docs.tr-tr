---
title: Ayrıntılı adımlar - SSH anahtar çifti Azure Linux Vm'leri için | Microsoft Docs
description: Oluşturmak ve azure'da Linux VM'ler için SSH ortak ve özel anahtar çifti yönetmek için ayrıntılı adımları öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: danlep
ms.openlocfilehash: 3784dd701b3ac44971e134f1b160fcfe2de2d9b3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60328706"
---
# <a name="detailed-steps-create-and-manage-ssh-keys-for-authentication-to-a-linux-vm-in-azure"></a>Ayrıntılı adımlar: Azure'da bir Linux sanal makinesi için kimlik doğrulaması için SSH anahtarları oluşturma ve yönetme 
Güvenli Kabuk (SSH) anahtar çiftiyle ihtiyacını ortadan oturum açmak parola kimlik doğrulaması için SSH anahtarlarının kullanımını varsayılan olarak Azure üzerinde bir Linux sanal makinesi oluşturabilirsiniz. Şablonları Azure portalı ile Azure CLI'yı Resource Manager Vm'leri oluşturulan veya diğer araçları SSH anahtar kimlik doğrulaması için SSH bağlantısı kurar dağıtımın bir parçası olarak SSH ortak anahtarınızı içerebilir. 

Bu makalede ayrıntılı arka plan ve oluşturmak ve SSH istemci bağlantıları için bir SSH RSA ortak-özel anahtar dosyası çifti yönetmek için adımlar sağlar. Hızlı komutlar isterseniz bkz [azure'da Linux VM'ler için SSH ortak-özel anahtar çifti oluşturma](mac-create-ssh-keys.md).

Oluşturma ve bir Windows bilgisayarda SSH anahtarları kullanma hakkında ek yollar için bkz. [azure'da Windows ile SSH kullanma anahtarları](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

### <a name="private-key-passphrase"></a>Özel anahtar parolası
SSH özel anahtarının koruma amaçlı için çok güvenli bir parola olmalıdır. Bu parola yalnızca özel SSH anahtar dosyasına erişim sağlamaktır ve *değil* kullanıcı hesabı parolası. SSH anahtarınıza parola eklediğinizde, böylece özel anahtar, şifresini çözmek için parola olmadan yararsızdır özel anahtarı 128 bit AES kullanarak şifreler. Bir saldırganın özel anahtarınızı çalması ve bu anahtarın bir parola yoktu, bu özel anahtarı ilgili ortak anahtara sahip sunucularınızda oturum açmak için kullanmanız mümkün olacaktır. Özel anahtar parola ile korunuyorsa, azure'daki altyapınız için ek bir güvenlik katmanı sağlayarak, bu saldırgan tarafından kullanılamaz.

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="ssh-keys-use-and-benefits"></a>SSH anahtarlarının kullanımı ve avantajları

Ortak anahtarı belirterek bir Azure VM oluşturduğunuzda, Azure ortak anahtarı kopyalar (içinde `.pub` biçimi) için `~/.ssh/authorized_keys` VM üzerinde bir klasör. SSH anahtarlarını `~/.ssh/authorized_keys` bir SSH bağlantısı ilgili özel anahtarla eşleşen istemciye eşleşip eşleşmediğini sınar. Bir Azure Linux kimlik doğrulaması için SSH anahtarları kullanan sanal, Azure, SSHD sunucusunu parola oturum açma, yalnızca SSH anahtarlarına izin vermeyecek şekilde yapılandırır. Bu nedenle SSH anahtarlarıyla Azure Linux VM oluşturarak VM dağıtımının güvenliğini ve parolaları devre dışı bırakma tipik dağıtım sonrası yapılandırma adımının size gibi `sshd_config` dosya.

SSH anahtarları kullanmak istemiyorsanız, Linux VM'NİZDE parola kimlik doğrulaması kullanacak şekilde ayarlayabilirsiniz. Sanal makinenizin Internet'e açık değilse, parolaları kullanma yeterli olabilir. Ancak, yine de her bir Linux VM için parolaları yönetme ve Sağlıklı parola ilkeleri ve en düşük parola uzunluğu ve düzenli güncelleştirmeler gibi uygulamaları korumak için ihtiyaç duyduğunuz. SSH anahtarları kullanılarak birden çok VM arasında bireysel kimlik bilgilerini yönetme karmaşıklığı azaltır.

## <a name="generate-keys-with-ssh-keygen"></a>Ssh-keygen ile anahtarları oluştur

Anahtarları oluşturmak için tercih edilen komuttur `ssh-keygen`, Azure Cloud Shell, macOS veya Linux ana OpenSSH yardımcı programları ile kullanılabilen [Linux için Windows alt sistemi](https://docs.microsoft.com/windows/wsl/about)ve diğer araçlar. `ssh-keygen` bir seri soru soran ve sonra özel bir anahtar ve eşleşen bir ortak anahtar yazar. 

SSH anahtarları, varsayılan olarak `~/.ssh` dizininde tutulur.  `~/.ssh` dizininiz yoksa `ssh-keygen` komutu, doğru izinler ile sizin için oluşturur.

### <a name="basic-example"></a>Temel örnek

Aşağıdaki `ssh-keygen` komutu varsayılan olarak 2048 bit SSH RSA ortak ve özel anahtar dosyalarını oluşturur `~/.ssh` dizin. Geçerli konumu bir SSH anahtar çiftiniz varsa bu dosyaların üzerine yazılır.

```bash
ssh-keygen -t rsa -b 2048
```

### <a name="detailed-example"></a>Ayrıntılı örnek
Aşağıdaki örnek, bir SSH RSA anahtar çifti oluşturmak için ek komut seçenekleri gösterir. Geçerli konumu bir SSH anahtar çiftiniz varsa bu dosyaların üzerine yazılır. 

```bash
ssh-keygen \
    -t rsa \
    -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N mypassphrase
```

**Komut açıklaması**

`ssh-keygen` = anahtarları oluşturmak için kullanılan program

`-t rsa` Bu durumda RSA biçiminde oluşturulacak anahtar türü =

`-b 4096` Bu durumda = 4096 bit anahtar sayısı

`-C "azureuser@myserver"` = kolayca tanımlamak için ortak anahtar dosyasının sonuna eklenen bir açıklama. Normalde açıklama olarak bir e-posta adresi kullanılan, ancak ne olursa olsun altyapınız için iyi kullanın.

`-f ~/.ssh/mykeys/myprivatekey` Varsayılan adı kullanmayı tercih ederseniz, özel anahtar dosyasının dosya adı =. Karşılık gelen ortak anahtar dosyası protokolün `.pub` aynı dizinde oluşturulur. Dizinin mevcut olması gerekir.

`-N mypassphrase` özel anahtar dosyasına erişmek için kullanılan bir ek parola =. 

### <a name="example-of-ssh-keygen"></a>ssh-keygen örneği

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
The keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

#### <a name="saved-key-files"></a>Kaydedilen anahtar dosyaları

`Enter file in which to save the key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

Bu makale için anahtar çifti adı. Adlı bir anahtar çiftine `id_rsa` ; varsayılan bazı araçlar `id_rsa` tane olması iyi bir fikirdir böylece özel anahtar dosyasının adı. `~/.ssh/` dizini, SSH anahtar çiftleri ve SSH yapılandırma dosyası için varsayılan konumdur. Tam yol belirtilmezse `ssh-keygen` tarafından oluşturulan anahtarlar varsayılan `~/.ssh` dizininde değil geçerli çalışma dizininde olur.

#### <a name="list-of-the-ssh-directory"></a>Listesi `~/.ssh` dizini

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

#### <a name="key-passphrase"></a>Anahtar parolası

`Enter passphrase (empty for no passphrase):`

Bu *kesin* Özel anahtarınıza bir parola eklemeniz önerilir. Anahtarı dosyasını korumak için bir parola dosyasına sahip herkes bunu ilgili ortak anahtara sahip herhangi bir sunucuda oturum açmak için kullanabilirsiniz. Bir parola katıştırılabilecek daha fazla koruma birinin özel anahtar dosyanıza erişim elde edebilir olması durumunda anahtarları değiştirmeniz için size zaman verir.

## <a name="generate-keys-automatically-during-deployment"></a>Anahtarları dağıtımı sırasında otomatik olarak oluştur

Kullanırsanız [Azure CLI](/cli/azure) VM'nizi oluşturmak için isteğe bağlı olarak SSH ortak ve özel anahtar dosyaları çalıştırarak oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm) komutunu `--generate-ssh-keys` seçeneği. Anahtarları ~/.ssh dizininde depolanır. Bu konumda zaten varsa bu komut seçeneği anahtarları üzerine yazmaz olduğunu unutmayın.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Bir VM dağıtılırken SSH ortak anahtarı sağlayın

Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için SSH ortak anahtarınızı CLI, Azure portalını kullanarak VM'yi oluştururken sağlamanız Resource Manager şablonları ya da diğer yöntemleri. Portalı kullanarak, ortak anahtarı girin. Kullanırsanız [Azure CLI](/cli/azure) mevcut bir ortak anahtar ile VM oluşturma için değer ya da bu ortak anahtarın konumunu çalıştırarak belirtin [az vm oluşturma](/cli/azure/vm) komutunu `--ssh-key-value` seçeneği. 

SSH ortak anahtarı biçimiyle ilgili bilgi sahibi değilseniz, çalıştırarak ortak anahtarınızı görebilirsiniz `cat` şu şekilde değiştirerek `~/.ssh/id_rsa.pub` kendi ortak anahtar dosyası konumunuz ile:

```bash
cat ~/.ssh/id_rsa.pub
```

Çıktı (burada Redaksiyon) aşağıdakine benzer:

```
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXG+/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Azure portalı veya bir Resource Manager şablonuna ortak anahtar dosyasının içeriğini kopyalayıp, fazladan boşluk kopyalayın veya ek satır sonları tanıtmak yoksa emin olun. Örneğin, macOS kullanırsanız, ortak anahtar dosyasını kanal oluşturarak aktarabilirsiniz (varsayılan olarak, `~/.ssh/id_rsa.pub`) için **pbcopy** içeriklerini kopyalamak için (aşağıdaki gibi aynı şeyi yapan diğer Linux programları var. `xclip`).

Çok satırlı biçimde bir ortak anahtar kullanmayı tercih ederseniz, bir pem kapsayıcısındaki daha önce oluşturduğunuz ortak anahtarından rfc4716 biçimli biçimlendirilmiş anahtar oluşturabilirsiniz.

Var olan bir SSH ortak anahtarından RFC4716 biçimli bir anahtar oluşturmak için:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="ssh-to-your-vm-with-an-ssh-client"></a>Bir SSH istemcisi ile sanal makinenize yönelik SSH
Ortak anahtarı, Azure VM üzerinde dağıtılmış ve yerel sisteminizde, IP adresini veya VM'nizi DNS adını kullanarak sanal makinenize yönelik SSH özel anahtarı ile. Değiştirin *azureuser* ve *myvm.westus.cloudapp.azure.com* yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi) ile aşağıdaki komutta:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola sağladıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin. (Sunucu `~/.ssh/known_hosts` klasörünüze eklenir ve Azure VM’nizdeki ortak anahtar değiştirilene veya sunucu adı `~/.ssh/known_hosts` konumundan kaldırılana kadar yeniden bağlanmanız istenmez.)

VM tam zamanında erişim ilkesi kullanıyorsa, sanal Makineye bağlanmadan önce erişim istemeniz gerekir. Tam zamanında İlkesi hakkında daha fazla bilgi için bkz: [yalnızca kullanarak sanal makine erişimini yönetme zamanında İlkesi](../../security-center/security-center-just-in-time.md).

## <a name="use-ssh-agent-to-store-your-private-key-passphrase"></a>Özel anahtar parolanızı depolamak için SSH-agent kullanma

Her SSH oturum açma ile özel anahtar dosya parolanızı yazmaya önlemek için kullanabileceğiniz `ssh-agent` özel anahtar dosya parolanızı önbelleğe almak için. Mac kullanıyorsanız, çağırdığınızda, Anahtarlık macOS güvenli bir şekilde özel anahtar parolası depolar `ssh-agent`.

Doğrulayabilir ve kullanabilirsiniz `ssh-agent` ve `ssh-add` için SSH sistemini anahtar dosyaları hakkında bilgilendirerek parola etkileşimli kullanma gerekmez.

```bash
eval "$(ssh-agent -s)"
```

Şimdi `ssh-add` komutunu kullanarak özel anahtarı `ssh-agent` öğesine ekleyin.

```bash
ssh-add ~/.ssh/id_rsa
```

Özel anahtar parolası şimdi depolanır `ssh-agent`.

## <a name="use-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a>Mevcut bir VM'ye anahtarı kopyalamak için SSH-kopyalama-ID kullanın
Bir VM oluşturduysanız, aşağıdakine benzer bir komut kullanarak Linux VM'nize yeni SSH ortak anahtarını yükleyebilirsiniz:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>SSH yapılandırma dosyası oluşturma ve yapılandırma

Oluşturma ve SSH yapılandırma dosyası yapılandırın (`~/.ssh/config`) oturum açma işlemleri hızlandırmak ve SSH istemci davranışınızı iyileştirmek için. 

Aşağıdaki örnekte basit bir yapılandırma, hızla özel bir VM için bir kullanıcı olarak oturum açmak için kullanabileceğiniz varsayılan SSH özel anahtarı kullanarak gösterir. 

### <a name="create-the-file"></a>Önbelleği oluşturma

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Yeni SSH yapılandırması eklemek için dosyayı Düzenle

```bash
vim ~/.ssh/config
```

### <a name="example-configuration"></a>Örnek yapılandırma

Konak VM'si için uygun yapılandırma ayarı ekleyin.

```bash
# Azure Keys
Host myvm
  Hostname 102.160.203.241
  User azureuser
# ./Azure Keys
```

Kendi özel anahtar çiftini kullanacak şekilde her etkinleştirmek ek konaklar için yapılandırmaları ekleyebilirsiniz. Bkz: [SSH yapılandırma dosyası](https://www.ssh.com/ssh/config/) daha fazla bilgi için Gelişmiş Yapılandırma Seçenekleri.

Artık bir SSH anahtar çiftiniz ve yapılandırılmış bir SSH yapılandırma dosyasına sahip olduğunuza göre Linux VM'NİZDE hızlı ve güvenli şekilde oturum açabilir. Aşağıdaki komutu çalıştırdığınızda, SSH bulur ve tüm ayarları yükler `Host myvm` SSH yapılandırma dosyasında engelleyin.

```bash
ssh myvm
```

İlk kez komut istemleri bir SSH anahtarı kullanarak bir sunucuda, bu anahtar dosyasına ilişkin parolayı oturum.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, yeni SSH ortak anahtarını kullanarak Azure Linux VM’ler oluşturmaktır. Oturum açma olarak SSH ortak anahtarı ile oluşturulan azure VM'ler, daha iyi VM'ler varsayılan oturum açma yöntemiyle parolaları oluşturulduğundan daha güvenlidir.

* [Azure portal ile Linux sanal makinesi oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI ile Linux sanal makinesi oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure şablonu kullanarak bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
