---
title: 'Sorun giderme: Oluşturma ve bir Machine Learning çalışma alanına bağlayın | Microsoft Docs'
description: Oluşturma ve bir Azure Machine Learning çalışma alanına bağlanırken yaygın sorunların çözümleri
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.custom: (previous ms.author hshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 678d422cc241a75c1e3dc0c644f0ac5055750ca3
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824158"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Sorun giderme kılavuzu: Machine Learning çalışma alanı oluşturun ve bağlanın
Bu kılavuz, Azure Machine Learning çalışma alanları ayarlarken çözümleri bazı zorluklar sık karşılaşıldı. sağlar.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Çalışma alanı sahibi
Bir çalışma alanında Machine Learning Studio'da açmak için çalışma alanı oluşturmak için kullanılan Microsoft Account oturum açmanız gerekir veya sahibinden çalışma alanına katılması için davet almak gerekir. Azure portalından, erişimi yapılandırmak için olanağını da içeren çalışma alanında, yönetebilirsiniz.

Bir çalışma alanı yönetme ile ilgili daha fazla bilgi için bkz: [bir Azure Machine Learning çalışma alanını yönetme].

[Bir Azure Machine Learning çalışma alanını yönetme]: manage-workspace.md

## <a name="allowed-regions"></a>İzin verilen bölgeler
Machine Learning sınırlı sayıdaki bölgede şu anda kullanılabilir. Aboneliğiniz bu bölgelerden birini içermiyorsa hata iletisini görebilirsiniz, "izin verilen bölgelerde aboneliğiniz yok."

Bir bölge aboneliğinize eklenmesini isteyin için Azure portalında yeni bir Microsoft destek isteği oluşturma, **faturalama** sorun türü olarak ve isteğinizi göndermek için yönergeleri izleyin.

## <a name="storage-account"></a>Depolama hesabı
Machine Learning hizmetinin veri depolamak için bir depolama hesabı gerekir. (Yeni bir depolama hesabı oluşturmak için kota varsa), yeni bir Machine Learning çalışma alanı oluşturduğunuzda, yeni bir depolama hesabı oluşturabilirsiniz veya mevcut bir depolama hesabını kullanabilirsiniz.

Yeni bir Machine Learning çalışma alanı oluşturulduktan sonra Machine Learning Studio çalışma alanı oluşturmak için kullandığınız Microsoft hesabı kullanarak oturum açabilirsiniz. Hata iletisi "Çalışma alanı bulunamadı" (aşağıdaki ekran görüntüsüne benzer) karşılaşırsanız, tarayıcı tanımlama bilgilerinizi silmek için aşağıdaki adımları kullanın.

![Çalışma alanı bulunamadı][screen3]

**Tarayıcı tanımlama bilgilerini silmek için**

1. Internet Explorer kullanıyorsanız, tıklatın **Araçları** sağ üst köşesinde'düğmesine tıklayın ve belirleyin **Internet Seçenekleri**.  

![Internet Seçenekleri][screen4]

2. Altında **genel** sekmesinde **Sil...**

![Genel sekmesi][screen5]

3. İçinde **Gözatma Geçmişini Sil** iletişim kutusunda, emin **tanımlama bilgileri ve Web sitesi verileriyle** seçilir ve tıklayın **Sil**.

![Tanımlama bilgilerini silin][screen6]

Tanımlama bilgilerini silindikten sonra tarayıcıyı yeniden başlatmak ve ardından Git [Microsoft Azure Machine Learning](https://studio.azureml.net) sayfası. Bir kullanıcı adı ve parola sorulduğunda, çalışma alanı oluşturmak için kullandığınız aynı Microsoft hesabını girin.

## <a name="comments"></a>Yorumlar

Machine Learning deneyiminizi mümkün olduğunca sorunsuz hale getirmek için Amacımız sağlamaktır. Lütfen herhangi bir yorum ve sorunu sonrası [Azure Machine Learning Forumu](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) daha iyi hizmet yardımcı olmak için.

[screen1]:media/troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/troubleshooting-creating-ml-workspace/screen6.png
