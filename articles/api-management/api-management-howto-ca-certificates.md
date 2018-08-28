---
title: Özel bir CA sertifikası - Azure API Management ekleyin | Microsoft Docs
description: Azure API Yönetimi'nde özel bir CA sertifikası eklemeyi öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: apimpm
ms.openlocfilehash: 9d3399ba6ee724d91117486744ad1431f53edbce
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053778"
---
# <a name="how-to-add-a-custom-ca-certificate-in-azure-api-management"></a>Azure API Yönetimi'nde özel bir CA sertifikası ekleme

Azure API yönetimi, makineye güvenilen kök ve ara sertifika depoları içinde CA sertifikaları yükleme sağlar. Bu işlev özel bir CA sertifikası hizmetlerinizi ihtiyaç duyuyorsanız kullanılmalıdır.

Bu makalede, Azure portalında bir Azure API Management hizmet örneğinin CA sertifikalarını yönetmek gösterilmektedir.

## <a name="step1"> </a>CA sertifikasını karşıya yükle

![CA sertifikaları Ekle](media/api-management-howto-ca-certificates/00.png)

Yeni bir CA sertifikası yüklemek için aşağıdaki adımları izleyin. Henüz bir API Management hizmet örneği oluşturmadıysanız, bkz [bir API Management hizmet örneği oluşturma](get-started-create-service-instance.md).

1. Azure API Management hizmet örneğinizin Azure portalındaki gidin.

2. Seçin **CA sertifikalarını** menüsünde.

3. Tıklayın **+ Ekle** düğmesi.  

    ![CA sertifikaları Ekle](media/api-management-howto-ca-certificates/01.png)  

4. Sertifika için göz atın ve bilgisayarınızdaki sertifika deposu karar verin. Parola gerekli değildir. Bu nedenle, yalnızca ortak anahtar gereklidir.

    ![CA sertifikaları Ekle](media/api-management-howto-ca-certificates/02.png)  

5. **Kaydet**’e tıklayın. Bu işlem birkaç dakika sürebilir.

    ![CA sertifikaları Ekle](media/api-management-howto-ca-certificates/03.png)  

> [!NOTE]
> Sertifika kullanarak bir CA yükleyebilirsiniz `New-AzureRmApiManagementSystemCertificate` Powershell komutu.

## <a name="step1a"> </a>Bir istemci sertifikasını Sil

Bir sertifikayı silmek için bağlam menüsünü tıklatın **...**  seçip **Sil** sertifikanın yanındaki.

![CA sertifikalarını Sil](media/api-management-howto-ca-certificates/04.png)  

[Upload a CA certificate]: #step1
[Delete a CA certificate]: #step1a