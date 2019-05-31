---
title: Kopyalayın veya v10 AzCopy kullanarak Azure Depolama'ya veri taşıma | Microsoft Docs
description: AzCopy, verileri kopyalamak için ya da depolama hesapları arasında kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makalede, AzCopy indirin, depolama hesabınıza bağlanın ve ardından dosya aktarımı yardımcı olur.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: cc65d6d3f7e7dcc08ea29ecc8a299b556563135b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236304"
---
# <a name="get-started-with-azcopy"></a>AzCopy’i kullanmaya başlama

AzCopy, BLOB veya dosyalar için veya bir depolama hesabına kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makalede, AzCopy indirin, depolama hesabınıza bağlanın ve ardından dosya aktarımı yardımcı olur.

> [!NOTE]
> AzCopy **V10** AzCopy şu anda desteklenen sürümüdür.
>
> AzCopy kullanmanız gerekiyorsa **v8.1**, bkz: [önceki AzCopy kullanmanız](#previous-version) bu makalenin.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>AzCopy indirin

İlk olarak, AzCopy V10 yürütülebilir dosyayı bilgisayarınızda herhangi bir klasöre indirin. Kolaylık olması için sistem yolunuzda kullanım kolaylığı için AzCopy klasör konumunu eklemeyi düşünün.

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

> [!NOTE]
> Veri kitaplıklarından kopyalamak istiyorsanız, [Azure tablo depolama](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) hizmet ve sonra Yükle [AzCopy sürüm 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>AzCopy çalıştırın

Bir komut istemi'nden dosyayı karşıdan yüklediğiniz dizine gidin.

AzCopy komutların bir listesini görüntülemek için şunu yazın `azCopy`ve ardından ENTER tuşuna basın.

Belirli bir komut hakkında daha fazla bilgi için şunu yazın `azCopy` ardından komut adı.

Örneğin, hakkında bilgi edinmek için `copy` , aşağıdakini yazın `azcopy copy`ve ardından ENTER tuşuna basın.

AzCopy ile anlamlı bir şey yapmak için önce yetkilendirme kimlik bilgilerini depolama hizmetine nasıl sağlarız karar vermeniz gerekir.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Yetkilendirme kimlik bilgilerini nasıl sağlarız seçin

Azure Active Directory (AD) kullanarak ya da bir paylaşılan erişim imzası (SAS) belirteci kullanarak Yetkilendirme kimlik bilgilerini sağlayabilir.

Bu tabloyu kılavuz olarak kullanın:

| Depolama türü | Şu anda desteklenen yetkilendirme yöntemi |
|--|--|
|**Blob depolama** | Azure AD & SAS |
|**BLOB Depolama (hiyerarşik ad alanı)** | Yalnızca Azure AD |
|**Dosya depolama** | Yalnızca SAS |

### <a name="option-1-use-azure-ad"></a>1. seçenek: Azure AD kullanma

Gereksinim duyduğunuz yetkilendirme düzeyi, dosyaları karşıya yükleme veya yalnızca karşıdan yüklemek planladığınız temel alır.

#### <a name="authorization-to-upload-files"></a>Dosyaları karşıya yüklemek için yetkilendirme

Bu rollerden kimliğinize atandığını doğrulayın:

- [Depolama Blob verileri katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Depolama Blob verileri sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Bu roller, kimliğinize herhangi birinde bu kapsamlara atanabilir:

- Kapsayıcı (dosya sistemi)
- Depolama hesabı
- Kaynak grubu
- Abonelik

Doğrulama ve roller atama konusunda bilgi almak için bkz: [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Bu rollerden erişim denetim listesine hedef kapsayıcının veya klasörün (ACL) kimliğinizi eklediyseniz, ıdentity'ye atanır olması gerekmez. ACL'de kimliğinizi izni hedef klasörde yazma ve kapsayıcı ve her bir üst klasörü üzerinde yürütme izni gerekiyor.

Daha fazla bilgi için bkz. [Azure Data Lake depolama Gen2'deki erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authorization-to-download-files"></a>Dosyaları indirmek için yetkilendirme

Bu rollerden kimliğinize atandığını doğrulayın:

- [Depolama Blob verileri okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader)
- [Depolama Blob verileri katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Depolama Blob verileri sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Bu roller, kimliğinize herhangi birinde bu kapsamlara atanabilir:

- Kapsayıcı (dosya sistemi)
- Depolama hesabı
- Kaynak grubu
- Abonelik

Doğrulama ve roller atama konusunda bilgi almak için bkz: [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Bu rollerden erişim denetim listesine hedef kapsayıcının veya klasörün (ACL) kimliğinizi eklediyseniz, ıdentity'ye atanır olması gerekmez. ACL'de kimliğinizi okuma izni hedef klasörde ve kapsayıcı ve her bir üst klasörü üzerinde yürütme izni gerekiyor.

Daha fazla bilgi için bkz. [Azure Data Lake depolama Gen2'deki erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-your-identity"></a>Kimliğinizi kimlik doğrulaması

Kimliğinizi gerekli yetki düzeyini verildi doğruladıktan sonra bir komut istemi açın, aşağıdaki komutu yazın ve ardından ENTER tuşuna basın.

```azcopy
azcopy login
```

Bu komut, kimlik doğrulaması kodu ve bir Web sitesinin URL'sini döndürür. Web sitesini açmak, kodu sağlayın ve ardından **sonraki** düğmesi.

![Bir kapsayıcı oluşturma](media/storage-use-azcopy-v10/azcopy-login.png)

Bir oturum açma penceresi görünür. Bu pencerede, Azure hesabı kimlik bilgilerinizi kullanarak Azure hesabınızda oturum açın. Oturumunuz başarıyla açıldıktan sonra tarayıcı penceresini kapatın ve AzCopy kullanarak başlayın.

### <a name="option-2-use-a-sas-token"></a>2. seçenek: Bir SAS belirteci kullanabilir

Her kaynak veya hedef URL'si için bir SAS belirteci, AzCopy komutları ekleyebilirsiniz.

Bu örnek komut yinelemeli olarak verileri yerel bir dizinden bir blob kapsayıcıya kopyalar. Kurgusal bir SAS belirteci sonuna eklenir kapsayıcı URL'si.

```azcopy
azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

SAS belirteçleri ve bir edinme hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="transfer-files"></a>Dosya aktarımı

Kimliği doğrulanmış kimlik bilgilerinizi veya bir SAS belirteci elde sonra dosyaları aktarma başlayabilirsiniz.

Örnek komutlar bulmak için şu makalelerden herhangi birine bakın.

- [AzCopy ve blob depolama ile veri aktarma](storage-use-azcopy-blobs.md)

- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

- [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme

Bkz: [yapılandırma, en iyi duruma getirmek ve AzCopy sorun giderme](storage-use-azcopy-configure.md)

## <a name="use-azcopy-in-storage-explorer"></a>Depolama Gezgini'nde Azcopy'yi kullanma

Daha sonra AzCopy performans avantajlarını artırmak istiyorsanız, ancak dosyalarınızı ile etkileşim kurmak için Depolama Gezgini'ni yerine komut satırını kullanmayı tercih, AzCopy depolama Gezgini'nde etkinleştirin.

Depolama Gezgini'nde **Önizleme**->**geliştirilmiş Blob karşıya yükleme ve indirme kullanımı AzCopy**.

![AzCopy, Azure depolama Gezgini'nde bir aktarım altyapısı olarak etkinleştir](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> Depolama hesabınızdaki bir hiyerarşik ad alanı etkinleştirdiyseniz, bu ayarı etkinleştirmeniz gerekmez. Depolama Gezgini'ni AzCopy hiyerarşik ad alanı olan depolama hesapları otomatik olarak kullanır. olmasıdır.  

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>AzCopy önceki sürümünü kullanın

AzCopy (AzCopy v8.1) önceki sürümünü kullanmanız gerekiyorsa, aşağıdaki bağlantılardan birini bakın:

- [(V8) Windows üzerinde AzCopy](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)
- [(V8) Linux üzerinde AzCopy](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="next-steps"></a>Sonraki adımlar

Sorular, sorunları veya genel bir geri bildirim varsa, bunları gönderme [github'da](https://github.com/Azure/azure-storage-azcopy) sayfası.
