---
title: "Service Fabric ve Azure CLI 2.0’ı kullanmaya başlama"
description: "Azure CLI sürüm 2.0’da Service Fabric komutları modülünü kullanma adımları, kümeye bağlanma ve uygulamaları yönetme işlemlerini içerir"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: c5cc6e54acf27456185eeb48858c4d981aa46b4b
ms.contentlocale: tr-tr
ms.lasthandoff: 07/01/2017

---
<a id="service-fabric-and-azure-cli-20" class="xliff"></a>

# Service Fabric ve Azure CLI 2.0

Yeni Azure CLI 2.0 artık Service Fabric kümelerini yönetmeye yönelik komutlar içermektedir. Bu belgede Azure CLI ile çalışmaya başlama adımları verilmektedir.

<a id="install-azure-cli-20" class="xliff"></a>

## Azure CLI 2.0’ı yükleme

Azure CLI artık Service Fabric kümeleri ile etkileşimde bulunma ve kümeleri yönetmeye yönelik komutlar içermektedir. En yeni Azure CLI sürümünü edinmek için [standart yükleme işlemini](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) izleyebilirsiniz.

Daha fazla bilgi için [Azure CLI 2.0 belgelerine](https://docs.microsoft.com/en-us/cli/azure/overview) bakın

<a id="cli-syntax" class="xliff"></a>

## CLI söz dizimi

Tüm Azure Service Fabric komutları Azure CLI’da `az sf` ön ekini alır. Kullanılabilir komutlar hakkında daha fazla bilgi için `az sf -h` komutunu çalıştırarak genel bilgi alabilirsiniz. Ya da tek bir komut hakkında ayrıntılı yardım için `az sf <command> -h` komutunu çalıştırabilirsiniz.

CLI’da Azure Service Fabric komutları bir adlandırma desenini izler

```azurecli
az sf <object> <action>
```

Burada `<object>`, `<action>` hedefidir.

<a id="selecting-a-cluster" class="xliff"></a>

## Küme seçme

Herhangi bir işlem gerçekleştirmeden önce bağlantı kurulacak bir küme seçmeniz gerekir. Örneğin, güvenli olmayan bir kümeye bağlanmak için aşağıdaki kod parçacığına bakın.

> [!WARNING]
> Güvenli olmayan Service Fabric kümelerini üretim ortamları için kullanmayın

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Küme uç noktasına `http` veya `https` ön eki getirilmeli ve HTTP ağ geçidinin bağlantı noktası dahil edilmelidir. Bu bağlantı noktası ve adres, Service Fabric Explorer URL'si ile aynıdır.

Güvenliği sertifika ile sağlanan kümeler için şifrelenmemiş `pem` veya `crt` ile `key` dosyaları desteklenir.

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Daha fazla bilgi için [güvenli kümelere bağlanma ile ilgili ayrıntılı belgelere](service-fabric-connect-to-secure-cluster.md) bakın.

> [!NOTE]
> Select komutu, döndürmeden önce herhangi bir isteği gerçekleştirmez. Bir kümenin doğru şekilde belirtildiğini doğrulamak için, `az sf cluster health` gibi bir komut çalıştırın ve komutun herhangi bir hata döndürüp döndürmediğini denetleyin.

<a id="performing-basic-operations" class="xliff"></a>

## Temel işlemleri gerçekleştirme

Küme bağlantı bilgileri farklı Azure CLI oturumları arasında kalıcıdır. Bir Service Fabric kümesi seçildikten sonra herhangi bir Service Fabric komutunu çalıştırabilirsiniz.

Örneğin, Service Fabric kümesinin sistem durumunu almak için aşağıdaki komutu çalıştırın

```azurecli
az sf cluster health
```

Komut, JSON çıktısının Azure CLI yapılandırmasında belirtildiğini varsayarak aşağıdaki çıktıyı oluşturur

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

<a id="tips-and-faq" class="xliff"></a>

## İpuçları ve SSS

Azure CLI’da Service Fabric komutlarını kullanırken sorun yaşadığınızda yararlı olabilecek bazı bilgiler aşağıda verilmiştir

<a id="converting-a-certificate-from-pfx-to-pem" class="xliff"></a>

### Sertifikayı PFX’ten PEM’ye dönüştürme

Azure CLI, PEM dosyası olarak istemci tarafı sertifikalarını destekler (`.pem` uzantısı). Windows’un PFX dosyalarını kullanıyorsanız, bu sertifikaların PEM biçimine dönüştürülmesi gerekir. Bir PFX dosyasını PEM dosyasına dönüştürmek için aşağıdaki komutu kullanın:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Ayrıntılar için [OpenSSL belgelerine](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) bakın.

<a id="connection-issues" class="xliff"></a>

### Bağlantı sorunları

İşlemleri yaparken aşağıdaki hatayla karşılaşabilirsiniz:

> `Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Bu durumda, belirtilen küme uç noktasının erişilebilir olduğunu ve dinlediğini doğrulayın. Ayrıca ana bilgisayar ve bağlantı noktasında Service Fabric Explorer Kullanıcı Arabiriminin erişilebilir olduğunu doğrulayın. Uç noktayı güncelleştirmek için `az sf cluster select` kullanın.

<a id="getting-detailed-logs" class="xliff"></a>

### Ayrıntılı günlükler alma

Hata ayıklama veya sorun bildirme sırasında ayrıntılı günlüklerin eklenmesi yararlıdır. Azure CLI, günlüklerin ayrıntı düzeyini artıran genel bir `--debug` bayrağı içerir.

<a id="command-help-and-syntax" class="xliff"></a>

### Komut yardımı ve söz dizimi

Service Fabric komutları, Azure CLI ile aynı kuralı izler. Belirli bir komut veya komut grubu hakkında yardım almak için `-h` bayrağını belirtin. Örneğin:

```azurecli
az sf application -h
```

or

```azurecli
az sf application create -h
```

