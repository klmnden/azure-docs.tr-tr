---
title: Konuşma Cihazları SDK’sını edinme
description: Konuşma cihaz SDK'sı erişin öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: f70b41cd7e3a7a6eddf32ae6ad024fa9ac040f29
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281791"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Bilişsel hizmetler konuşma cihaz SDK'sı Al

## <a name="requesting-access"></a>Erişim isteği

Konuşma cihaz SDK'sı, sınırlı Önizleme aşamasındadır ve programa kaydedilmesini gerektirir. Şu anda, Microsoft bu ürüne erişim için aday olarak büyük şirketler tercih eder.

Konuşma cihaz SDK'sı erişim elde etmek için şu adımları izleyin:

1. Microsoft konuşma cihaz SDK'sı Git [kayıt formunu](https://aka.ms/sdsdk-signup).
1. Okuma [lisans sözleşmesini](speech-devices-sdk-license.md).
1. Bu lisans sözleşmesinin şartlarını kabul ediyorsanız, "Kabul ediyorum." seçin
1. Formda soruları yanıtlayın.
1. Form gönderilemiyor. 
1. E-posta adresiniz zaten Azure Active Directory'nin parçası değilse, onay sonrasında benzeyen bir davet e-posta alırsınız. E-posta adresiniz zaten Azure Active Directory'de ise, Microsoft Speech ekibinden onay sonrasında bir e-posta iletisi alırsınız ve İleri atlayabilirsiniz [konuşma cihaz SDK'sını indirin](#download-the-speech-devices-sdk).

## <a name="approval-e-mail"></a>Onay e-postası

```
From: Microsoft Speech Team from Microsoft (via Microsoft) <invites@microsoft.com> 
Subject: You're invited to the Microsoft organization 
```

![e-posta iletisi](media/speech-devices-sdk/get-sdk-1.png)

## <a name="accept-access"></a>Erişim
Kayıt sırasında sağladığınız e-posta adresi ile Azure Active Directory'e katılması için aşağıdaki adımları gerçekleştirin. Bu işlem konuşma cihaz SDK'SININ 's erişiminizi [yükleme sitesine](https://shares.datatransfer.microsoft.com/).

1. Tıklayın **Başlarken** aldığınız e-posta iletisi. Kuruluşunuz zaten bir Office 365 müşterisi ise, oturum açmak için istenir ve 8. adımına atlayabilirsiniz.

2. Tıklayın **sonraki** başlatılan bir tarayıcı penceresinde.

    ![kimlik doğrulama penceresi](media/speech-devices-sdk/get-sdk-2.png)

3. Zaten yoksa, bir Microsoft hesabı oluşturun. Davet e-posta yukarıda 6. adımda aldığınız aynı e-posta adresi girin.

    ![Microsoft hesabı oluşturun](media/speech-devices-sdk/get-sdk-3.png)

4. Tıklayın **sonraki** bir parola oluşturmak için.

5. Size gönderilen doğrulama kodu almak için e-posta Gelen Kutunuza e-postanızı doğrulamak için istendiğinde döndürür.
 
7. Yapıştırın veya iletişim kutusunda e-posta iletisi güvenlik kodunu girin. Bu örnekte "8406.": Ardından **İleri**'ye tıklayın.

    ![e-posta doğrulama](media/speech-devices-sdk/get-sdk-6.png)
 
8. Erişim paneli uygulama tarayıcı penceresinde gördüğünüzde, e-posta adresinden (6. adım) artık Azure Active Directory'nin parçası olduğunu doğruladı. Artık konuşma cihaz SDK'sını indirme sitesine erişebilirsiniz.

## <a name="download-the-speech-devices-sdk"></a>Konuşma cihaz SDK'sını indirin

Git [konuşma cihazları SDK indirme sitesi](https://shares.datatransfer.microsoft.com/) daha önce oluşturduğunuz Microsoft Account bilgilerinizle oturum açın. Konuşma cihaz SDK'sı, ilişkili örnek kod ve başvuru malzemesi aşağıdaki adımları izleyerek indirebilirsiniz.

![SDK indirme sitesi](media/speech-devices-sdk/get-sdk-7.png)

1. Bunu yapmak tarayıcı tarafından istendiğinde Aspera Connect aracını yükleyip yeniden açın.

    ![Aspera Connect indirin](media/speech-devices-sdk/get-sdk-8.png)
 
1. Tıklayın **Evet** Aspera bağlanmak için geçiş yapmak için.

    ![Aspera bağlanmak için geçiş](media/speech-devices-sdk/get-sdk-9.png)
 
1. Tıklayın **izin** Connect Aspera ile dosyaları indirme onaylamak için.

    ![Aspera Connect ile birlikte indirin](media/speech-devices-sdk/get-sdk-10.png)
 
1. Dosyaları indirdikten sonra Aspera Bağlantısı'nın aktarımları penceresini kapatın.

    ![Aspera Bağlan'ın aktarımları penceresi](media/speech-devices-sdk/get-sdk-11.png)
 
Varsayılan olarak, dosyalar halinde indirilir, **indirir** klasör. Bu site dışında artık oturum açabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı ile çalışmaya başlama](speech-devices-sdk-qsg.md)
