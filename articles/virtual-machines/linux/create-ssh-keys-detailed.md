---
title: Ayrıntılı adımlar - SSH anahtar çifti Azure Linux VM'ler için | Microsoft Docs
description: Oluşturma ve Azure Linux VM'ler için bir SSH ortak ve özel anahtar çifti yönetmek için ayrıntılı adımlar öğrenin.
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
ms.openlocfilehash: 827c80a70047fd0f1ad67e4f19cb2300e45b2c6b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31600351"
---
# <a name="detailed-steps-create-and-manage-ssh-keys-for-authentication-to-a-linux-vm-in-azure"></a>Ayrıntılı adımlar: oluşturmak ve azure'da bir Linux VM için kimlik doğrulaması için SSH anahtarları Yönet 
Güvenli Kabuk (SSH) anahtar çifti ile kimlik doğrulaması için SSH anahtarları kullanmayı varsayılan olarak oturum açmak parola gereksinimini azure'da bir Linux sanal makine oluşturabilirsiniz. Azure portalı ile Azure CLI, Resource Manager şablonları VM'ler oluşturulduktan veya diğer araçları SSH anahtar kimlik doğrulaması için SSH bağlantı kurar dağıtımın parçası olarak SSH ortak anahtarınızı içerebilir. 

Bu makale, ayrıntılı arka plan ve oluşturmak ve SSH istemci bağlantıları için bir SSH RSA genel-özel anahtar dosyası çifti yönetmek için adımlar sağlar. Hızlı komutlar istiyorsanız, bkz: [Azure Linux VM'ler için bir SSH genel-özel anahtar çifti oluşturma](mac-create-ssh-keys.md).

Oluşturma ve bir Windows bilgisayarda SSH anahtarları kullanmak üzere ek yöntemler için bkz: [SSH kullanma anahtarları azure'da Windows ile](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

### <a name="private-key-passphrase"></a>Özel anahtar parolası
SSH özel anahtarı bunu korumak için çok güvenli bir parola olmalıdır. Bu parola yalnızca özel SSH anahtar dosyası erişim içindir ve *değil* kullanıcı hesabı parolası. SSH anahtarınıza bir parola eklediğinizde, böylece özel anahtarın şifresini için parola gereksizdir 128 bit AES kullanarak özel anahtarı şifreler. Bir saldırgan özel anahtarınızı çalıntı ve bu anahtarı bir parola içermiyor, bu özel anahtarı ilgili ortak anahtara sahip bir sunucuya oturum açmak için kullandığınız mümkün olacaktır. Özel anahtarı bir parola tarafından korunuyorsa, Azure'daki altyapınız için ek bir güvenlik katmanı sağlayarak, bu saldırgan tarafından kullanılamaz.

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="ssh-keys-use-and-benefits"></a>SSH anahtarlarının kullanımı ve avantajları

Ortak anahtar belirterek bir Azure VM oluşturduğunuzda, Azure ortak anahtarı kopyalar (içinde `.pub` biçimi) için `~/.ssh/authorized_keys` VM klasörü. `~/.ssh/authorized_keys` içindeki SSH anahtarları, bir istemcinin SSH oturum açma bağlantısındaki ilgili özel anahtarla eşleşip eşleşmediğini sınar. Bir Azure Linux kimlik doğrulaması için SSH anahtarları kullanan VM, Azure yalnızca SSH anahtarları parolalı oturum açma izin vermeyecek şekilde SSHD sunucusunu yapılandırır. Bu nedenle, bir Azure Linux VM ile SSH anahtarları oluşturarak, güvenli VM dağıtımı ve kendiniz parolalarda devre dışı bırakma tipik dağıtım sonrası yapılandırma adımı kaydedin yardımcı olabilir `sshd_config` dosya.

SSH anahtarları kullanmayı istemiyorsanız, Linux VM'NİZDE parola kimlik doğrulaması kullanacak şekilde ayarlayabilirsiniz. VM Internet'e açık değilse, parolaları kullanarak yeterli olabilir. Ancak, yine için her bir Linux VM Parolalarınızı yönetin ve Sağlıklı parola ilkeleri ve uygulamaları, parola uzunluğu alt sınırı ve normal güncelleştirmeleri gibi korumak gerekir. SSH anahtarları kullanarak birden çok VM üzerinde tek tek kimlik bilgilerini yönetme karmaşıklığını azaltır.

## <a name="generate-keys-with-ssh-keygen"></a>Ssh-keygen ile anahtarları oluştur

Anahtarları oluşturmak için tercih edilen komuttur `ssh-keygen`, Azure bulut Kabuk, macOS veya Linux ana OpenSSH yardımcı programları ile kullanılabilen [Linux için Windows alt](https://docs.microsoft.com/windows/wsl/about)ve diğer araçları. `ssh-keygen` bir seri soru soran ve özel anahtar ve eşleşen ortak anahtarı yazar. 

SSH anahtarları, varsayılan olarak `~/.ssh` dizininde tutulur.  `~/.ssh` dizininiz yoksa `ssh-keygen` komutu, doğru izinler ile sizin için oluşturur.

### <a name="basic-example"></a>Temel örnek

Aşağıdaki `ssh-keygen` komut, varsayılan olarak 2048 bit SSH RSA ortak ve özel anahtar dosyaları oluşturur `~/.ssh` dizin. SSH anahtar çifti geçerli konumdan birinde varsa, bu dosyaların üzerine yazılır.

```bash
ssh-keygen -t rsa -b 2048
```

### <a name="detailed-example"></a>Ayrıntılı bir örnek
Aşağıdaki örnek, bir SSH RSA anahtar çifti oluşturmak için ek komut seçenekleri gösterir. SSH anahtar çifti geçerli konumdan birinde varsa, bu dosyaların üzerine yazılır. 

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

`-C "azureuser@myserver"` = kolayca tanımlamak için ortak anahtar dosyasının sonuna eklenen bir açıklama. Normalde açıklama olarak bir e-posta adresi kullanılır, ancak iyi ne olursa olsun, altyapınız için kullanın.

`-f ~/.ssh/mykeys/myprivatekey` Varsayılan adı kullanmayı seçerseniz özel anahtar dosyasının, dosya adı =. Karşılık gelen bir genel anahtar dosyasının eklenen `.pub` aynı dizinde oluşturulur. Dizini mevcut olması gerekir.

`-N mypassphrase` özel anahtar dosyası erişmek için kullanılan bir ek parola =. 

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

Bu makale için anahtar çifti adı. Adlı bir anahtar çiftine `id_rsa` ; varsayılan bazı araçlar bekleyebilirsiniz `id_rsa` tane olması iyi bir fikirdir böylece özel anahtar dosya adı. `~/.ssh/` dizini, SSH anahtar çiftleri ve SSH yapılandırma dosyası için varsayılan konumdur. Tam yol belirtilmezse `ssh-keygen` tarafından oluşturulan anahtarlar varsayılan `~/.ssh` dizininde değil geçerli çalışma dizininde olur.

#### <a name="list-of-the-ssh-directory"></a>Listesi `~/.ssh` dizini

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

#### <a name="key-passphrase"></a>Anahtar parolası

`Enter passphrase (empty for no passphrase):`

Bu *kesinlikle* özel anahtarınızı bir parola eklemek için önerilir. Anahtarı dosyasını korumak için bir parola dosya kimseyle bunu ilgili ortak anahtara sahip herhangi bir sunucuya oturum açmak için kullanabilirsiniz. Bir parola katıştırılabilecek daha fazla koruma birinin özel anahtar dosyanıza erişim elde edebilir olması durumunda anahtarları değiştirmek üzere size zaman verir.

## <a name="generate-keys-automatically-during-deployment"></a>Anahtarları dağıtımı sırasında otomatik olarak oluştur

Kullanırsanız [Azure CLI 2.0](/cli/azure) VM oluşturmak için isteğe bağlı olarak SSH ortak ve özel anahtar dosyaları çalıştırarak oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm#az_vm_create) komutunu `--generate-ssh-keys` seçeneği. Anahtarlar ~/.ssh dizininde depolanır. Bu konumda zaten varsa bu komut seçeneği anahtarları üzerine yazmaz olduğunu unutmayın.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>SSH ortak anahtarı bir VM dağıtırken sağlayın

Kimlik doğrulaması için SSH anahtarları kullanan bir Linux VM oluşturmak için SSH ortak anahtarınızı CLI, Azure portalını kullanarak VM oluşturulurken sağlamak Resource Manager şablonları ya da diğer yöntemleri. Portal kullanırken, ortak anahtarı girin. Kullanırsanız [Azure CLI 2.0](/cli/azure) var olan bir ortak anahtar ile VM oluşturmak için değer konumunu veya bu ortak anahtar çalıştırarak belirtmek [az vm oluşturma](/cli/azure/vm#az_vm_create) komutunu `--ssh-key-value` seçeneği. 

Bir SSH ortak anahtarı biçimiyle bilmiyorsanız, çalıştırarak ortak anahtarınız görebilirsiniz `cat` aşağıdaki gibi değiştirme `~/.ssh/id_rsa.pub` kendi ortak anahtar dosyası konumu:

```bash
cat ~/.ssh/id_rsa.pub
```

Çıktı (burada Redaksiyon) aşağıdakine benzer:

```
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXG+/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Kopyala ve Azure portalını veya Resource Manager şablonu ortak anahtar dosyasının içeriğini yapıştırın, herhangi bir ek boşluk kopyalayın veya ek satır sonlarını oluşabileceğini yok emin olun. MacOS kullanırsanız, örneğin, ortak anahtar dosyasını iletebildiğiniz (varsayılan olarak, `~/.ssh/id_rsa.pub`) için **pbcopy** içeriği kopyalamak için (aynı gibi şeyler da diğer Linux programlar **xclip**).

Çok satırlı bir biçimde bir ortak anahtarı kullanmayı tercih ederseniz, önceden oluşturduğunuz ortak anahtarı pem kapsayıcısında bir RFC4716 biçimlendirilmiş anahtarı oluşturabilir.

Var olan bir SSH ortak anahtarından RFC4716 biçimli bir anahtar oluşturmak için:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="ssh-to-your-vm-with-an-ssh-client"></a>SSH, VM'ye bir SSH istemcisi ile
Azure VM'nizi dağıtılan ortak anahtar ve özel anahtarı yerel sisteminizde, IP adresi veya VM DNS adını kullanarak, VM SSH ile. Değiştir *azureuser* ve *myvm.westus.cloudapp.azure.com* aşağıdaki komut yönetici kullanıcı adı ve tam etki alanı adı (veya IP adresi ile):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluştururken bir parola sağladıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin. (Sunucu `~/.ssh/known_hosts` klasörünüze eklenir ve Azure VM’nizdeki ortak anahtar değiştirilene veya sunucu adı `~/.ssh/known_hosts` konumundan kaldırılana kadar yeniden bağlanmanız istenmez.)

## <a name="use-ssh-agent-to-store-your-private-key-passphrase"></a>Özel anahtar parolanızı depolamak için SSH aracı kullanın

Her SSH oturumu açışınızda özel anahtar dosya parolanızı yazmaya önlemek için kullanabileceğiniz `ssh-agent` özel anahtar dosya parolanızı önbelleğe almak için. Mac kullanıyorsanız, çağırdığınızda, Anahtarlık macOS güvenli bir şekilde özel anahtar parolası depolar `ssh-agent`.

Doğrulayın ve kullanmak `ssh-agent` ve `ssh-add` böylece parola etkileşimli olarak kullanmak gerekmez SSH sistem anahtar dosyaları hakkında bilgilendirmek için.

```bash
eval "$(ssh-agent -s)"
```

Şimdi `ssh-add` komutunu kullanarak özel anahtarı `ssh-agent` öğesine ekleyin.

```bash
ssh-add ~/.ssh/id_rsa
```

Özel anahtar parolası şimdi depolanır `ssh-agent`.

## <a name="use-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a>Mevcut bir VM'yi anahtar kopyalamak için SSH-kopya-ID kullanın
Bir VM zaten oluşturduysanız, Linux VM'ye aşağıdakine benzer bir komutla yeni SSH ortak anahtarını yükleyebilirsiniz:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>SSH yapılandırma dosyası oluşturma ve yapılandırma

Oluşturma ve SSH yapılandırma dosyası yapılandırma (`~/.ssh/config`) günlük bileşenler hızlandırmak için ve SSH istemci davranışını en iyi duruma getirme. 

Aşağıdaki örnekte basit bir yapılandırma, hızlı bir şekilde belirli bir VM'yi kullanıcı olarak oturum açmak için kullanabileceğiniz varsayılan SSH özel anahtar kullanarak gösterir. 

### <a name="create-the-file"></a>Önbelleği oluşturma

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Yeni SSH yapılandırması eklemek için dosyayı düzenleyin.

```bash
vim ~/.ssh/config
```

### <a name="example-configuration"></a>Örnek yapılandırma

VM konağınız için uygun yapılandırma ayarlarını ekleyin.

```bash
# Azure Keys
Host myvm
  Hostname 102.160.203.241
  User azureuser
# ./Azure Keys
```

Her kendi özel anahtar çiftini kullanmak üzere etkinleştirmek ek ana bilgisayarlar için yapılandırmaları ekleyebilirsiniz. Bkz: [SSH yapılandırma dosyası](https://www.ssh.com/ssh/config/) daha fazla bilgi için Gelişmiş Yapılandırma Seçenekleri.

Artık bir SSH anahtar çiftiniz ve yapılandırılmış bir SSH yapılandırma dosyasına sahip olduğunuza göre Linux VM'nizde hızlı ve güvenli şekilde oturum açabilirsiniz. Aşağıdaki komutu çalıştırdığınızda, SSH bulur ve tüm ayarları yükler `Host myvm` SSH yapılandırma dosyasında engelleyin.

```bash
ssh myvm
```

Komut istemlerini bir SSH anahtarı kullanarak bir sunucuda, bu anahtar dosyası için parolayı oturum ilk kez.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, yeni SSH ortak anahtarını kullanarak Azure Linux VM’ler oluşturmaktır. Oturum açma işlemi için SSH ortak anahtarıyla oluşturulan Azure VM'ler, varsayılan oturum açma yöntemiyle (parolalar) oluşturulan VM'lere göre daha güvenlidir.

* [Azure portalıyla Linux sanal makine oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI ile Linux sanal makine oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Bir Azure şablonu kullanarak bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
