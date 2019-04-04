---
title: Hizmet olarak Azure Stack doğrulama iş akışı ortak parametrelerinin | Microsoft Docs
description: İş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 9513b552ce6bfd525077270b90d3d10e31c015c5
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484971"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>İş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Ortak parametreleri içeren değerleri gibi ortam değişkenlerini ve kullanıcı kimlik bilgilerini doğrulama (VaaS) hizmet olarak tüm testler için gerekli. Bu değerler, oluşturduğunuzda veya bir iş akışını değiştirme iş akışı düzeyinde tanımlanır. Testleri planlama, bu değerler her test altındaki iş akışı parametreleri olarak geçirilir.

> [!NOTE]
> Her test, kendi parametreleri kümesini tanımlar. Zamanlama süresi, test, ortak parametreleri bağımsız olarak bir değer girmesini gerektirebilir veya ortak parametre değeri geçersiz kılmak izin verebilir.

## <a name="environment-parameters"></a>Ortam parametreleri

Test edilen Azure Stack ortamınıza ortam parametrelerini açıklar. Bu değerleri oluşturmak ve test ettiğiniz belirli örneği için bir Azure Stack damga bilgi dosyası karşıya yükleme tarafından sağlanmalıdır.

> [!NOTE]
> Resmi doğrulama iş akışları, iş akışını oluşturduktan sonra ortam parametrelerini değiştirilemez.

### <a name="generate-the-stamp-information-file"></a>Damga bilgi dosyası oluştur

1. DVM veya Azure Stack ortamınıza erişimi olan herhangi bir makineye oturum açın.
2. Yükseltilmiş bir PowerShell penceresinde aşağıdaki komutları çalıştırın:

    ```powershell  
    $CloudAdminUser = "<cloud admin username>"
    $CloudAdminPassword = ConvertTo-SecureString "<cloud admin password>" -AsPlainText -Force
    $stampInfoCreds = New-Object System.Management.Automation.PSCredential($CloudAdminUser, $CloudAdminPassword)
    $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation' -Credential $stampInfoCreds
    ConvertTo-Json $params > stampinfoproperties.json
    ```

### <a name="locate-values-in-the-ece-configuration-file"></a>Değerleri ECE yapılandırma dosyasında bulunamıyor

Ortam parametre değerlerini de el ile konumlandırılabilir içinde **ECE yapılandırma dosyası** konumundaki `C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787` DVM üzerinde.

## <a name="test-parameters"></a>Test parametreleri

Genel test parametreleri yapılandırma dosyalarında depolanan hassas bilgiler içerir. Bu el ile sağlanmalı.

Parametre    | Açıklama
-------------|-----------------
Kiracı Yöneticisi kullanıcı                            | Azure Active Directory Kiracı bir AAD dizini Hizmet Yöneticisi tarafından sağlanan Yöneticisi. Bu kullanıcı kaynakları (VM'ler, depolama hesapları, vb.) ayarlamak için şablonları dağıtma ve iş yüklerini çalıştırma gibi Kiracı düzeyinde Eylemler gerçekleştirir. Kiracı hesabı sağlama hakkında daha fazla bilgi için bkz [yeni bir Azure Stack kiracısı ekleme](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad).
Hizmet Yöneticisi kullanıcı             | AAD dizin Kiracısında Azure Stack dağıtım sırasında belirtilen Azure Active Directory Yöneticisi. Arama `AADTenant` ECE yapılandırma dosyası ve bir değer seçin `UniqueName` öğesi.
Bulut yönetici kullanıcı               | Azure Stack etki alanı yöneticisi hesabı (örneğin, `contoso\cloudadmin`). Arama `User Role="CloudAdmin"` ECE yapılandırma dosyası ve bir değer seçin `UserName` öğesi.
Tanılama bağlantı dizesi          | Bir Azure depolama hesabı için hangi tanılama günlükleri test yürütme sırasında kopyalanacak bir SAS URL'si. SAS URL'si oluşturma yönergeleri için bkz: [tanılama bağlantı dizesi oluştur](#generate-the-diagnostics-connection-string). |

> [!IMPORTANT]
> **Tanılama bağlantı dizesi** devam etmeden önce geçerli olmalıdır.

### <a name="generate-the-diagnostics-connection-string"></a>Tanılama bağlantı dizesi oluştur

Tanılama bağlantı dizesi, test yürütme sırasında tanılama günlükleri depolamak için gereklidir. Kurulum sırasında oluşturulan Azure depolama hesabı kullanın (bkz [doğrulama hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)) VaaS günlükleri karşıya yükleme, depolama hesabınıza erişimi vermek için paylaşılan erişim imzası (SAS) URL'si oluşturmak için.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Seçin **Blob** gelen **izin verilen hizmetler seçenekleri**. Diğer seçenekleri seçimini kaldırın.

1. Seçin **hizmet**, **kapsayıcı**, ve **nesne** gelen **izin verilen kaynak türleri**.

1. Seçin **okuma**, **yazma**, **listesi**, **ekleme**, **oluşturma** gelen **izin izinleri**. Diğer seçenekleri seçimini kaldırın.

1. Ayarlama **başlangıç zamanı** geçerli saat ve **bitiş saati** geçerli saatten üç ay için.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]

> [!NOTE]  
> SAS URL'sini URL oluşturulduğunda, belirtilen bitiş zaman süresi dolar.  
Testleri planlama yaparken, en az 30 gün boyunca URL'SİNİN geçerli olduğundan artı (üç ay önerilir) test çalıştırması için gerekli süre emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [doğrulama olarak hizmet temel kavramları](azure-stack-vaas-key-concepts.md)