---
title: "Azure Service Fabric CLI’sı (sfctl) ile çalışmaya başlama"
description: "Azure Service Fabric CLI’sını kullanmayı öğrenin. Kümeye bağlanmayı ve uygulamaları yönetmeyi öğrenin."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 2faca2887f25b45d833dea7b2259277466290670
ms.contentlocale: tr-tr
ms.lasthandoff: 08/29/2017

---
# <a name="azure-service-fabric-command-line"></a>Azure Service Fabric komut satırı

Azure Service Fabric CLI’sı (sfctl) Azure Service Fabric varlıklarıyla etkileşimde bulunmak ve bunları yönetmek için kullanılan bir komut satırı yardımcı programıdır. Sfctl, Windows veya Linux kümeleriyle kullanılabilir. Sfctl python’ın desteklendiği tüm platformlarda çalışır.

## <a name="prerequisites"></a>Ön koşullar

Yüklemeden önce ortamınızda python ve PIP’in yüklü olduğundan emin olun. Daha fazla bilgi için [PIP hızlı başlangıç belgelerine](https://pip.pypa.io/en/latest/quickstart/) ve resmi [python yükleme belgelerine](https://wiki.python.org/moin/BeginnersGuide/Download) göz atın.

Hem python 2.7 hem de 3.6 desteklenir, ancak python 3.6 kullanmanız önerilir. Aşağıdaki bölümde, tüm önkoşulların ve CLI’nin nasıl yükleneceği gösterilir.

## <a name="install-pip-python-and-sfctl"></a>pip, python ve sfctl yükleme

pip ve python’u platformunuza yüklemek için birçok yöntem olsa da, ana işletim sistemleri için python 3.6 ve pip’i hızlıca ayarlamak için bazı adımlar şunlardır:

### <a name="windows"></a>Windows

Windows 10, Server 2016 ve Server 2012R2 için standart resmi yükleme yönergelerini kullanabilirsiniz. Python yükleyici pip’i de varsayılan olarak yükler.

- Resmi [python indirmeler sayfasına](https://www.python.org/downloads/) gidin ve python 3.6’nın en son sürümünü indirin
- Yükleyiciyi başlatın
- `Add Python 3.6 to PATH` için komut isteminin alt kısmındaki seçeneğini belirleyin
- `Install Now` seçeneğini belirleyin
- Yüklemeyi tamamlayın

Artık yeni bir komut penceresi açabiliyor, python ve PIP sürümlerini alabiliyor olmanız gerekir:

```bat
python --version
pip --version
```

Ardından Service Fabric CLI’yı yüklemek için aşağıdakini çalıştırın

```
pip install sfctl
sfctl -h
```

### <a name="ubuntu"></a>Ubuntu

Ubuntu 16.04 Desktop için üçüncü taraf bir PPA kullanarak python 3.6’yı yükleyebilirsiniz:

Terminalden aşağıdaki komutları çalıştırın:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Ardından aşağıdaki komutu çalıştırarak yalnızca sizin python 3.6 yüklemeniz için sfctl’yi yükleyin:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

Bu adımlar, sistem tarafından yüklenen python 3.5 ve 2.7 sürümlerini etkilemez. Ubuntu kullanmaya alışık değilseniz bu yüklemeleri değiştirmeyi denemeyin.

### <a name="macos"></a>macOS

MacOS için [HomeBrew paket yöneticisini](https://brew.sh) kullanmanız önerilir. HomeBrew yüklü değilse aşağıdaki komutu çalıştırarak yükleyin:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Ardından terminalden python 3.6, pip ve sfctl’yi yükleyin

```bash
brew install python3
pip3 install sfctl
sfctl -h
```

Bu adımlar sistem tarafından yüklenen python 2.7 sürümünü değiştirmez.

## <a name="cli-syntax"></a>CLI söz dizimi

Komutlarda her zaman `sfctl` ön eki bulunur. Kullanabileceğiniz tüm komutlarla ilgili genel bilgi için, `sfctl -h` kullanın. Tek bir komutla ilgili yardım için, `sfctl <command> -h` kullanın.

Komutlar, komut hedefinin fiilin veya eylemin öncesinde olduğu yinelenebilir bir yapıya sahiptir:

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

Güvenliği bir sertifika ile sağlanan kümeler için PEM kodlu bir sertifika belirtebilirsiniz. Sertifika, tek bir dosya veya sertifika ve anahtar çifti olarak belirtilebilir.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Daha fazla bilgi için bkz. [Güvenli Azure Service Fabric kümesine bağlanma](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Temel işlemler

Küme bağlantı bilgileri birden çok sfctl oturumu arasında kalıcıdır. Service Fabric kümesini seçtikten sonra, kümede tüm Service Fabric komutlarını çalıştırabilirsiniz.

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

Bazı öneriler ve sık karşılaşılan sorunları çözmek için ipuçları.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Sertifikayı PFX’ten PEM biçimine dönüştürme

Service Fabric CLI’sı de PEM (.pem uzantısı) dosyaları gibi istemci tarafı sertifikalarını destekler. Windows'dan PFX dosyalarını kullanıyorsanız, söz konusu sertifikaları PEM biçimine dönüştürmelisiniz. PFX dosyasını PEM dosyasına dönüştürmek için aşağıdaki komutu kullanın:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Daha fazla bilgi için bkz. [OpenSSL belgeleri](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Bağlantı sorunları

Bazı işlemler aşağıdaki iletiyi oluşturabilir:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Belirtilen küme uç noktasının kullanılabilir olduğunu ve dinlediğini doğrulayın. Ayrıca söz konusu konakta ve bağlantı noktasında Service Fabric Explorer Kullanıcı Arabiriminin kullanılabilir olduğunu da doğrulayın. Uç noktayı güncelleştirmek için `sfctl cluster select` kullanın.

### <a name="detailed-logs"></a>Ayrıntılı günlükler

Hata ayıkladığınız veya sorun bildirdiğiniz sırada ayrıntılı günlükler çoğunlukla yararlı olur. Günlük dosyalarının ayrıntı düzeyini artıran genel bir `--debug` bayrağı vardır.

### <a name="command-help-and-syntax"></a>Komut yardımı ve söz dizimi

Belirli bir komut veya komut grubuyla ilgili yardım almak için `-h` bayrağını kullanın:

```azurecli
sfctl application -h
```

Bir örnek daha:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Sonraki adımlar

* [Bir uygulamayı Azure Service Fabric CLI ile dağıtma](service-fabric-application-lifecycle-sfctl.md)
* [Linux’ta Service Fabric kullanmaya başlama](service-fabric-get-started-linux.md)

