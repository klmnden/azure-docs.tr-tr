---
title: Bir çalışma sorunlarını giderme
titleSuffix: Azure Machine Learning Studio
description: Bu kılavuz, Azure Machine Learning Studio çalışma alanları ayarlarken çözümleri bazı zorluklar sık karşılaşıldı. sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 7cc825daa29a0398793f3c6fc5ce8ee426ad79e6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193874"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-azure-machine-learning-studio-workspace"></a>Sorun giderme kılavuzu: Oluşturma ve bir Azure Machine Learning Studio çalışma alanına bağlayın
Bu kılavuz, Azure Machine Learning Studio çalışma alanları ayarlarken çözümleri bazı zorluklar sık karşılaşıldı. sağlar.



## <a name="workspace-owner"></a>Çalışma alanı sahibi
Bir çalışma alanında Machine Learning Studio'da açmak için çalışma alanı oluşturmak için kullanılan Microsoft Account oturum açmanız gerekir veya sahibinden çalışma alanına katılması için davet almak gerekir. Azure portalından, erişimi yapılandırmak için olanağını da içeren çalışma alanında, yönetebilirsiniz.

Bir çalışma alanı yönetme ile ilgili daha fazla bilgi için bkz: [bir Azure Machine Learning Studio çalışma alanını yönetme].

[Bir Azure Machine Learning Studio çalışma alanını yönetme]: manage-workspace.md

## <a name="allowed-regions"></a>İzin verilen bölgeler
Machine Learning sınırlı sayıdaki bölgede şu anda kullanılabilir. Aboneliğiniz bu bölgelerden birini içermiyorsa hata iletisini görebilirsiniz, "izin verilen bölgelerde aboneliğiniz yok."

Bir bölge aboneliğinize eklenmesini isteyin için Azure portalında yeni bir Microsoft destek isteği oluşturma, **faturalama** sorun türü olarak ve isteğinizi göndermek için yönergeleri izleyin.

## <a name="storage-account"></a>Depolama hesabı
Machine Learning hizmetinin veri depolamak için bir depolama hesabı gerekir. (Yeni bir depolama hesabı oluşturmak için kota varsa), yeni bir Machine Learning Studio çalışma alanı oluşturduğunuzda, yeni bir depolama hesabı oluşturabilirsiniz veya mevcut bir depolama hesabını kullanabilirsiniz.

Yeni bir Machine Learning Studio çalışma alanı oluşturulduktan sonra Machine Learning Studio çalışma alanı oluşturmak için kullandığınız Microsoft hesabı kullanarak oturum açabilirsiniz. Hata iletisi "Çalışma alanı bulunamadı" (aşağıdaki ekran görüntüsüne benzer) karşılaşırsanız, tarayıcı tanımlama bilgilerinizi silmek için aşağıdaki adımları kullanın.

![Çalışma alanı bulunamadı](media/troubleshooting-creating-ml-workspace/screen3.png)

**Tarayıcı tanımlama bilgilerini silmek için**

1. Internet Explorer kullanıyorsanız, tıklatın **Araçları** sağ üst köşesinde'düğmesine tıklayın ve belirleyin **Internet Seçenekleri**.  

   ![Internet Seçenekleri](media/troubleshooting-creating-ml-workspace/screen4.png)

2. Altında **genel** sekmesinde **Sil...**

   ![Genel sekmesi](media/troubleshooting-creating-ml-workspace/screen5.png)

3. İçinde **Gözatma Geçmişini Sil** iletişim kutusunda, emin **tanımlama bilgileri ve Web sitesi verileriyle** seçilir ve tıklayın **Sil**.

   ![Tanımlama bilgilerini silin](media/troubleshooting-creating-ml-workspace/screen6.png)

Tanımlama bilgilerini silindikten sonra tarayıcıyı yeniden başlatmak ve ardından Git [Microsoft Azure Machine Learning Studio](https://studio.azureml.net) sayfası. Bir kullanıcı adı ve parola sorulduğunda, çalışma alanı oluşturmak için kullandığınız aynı Microsoft hesabını girin.

## <a name="comments"></a>Yorumlar

Machine Learning deneyiminizi mümkün olduğunca sorunsuz hale getirmek için Amacımız sağlamaktır. Lütfen herhangi bir yorum ve sorunu sonrası [Azure Machine Learning Forumu](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) daha iyi hizmet yardımcı olmak için.
