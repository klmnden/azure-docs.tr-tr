<properties
    pageTitle="Linux ve Mac’de SSH anahtarları oluşturma | Microsoft Azure"
    description="Linux ve Mac’de Resource Manager için SSH anahtarları ve Azure’da klasik dağıtım modelleri oluşturma ve kullanma"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="v-livech"/>

# Linux ve Mac’de Linux VM’ler için Azure’da SSH anahtarları oluşturma

Parola korumalı bir SSH ortak ve özel anahtar oluşturmak için iş istasyonunuzda bir terminal açmanız gerekir.  SSH anahtarlarınız olduğunda, varsayılan olan yeni VM’ler oluşturabilir ya da Azure CLI ve Azure şablonlarını kullanarak mevcut VM’lere ortak anahtar ekleyebilirsiniz.  Bu, parolalara göre çok daha güvenli kimlik doğrulama yöntemi kullanarak SSH üzerinde parolasız oturum açmaya olanak tanır.

## Hızlı Komut Listesi

Aşağıdaki komut örneklerinde, &lt; ile &gt; arasındaki değerleri kendi ortamınızdaki değerlerle değiştirin.

```bash
ssh-keygen -t rsa -b 2048 -C "<your_user@yourdomain.com>"
```

`~/.ssh/` dizinine kaydedilecek dosyanın adını girin:

```bash
<azure_fedora_id_rsa>
```

azure_fedora_id_rsa için parolayı girin:

```bash
<correct horse battery staple>
```

Linux ve Mac’de yeni oluşturulan anahtarı `ssh-agent` üzerine ekleyin (OSX Anahtarlığa da eklenir):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/azure_fedora_id_rsa
```

SSH ortak anahtarını Linux Sunucunuza kopyalayın:

```bash
ssh-copy-id -i ~/.ssh/azure_fedora_id_rsa.pub <youruser@yourserver.com>
```

Parola yerine anahtarları kullanarak oturum açmayı test edin:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/azure_fedora_id_rsa <youruser@yourserver.com>
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## Giriş

SSH ortak ve özel anahtarları kullanmak Linux sunucularınızda oturum açmanın en kolay yoludur ancak [ortak anahtar şifreleme](https://en.wikipedia.org/wiki/Public-key_cryptography)ye ek olarak, kolayca deneme yanılmayla zorlanabilecek parolalara göre Azure’da Linux’da ya da BSD VM’de oturum açmak için daha güvenli bir yol da sağlar. Ortak anahtarınız herkesle paylaşılabilir; ancak, özel anahtarınıza yalnızca siz (veya yerel güvenlik altyapınız) sahip olursunuz.  Oluşturulan SSH özel anahtarı bunu korumak için [güvenli parola](https://www.xkcd.com/936/)ya sahip olur ve bu parola yalnızca özel SSH anahtarına erişim içindir ve kullanıcı hesabı parolası **değildir**.  SSH anahtarınıza bir parola eklediğinizde, bu özel anahtarı şifreler, böylece özel anahtar kilidini açmak için parola olmadan kullanılamaz.  Bir saldırgan özel anahtarınızı çalmayı başarabilirse ve bir parolaya sahip değilse, bu özel anahtarı ilgili ortak anahtarınız kurulu olan sunucularınızda oturum açmak için kullanabilir.  Özel anahtar parola korumalı ise, Azure’daki altyapınız için ek bir güvenlik katmanı sağlayarak, bu saldırgan tarafından kullanılamaz.


Bu makale, Resource Manager’daki dağıtımlar için önerilen ve hem klasik hem de Resource Manager dağıtımları için [portal](https://portal.azure.com)da gereken, *ssh-rsa* biçimli anahtar dosyalarını oluşturur.


## SSH Anahtarları oluşturma

Azure en az 2048 bit, ssh-rsa biçimli ortak ve özel anahtarlar gerektirir. Çifti oluşturmak için, bir seri soru soran ve sonra özel anahtar ve eşleşen ortak anahtarı yazan `ssh-keygen` komutunu kullanacağız. Azure VM’nizi oluşturduğunuzda, Linux VM’ye kopyalanan ve yerel ve güvenli şekilde depolanan özel anahtarınız ile oturum açtığınızda kimlik doğrulaması yapmak için kullanılan ortak anahtar içeriğini geçirirsiniz.

## SSH-keygen’i kullanma

Bu komut, 2048 bir RSA kullanarak parola korumalı (şifrelenmiş) bir SSH Anahtarlığı oluşturur ve kolayca tanımlamak için açıklanır.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
```

_Komut açıklaması_

`ssh-keygen` = anahtarları oluşturmak için kullanılan program

`-t rsa` = [RSA biçimi]olan, oluşturulacak anahtar türü (https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = anahtar bitleri

`-C "ahmet@fedoraVMAzure"` = kolayca tanımlamak için ortak anahtar dosyasının sonuna eklenen bir açıklama.  Normalde açıklama olarak bir e-posta kullanılır ancak altyapınızda en iyi şekilde çalışan her şeyi kullanabilirsiniz.

## Ssh-keygen kılavuzu

Her adım ayrıntılı olarak açıklanmıştır.  `ssh-keygen` çalıştırarak başlayın.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in azure_fedora_id_rsa.
Your public key has been saved in azure_fedora_id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@fedoraVMAzure
The key's randomart image is:
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

Kaydedilen anahtar dosyaları:

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa`

Bu makale için anahtar çifti adı.  Anahtar çiftine **id_rsa** adı verilmesi varsayılandır ve bazı araçlar **id_rsa** özel anahtar adı bekleyebilir, bu nedenle bir tane olması iyi bir fikirdir. (`~/.ssh/` tüm, SSH anahtar çiftleriniz ve SSH yapılandırma dosyanız için tipik varsayılan konumdur.)

```bash
ahmet@fedora$ ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 azure_fedora_id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 azure_fedora_id_rsa.pub
```
Bu, yeni anahtar çiftlerinizi ve bunların izinlerini gösterir. `ssh-keygen` , yoksa, `~/.ssh` dizinin oluşturur ve ayrıca doğru sahipliği ve dosya modlarını ayarlar.

Anahtar Parolası:

`Enter passphrase (empty for no passphrase):`

Anahtar çiftlerinize bir parola eklenmesi önemle önerilir (`ssh-keygen`, bunu "parola" olarak çağırır). Anahtar çiftini koruyan bir parola olmadan, özel anahtar dosyasının bir kopyasına sahip olan herkes, bunu ilgili ortak anahtara sahip olan sunucuda (sunucunuzda) oturum açmak için kullanabilir. Bu nedenle bir parola eklemek, birinin özel anahtar dosyanıza erişim sağlama ihtimaline karşı, kimliğinizi doğrulamak için kullanılan anahtarları değiştirmek üzere size zaman vermesiyle daha fazla koruma sağlar.

## Özel anahtar parolanızı depolamak için ssh-agent kullanma

Her SSH oturum açışınızda özel anahtar dosya parolanızı yazmanızı önlemek için, Linux VM’nizde hızlı ve güvenli oturum açmanıza olanak tanır şekilde, `ssh-agent` kullanarak özel anahtar dosya parolanızı önbelleğe kaydedebilirsiniz.  OSX kullanıyorsanız, `ssh-agent` çağırdığınızda, Anahtarlık özel anahtar parolalarınızı güvenli bir şekilde depolar.

Önce `ssh-agent`’in çalıştığını doğrulayın

```bash
eval "$(ssh-agent -s)"
```

Şimdi, `ssh-add` komutunu kullanarak özel anahtarı `ssh-agent`’e ekleyin, bu kimlik bilgilerini depolayan Anahtarlığı OSX üzerinde yeniden başlatacaktır.

```bash
ssh-add ~/.ssh/azure_fedora_id_rsa
```

Özel anahtar parolası şimdi depolanır, böylece her SSH oturumu açışınızda anahtar parolasını yazmak zorunda kalmazsınız.

## SSH yapılandırma dosyası oluşturma ve yapılandırma

Bir Linux VM oluşturup çalıştırmak kesinlikle gerekli olmamakla birlikte, VM’lerinizle oturum açmak için yanlışlıkla parolaları kullanmanızı önlemek, farklı Azure VM’leri için farklı anahtar çiftlerini kullanmayı otomatik hale getirmek ve birden fazla sunucuyu hedeflemek için **git** gibi diğer programları yapılandırmak amacıyla bir `~/.ssh/config` dosyası oluşturmanız ve yapılandırmanız en iyi uygulamadır.

Aşağıdaki örnek, standart bir yapılandırmayı gösterir.

### Önbelleği oluşturma

```bash
touch ~/.ssh/config
```

### Yeni SSH yapılandırması eklemek için dosyayı düzenleme:

```bash
vim ~/.ssh/config
```

### Örnek `~/.ssh/config` dosyası:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
  PubkeyAuthentication yes
  IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=no
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath /home/ahmet/.ssh/Connections/ssh-%r@%h:%p
  ControlPersist 4h
  StrictHostKeyChecking=no
  IdentityFile /home/ahmet/.ssh/id_rsa
  UseRoaming=no
```

Bu SSH yapılandırması, her hizmetin kendi özel anahtar çiftini etkinleştirmesini sağlamak için size bölümler sağlar. Varsayılan ayarlar, oturum açtığınız, yukarıdaki gruplardan biriyle eşleşmeyen ana bilgisayarlar içindir.


### Yapılandırma dosyası açıklaması

`Host` = terminal üzerinde çağrılan ana bilgisayar adı.  `ssh fedora22` , `SSH`’ye `Host fedora22` blok etiketli ayarlardaki değerleri kullanmasını söyler. NOT: Bu kullanımınız için mantıklı olan ve bir sunucunun gerçek ana bilgisayar adını temsil etmeyen her etiket olabilir.

`Hostname 102.160.203.241` =oturum açılan sunucunun IP adresi ya da DNS adı. Bu, sunucuyu yönlendirmek için kullanılır ve dahili IP’ye eşleşen bir harici IP olabilir.

`User git` = Azure VM’de oturum açarken kullanılacak uzak kullanıcı hesabı.

`PubKeyAuthentication yes` = bu, SSH’ye oturum açma bilgisi olarak bir SSH kullanmak istediğinizi söyler.

`IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa` = bu SSH’ye oturum açma kimliğini doğrulamak için sunucuya hangi anahtar çiftini göstereceğini söyler.


## Parolasız Linux içine SSH

Artık bir SSH anahtar çiftiniz ve yapılandırılmış bir yapılandırma dosyasına sahip olduğunuza göre Linux VM’nizde hızlı ve güvenli şekilde oturum açabilirsiniz. SSH kullanarak bir sunucuda ilk oturum açışınızda, komut sizden bu anahtar dosyası için parolayı ister.

```bash
ssh fedora22
```

### Komut açıklaması

`ssh fedora22` yürütüldüğünde, SSH önce `Host fedora22` bloğundaki tüm ayarları bulur ve yükler ve sonra son blok olan `Host *` içindeki kalan tüm ayarları yükler.

## Sonraki Adımlar

Sonraki adım, yeni SSH ortak anahtarını kullanarak Azure Linux VM’ler oluşturmaktır.  Oturum açma bilgisi olarak SSH ortak anahtarıyla oluşturulan Azure VM’ler, varsayılan oturum açma yöntemi parolalarıyla oluşturulanlara göre daha güvenlidir.  Oturum açma bilgisi olarak SSH anahtarları kullanılarak oluşturulan Azure VM’ler, varsayılan olarak deneme yanılma yoluyla yapılan zorla girme denemelerini önleyerek parolalı oturum açma bilgilerini devre dışı bırakmak üzere yapılandırılmıştır.

- [Azure şablonu kullanarak Güvenli bir Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Azure Portal kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-quick-create-portal.md)
- [Azure CLI kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-quick-create-cli.md)



<!--HONumber=Jun16_HO2-->


