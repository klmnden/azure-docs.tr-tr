---
title: Bir Azure Machine Learning işlem (CBE) kümede SSL'yi | Microsoft Docs
description: Bir Azure Machine Learning işlem (CBE) kümede çağrıları Puanlama için SSL kurma ayarlama yönergeleri alır
services: machine-learning
author: SerinaKaye
ms.author: serinak
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ms.openlocfilehash: 444803eeb77e79a8bfe3271ddf27bd428042c875
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="enable-ssl-on-an-azure-machine-learning-compute-mlc-cluster"></a>Bir Azure Machine Learning işlem (CBE) kümede SSL etkinleştir 

Bu yönergeler, Machine Learning işlem (CBE) kümede çağrıları Puanlama için SSL kurma ayarlamanıza olanak tanır. 

## <a name="prerequisites"></a>Önkoşullar 

1. Gerçek zamanlı Puanlama çağrıları yapılırken kullanılacak bir CNAME (DNS adı) karar verin.

2. Oluşturma bir *parolasını serbest* bu CNAME sertifikayla.

3. Sertifika PEM biçiminde değilse, gibi araçları kullanılarak PEM Dönüştür *openssl*.

Önkoşulları tamamladıktan sonra iki dosya gerekir:

* Bu gibi bir durumda sertifikası için bir dosya `cert.pem`. Dosyanın tam sertifika zinciri olduğundan emin olun.
* Bir dosya için anahtar, örneğin `key.pem`



## <a name="set-up-an-ssl-certificate-on-a-new-acs-cluster"></a>Yeni bir ACS kümedeki bir SSL sertifikası ayarlama

Azure Machine Learning CLI kullanarak ile aşağıdaki komutu çalıştırın `-c` anahtara bağlı bir SSL sertifikası ile bir ACS küme oluşturmak için:

```
az ml env create -c -g <resource group name> -n <cluster name> --cert-cname <CNAME> --cert-pem <path to cert.pem file> --key-pem <path to key.pem file>
```


## <a name="set-up-an-ssl-certificate-on-an-existing-acs-cluster"></a>Bir SSL sertifikası var olan bir ACS kümede ayarlama

SSL oluşturulan bir küme hedefliyorsanız, Azure PowerShell cmdlet'lerini kullanarak bir sertifika ekleyebilirsiniz: 

```
Set-AzureRmMlOpCluster -ResourceGroupName my-rg -Name my-cluster -SslStatus Enabled -SslCertificate $certValueInPemFormat -SslKey $keyValueInPemFormat -SslCName foo.mycompany.com
```

## <a name="map-the-cname-and-the-ip-address"></a>CNAME ve IP adresi eşleme

Önkoşullar seçili CNAME ve gerçek zamanlı ön uç (FE) IP adresi arasında bir eşleme oluşturun. FE IP adresini bulmak için aşağıdaki komutu çalıştırın. Çıktıyı "ön uç gerçek zamanlı küme IP adresini içeren Publicıpaddress" adlı bir alan görüntüler. Genel IP adresi CNAME içinde kullanılan FQDN bir kaydı kümeden için DNS sağlayıcınız yönergelerine bakın.



## <a name="auto-detection-of-a-certificate"></a>Bir sertifika otomatik algılama 

Gerçek zamanlı web hizmetini kullanarak oluşturduğunuzda "`az ml service create realtime`" komutu, Azure Machine Learning kümedeki ayarlamak SSL otomatik olarak algılar ve otomatik olarak belirlenen sertifikayla Puanlama URL'sini ayarlar. 

Sertifika durumunu görmek için aşağıdaki komutu çalıştırın. URI ve CNAME Puanlama ana bilgisayar adı, "https" önekini dikkat edin. 

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
