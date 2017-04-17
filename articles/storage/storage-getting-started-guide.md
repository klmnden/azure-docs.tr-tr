---
title: "Beş dakikada Azure Depolama’yı kullanmaya başlama | Microsoft Belgeleri"
description: "Azure Storage Hızlı Başlangıç, Visual Studio ve Azure Storage öykünücüsü kullanarak Microsoft Azure Blob’ları, Tablo ve Kuyruklarını hızla kullanmaya başlayın. İlk Azure Storage uygulamanızı beş dakikada çalıştırın."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 582f76f8-c814-4a69-8a5c-1fd0e0d5d8f2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 02/23/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 6508b5a5d7d654954c68f9390c62a7871c1f032c
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-storage-in-five-minutes"></a>Beş dakikada Azure Storage’a başlayın
## <a name="overview"></a>Genel Bakış
Azure Storage ile geliştirmeye başlamak kolaydır. Bu öğretici bir Azure Storage uygulamasını nasıl hızla çalıştırabileceğinizi gösterir. .NET için Azure SDK ile birlikte gelen Hızlı Başlangıç şablonlarını kullanacaksınız. Bu Hızlı Başlangıçlar Azure Storage ile bazı temel programlama senaryolarını gösteren çalıştırılmaya hazır kodlar içerir.

Kodlara başlamadan önce Azure Depolama hakkında daha fazla bilgi edinmek için bkz. [Sonraki adımlar](#next-steps).

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce aşağıdaki ön koşulları yerine getirmeniz gerekir:

1. Bir uygulama derlemek ve oluşturmak için bilgisayarınızda [Visual Studio 2015](https://www.visualstudio.com/) veya üzeri yüklü olması gerekir.
2. [.NET için Azure SDK](https://azure.microsoft.com/downloads/) uygulamasının en yeni sürümünü yükleyin. SDK Azure QuickStart örnek projelerini, Azure Storage öykünücüsünü ve [.NET için Azure Storage İstemcisi](https://msdn.microsoft.com/library/azure/dn261237.aspx)ni içerir.
3. Bu öğretide kullanacağımız Azure Hızlı Başlangıç örnek projeler için gerekli olduğundan [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653)’in bilgisayarınızda yüklü olduğundan emin olun.

    Bilgisayarınızda yüklü .NET Framework’ün sürümünü bilmiyorsanız, bkz. [Nasıl Yapılır: Hangi .NET Framework Sürümlerinin Yüklendiğini Belirleme](https://msdn.microsoft.com/vstudio/hh925568.aspx). Veya **Başlat** düğmesine ya da Windows tuşuna basın, **Denetim Paneli** yazın. Ardından **Programlar** > **Programlar ve Özellikler**’e tıklayarak yüklü programlar arasında .NET Framework 4.5’in yüklü olup olmadığını görün.
4. Bir Azure aboneliği ve Azure Storage hesabına ihtiyaç duyacaksınız.

   * Bir Azure aboneliği edinmek için bkz. [Ücretiz Deneme](https://azure.microsoft.com/pricing/free-trial/), [Satın Alma Seçenekleri](https://azure.microsoft.com/pricing/purchase-options/) ve [Üye Teklifleri](https://azure.microsoft.com/pricing/member-offers/) (MSDN, Microsoft İş Ortağı Ağı, BizSpark ve diğer Microsoft programları üyeleri).
   * Azure’da bir depolama hesabı oluşturmak için bkz. [Depolama hesabı oluşturma](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>İlk Azure Storage uygulamanızı bulutta Azure Storage’a karşı çalıştırma
Hesabınızı edindikten sonra Visual Studio’da Azure Hızlı Başlangıç örnek projelerinden birini kullanarak basit bir Azure Storage uygulaması oluşturabilirsiniz. Bu öğreti şu Azure Storage örnek projelerine odaklanır : **Azure Storage: Blob’lar**, **Azure Storage: Dosyalar**, **Azure Storage: Kuyruklar** ve **Azure Storage: Tablolar**:

1. Visual Studio’yu çalıştırın.
2. **Dosya** menüsünden **Yeni Proje**’ye tıklayın.
3. **Yeni Proje** iletişim kutusundan **Yüklenen** > **Şablonlar** > **Visual C#** > **Bulut** > **QuickStarts** > **Veri Hizmetleri**’ne tıklayın.
   
   a. Aşağıdaki şablonlardan birini seçin: **Azure Storage: Blob’lar**, **Azure Storage: Dosyalar**, **Azure Storage: Kuyruklar** veya **Azure Storage: Tablolar**.
   
   b. Hedef altyapı olarak **.NET Framework 4.5**’in seçildiğinden emin olun.
   
   c. Aşağıda gösterildiği şekilde projeniz için bir ad belirleyin ve yeni Visual Studio çözümünü oluşturun:

    ![Azure Hızlı Başlangıçlar][Image1]

Uygulamayı çalıştırmadan önce kaynak kodunu incelemek isteyebilirsiniz. Kodu incelemek için Visual Studio’da **Görüntüle** menüsünde **Çözüm Gezgini**’ni seçin. Ardından Program.cs dosyasına çift tıklayın.

Ardından örnek uygulamayı çalıştırın:

1. Visual Studio’da **Görünüm** menüsünde **Çözüm Gezgini**’ni seçin. **App.config** dosyasını açın ve Azure Depolama öykünücüsü için bağlantı dizesine açıklama yerleştirin:

   `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2. Azure Depolama hizmeti için bağlantı dizesinin açıklamasını kaldırın ve App.config dosyasında `[AccountName]` ve `[AccountKey]` yerine hesap bilgilerinizi yazın:

   `<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

   Depolama hesabı erişim tuşunuzı almak için bkz. [Depolama erişim tuşlarınızı yönetme](storage-create-storage-account.md#manage-your-storage-access-keys).
3. App.config dosyasında depolama hesap adı ve erişim tuşunu sağladıktan sonra **Dosya** menüsünde, **Tümünü Kaydet**’e tıklayarak tüm proje dosyalarını kaydedin.
4. **Yapı** menüsünde **Yapı Çözümü**’ne tıklayın.
5. **Hata Ayıklama** menüsünde **F11**’e basarak adım adım çözümü çalıştırın veya **F5**’e basarak çözümü çalıştırın.

## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>İlk Azure Storage uygulamanızı Azure Storage Öykünücüsüne karşı çalıştırma
[Azure Storage Öykünücüsü](storage-use-emulator.md) geliştirme amaçlı olarak Azure Blob, Kuyruk ve Tablo hizmetlerine öykünen yerel bir ortam sağlar. Bir Azure aboneliği veya depolama hesabı oluşturmadan ve herhangi bir masraf yapmadan depolama uygulamanızı yerel olarak test etmek üzere depolama öykünücüsünü kullanabilirsiniz.

Denemek için Visual Studio’da Azure Hızlı Başlangıç örnek projelerinden birini kullanarak basit bir Azure Depolama uygulaması oluşturalım. Bu öğretici **Azure Blob Storage**, **Azure Table Storage** ve **Azure Kuyruk Depolama** örnek projelerine odaklanır:

1. Visual Studio’yu çalıştırın.
2. **Dosya** menüsünden **Yeni Proje**’ye tıklayın.
3. **Yeni Proje** iletişim kutusundan **Yüklenen** > **Şablonlar** > **Visual C#** > **Bulut** > **QuickStarts** > **Veri Hizmetleri**’ne tıklayın.
    
    a. Aşağıdaki şablonlardan birini seçin: **Azure Storage: Blob’lar**, **Azure Storage: Dosyalar**, **Azure Storage: Kuyruklar** veya **Azure Storage: Tablolar**.
    
    b. Hedef altyapı olarak **.NET Framework 4.5**’in seçildiğinden emin olun.
    
    c. Aşağıda gösterildiği şekilde projeniz için bir ad belirleyin ve yeni Visual Studio çözümünü oluşturun:

    ![Azure Hızlı Başlangıçlar][Image1]

4. Visual Studio’da **Görünüm** menüsünde **Çözüm Gezgini**’ni seçin. App.config dosyasını açın ve varsa önceden eklediğiniz Azure depolama hesabı bağlantı dizesini yorum olarak yazın. Ardından bağlantı dizesi için Azure Storage öykünücüsü açıklamadan çıkarın:

   `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Uygulamayı çalıştırmadan önce kaynak kodunu incelemek isteyebilirsiniz. Kodu incelemek için Visual Studio’da **Görüntüle** menüsünde **Çözüm Gezgini**’ni seçin. Ardından Program.cs dosyasına çift tıklayın.

Ardından Azure Storage Öykünücüsünde örnek uygulamayı çalıştırın:

1. **Başlat** düğmesine veya Windows tuşuna basın ve *Microsoft Azure Storage öykünücüsü* nü arayın ve uygulamayı başlatın. Öykünücü başlatıldığında Windows Görev Görünüm alanında bir simge ve bir bildirim göreceksiniz.
2. Visual Studio’da **Yapı** menüsünde **Yapı Çözümü**’ne tıklayın.
3. **Hata Ayıklama** menüsünde **F11**’e basarak adım adım çözümü çalıştırın veya **F5**’e basarak çözümü başından sonuna kadar çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Storage ile ilgili daha fazla bilgi edinmek için şu kaynaklara bakın:

* [Microsoft Azure Depolama'ya Giriş](storage-introduction.md)
* [Azure Depolama Gezgini ile çalışmaya başlama](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-blobs.md)
* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-tables.md)
* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-queues.md)
* [Windows'da Azure Dosya Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-files.md)
* [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)
* [.NET için Microsoft Azure Depolama İstemcisi Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png

