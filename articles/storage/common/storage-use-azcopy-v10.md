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
ms.openlocfilehash: 94aca33b2f12c1c39297221a856296dcca052b0f
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565796"
---
# <a name="get-started-with-azcopy"></a>AzCopy’i kullanmaya başlama

AzCopy, BLOB veya dosyalar için veya bir depolama hesabına kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makalede, AzCopy indirin, depolama hesabınıza bağlanın ve ardından dosya aktarımı yardımcı olur.

> [!NOTE]
> AzCopy **V10** AzCopy şu anda desteklenen sürümüdür.
>
> AzCopy kullanmanız gerekiyorsa **v8.1**, bkz: [önceki AzCopy kullanmanız](#previous-version) bu makalenin.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>AzCopy indirin

İlk olarak, AzCopy V10 yürütülebilir dosyayı bilgisayarınızda herhangi bir dizine indirin. 

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

> [!NOTE]
> Veri kitaplıklarından kopyalamak istiyorsanız, [Azure tablo depolama](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) hizmet ve sonra Yükle [AzCopy sürüm 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>AzCopy çalıştırın

Kolaylık olması için sistem yolunuzda kullanım kolaylığı için AzCopy yürütülebilir dizin konumunu eklemeyi düşünün. Bu şekilde yazabilirsiniz `azcopy` sisteminize herhangi bir dizinden.

Yolunuza AzCopy dizini eklememeyi seçerseniz, türü ve AzCopy yürütülebilir dosyasının konumuna dizinleri gerekecektir `azcopy` veya `.\azcopy` Windows PowerShell komut istemleri içinde.

Komutların bir listesini görmek için şunu yazın `azcopy -h` ve ardından ENTER tuşuna basın.

Belirli bir komut hakkında bilgi edinmek için yalnızca komut adını içerir (örneğin: `azcopy list -h`).

![Satır içi Yardım](media/storage-use-azcopy-v10/azcopy-inline-help.png)

> [!NOTE] 
> Azure depolama hesabınızdaki bir sahibi olarak, veri erişim izni otomatik olarak atanmamış. AzCopy ile anlamlı bir şey yapmak için önce yetkilendirme kimlik bilgilerini depolama hizmetine nasıl sağlarız karar vermeniz gerekir. 

## <a name="choose-how-youll-provide-authorization-credentials"></a>Yetkilendirme kimlik bilgilerini nasıl sağlarız seçin

Azure Active Directory (AD) kullanarak ya da bir paylaşılan erişim imzası (SAS) belirteci kullanarak Yetkilendirme kimlik bilgilerini sağlayabilir.

Bu tabloyu kılavuz olarak kullanın:

| Depolama türü | Şu anda desteklenen yetkilendirme yöntemi |
|--|--|
|**Blob depolama** | Azure AD & SAS |
|**BLOB Depolama (hiyerarşik ad alanı)** | Azure AD & SAS |
|**Dosya depolama** | Yalnızca SAS |

### <a name="option-1-use-azure-ad"></a>1\. seçenek: Azure AD kullanma

Azure AD kullanarak bir kez yerine her komut için bir SAS belirteci eklemek kimlik bilgilerini sağlayabilirsiniz.  

Gereksinim duyduğunuz yetkilendirme düzeyi, dosyaları karşıya yükleme veya yalnızca karşıdan yüklemek planladığınız temel alır.

Dosyaları indirmek istiyorsanız, doğrulama [depolama Blob verileri okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) , kullanıcı kimliği veya hizmet sorumlusu atanmıştır. 

> [!NOTE]
> Kullanıcı kimliklerini ve hizmet sorumluları, her bir tür *güvenlik sorumlusu*terimini kullanacağız. Bu nedenle *güvenlik sorumlusu* için bu makalenin geri kalanında.

Dosyaları yüklemek istiyorsanız, bu rollerden güvenlik sorumlunuzu atandığını doğrulayın:

- [Depolama Blob verileri katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Depolama Blob verileri sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Bu roller, kimliğinize herhangi birinde bu kapsamlara atanabilir:

- Kapsayıcı (dosya sistemi)
- Depolama hesabı
- Kaynak grubu
- Abonelik

Doğrulama ve roller atama konusunda bilgi almak için bkz: [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE] 
> RBAC rolü atamalarını yayılması için beş dakika sürebilir göz önünde bulundurun.

Bu rollerden erişim denetim listesine hedef kapsayıcı ya da dizinin (ACL), güvenlik sorumlusu eklediyseniz, güvenlik Sorumlusu'na atanan olması gerekmez. ACL'de güvenlik sorumlusu hedef dizin üzerinde yazma izni ve kapsayıcı ve her bir üst dizin yürütme izni gerekiyor.

Daha fazla bilgi için bkz. [Azure Data Lake depolama Gen2'deki erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-a-user-identity"></a>Bir kullanıcı kimliğini

Kullanıcı kimliğinizi gerekli yetki düzeyini verilen olduğunu doğruladıktan sonra bir komut istemi açın, aşağıdaki komutu yazın ve ardından ENTER tuşuna basın.

```azcopy
azcopy login
```

Birden fazla kuruluşa aitse, depolama hesabına ait olduğu kuruluş Kiracı Kimliğini içerir.

```azcopy
azcopy login --tenant-id=<tenant-id>
```

Değiştirin `<tenant-id>` yer tutucu depolama hesabına ait olduğu kuruluş Kiracı kimliği. Kiracı Kimliğini bulmak için seçin **Azure Active Directory > Özellikler > dizin kimliği** Azure portalında.

Bu komut, kimlik doğrulaması kodu ve bir Web sitesinin URL'sini döndürür. Web sitesini açmak, kodu sağlayın ve ardından **sonraki** düğmesi.

![Bir kapsayıcı oluşturma](media/storage-use-azcopy-v10/azcopy-login.png)

Bir oturum açma penceresi görünür. Bu pencerede, Azure hesabı kimlik bilgilerinizi kullanarak Azure hesabınızda oturum açın. Oturumunuz başarıyla açıldıktan sonra tarayıcı penceresini kapatın ve AzCopy kullanarak başlayın.

<a id="service-principal" />

#### <a name="authenticate-a-service-principal"></a>Bir hizmet sorumlusu kimlik doğrulaması

Bu, kullanıcı etkileşimi olmadan çalışan bir komut dosyası içinde AzCopy kullanmayı planlıyorsanız, harika bir seçenektir. 

Bu betiği çalıştırmadan önce etkileşimli olarak en az bir kez, hizmet sorumlusu kimlik bilgileri ile AzCopy sağlayabilmesi oturum gerekir.  Betiğinizi bu hassas bilgileri sağlamaya gerek kalmaz, bu kimlik bilgilerini bir güvenli ve şifrelenmiş dosyada depolanır.

İstemci gizli anahtarı kullanarak veya hizmet sorumlusunun uygulama kaydı ile ilişkilendirilmiş sertifika parolasını kullanarak hesabınızda oturum açmak. 

Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için bkz: [nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Hizmet sorumluları hakkında daha fazla genel bilgi için bkz: [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)

##### <a name="using-a-client-secret"></a>İstemci gizli anahtarı kullanarak

Başlangıç ayarlayarak `AZCOPY_SPA_CLIENT_SECRET` ortam değişkeni, bir hizmet sorumlusunun istemci gizli anahtarı için kullanıcının uygulama kaydı. 

> [!NOTE]
> Bu değer, komut isteminden ve ortamdaki değişken ayarlar işletim sisteminizin ayarladığınızdan emin olun. Böylece, değer yalnızca mevcut oturum için kullanılabilir.

Bu örnek nasıl PowerShell'de bunu gösterir.

```azcopy
$env:AZCOPY_SPA_CLIENT_SECRET="$(Read-Host -prompt "Enter key")"
```

> [!NOTE]
> Bu örnekte gösterildiği gibi bir komut istemi kullanmayı düşünün. Bu şekilde, istemci parolası, konsolun komut geçmişindeki görünmez. 

Ardından, aşağıdaki komutu yazın ve ardından ENTER tuşuna basın.

```azcopy
azcopy login --service-principal --application-id <application-id>
```

Değiştirin `<application-id>` yer tutucusunu, hizmet sorumlusunun uygulama kimliği ile kullanıcının uygulama kaydı.

##### <a name="using-a-certificate"></a>Bir sertifika kullanıyor

Yetkilendirme için kendi kimlik bilgilerini kullanmayı tercih ederseniz, uygulama kaydı için bir sertifika yükleyin ve sonra oturum açmak için bu sertifikayı kullanın.

Uygulama kaydınızı sertifikanızı karşıya yüklemeyi yanı sıra, ayrıca makine veya sanal makine AzCopy çalışacağı kaydedilen sertifikayı bir kopyasına sahip yapmanız gerekir. Bu sertifikanın kopyasını olması gerekir. PFX veya. PEM biçimlendirmek ve özel anahtar içermelidir. Özel anahtar parola korumalı olmalıdır. Windows kullanıyorsanız ve yalnızca bir sertifika deposunda sertifikanızı var, bu sertifika (özel anahtar dahil) bir PFX dosyasına aktarmak emin olun. Yönergeler için bkz [dışarı aktarma-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps)

Ardından, ayarlama `AZCOPY_SPA_CERT_PASSWORD` ortam değişkenine sertifika parolası.

> [!NOTE]
> Bu değer, komut isteminden ve ortamdaki değişken ayarlar işletim sisteminizin ayarladığınızdan emin olun. Böylece, değer yalnızca mevcut oturum için kullanılabilir.

Bu örnek nasıl PowerShell'de bunu gösterir.

```azcopy
$env:AZCOPY_SPA_CERT_PASSWORD="$(Read-Host -prompt "Enter key")"
```

Ardından, aşağıdaki komutu yazın ve ardından ENTER tuşuna basın.

```azcopy
azcopy login --service-principal --certificate-path <path-to-certificate-file>
```

Değiştirin `<path-to-certificate-file>` sertifika dosyasının göreli veya tam olarak nitelenmiş yolu ile yer tutucu. AzCopy, bu sertifika için yol kaydeder ancak sertifikanın bir kopyasını Kaydet değil, bu nedenle bu sertifikanın bir yerde sakladığınızdan emin olun.

> [!NOTE]
> Bu örnekte gösterildiği gibi bir komut istemi kullanmayı düşünün. Bu şekilde, parolanızı, konsolun komut geçmişindeki görünmez. 

### <a name="option-2-use-a-sas-token"></a>2\. seçenek: Bir SAS belirteci kullanabilir

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

- [Depolama, AzCopy ve Azure Stack ile veri aktarma](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#azcopy)

## <a name="use-azcopy-in-a-script"></a>Bir betikte Azcopy'yi kullanma

Bu betiği çalıştırmadan önce etkileşimli olarak en az bir kez, hizmet sorumlusu kimlik bilgileri ile AzCopy sağlayabilmesi oturum gerekir.  Betiğinizi bu hassas bilgileri sağlamaya gerek kalmaz, bu kimlik bilgilerini bir güvenli ve şifrelenmiş dosyada depolanır. Örnekler için bkz [, hizmet sorumlusu kimlik doğrulaması](#service-principal) bu makalenin.

AzCopy, zaman içinde [indirme bağlantısı](#download-and-install-azcopy) AzCopy yeni sürümlerini gösterir. Betiğinizi AzCopy yüklerse, betik AzCopy daha yeni bir sürümü betiğinizi bağımlı özellikler değiştirirse çalışmamaya başlayabilir. 

Bu sorunlarla karşılaşmamak için AzCopy'nın geçerli sürümü (beklemediğiniz değişen) statik bağlantı edinin. Bu şekilde, betiğinizi çalıştığı her zaman tam olarak aynı AzCopy sürümünü yükler.

Bağlantısını almak için şu komutu çalıştırın:

| İşletim sistemi  | Komut |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Linux için `--strip-components=1` üzerinde `tar` komut sürüm adı içerir ve bunun yerine ikili doğrudan geçerli klasöre ayıklar en üst düzey klasör kaldırır. Bu betik yeni bir sürümü ile güncelleştirilmesi sağlar `azcopy` yalnızca güncelleştirerek `wget` URL'si.

URL, bu komut çıktısında görüntülenir. Kodunuzu daha sonra AzCopy bu URL'yi kullanarak indirebilirsiniz.

| İşletim sistemi  | Komut |
|--------|-----------|
| **Linux** | `wget -O azcopyv10.tar https://azcopyvnext.azureedge.net/release20190301/azcopy_linux_amd64_10.0.8.tar.gz tar -xf azcopyv10.tar --strip-components=1 ./azcopy` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

## <a name="use-azcopy-in-storage-explorer"></a>Depolama Gezgini'nde Azcopy'yi kullanma

Daha sonra AzCopy performans avantajlarını artırmak istiyorsanız, ancak dosyalarınızı ile etkileşim kurmak için Depolama Gezgini'ni yerine komut satırını kullanmayı tercih, AzCopy depolama Gezgini'nde etkinleştirin. 

Depolama Gezgini'nde **Önizleme**->**geliştirilmiş Blob karşıya yükleme ve indirme kullanımı AzCopy**.

![AzCopy, Azure depolama Gezgini'nde bir aktarım altyapısı olarak etkinleştir](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> Depolama hesabınızdaki bir hiyerarşik ad alanı etkinleştirdiyseniz, bu ayarı etkinleştirmeniz gerekmez. Depolama Gezgini'ni AzCopy hiyerarşik ad alanı olan depolama hesapları otomatik olarak kullanır. olmasıdır.  

Depolama Gezgini işlemlerini gerçekleştirmek üzere hesap anahtarınızı kullanan depolama Gezgini'ne işaretinden sonra ek yetkilendirme kimlik bilgilerini sağlamanız gerekmez.

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>AzCopy önceki sürümünü kullanın

AzCopy (AzCopy v8.1) önceki sürümünü kullanmanız gerekiyorsa, aşağıdaki bağlantılardan birini bakın:

- [(V8) Windows üzerinde AzCopy](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)

- [(V8) Linux üzerinde AzCopy](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Yapılandırma, en iyi duruma getirmek ve AzCopy sorunlarını giderme

Bkz: [yapılandırma, en iyi duruma getirmek ve AzCopy sorun giderme](storage-use-azcopy-configure.md)

## <a name="next-steps"></a>Sonraki adımlar

Sorular, sorunları veya genel bir geri bildirim varsa, bunları gönderme [github'da](https://github.com/Azure/azure-storage-azcopy) sayfası.
