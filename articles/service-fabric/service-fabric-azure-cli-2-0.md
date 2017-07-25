---
title: "Azure Service Fabric ve Azure CLI 2.0 ile çalışmaya başlama"
description: "Azure CLI, sürüm 2.0'da Azure Service Fabric komutları modülünü kullanmayı öğrenin. Kümeye bağlanmayı ve uygulamaları yönetmeyi öğrenin."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: ee3302b984ca2f5509755dc17b0a5fd06ace0afe
ms.contentlocale: tr-tr
ms.lasthandoff: 07/14/2017

---
# <a name="azure-service-fabric-and-azure-cli-20"></a>Azure Service Fabric ve Azure CLI 2.0

Azure komut satırı aracı (Azure CLI) sürüm 2.0, Azure Service Fabric kümelerini yönetmenize yardımcı olan komutlar içerir. Azure CLI ve Service Fabric ile çalışmaya başlamayı öğrenin.

## <a name="install-azure-cli-20"></a>Azure CLI 2.0’ı yükleme

Azure CLI 2.0 komutlarını kullanarak Service Fabric kümeleriyle etkileşim kurabilir ve bu kümeleri yönetebilirsiniz. Azure CLI'nin en son sürümünü almak için [Azure CLI 2.0 standart yükleme işlemini](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) izleyin.

Daha fazla bilgi için bkz. [Azure CLI 2.0 aracına genel bakış](https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="azure-cli-syntax"></a>Azure CLI söz dizimi

Azure CLI'de, tüm Service Fabric komutlarının `az sf` ön eki vardır. Kullanabileceğiniz komutlarla ilgili genel bilgi için, `az sf -h` kullanın. Tek bir komutla ilgili yardım için, `az sf <command> -h` kullanın.

Azure CLI’de Service Fabric komutları şu adlandırma desenini izler:

```azurecli
az sf <object> <action>
```

`<object>`, `<action>` hedefidir.

## <a name="select-a-cluster"></a>Küme seçme

Herhangi bir işlem gerçekleştirmeden önce bağlantı kurulacak bir küme seçmeniz gerekir. Örneğin, aşağıdaki kodu bakın. Kod güvenli olmayan bir kümeye bağlanır.

> [!WARNING]
> Üretim ortamında güvenli olmayan Service Fabric kümelerini kullanmayın.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Küme uç noktasının `http` veya `https` ön eki olmalıdır. HTTP ağ geçidi için bağlantı noktasını içermelidir. Bağlantı noktası ve adres, Service Fabric Explorer URL'si ile aynıdır.

Sertifikayla güvenliği sağlanan kümelerde, şifrelenmemiş .pem dosyaları veya .crt ve .key dosyaları kullanabilirsiniz. Örneğin:

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Daha fazla bilgi için bkz. [Güvenli Azure Service Fabric kümesine bağlanma](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> `select` komutu, döndürülmeden önce hiçbir istek üzerinde işlem yapmaz. Kümeyi doğru belirttiğinizden emin olmak için, `az sf cluster health` gibi bir komut kullanın. Komutun herhangi bir hata döndürmediğini doğrulayın.

## <a name="basic-operations"></a>Temel işlemler

Küme bağlantı bilgileri birden çok Azure CLI oturumu arasında kalıcıdır. Service Fabric kümesini seçtikten sonra, kümede tüm Service Fabric komutlarını çalıştırabilirsiniz.

Örneğin, Service Fabric kümesinin sistem durumunu almak için aşağıdaki komutu kullanın:

```azurecli
az sf cluster health
```

Komut sonuçta şu çıkışı oluşturur (JSON çıkışının Azure CLI yapılandırmasında belirtildiği varsayılarak):

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

Azure CLI'de Service Fabric komutlarını kullanırken sorunlarla karşılaşıyorsanız, aşağıdaki bilgileri yararlı bulabilirsiniz.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Sertifikayı PFX’ten PEM biçimine dönüştürme

Azure CLI de PEM (.pem uzantısı) dosyaları gibi istemci tarafı sertifikalarını destekler. Windows'dan PFX dosyalarını kullanıyorsanız, söz konusu sertifikaları PEM biçimine dönüştürmelisiniz. PFX dosyasını PEM dosyasına dönüştürmek için aşağıdaki komutu kullanın:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Daha fazla bilgi için bkz. [OpenSSL belgeleri](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Bağlantı sorunları

Bazı işlemler aşağıdaki iletiyi oluşturabilir:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Belirtilen küme uç noktasının kullanılabilir olduğunu ve dinlediğini doğrulayın. Ayrıca söz konusu konakta ve bağlantı noktasında Service Fabric Explorer Kullanıcı Arabiriminin kullanılabilir olduğunu da doğrulayın. Uç noktayı güncelleştirmek için `az sf cluster select` kullanın.

### <a name="detailed-logs"></a>Ayrıntılı günlükler

Hata ayıkladığınız veya sorun bildirdiğiniz sırada ayrıntılı günlükler çoğunlukla yararlı olur. Azure CLI, günlük dosyalarının ayrıntı düzeyini artıran genel bir `--debug` bayrağı sunar.

### <a name="command-help-and-syntax"></a>Komut yardımı ve söz dizimi

Service Fabric komutları, Azure CLI ile aynı kuralları izler. Belirli bir komut veya komut grubuyla ilgili yardım almak için `-h` bayrağını kullanın:

```azurecli
az sf application -h
```

Bir örnek daha:

```azurecli
az sf application create -h
```

