---
title: Konuşma aygıtları SDK alma | Microsoft Docs
description: Konuşma aygıtları SDK'sı erişmek öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 4706ea623dccd2dbb4164bd9cccf22cff121884a
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356290"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Bilişsel hizmetler konuşma aygıtları SDK'sı Al

## <a name="requesting-access"></a>Erişim isteği

Konuşma aygıtları SDK'sına kısıtlı önizlemede ve programa kayıtlı olması gerekir. Şu anda, Microsoft access için aday olarak büyük şirketler bu ürüne tercih eder.

Konuşma aygıtları SDK'sı erişmek için şu adımları izleyin:

1. Microsoft konuşma aygıtları SDK'sı gidin [kayıt formu](https://aka.ms/sdsdk-signup).
1. Okuma [lisans sözleşmesini](speech-devices-sdk-license.md).
1. Bu lisans sözleşmesinin koşullarını kabul ediyorsanız, "Kabul ediyorum." seçin
1. Formun soruları yanıtlayın.
1. Form gönderme. 
1. E-posta adresinizi zaten Azure Active Directory'nin parçası değilse, bir onay bağlı gibi bir davet e-posta alırsınız. E-posta adresinizi Azure Active Directory'de ise, onay bağlı Microsoft Speech ekibinden bir e-posta iletisi alırsınız ve İleri atlayabilirsiniz [konuşma aygıtları SDK'sını indirin](#download-the-speech-devices-sdk).

## <a name="approval-e-mail"></a>Onay e-postası

```
From: Microsoft Speech Team from Microsoft (via Microsoft) <invites@microsoft.com> 
Subject: You're invited to the Microsoft organization 
```

![E-posta iletisi](media/speech-devices-sdk/get-sdk-1.png)

## <a name="accept-access"></a>Erişim onay
Azure Active Directory, kayıt sırasında sağladığınız e-posta adresi ile katılmak için aşağıdaki adımları gerçekleştirin. Bu işlem konuşma aygıtları SDK's erişiminizi [yükleme sitesine](https://shares.datatransfer.microsoft.com/).

1. Tıklatın **Get Started** aldığınız e-posta iletisi. Kuruluşunuz zaten bir Office 365 müşteri ise, oturum açmak için istenir ve şimdi 8. adımına atlayabilirsiniz.

2. Tıklatın **sonraki** başlatılan tarayıcı penceresinde.

    ![kimlik doğrulama penceresi](media/speech-devices-sdk/get-sdk-2.png)

3. Zaten yoksa, bir Microsoft hesabı oluşturun. Davet e-posta yukarıda 6. adımda aldığınız aynı e-posta adresi girin.

    ![bir Microsoft hesabı oluşturun](media/speech-devices-sdk/get-sdk-3.png)

4. Tıklatın **sonraki** bir parola oluşturmak için.

5. E-postanızı doğrulamak için istendiğinde, size gönderilen doğrulama kodunu almak için e-posta kutunuza döndür.
 
7. İletişim kutusunda e-posta iletisi güvenlik kodunu yazın veya yapıştırın. Bu örnekte "8406." Ardından **İleri**'ye tıklayın.

    ![e-posta doğrulayın](media/speech-devices-sdk/get-sdk-6.png)
 
8. Erişim paneli uygulama tarayıcı penceresinde gördüğünüzde, e-posta adresinden (6. adım) artık Azure Active Directory'nin parçası olduğunu doğruladı. Şimdi konuşma aygıtları SDK yükleme sitesine erişebilirsiniz.

## <a name="download-the-speech-devices-sdk"></a>Konuşma aygıtları SDK yükle

Git [konuşma aygıtları SDK yükleme site](https://shares.datatransfer.microsoft.com/) ve daha önce oluşturduğunuz Microsoft Account oturum. Konuşma aygıtları SDK'sı, ilişkili örnek kod ve başvuru bilgileri aşağıdaki adımları izleyerek şimdi yükleyebilirsiniz.

![SDK yükleme sitesi](media/speech-devices-sdk/get-sdk-7.png)

1. Bunu yapmak tarayıcı tarafından istendiğinde Aspera Connect aracını yükleyip yeniden açın.

    ![Aspera Connect indirin](media/speech-devices-sdk/get-sdk-8.png)
 
1. Tıklatın **Evet** Aspera bağlanmak için geçiş yapmak için.

    ![Aspera bağlanmak için geçiş](media/speech-devices-sdk/get-sdk-9.png)
 
1. Tıklatın **izin** Aspera Connect ile dosyaları indirme onaylamak için.

    ![Aspera bağlantı ile indirme](media/speech-devices-sdk/get-sdk-10.png)
 
1. İndirilen dosyaları sonra Aspera Bağlan'ın aktarımları penceresini kapatın.

    ![Aspera Bağlan'ın aktarımları penceresi](media/speech-devices-sdk/get-sdk-11.png)
 
Varsayılan olarak, dosyalar halinde indirilir, **indirmeleri** klasör. Bu site dışında artık oturum açabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma aygıtları SDK'sı ile çalışmaya başlama](speech-devices-sdk-qsg.md)
