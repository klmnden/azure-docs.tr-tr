---
title: Azure Machine Learning işlem (MLC) kümesinde SSL'i etkinleştirin | Microsoft Docs
description: Azure Machine Learning işlem (MLC) kümesinde Puanlama çağrıları için SSL ayarlama ayarlamaya yönelik yönergeler alın
services: machine-learning
author: SerinaKaye
ms.author: serinak
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 2a7733468ec082c8954f623f3ebe2cea1fbad561
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976246"
---
# <a name="enable-ssl-on-an-azure-machine-learning-compute-mlc-cluster"></a>Azure Machine Learning işlem (MLC) kümesinde SSL'i etkinleştirin 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Bu yönergeler için Puanlama çağrıları bir Machine Learning işlem (MLC) kümesinde SSL ayarlama ayarlamanıza olanak sağlar. 

## <a name="prerequisites"></a>Önkoşullar 

1. Gerçek zamanlı Puanlama çağrıları yapılırken kullanılacak bir CNAME (DNS adı) karar verin.

2. Oluşturma bir *parola ücretsiz* bu CNAME sertifikasıyla.

3. Sertifika PEM biçiminde değilse, dönüştürülebilmesi için PEM gibi araçları kullanarak *openssl*.

Önkoşulları tamamladıktan sonra iki dosya gerekir:

* Bu gibi bir durumda sertifikası için bir dosya `cert.pem`. Dosyanın tam sertifika zincirine sahip olduğundan emin olun.
* Bir dosya için anahtar, örneğin `key.pem`



## <a name="set-up-an-ssl-certificate-on-a-new-acs-cluster"></a>Yeni bir ACS kümesi üzerinde bir SSL sertifikası ayarlama

Azure Machine Learning CLI kullanarak aşağıdaki komutla çalıştırmak `-c` anahtara bağlı bir SSL sertifikası ile bir ACS kümesi oluşturmak için:

```
az ml env create -c -g <resource group name> -n <cluster name> --cert-cname <CNAME> --cert-pem <path to cert.pem file> --key-pem <path to key.pem file>
```


## <a name="set-up-an-ssl-certificate-on-an-existing-acs-cluster"></a>Var olan bir ACS kümesi üzerinde bir SSL sertifikası ayarlama

SSL oluşturulan bir küme hedefliyorsanız, Azure PowerShell cmdlet'lerini kullanarak bir sertifika ekleyebilirsiniz.

Sertifika ham PEM biçiminde ve anahtarı sağlamanız gerekir. Bu PowerShell değişkenlere okuyabilirsiniz:

```
$keyValueInPemFormat = [IO.File]::ReadAllText('<path to key.pem file>')
$certValueInPemFormat = [IO.File]::ReadAllText('<path to cert.pem file>')
```
Sertifika kümeye ekleyin: 

```
Set-AzureRmMlOpCluster -ResourceGroupName my-rg -Name my-cluster -SslStatus Enabled -SslCertificate $certValueInPemFormat -SslKey $keyValueInPemFormat -SslCName foo.mycompany.com
```

## <a name="map-the-cname-and-the-ip-address"></a>CNAME ve IP adresini Eşle

Önkoşullarda seçtiğiniz CNAME ve gerçek zamanlı ön uç (FE) IP adresi arasında bir eşleme oluşturun. FE IP adresini bulmak için aşağıdaki komutu çalıştırın. Gerçek zamanlı küme ön uç IP adresini içeren "Publicıpaddress" adlı bir alan çıktıyı görüntüler. Bir kaydı CNAME genel IP adresi için kullanılan FQDN ayarlanacak DNS sağlayıcınızın yönergelerine bakın.



## <a name="auto-detection-of-a-certificate"></a>Bir sertifika otomatik algılama 

Gerçek zamanlı web hizmetini kullanarak oluştururken "`az ml service create realtime`" komutu, Azure Machine Learning, küme üzerinde ayarlanmış SSL otomatik olarak algılar ve otomatik olarak belirlenen sertifikasıyla Puanlama URL'sini ayarlar. 

Sertifika durumunu görmek için aşağıdaki komutu çalıştırın. URI ve CNAME puanlamaların ana bilgisayar adı "https" ön ekini dikkat edin. 

``` 
az ml service show realtime -n <service name>
{
                "id" : "<your service id>",
                "name" : "<your service name >",
                "state" : "Succeeded",
                "createdAt" : "<datetime>”,
                "updatedAt" : "< datetime>",
                "scoringUri" : "https://<your CNAME>/api/v1/service/<service name>/score"
}
```
