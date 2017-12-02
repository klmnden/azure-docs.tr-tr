---
title: "Azure Machine Learning modeli yönetimi için Docker görüntü oluşturma | Microsoft Docs"
description: "Bu makalede, web hizmetiniz için bir Docker görüntüsü oluşturma adımları açıklanmaktadır."
services: machine-learning
author: chhavib
ms.author: chhavib
manager: neerajkh
editor: jasonwhowell
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 03e51ab298a08386f0094d6d0290aa1ec85d337f
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Azure Machine Learning modeli yönetim hesabı API'si başvurusu

Dağıtım ortamı ayarlama hakkında daha fazla bilgi için bkz: [Model yönetim hesabı kurulumu](deployment-setup-configuration.md).

Azure Machine Learning modeli yönetim hesabı API'si aşağıdaki işlemleri gerçekleştirir:

- Modeli kaydı
- Bildirim oluşturma
- Docker görüntüsü oluşturma
- Web hizmeti oluşturma

Bu görüntü, yerel veya uzak bir Azure kapsayıcı hizmeti kümesi ya da tercih ettiğiniz başka bir Docker desteklenen ortamını üzerinde bir web hizmeti oluşturmak için kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Yükleme adımlarını gitti olduğundan emin olun [yükleyin ve hızlı başlangıç oluşturmak](quickstart-installation.md) belge.

Devam etmeden önce aşağıdakiler gereklidir:
1. Yönetim hesabı sağlama modeli
2. Dağıtma ve yönetme modelleri ortamı oluşturma
3. Bir Machine Learning modeli

### <a name="azure-ad-token"></a>Azure AD simgesi
Azure CLI kullanırken kullanarak oturum açmasına `az login`. CLI .azure dosyasından, Azure Active Directory (Azure AD) belirteci kullanır. API'ları kullanmak istiyorsanız, aşağıdaki seçeneklere sahipsiniz.

##### <a name="acquire-the-azure-ad-token-manually"></a>Azure AD belirteci el ile Al
Kullanabileceğiniz `az login` ve en son, giriş dizininize .azure dosyasından belirteci alma.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Program aracılığıyla Azure AD belirteci alma
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Hizmet sorumlusu oluşturduktan sonra çıkış kaydedin. Şimdi, Azure AD'den bir belirteç almak üzere kullanabilirsiniz:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
Bir yetkilendirme üstbilgisinde API çağrıları için belirteç alın.


## <a name="register-a-model"></a>Bir model Kaydet

Model kayıt adımı, Machine Learning modelini oluşturduğunuz Azure Model yönetim hesabı ile kaydeder. Bu kayıt modelleri ve kayıt aynı anda atanır sürümlerine izleme sağlar. Kullanıcı modeli adı sağlar. Yeni sürüm ve kimliği aynı adla modellerin sonraki kayıt oluşturur

### <a name="request"></a>İstek

| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modeller 
 |
### <a name="description"></a>Açıklama
Bir model kaydeder.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| model | body | Bir model kaydetmek için kullanılan yükü. | Evet | [Modeli](#model) |


### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | TAMAM. Modeli kaydı başarılı oldu. | [Modeli](#model) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Sorgu bir hesap modellerinde listesi
### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modeller 
 |
### <a name="description"></a>Açıklama
Bir hesap modellerinde listesini sorgular. Sonuç listesi etiketi ve adına göre filtre uygulayabilirsiniz. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm modelleri listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| ad | sorgu | Nesne adı. | Hayır | Dize |
| Etiket | sorgu | Model etiketi. | Hayır | Dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | Dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedModelList](#paginatedmodellist) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Model ayrıntıları alma
### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /models/ {id}  
 |

### <a name="description"></a>Açıklama
Kimliğe göre bir modeli alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Nesne Kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Modeli](#model) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Bir bildirim kayıtlı modeli ve tüm bağımlılıkları ile kaydetme

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / bildirimleri | 

### <a name="description"></a>Açıklama
Bir bildirim kayıtlı modeli ve tüm bağımlılıklarını kaydeder.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| manifestRequest | body | Bir bildirim kaydetmek için kullanılan yükü. | Evet | [Bildirimi](#manifest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Bildirim kayıt başarılı oldu. | [Bildirimi](#manifest) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Sorgu bildirimleri bir hesap listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / bildirimleri | 

### <a name="description"></a>Açıklama
Bir hesap bildirimleri listesini sorgular. Model kimliği sonuç listeyi filtrelemek ve yayılma adı. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm bildirimler listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| modelId | sorgu | Model kimliği | Hayır | Dize |
| ManifestName | sorgu | Bildirim adı. | Hayır | Dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | Dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedManifestList](#paginatedmanifestlist) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Bildirim ayrıntıları

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /manifests/ {id} | 

### <a name="description"></a>Açıklama
Kimliğe göre bildirimini alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Nesne Kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Bildirimi](#manifest) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Görüntü oluştur

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / görüntüleri | 

### <a name="description"></a>Açıklama
Bir görüntü, Azure kapsayıcı kayıt defterinde Docker görüntü olarak oluşturur.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| imageRequest | body | Bir görüntü oluşturmak için kullanılan yükü. | Evet | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. GET çağrı görüntü oluşturma görevinin durumunu gösterir. | İşlemi konumu |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Sorgu bir hesap da görüntü listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / görüntüleri | 

### <a name="description"></a>Açıklama
Bir hesap görüntülerinde listesini sorgular. Sonuç listesi bildirim kimliği ve adına göre filtre uygulayabilirsiniz. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm görüntüleri listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| Bildirim kimliği | sorgu | Bildirim kimliği. | Hayır | Dize |
| ManifestName | sorgu | Bildirim adı. | Hayır | Dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | Dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedImageList](#paginatedimagelist) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Görüntü ayrıntıları alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /images/ {id} | 

### <a name="description"></a>Açıklama
Kimliğe göre bir görüntü alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Görüntü Kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Görüntü](#image) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Hizmet oluşturma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / Hizmetleri | 

### <a name="description"></a>Açıklama
Bir hizmeti bir görüntüden oluşturur.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| Servicerequest'te | body | Bir hizmet oluşturmak için kullanılan yükü. | Evet | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. GET çağrı hizmet oluşturma görevinin durumunu gösterir. | İşlemi konumu |
| 409 | Belirtilen ada sahip bir hizmet zaten var. |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Sorgu bir hesap hizmetlerin listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / Hizmetleri | 

### <a name="description"></a>Açıklama
Bir hesap hizmetlerin listesini sorgular. Model adı/kimliği, bildirim ad/kimliği, görüntü kimliği, hizmet adı veya Machine Learning işlem kaynak kimliği sonuç listesini filtreleyebilirsiniz Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm hizmetleri listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| ServiceName | sorgu | Hizmet adı. | Hayır | Dize |
| modelId | sorgu | Model adı. | Hayır | Dize |
| modelName | sorgu | Model kimliği | Hayır | Dize |
| Bildirim kimliği | sorgu | Bildirim kimliği. | Hayır | Dize |
| ManifestName | sorgu | Bildirim adı. | Hayır | Dize |
| imageId | sorgu | Görüntü Kimliği | Hayır | Dize |
| computeResourceId | sorgu | Machine Learning işlem kaynak kimliği | Hayır | Dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | Dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedServiceList](#paginatedservicelist) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Hizmet Ayrıntıları

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Açıklama
Kimliğe göre bir hizmetini alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Nesne Kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [ServiceResponse](#serviceresponse) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Bir hizmeti güncelleştir

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| PUT |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Açıklama
Var olan bir hizmeti güncelleştirir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Nesne Kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| serviceUpdateRequest | body | Var olan bir hizmeti güncelleştirmek için kullanılan yükü. | Evet |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. GET çağrı güncelleştirme hizmeti görevin durumunu gösterir. | İşlemi konumu |
| 404 | Belirtilen Kimliğe sahip hizmet yok. |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Bir hizmeti silin

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| DELETE |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} | 

### <a name="description"></a>Açıklama
Bir hizmet siler.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Nesne Kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. |  |
| 204 | Belirtilen Kimliğe sahip hizmet yok. |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Hizmet anahtarı alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / anahtarları | 

### <a name="description"></a>Açıklama
Hizmet anahtarları alır.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Hizmet kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [AuthKeys](#authkeys)
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Hizmet anahtarlarını yeniden oluştur

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / anahtarları | 

### <a name="description"></a>Açıklama
Hizmet anahtarlarını yeniden oluşturur ve bunları döndürür.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Hizmet kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| regenerateKeyRequest | body | Var olan bir hizmeti güncelleştirmek için kullanılan yükü. | Evet | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [AuthKeys](#authkeys)
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Sorgu bir hesapta dağıtımlarını listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / dağıtımları | 

### <a name="description"></a>Açıklama
Bir hesapta dağıtımlarını listesini sorgular. Sonuç listesi belirli bir hizmet için oluşturulan dağıtımlarda döndürülecek hizmeti Kimliğine göre filtreleyebilirsiniz. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm dağıtımlar listeler.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |
| ServiceId | sorgu | Hizmet kimliği | Hayır | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [DeploymentList](#deploymentlist) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Dağıtım ayrıntıları alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /deployments/ {id} | 

### <a name="description"></a>Açıklama
Kimliği dağıtılmasının alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | Dağıtım kimliği | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Dağıtım](#deployment) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>İşlem ayrıntıları alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /operations/ {id} | 

### <a name="description"></a>Açıklama
İşlemi kimliğe göre zaman uyumsuz işlem durumunu alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | Dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | Dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | Dize |
| id | yol | İşlem kimliği. | Evet | Dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | Dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | Dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [OperationStatus](#asyncoperationstatus) |
| varsayılan | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Tanımlar

<a name="asset"></a>
### <a name="asset"></a>Varlık
Docker görüntü oluşturma sırasında gerekli varlık nesnesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kimliği**  <br>*İsteğe bağlı*|Varlık Kimliği|Dize|
|**mime türü**  <br>*İsteğe bağlı*|Model içeriğin MIME türü. MIME türü hakkında daha fazla bilgi için bkz: [IANA medya türleri listesini](https://www.iana.org/assignments/media-types/media-types.xhtml).|Dize|
|**paketini açma**  <br>*İsteğe bağlı*|Burada Docker görüntü oluşturma sırasında içerik paketinden ihtiyacımız gösterir.|boole|
|**URL**  <br>*İsteğe bağlı*|Varlık konum URL'si.|Dize|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Zaman uyumsuz işlem durumu.

*Tür*: enum (NotStarted, çalışan, iptal edildi, başarılı, başarısız)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
İşlem durumu.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdTime**  <br>*İsteğe bağlı*  <br>*salt okunur*|Zaman uyumsuz işlemi oluşturma saati (UTC).|dize (tarih)|
|**endTime**  <br>*İsteğe bağlı*  <br>*salt okunur*|Zaman uyumsuz işlemi bitiş zamanı (UTC).|dize (tarih)|
|**hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|
|**Kimliği**  <br>*İsteğe bağlı*|Zaman uyumsuz işlem kimliği|Dize|
|**operationType**  <br>*İsteğe bağlı*|Zaman uyumsuz işlem türü.|Enum (görüntüsü, hizmet)|
|**resourceLocation**  <br>*İsteğe bağlı*|Kaynak veya zaman uyumsuz işlemin güncelleştirilemedi.|Dize|
|**durumu**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Bir hizmet için kimlik doğrulama anahtarları.


|Ad|Açıklama|Şema|
|---|---|---|
|**primaryKey**  <br>*İsteğe bağlı*|Birincil anahtar.|Dize|
|**secondaryKey**  <br>*İsteğe bağlı*|İkincil anahtar.|Dize|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Autoscaler yönelik ayarlar.


|Ad|Açıklama|Şema|
|---|---|---|
|**autoscaleEnabled**  <br>*İsteğe bağlı*|Etkinleştirmek veya devre dışı autoscaler.|boole|
|**maxReplicas**  <br>*İsteğe bağlı*|En fazla ölçeklendirmek için pod çoğaltmaları sayısının üst sınırı.  <br>**En düşük değer**:`1`|integer|
|**minReplicas**  <br>*İsteğe bağlı*|Aşağı ölçeklendirmek için pod çoğaltmaları minimum sayısı.  <br>**En düşük değer**:`0`|integer|
|**refreshPeriodInSeconds**  <br>*İsteğe bağlı*|Otomatik ölçeklendirmeyi tetikleyici süredir yenileyin.  <br>**En düşük değer**:`1`|integer|
|**targetUtilization**  <br>*İsteğe bağlı*|Otomatik ölçeklendirmeyi tetikler kullanım yüzdesi.  <br>**En düşük değer**:`0`  <br>**En büyük değer**:`100`|integer|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Machine Learning işlem kaynağı.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kimliği**  <br>*İsteğe bağlı*|Kaynak Kimliği|Dize|
|**türü**  <br>*İsteğe bağlı*|Kaynak türü.|Enum (küme)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Kaynaklar küme içindeki bir kapsayıcı için ayrılacak yapılandırması.


|Ad|Açıklama|Şema|
|---|---|---|
|**CPU**  <br>*İsteğe bağlı*|CPU ayırma belirtir. Kubernetes biçimi: bkz [anlamı, CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|Dize|
|**bellek**  <br>*İsteğe bağlı*|Bellek ayırma belirtir. Kubernetes biçimi: bkz [bellek anlamını](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|Dize|


<a name="deployment"></a>
### <a name="deployment"></a>Dağıtım
Bir Azure Machine Learning dağıtımının bir örneği.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdAt**  <br>*İsteğe bağlı*  <br>*salt okunur*|Dağıtım oluşturma zamanı (UTC).|dize (tarih)|
|**expiredAt**  <br>*İsteğe bağlı*  <br>*salt okunur*|Dağıtım süresi saat (UTC).|dize (tarih)|
|**Kimliği**  <br>*İsteğe bağlı*|Dağıtım kimliği|Dize|
|**imageId**  <br>*İsteğe bağlı*|Bu dağıtım ile ilişkili resim kimliği.|Dize|
|**serviceName**  <br>*İsteğe bağlı*|Hizmet adı.|Dize|
|**durumu**  <br>*İsteğe bağlı*|Geçerli dağıtım durumu.|Dize|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Dağıtım nesnelerinin bir dizisi.

*Tür*: <[dağıtım](#deployment)> dizisi


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Yönetim Hizmeti hata ayrıntısı model.


|Ad|Açıklama|Şema|
|---|---|---|
|**kod**  <br>*Gerekli*|Hata kodu.|Dize|
|**İleti**  <br>*Gerekli*|Hata iletisi.|Dize|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Model yönetim hizmeti hata nesnesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**kod**  <br>*Gerekli*|Hata kodu.|Dize|
|**Ayrıntıları**  <br>*İsteğe bağlı*|Hata ayrıntı nesneler dizisi.|<[ErrorDetail](#errordetail)> dizisi|
|**İleti**  <br>*Gerekli*|Hata iletisi.|Dize|
|**statusCode**  <br>*İsteğe bağlı*|HTTP durum kodu.|integer|


<a name="image"></a>
### <a name="image"></a>Görüntü
Azure Machine Learning resim.


|Ad|Açıklama|Şema|
|---|---|---|
|**computeResourceId**  <br>*İsteğe bağlı*|Machine Learning işlem kaynak oluşturulan ortam kimliği.|Dize|
|**createdTime**  <br>*İsteğe bağlı*|Görüntü oluşturma zamanı (UTC).|dize (tarih)|
|**creationState**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|
|**Açıklama**  <br>*İsteğe bağlı*|Görüntü açıklama metni.|Dize|
|**hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|
|**Kimliği**  <br>*İsteğe bağlı*|Görüntü Kimliği|Dize|
|**imageBuildLogUri**  <br>*İsteğe bağlı*|Karşıya yüklediğiniz günlükler görüntü yapıdan URI'si.|Dize|
|**ImageLocation**  <br>*İsteğe bağlı*|Oluşturulan görüntüyü Azure kapsayıcı kayıt defteri konumu dizesi.|Dize|
|**imageType**  <br>*İsteğe bağlı*||[ImageType](#imagetype)|
|**bildirimi**  <br>*İsteğe bağlı*||[Bildirimi](#manifest)|
|**adı**  <br>*İsteğe bağlı*|Görüntü adı.|Dize|
|**Sürüm**  <br>*İsteğe bağlı*|Model yönetim hizmeti tarafından kümesi görüntü sürümü.|integer|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Azure Machine Learning görüntü oluşturma isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**computeResourceId**  <br>*Gerekli*|Machine Learning işlem kaynak oluşturulan ortam kimliği.|Dize|
|**Açıklama**  <br>*İsteğe bağlı*|Görüntü açıklama metni.|Dize|
|**imageType**  <br>*Gerekli*||[ImageType](#imagetype)|
|**Bildirim kimliği**  <br>*Gerekli*|Görüntü oluşturulacak bildirimi kimliği.|Dize|
|**adı**  <br>*Gerekli*|Görüntü adı.|Dize|


<a name="imagetype"></a>
### <a name="imagetype"></a>Resim Türü
Resim türünü belirtir.

*Tür*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Bildirim
Azure Machine Learning bildirimi.


|Ad|Açıklama|Şema|
|---|---|---|
|**varlıklar**  <br>*Gerekli*|Varlıklar listesi.|<[Varlık](#asset)> dizisi|
|**createdTime**  <br>*İsteğe bağlı*  <br>*salt okunur*|Bildirim oluşturma zamanı (UTC).|dize (tarih)|
|**Açıklama**  <br>*İsteğe bağlı*|Açıklama metin bildirimi.|Dize|
|**driverProgram**  <br>*Gerekli*|Sürücü program bildiriminin.|Dize|
|**Kimliği**  <br>*İsteğe bağlı*|Bildirim kimliği.|Dize|
|**modelIds**  <br>*İsteğe bağlı*|Model kimliklerini kayıtlı modellerin listesi. Dahil edilen modellerinden herhangi biri kaydettirilmedi isteği başarısız olur.|<string>dizi|
|**modelType**  <br>*İsteğe bağlı*|Modeller, Model yönetim hizmetiyle zaten kayıtlı belirtir.|Enum (kayıtlı)|
|**adı**  <br>*Gerekli*|Bildirim adı.|Dize|
|**targetRuntime**  <br>*Gerekli*||[TargetRuntime](#targetruntime)|
|**Sürüm**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model yönetim hizmeti tarafından atanan bildirim sürüm.|integer|
|**webserviceType**  <br>*İsteğe bağlı*|Bildirimden oluşturulacak web hizmeti istenen türünü belirtir.|Enum (gerçek zamanlı)|


<a name="model"></a>
### <a name="model"></a>Model
Bir Azure Machine Learning model örneği.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdAt**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model oluşturma zamanı (UTC).|dize (tarih)|
|**Açıklama**  <br>*İsteğe bağlı*|Açıklama metnini model.|Dize|
|**Kimliği**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model kimliği|Dize|
|**mime türü**  <br>*Gerekli*|Model içeriğin MIME türü. MIME türü hakkında daha fazla bilgi için bkz: [IANA medya türleri listesini](https://www.iana.org/assignments/media-types/media-types.xhtml).|Dize|
|**adı**  <br>*Gerekli*|Model adı.|Dize|
|**etiketler**  <br>*İsteğe bağlı*|Model etiketi listesi.|<string>dizi|
|**paketini açma**  <br>*İsteğe bağlı*|Docker görüntü oluşturma sırasında model paketten ihtiyacımız olup olmadığını gösterir.|boole|
|**URL**  <br>*Gerekli*|Model URL'si. Genellikle biz bir paylaşılan erişim imzası URL'SİNİN buraya yerleştirin.|Dize|
|**Sürüm**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model yönetim hizmeti tarafından atanan model sürüm.|integer|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Model veri toplama bilgileri.


|Ad|Açıklama|Şema|
|---|---|---|
|**eventHubEnabled**  <br>*İsteğe bağlı*|Bir hizmet için bir olay hub'ı etkinleştirin.|boole|
|**storageEnabled**  <br>*İsteğe bağlı*|Bir hizmet için depolama etkinleştirin.|boole|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Numaralı listesini görüntüler.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|Dize|
|**değer**  <br>*İsteğe bağlı*|Model nesneleri dizisi.|<[Görüntü](#image)> dizisi|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Bildirimleri numaralı bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|Dize|
|**değer**  <br>*İsteğe bağlı*|Bildirim nesneler dizisi.|<[Bildirim](#manifest)> dizisi|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Modelleri numaralı bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|Dize|
|**değer**  <br>*İsteğe bağlı*|Model nesneleri dizisi.|<[Model](#model)> dizisi|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Hizmetleri numaralı bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|Dize|
|**değer**  <br>*İsteğe bağlı*|Hizmet nesneler dizisi.|<[ServiceResponse](#serviceresponse)> dizisi|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Bir hizmet oluşturma isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights etkinleştirin.|boole|
|**autoScaler**  <br>*İsteğe bağlı*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Gerekli*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Gerekli*|Bir hizmet oluşturmak için resim.|Dize|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**:`1`|integer|
|**adı**  <br>*Gerekli*|Hizmet adı.|Dize|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalıştıran pod çoğaltmaların sayısı. Autoscaler etkin olup olmadığını belirtilemez.  <br>**En düşük değer**:`0`|integer|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Bir hizmet için bir anahtarı yeniden oluşturmak için bir istek.


|Ad|Açıklama|Şema|
|---|---|---|
|**keyType**  <br>*İsteğe bağlı*|Yeniden oluşturmak için hangi anahtarını belirtir.|Enum (birincil, ikincil)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Hizmet ayrıntılı durumu.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdAt**  <br>*İsteğe bağlı*|Hizmet oluşturma zamanı (UTC).|dize (tarih)|
|**ID**  <br>*İsteğe bağlı*|Hizmet kimliği|Dize|
|**Görüntü**  <br>*İsteğe bağlı*||[Görüntü](#image)|
|**bildirimi**  <br>*İsteğe bağlı*||[Bildirimi](#manifest)|
|**modelleri**  <br>*İsteğe bağlı*|Model listesi.|<[Model](#model)> dizisi|
|**adı**  <br>*İsteğe bağlı*|Hizmet adı.|Dize|
|**scoringUri**  <br>*İsteğe bağlı*|Hizmet Puanlama için URI.|Dize|
|**durumu**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*İsteğe bağlı*|Son saat (UTC) güncelleştirin.|dize (tarih)|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights etkinleştirin.|boole|
|**autoScaler**  <br>*İsteğe bağlı*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Gerekli*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**:`1`|integer|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalıştıran pod çoğaltmaların sayısı. Autoscaler etkin olup olmadığını belirtilemez.  <br>**En düşük değer**:`0`|integer|
|**hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>serviceUpdateRequest
Bir hizmeti güncelleştirmek için bir istek.


|Ad|Açıklama|Şema|
|---|---|---|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights etkinleştirin.|boole|
|**autoScaler**  <br>*İsteğe bağlı*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*İsteğe bağlı*|Bir hizmet oluşturmak için resim.|Dize|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**:`1`|integer|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalıştıran pod çoğaltmaların sayısı. Autoscaler etkin olup olmadığını belirtilemez.  <br>**En düşük değer**:`0`|integer|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Hedef çalışma zamanı türü.


|Ad|Açıklama|Şema|
|---|---|---|
|**özellikleri**  <br>*Gerekli*||< dize, dize > eşleme|
|**runtimeType**  <br>*Gerekli*|Çalışma zamanı belirtir.|Enum (SparkPython, Python)|

