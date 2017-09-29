---
title: "Azure Service Fabric CLI'sı ile çalışmaya başlama"
description: "Azure Service Fabric CLI’sını kullanmayı öğrenin. Kümeye bağlanmayı ve uygulamaları yönetmeyi öğrenin."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: f246ee8aaecf3a398182debdea07832c75c1bd9c
ms.contentlocale: tr-tr
ms.lasthandoff: 09/22/2017

---
# <a name="azure-service-fabric-cli"></a>Azure Service Fabric CLI'sı

Azure Service Fabric komut satırı arabirimi (CLI) Service Fabric varlıklarıyla etkileşimde bulunmak ve bunları yönetmek için kullanılan bir komut satırı yardımcı programıdır. Service Fabric CLI'sı, Windows veya Linux kümeleriyle kullanılabilir. Service Fabric CLI'sı Python'ın desteklendiği tüm platformlarda çalışır.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-sfctl.md)]

## <a name="prerequisites"></a>Ön koşullar

Yüklemeden önce ortamınızda Python ve pip uygulamalarının yüklü olduğundan emin olun. Daha fazla bilgi için [pip hızlı başlangıç belgelerine](https://pip.pypa.io/en/latest/quickstart/) ve resmi [Python yükleme belgelerine](https://wiki.python.org/moin/BeginnersGuide/Download) bakın.

Python uygulamasının hem 2.7 hem de 3.6 sürümü desteklenir ancak Python 3.6 sürümünü kullanmanızı öneririz. Aşağıdaki bölümde, tüm önkoşulların ve CLI'nin nasıl yükleneceği gösterilmektedir.

## <a name="install-pip-python-and-the-service-fabric-cli"></a>pip, Python ve Service Fabric CLI'sını yükleme

 pip ve Python'u platformunuza yüklemek için kullanabileceğiniz birçok yol vardır. Çok kullanılan işletim sistemlerinde Python 3.6 ve pip uygulamalarını hızlıca kurmak için aşağıdaki adımlardan faydalanabilirsiniz.

### <a name="windows"></a>Windows

Windows 10, Windows Server 2016 ve Windows Server 2012 R2 için standart resmi yükleme talimatlarını kullanın. Python yükleyici pip'i de varsayılan olarak yükler.

1. Resmi [Python indirmeler sayfasına](https://www.python.org/downloads/) gidin ve Python 3.6'nın en son sürümünü indirin.

2. Yükleyiciyi başlatın.

3. İstemin en altında bulunan **Python 3.6'yı PATH'e ekle**'yi seçin.

4. **Şimdi Yükle**'yi seçin ve yüklemeyi tamamlayın.

Şimdi yeni bir komut penceresi açıp hem Python hem de pip sürümünü öğrenebilirsiniz.

```bat
python --version
pip --version
```

Ardından aşağıdaki komutu çalıştırarak Service Fabric CLI'sını yükleyin:

```
pip install sfctl
sfctl -h
```

`sfctl` öğesinin bulunamadığını belirten bir hatayla karşılaşırsanız, aşağıdaki komutları çalıştırın:

```bash
export PATH=$PATH:~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> .bashrc
```

### <a name="ubuntu"></a>Ubuntu

Ubuntu 16.04 Desktop için üçüncü taraf bir kişisel paket arşivini (PPA) kullanarak Python 3.6'yı yükleyebilirsiniz.

Terminalden aşağıdaki komutları çalıştırın:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Ardından aşağıdaki komutu çalıştırarak yalnızca sizin Python 3.6 yüklemeniz için Service Fabric CLI'sını yükleyin:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

`sfctl` öğesinin bulunamadığını belirten bir hatayla karşılaşırsanız, aşağıdaki komutları çalıştırın:

```bash
export PATH=$PATH:~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> .bashrc
```

Bu adımlar, sistem tarafından yüklenen Python 3.5 ve 2.7 sürümlerini etkilemez. Ubuntu kullanmaya alışık değilseniz bu yüklemeleri değiştirmeyi denemeyin.

### <a name="macos"></a>macOS

MacOS için [HomeBrew paket yöneticisini](https://brew.sh) kullanmanızı öneririz. HomeBrew yüklü değilse aşağıdaki komutu çalıştırarak yükleyin:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Ardından aşağıdaki komutları çalıştırarak terminalden Python 3.6, pip ve Service Fabric CLI'sını yükleyin:

```bash
brew install python3
pip3 install sfctl
sfctl -h
```


`sfctl` öğesinin bulunamadığını belirten bir hatayla karşılaşırsanız, aşağıdaki komutları çalıştırın:

```bash
export PATH=$PATH:~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> .bashrc
```


Bu adımlar sistem tarafından yüklenen Python 2.7 sürümünü değiştirmez.

## <a name="cli-syntax"></a>CLI söz dizimi

Komutlarda her zaman `sfctl` ön eki bulunur. Kullanabileceğiniz tüm komutlarla ilgili genel bilgi için `sfctl -h` kullanın. Tek bir komutla ilgili yardım için, `sfctl <command> -h` kullanın.

Komutlar, komut hedefinin fiilin veya eylemin öncesinde olduğu yinelenebilir bir yapıya sahiptir.

```azurecli
sfctl <object> <action>
```

Bu örnekte, `<action>` için `<object>` hedeftir.

## <a name="select-a-cluster"></a>Küme seçme

Herhangi bir işlem gerçekleştirmeden önce bağlantı kurulacak bir küme seçmeniz gerekir. Örneğin, `testcluster.com` adına sahip bir kümeyi seçip bu kümeye bağlanmak için aşağıdaki komutu çalıştırın:

> [!WARNING]
> Üretim ortamında güvenli olmayan Service Fabric kümelerini kullanmayın.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Küme uç noktasının `http` veya `https` ön eki olmalıdır. HTTP ağ geçidi için bağlantı noktasını içermelidir. Bağlantı noktası ve adres, Service Fabric Explorer URL'si ile aynıdır.

Güvenliği bir sertifika ile sağlanan kümeler için PEM kodlu bir sertifika belirtebilirsiniz. Sertifika, tek bir dosya veya sertifika ve anahtar çifti olarak belirtilebilir. CA imzalı olmayan bir otomatik olarak imzalanan sertifika ise, CA doğrulamasını atlamak için `--no-verify` seçeneğini geçebilirsiniz.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Daha fazla bilgi için bkz. [Güvenli Azure Service Fabric kümesine bağlanma](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Temel işlemler

Küme bağlantı bilgileri birden çok Service Fabric CLI'sı oturumu arasında kalıcıdır. Service Fabric kümesini seçtikten sonra, kümede tüm Service Fabric komutlarını çalıştırabilirsiniz.

Örneğin, Service Fabric kümesinin sistem durumunu almak için aşağıdaki komutu kullanın:

```azurecli
sfctl cluster health
```

Komut şu çıktıyı verir:

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a>İpuçları ve sorun giderme

Aşağıda bazı öneriler ve sık karşılaşılan sorunları çözmek için ipuçları verilmiştir.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Sertifikayı PFX’ten PEM biçimine dönüştürme

Service Fabric CLI’sı de PEM (.pem uzantısı) dosyaları gibi istemci tarafı sertifikalarını destekler. Windows'dan PFX dosyalarını kullanıyorsanız, söz konusu sertifikaları PEM biçimine dönüştürmelisiniz. PFX dosyasını PEM dosyasına dönüştürmek için aşağıdaki komutu kullanın:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Benzer şekilde, PEM dosyasından PFX dosyasına dönüştürmek için de aşağıdaki komutu kullanabilirsiniz (burada parola sağlanmaz):

```bash
openssl  pkcs12 -export -out Certificates.pfx -inkey Certificates.pem -in Certificates.pem -passout pass:'' 
```

Daha fazla bilgi için bkz. [OpenSSL belgeleri](https://www.openssl.org/docs/).

### <a name="connection-problems"></a>Bağlantı sorunları

Bazı işlemler aşağıdaki iletiyi oluşturabilir:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Belirtilen küme uç noktasının kullanılabilir olduğunu ve dinlediğini doğrulayın. Ayrıca söz konusu konakta ve bağlantı noktasında Service Fabric Explorer Kullanıcı Arabiriminin kullanılabilir olduğunu da doğrulayın. Uç noktayı güncelleştirmek için `sfctl cluster select` kullanın.

### <a name="detailed-logs"></a>Ayrıntılı günlükler

Hata ayıkladığınız veya sorun bildirdiğiniz sırada ayrıntılı günlükler çoğunlukla yararlı olur. Genel bir `--debug` bayrağı, günlük dosyalarının ayrıntı düzeyini artırır.

### <a name="command-help-and-syntax"></a>Komut yardımı ve söz dizimi

Belirli bir komut veya komut grubuyla ilgili yardım almak için `-h` bayrağını kullanın.

```azurecli
sfctl application -h
```

Bir örnek daha:

```azurecli
sfctl application create -h
```

## <a name="updating-the-service-fabric-cli"></a>Service Fabric CLI’sını güncelleştirme 

Service Fabric CLI’yı güncelleştirmek için aşağıdaki komutları çalıştırın (özgün yüklemenizde belirlediğiniz seçeneklere bağlı olarak `pip` öğesini `pip3` olarak değiştirin):

```bash
pip uninstall sfctl 
pip install sfctl 
```


## <a name="next-steps"></a>Sonraki adımlar

* [Bir uygulamayı Azure Service Fabric CLI ile dağıtma](service-fabric-application-lifecycle-sfctl.md)
* [Linux’ta Service Fabric kullanmaya başlama](service-fabric-get-started-linux.md)

