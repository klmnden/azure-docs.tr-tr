---
title: Faturalama REST API'si ile verileri gözden geçirme Azure Kurumsal kayıt | Microsoft Docs
description: Kurumsal kayıt faturalama bilgileri gözden geçirmek için Azure REST API'lerini kullanmayı öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: erikre
ms.openlocfilehash: 9a0b536426ab024d5af7b257e44a2d5e20f14def
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371045"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>Kurumsal kayıt faturalandırma REST API'lerini kullanarak gözden geçirin

Gözden geçirin ve Azure maliyetlerinizi yönetin, azure API'leri raporlama Yardımı.

Bu makalede, fatura hesapları, bölüme veya Azure REST API'lerini kullanarak kurumsal anlaşma (EA) kayıt hesapları ile ilişkili faturalandırma bilgileri almak öğrenin. 

## <a name="individual-account-billing"></a>Bireysel hesabı faturalama

Bir departmandaki hesapları için kullanım ayrıntılarını almak için:

```http
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

`{billingAccountId}` Parametresi gereklidir ve hesap kimliği içermelidir.

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|  
|*Authorization:*|Gereklidir. Geçerli bir kümesi `Bearer` [API anahtarı](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Bu örnek, geçerli fatura dönemi için ayrıntıları döndüren bir zaman uyumlu çağrısı gösterir. Performansla ilgili nedenlerden dolayı zaman uyumlu çağrılar geçen ay için bilgi döndürür.  Ayrıca, çağırabilirsiniz [API zaman uyumsuz olarak](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) 36 ay için verileri döndürmek için.


## <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam) hesabı için ayrıntılı maliyetleri listesini içeren başarılı bir yanıt döndürdü.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/BillingAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        ...
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        ...
      }
    }
  ]
}
```  

Bu örnekte kadar kısaltılmıştır; bkz: [kullanım ayrıntılarını almak için bir faturalama hesabı](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslist) her bir yanıtı alan ve hata işleme eksiksiz bir açıklaması.

## <a name="department-billing"></a>Departman faturalandırma 

Bir departmandaki tüm hesaplar için toplanan kullanım ayrıntılarını alın. 

```http
GET https://management.azure.com/providers/Microsoft.Billing/departments/{departmentId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

`{departmentId}` Parametresi gereklidir ve kayıt hesabı bölümünde kimliği içermelidir.

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|  
|*Authorization:*|Gereklidir. Geçerli bir kümesi `Bearer` [API anahtarı](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Bu örnek, geçerli fatura dönemi için ayrıntıları döndüren bir zaman uyumlu çağrısı gösterir. Performansla ilgili nedenlerden dolayı zaman uyumlu çağrılar geçen ay için bilgi döndürür.  Ayrıca, çağırabilirsiniz [API zaman uyumsuz olarak](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) 36 ay için verileri döndürmek için.

### <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam) belirtilen bir fatura dönemi ve fatura Kimliğini departmanı için ayrıntılı kullanım bilgilerini ve maliyetleri listesini içeren başarılı bir yanıt döndürdü.


Aşağıdaki örnek, departman için REST API çıktısını gösterir `1234`.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        ...
      }
    }
  ]
}
```  

Bu örnekte kadar kısaltılmıştır; bkz: [bir bölüm için kullanım ayrıntılarını alma](/rest/api/consumption/usagedetails/list#departmentusagedetailslist) her bir yanıtı alan ve hata işleme eksiksiz bir açıklaması.

## <a name="enrollment-account-billing"></a>Kayıt hesabı faturalama

Kayıt hesabı için toplanan kullanım ayrıntılarını alın.

```http
GET GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/{enrollmentAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

`{enrollmentAccountId}` Parametresi gereklidir ve kayıt hesabı kimliği içermelidir.

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|  
|*Authorization:*|Gereklidir. Geçerli bir kümesi `Bearer` [API anahtarı](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Bu örnek, geçerli fatura dönemi için ayrıntıları döndüren bir zaman uyumlu çağrısı gösterir. Performansla ilgili nedenlerden dolayı zaman uyumlu çağrılar geçen ay için bilgi döndürür.  Ayrıca, çağırabilirsiniz [API zaman uyumsuz olarak](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) 36 ay için verileri döndürmek için.

### <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam) belirtilen bir fatura dönemi ve fatura Kimliğini departmanı için ayrıntılı kullanım bilgilerini ve maliyetleri listesini içeren başarılı bir yanıt döndürdü.

Aşağıdaki örnek, Kurumsal kayıt için REST API çıktısını gösterir `1234`.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        ....
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        ...
      }
    }
  ]
}
``` 

Bu örnekte kadar kısaltılmıştır; bkz: [kullanım ayrıntılarını almak için bir kayıt hesabı](/rest/api/consumption/usagedetails/list#enrollmentaccountusagedetailslist) her bir yanıtı alan ve hata işleme eksiksiz bir açıklaması.

## <a name="next-steps"></a>Sonraki adımlar 
- Gözden geçirme [Kurumsal raporlama genel bakış](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Araştırma [Kurumsal faturalama REST API'si](https://docs.microsoft.com/rest/api/billing/)   
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
