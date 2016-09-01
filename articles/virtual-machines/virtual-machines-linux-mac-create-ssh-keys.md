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
    ms.date="08/08/2016"
    ms.author="v-livech"/>

# Linux ve Mac’de Linux VM’ler için Azure’da SSH anahtarları oluşturma

SSH anahtar çiftiyle Azure'da Sanal Makineler oluşturabilirsiniz. Bu sayede kimlik doğrulaması için SSH anahtarlarının kullanımını varsayılan hale getirerek oturum açmak için parolalara duyulan gereksinimi ortadan kaldırırsınız.  Parolalar, tahmin edilebilir olup sanal makinelerinizi parolanızı tahmin etmeye yönelik sayısız girişime maruz bırakır. Azure Şablonları veya `azure-cli` ile oluşturulan sanal makineler, dağıtımın bir parçası olarak SSH ortak anahtarınızı içerebilir; böylece dağıtım sonrası yapılandırma gereksinimini ortadan kaldırır.  Linux VM'ye Windows üzerinden bağlanıyorsanız [bu belgeye](virtual-machines-linux-ssh-from-windows.md) göz atın.

## Hızlı Komut Listesi

Aşağıdaki komut örneklerinde, &lt; ile &gt; arasındaki değerleri kendi ortamınızdaki değerlerle değiştirin.

```bash
ssh-keygen -t rsa -b 2048 -C "<your_user@yourdomain.com>"
```

`~/.ssh/` dizinine kaydedilen dosyanın adını girin:

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

SSH ortak ve özel anahtarlarını kullanmak, Linux sunucularınızda oturum açmanın en kolay yoludur. [Ortak anahtar şifrelemesi](https://en.wikipedia.org/wiki/Public-key_cryptography), Azure'daki Linux veya BSD VM'nizde oturum açmak için parolalara kıyasla çok daha güvenli bir yol sunar. Parolalar, saldırılara çok daha kolay şekilde maruz kalır. Ortak anahtarınız herkesle paylaşılabilir; ancak, özel anahtarınıza yalnızca siz (veya yerel güvenlik altyapınız) sahip olursunuz.  SSH özel anahtarının koruma amaçlı olarak bir [parolası](https://www.xkcd.com/936/) olabilir.  Bu parola yalnızca özel SSH anahtarına erişim için kullanılır ve kullanıcı hesabı parolası **değildir**.  SSH anahtarınıza parola eklediğinizde bu parola özel anahtarı şifreler; böylece özel anahtar, kilidinin açılması için parola olmadan da kullanılabilir.  Bir saldırganın özel anahtarınızı çalması ve özel anahtarınızın bir parolasının olmaması halinde saldırgan, bu özel anahtarı ilgili ortak anahtara sahip sunucularınızda oturum açmak için kullanabilir.  Özel anahtar parola korumalı ise, Azure’daki altyapınız için ek bir güvenlik katmanı sağlayarak, bu saldırgan tarafından kullanılamaz.


Bu makalede, Resource Manager üzerindeki dağıtımlar için önerilen *ssh-rsa* biçimli anahtar dosyaları oluşturulmaktadır.  Hem Klasik hem de Resource Manager dağıtımları için [portalda](https://portal.azure.com) *ssh-rsa* anahtarları gereklidir.


## SSH Anahtarları oluşturma

Azure en az 2048 bit, ssh-rsa biçimli ortak ve özel anahtarlar gerektirir. Anahtarları oluşturmak için `ssh-keygen` kullanın. Bu, bir dizi soru sorduktan sonra özel bir anahtar ve eşleşen bir ortak anahtar yazar. Azure VM oluşturulduğunda ortak anahtar şuraya kopyalanır: `~/.ssh/authorized_keys`.  `~/.ssh/authorized_keys` içindeki SSH anahtarları, bir istemcinin SSH oturum açma bağlantısındaki ilgili özel anahtarla eşleşip eşleşmediğini sınar.


## SSH-keygen’i kullanma

Bu komut, 2048 bit RSA kullanarak parola korumalı (şifrelenmiş) bir SSH Anahtar Çifti oluşturur; kolayca anlaşılabilmesi için komuta açıklama eklenir.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
```

_Komut açıklaması_

`ssh-keygen` = anahtarları oluşturmak için kullanılan program

`-t rsa` = olan, oluşturulacak anahtar türü [RSA biçimi](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = anahtar bitleri

`-C "ahmet@fedoraVMAzure"` = kolayca tanımlamak için ortak anahtar dosyasının sonuna eklenen bir açıklama.  Normalde açıklama olarak bir e-posta kullanılır ancak altyapınız için en uygun seçenek hangisiyse onu kullanabilirsiniz.

### PEM anahtarlarını kullanma

Klasik dağıtım modelini kullanıyorsanız (Klasik Azure Portalı veya Azure Hizmet yönetimi CLI'si `asm`) Linux VM'lerinize erişmek için PEM biçimli SSH anahtarları kullanmanız gerekebilir.  Var olan bir SSH Ortak anahtarından ve x509 sertifikasından nasıl PEM anahtarı oluşturacağınızı burada öğrenebilirsiniz.

Var olan bir SSH ortak anahtarından PEM biçimli bir anahtar oluşturmak için:

```bash
ssh-keygen -f id_rsa.pub -m 'PEM' -e > id_rsa.pem
```

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

Bu makale için anahtar çifti adı.  Anahtar çiftine **id_rsa** adı verilmesi varsayılandır ve bazı araçlar **id_rsa** özel anahtar adı bekleyebilir, bu nedenle bir tane olması iyi bir fikirdir. `~/.ssh/` dizini, SSH anahtar çiftleri ve SSH yapılandırma dosyası için varsayılan konumdur.

```bash
ahmet@fedora$ ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 azure_fedora_id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 azure_fedora_id_rsa.pub
```
`~/.ssh` dizininin listesi. `ssh-keygen` , yoksa, `~/.ssh` dizinin oluşturur ve ayrıca doğru sahipliği ve dosya modlarını ayarlar.

Anahtar Parolası:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` "metin biçiminde" bir parola anlamına gelir.  Anahtar çiftlerinize bir parola eklemeniz *önemle* önerilir. Anahtar çiftini koruyan bir parola olmadığında, özel anahtar dosyasına sahip herkes bunu ilgili ortak anahtara sahip herhangi bir sunucuda oturum açmak için kullanabilir. Bir parola eklemek, birinin özel anahtar dosyanıza erişim sağlama ihtimaline karşı, kimliğinizi doğrulamak için kullanılan anahtarları değiştirmeniz için size zaman tanıyarak daha fazla koruma sağlar.

## Özel anahtar parolanızı depolamak için ssh-agent kullanma

Her SSH oturum açma işleminde özel anahtar dosya parolanızı yazmamak için `ssh-agent` kullanarak özel anahtar dosya parolanızı önbelleğe alabilirsiniz. Mac kullanıyorsanız `ssh-agent` öğesini çağırdığınızda OSX Anahtar Zinciri, özel anahtar parolalarını güvenli bir şekilde depolar.

Önce `ssh-agent`’in çalıştığını doğrulayın

```bash
eval "$(ssh-agent -s)"
```

Şimdi `ssh-add` komutunu kullanarak özel anahtarı `ssh-agent` öğesine ekleyin.

```bash
ssh-add ~/.ssh/azure_fedora_id_rsa
```

Özel anahtar parolası artık `ssh-agent` içinde depolanmış olur.

## SSH yapılandırma dosyası oluşturma ve yapılandırma

Oturum açma işlemlerini hızlandırmak ve SSH istemci davranışınızı iyileştirmek için en iyi uygulama olarak bir `~/.ssh/config` dosyası oluşturmanız ve yapılandırmanız önerilir.

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
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Bu SSH yapılandırması, size her sunucu için bölümler sunarak her birinin kendi özel anahtar çiftinin olmasını sağlar. Varsayılan ayarlar (`Host *`), üst düzey yapılandırma dosyasındaki belirli ana bilgisayarların hiçbiriyle eşleşmeyen tüm ana bilgisayarlar içindir.


### Yapılandırma dosyası açıklaması

`Host` = terminal üzerinde çağrılan ana bilgisayar adı.  `ssh fedora22` , `SSH`’ye `Host fedora22` blok etiketli ayarlardaki değerleri kullanmasını söyler. NOT: Bu kullanımınız için mantıklı olan ve bir sunucunun gerçek ana bilgisayar adını temsil etmeyen her etiket olabilir.

`Hostname 102.160.203.241` = erişim sağlanan sunucuya ilişkin IP adresi veya DNS adı.

`User git` = kullanılacak uzak kullanıcı hesabı.

`PubKeyAuthentication yes` = SSH'ye oturum açmak için bir SSH anahtarı kullanmak istediğinizi iletir.

`IdentityFile /home/ahmet/.ssh/id_id_rsa` = kimlik doğrulaması için kullanılacak SSH özel anahtarı ve ilgili ortak anahtar.


## Parolasız Linux içine SSH

Artık bir SSH anahtar çiftiniz ve yapılandırılmış bir SSH yapılandırma dosyasına sahip olduğunuza göre Linux VM'nizde hızlı ve güvenli şekilde oturum açabilirsiniz. SSH anahtarı kullanarak bir sunucuda ilk oturum açışınızda, komut sizden bu anahtar dosyasına ilişkin parolayı ister.

```bash
ssh fedora22
```

### Komut açıklaması

`ssh fedora22` yürütüldüğünde, SSH önce `Host fedora22` bloğundaki tüm ayarları bulur ve yükler ve sonra son blok olan `Host *` içindeki kalan tüm ayarları yükler.

## Sonraki Adımlar

Sonraki adım, yeni SSH ortak anahtarını kullanarak Azure Linux VM’ler oluşturmaktır.  Oturum açma işlemi için SSH ortak anahtarıyla oluşturulan Azure VM'ler, varsayılan oturum açma yöntemiyle (parolalar) oluşturulan VM'lere göre daha güvenlidir.  SSH anahtarları kullanılarak oluşturulan Azure VM'ler, varsayılan olarak parola kullanımı devre dışı bırakılmış şekilde yapılandırılır; böylece parola tahminiyle gerçekleştirilen saldırı girişimleri önlenir.

- [Azure şablonu kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Azure portalını kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-quick-create-portal.md)
- [Azure CLI kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-quick-create-cli.md)



<!--HONumber=Aug16_HO4-->


