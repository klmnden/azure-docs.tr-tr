---
title: Bir web uygulaması için özel DNS kayıtlarını oluşturun | Microsoft Docs
description: Azure DNS kullanarak web uygulaması için DNS kayıtlarını özel etki alanı oluşturma
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: kumud
ms.openlocfilehash: 00a56a2683e95e70bb13acd6b936e766f044e1cd
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Özel bir etki alanındaki bir web uygulaması için DNS kayıtlarını oluşturun

Azure DNS, özel bir etki alanı için web uygulamalarınızı barındırmak için kullanabilirsiniz. Örneğin, bir Azure web uygulaması oluşturma ve kullanıcılarınızın ya da erişmek istediğiniz contoso.com ya da www.contoso.com bir FQDN kullanarak.

Bunu yapmak için iki kaydı oluşturmanız gerekir:

* Contoso.com etki alanına işaret eden bir kök "A" kaydı
* A kaydına işaret www adı için bir "CNAME" kaydı

Unutmayın web uygulama değişikliklerini alttaki IP adresi güncelleştirilmiş Azure'da bir web uygulaması için bir A kaydı oluşturursanız, A kaydını el ile olması gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce öncelikle Azure DNS'de bir DNS bölgesi oluşturma ve Azure DNS'ye kayıt içinde bölgeye temsilci.

1. Bir DNS bölgesi oluşturmak için adımları [bir DNS bölgesi oluşturma](dns-getstarted-create-dnszone.md).
2. Azure DNS, DNS temsilci seçmek için adımları [DNS etki alanı temsilcisi](dns-domain-delegation.md).

Bölge oluşturma ve Azure DNS'ye temsilci sonra özel etki alanınız için daha sonra kayıt oluşturabilirsiniz.

## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Özel etki alanınız için bir A kaydı oluşturun

Bir A kaydı bir adı IP adresine eşlemek için kullanılır. Aşağıdaki örnekte biz @ bir IPv4 adresi için bir A kaydı olarak atanmaktadır:

### <a name="step-1"></a>1. Adım

Bir A kaydını oluşturun ve bir değişkene $rs atayın

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a>2. Adım

Önceden oluşturulmuş kayıt kümesine IPv4 değer ekleme "@" atanan $rs değişkenini kullanarak. Atanan IPv4 değerini web uygulamanız için IP adresi olacaktır.

Bir web uygulaması için IP adresini bulmak için adımları [Azure App Service'te özel etki alanı adı yapılandırma](../app-service/app-service-web-tutorial-custom-domain.md).

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a>3. Adım

Kayıt kümesi değişiklikleri. Kullanım `Set-AzureRMDnsRecordSet` değişiklikleri kayıt Azure DNS'ye kümesine karşıya yüklemek için:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Özel etki alanınız için bir CNAME kaydı oluşturun

Etki alanınız zaten Azure DNS tarafından yönetiliyorsa (bkz [DNS etki alanı temsilcisi](dns-domain-delegation.md), aşağıdakileri kullanabilirsiniz örnek contoso.azurewebsites.net için bir CNAME kaydı oluşturun.

### <a name="step-1"></a>1. Adım

PowerShell'i açın ve yeni bir CNAME kayıt kümesi oluşturma ve bir değişkene $rs atayın. Bu örnek DNS bölgesinde "contoso.com" adlı 600 saniye olarak "Canlı zaman" bir kayıt kümesi türüyle CNAME oluşturun.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

Aşağıdaki örnek, yanıttır.

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>2. Adım

CNAME kayıt kümesi oluşturulduktan sonra web uygulaması'na işaret edecek bir diğer ad değeri oluşturmanız gerekir.

Önceden atanmış değişken "$rs" kullanarak web uygulaması contoso.azurewebsites.net için diğer adı oluşturmak için aşağıdaki PowerShell komutu kullanabilirsiniz.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

Aşağıdaki örnek, yanıttır.

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>3. Adım

Kullanarak değişiklikleri `Set-AzureRMDnsRecordSet` cmdlet:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

Kaydı doğrulamak için aşağıda gösterildiği gibi "www.contoso.com" nslookup kullanarak sorgulayarak doğru bir şekilde oluşturuldu:

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a>Web uygulamaları için bir "awverify" kaydı oluşturun

Web uygulamanız için bir A kaydını kullanmaya karar verirseniz, özel etki alanına sahip olmak için bir doğrulama işlem yapması gerekir. Bu doğrulama adımı "awverify" adlı özel bir CNAME kaydı oluşturarak yapılır. Bu bölüm, yalnızca bir kayıtlar için geçerlidir.

### <a name="step-1"></a>1. Adım

"Awverify" kaydı oluşturun. Aşağıdaki örnekte, özel etki alanı için sahipliği doğrulamak contoso.com "aweverify" kaydı oluşturacağız.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

Aşağıdaki örnek, yanıttır.

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>2. Adım

"Awverify" kayıt kümesi oluşturulduktan sonra diğer adı ayarlama CNAME kaydı atayın. Aşağıdaki örnekte, biz awverify.contoso.azurewebsites.net için diğer adı ayarlama CNAMe kaydı atar.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

Aşağıdaki örnek, yanıttır.

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>3. Adım

Kullanarak değişiklikleri `Set-AzureRMDnsRecordSet cmdlet`komutta gösterildiği gibi.

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a>Sonraki adımlar

Adımları [uygulama hizmeti için bir özel etki alanı adı yapılandırma](../app-service/app-service-web-tutorial-custom-domain.md) özel bir etki alanı kullanmak için web Uygulamanızı yapılandırmak için.
