---
title: 'Sorunlarını giderme: Oluşturma ve Machine Learning çalışma alanına bağlayın | Microsoft Docs'
description: Oluşturma ve bir Azure Machine Learning çalışma alanına bağlanma ortak sorunlar için çözümleri
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 5c265b14a88e993512811de365f1ba51f7ba6028
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Sorun giderme kılavuzu: Machine Learning çalışma alanı oluşturun ve bağlanın
Bu kılavuz, Azure Machine Learning çalışma alanlarınızı ayarlarken çözümleri için bazı sık zorluklar karşılaşılan sağlar.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Çalışma alanı sahibi
Bir çalışma alanı Machine Learning Studio'da açmak için çalışma alanı oluşturmak için kullanılan Microsoft Account oturum açmanız gerekir veya çalışma alanına katılmaya sahibinden davet almak gerekir. Azure portalından erişim yapılandırma yeteneğini içerir çalışma yönetebilirsiniz.

Bir çalışma alanı yönetme ile ilgili daha fazla bilgi için bkz: [bir Azure Machine Learning çalışma alanını yönetme].

[bir Azure Machine Learning çalışma alanını yönetme]: manage-workspace.md

## <a name="allowed-regions"></a>İzin verilen bölgeleri
Machine Learning bölgeler sınırlı bir süre içinde şu anda kullanılamıyor. Aboneliğiniz bu bölgelerinden içermiyorsa, hata iletisi görebilirsiniz, "İzin verilen bölgelerde hiç aboneliğiniz yok."

Bir bölge aboneliğinize eklenmesi isteği için Azure portalından yeni bir Microsoft destek isteği oluşturun, **faturalama** sorun türü olarak ve isteğinizi göndermek için istemleri izleyin.

## <a name="storage-account"></a>Depolama hesabı
Machine Learning hizmetinin verileri depolamak için bir depolama hesabı gerekiyor. Mevcut bir depolama hesabını kullanabilir veya (yeni bir depolama hesabı oluşturmak için kota varsa) yeni Machine Learning çalışma alanı oluşturduğunuzda, yeni bir depolama hesabı oluşturabilirsiniz.

Yeni Machine Learning çalışma alanı oluşturulduktan sonra Machine Learning Studio çalışma alanı oluşturmak için kullanılan Microsoft hesabı kullanarak oturum açabilirsiniz. Hata iletisi "Çalışma alanı bulunamadı" (aşağıdaki ekran görüntüsüne benzer) karşılaşırsanız, tarayıcı tanımlama bilgilerini silmek için aşağıdaki adımları kullanın.

![Çalışma alanı bulunamadı][screen3]

**Tarayıcı tanımlama bilgilerini silmek için**

1. Internet Explorer kullanıyorsanız, tıklatın **Araçları** sağ üst köşesinde düğmesine tıklayın ve ardından **Internet Seçenekleri**.  

![Internet Seçenekleri][screen4]

2. Altında **genel** sekmesini tıklatın, **silin...**

![Genel sekmesi][screen5]

3. İçinde **Gözatma Geçmişini Sil** iletişim kutusunda, emin olun **tanımlama bilgileri ve Web sitesi verileri** seçilir ve tıklatın **silmek**.

![Tanımlama bilgilerini silin][screen6]

Tanımlama bilgilerini silindikten sonra tarayıcıyı yeniden ve gidin [Microsoft Azure Machine Learning](https://studio.azureml.net) sayfası. Bir kullanıcı adı ve parola istendiğinde, çalışma alanı oluşturmak için kullanılan aynı Microsoft hesabını girin.

## <a name="comments"></a>Yorumlar

Amacımız, Machine Learning deneyiminizi mümkün olduğunca sorunsuz olmasını sağlamaktır. Lütfen tüm yorumlar ve sorunu sonrası [Azure Machine Learning Forumu](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) daha iyi hizmet yardımcı olmak için.

[screen1]:media/troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/troubleshooting-creating-ml-workspace/screen6.png
