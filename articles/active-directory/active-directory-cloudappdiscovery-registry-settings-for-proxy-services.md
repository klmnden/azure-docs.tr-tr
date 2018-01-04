---
title: "Cloud App Discovery kayıt defteri ayarları Proxy Hizmetleri | Microsoft Docs"
description: "Bu konunun amacı, Cloud App Discovery Aracısı çalıştıran bilgisayarlarda gerekli bağlantı noktası ayarlamak için gerçekleştirmeniz gereken adımları sağlamaktır."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: f633e76ea7c0df456bff41c450eb136809de12a8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Proxy Hizmetleri için cloud App Discovery kayıt defteri ayarları
Cloud App Discovery Aracısı çalıştıran bilgisayarlarda gerekli bağlantı noktası ayarlamak için gerçekleştirmek nasıl bildirmek için bu konunun amacı budur. Varsayılan olarak, Cloud App Discovery Aracısı yalnızca bağlantı noktası 80 veya 443 kullanmak üzere yapılandırılır. Cloud App Discovery, özel bir bağlantı noktası (80 ne 443) kullanarak bir proxy sunucusu olan bir ortamda yüklemeyi planlıyorsanız, bu bağlantı noktasını kullanacak biçimde aracılarınızı yapılandırmanız gerekir. Yapılandırma, bir kayıt defteri anahtarı temel alır.

> [!TIP] 
> Azure Active Directory'de tarafından geliştirilmiş (Azure AD), artık aracısız Cloud App Discovery geliştirmeleri kullanıma [Microsoft Cloud App Security ile tümleştirme](https://portal.cloudappsecurity.com).

## <a name="modify-the-port-used-by-the-computer-running-the-cloud-app-discovery-agent"></a>Cloud App Discovery Aracısı'nı çalıştıran bilgisayar tarafından kullanılan bağlantı noktasını Değiştir

1. Kayıt Defteri Düzenleyicisi'ni başlatın.
  ![Çalıştırma](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Gidin veya aşağıdaki kayıt defteri anahtarı oluşturun: **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud uygulama Discovery\Endpoint**
3. Yeni bir **çok dizeli** adlı değeri **bağlantı noktalarını**. 
  ![Yeni](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Açmak için **Çoklu Dize Düzenle** iletişim kutusunda, çift **bağlantı noktalarını** değeri.
5. İçinde **değer verisi**, aşağıdaki değerleri girin ve kuruluşunuz tarafından kullanılan tüm özel bağlantı noktalarını ekleyin: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Çoklu Dize Düzenle](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Tıklatın **Tamam** kapatmak için **Çoklu Dize Düzenle** iletişim.

## <a name="next-steps"></a>Sonraki adımlar

* [Nasıl ı Kuruluşum içinde kullanılan onaylanmamış bulut uygulamaları bulabilir](active-directory-cloudappdiscovery-whatis.md) 

