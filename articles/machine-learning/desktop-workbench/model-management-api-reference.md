---
title: Azure Machine Learning Model yönetimi için bir Docker görüntüsü oluşturun. | Microsoft Docs
description: Bu makalede web hizmetiniz için bir Docker görüntüsü oluşturmak için gereken adımlar açıklanmaktadır.
services: machine-learning
author: chhavib
ms.author: chhavib
manager: hjerez
editor: jasonwhowell
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: c0f51e47038737d6aa743be718ad6b28c161c766
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651061"
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Azure Machine Learning Model Yönetimi hesabı API Başvurusu

Dağıtım ortamını ayarlama hakkında daha fazla bilgi için bkz. [Model Yönetimi hesabı kurulumu](deployment-setup-configuration.md).

Azure Machine Learning Model Yönetimi hesabı API aşağıdaki işlemleri gerçekleştirir:

- Model kaydı
- Bildirim oluşturma
- Docker görüntüsü oluşturma
- Web hizmeti oluşturma

Bu görüntü, yerel veya uzak bir Azure Container Service kümesi ya da tercih ettiğiniz başka bir Docker desteklenen ortam üzerinde bir web hizmeti oluşturmak için kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Yükleme adımları aracılığıyla gitti olduğundan emin olun [yükleme ve oluşturma Hızlı Başlangıç](../service/quickstart-installation.md) belge.

Devam etmeden önce aşağıdakiler gereklidir:
1. Model Yönetimi hesabı sağlama
2. Modelleri dağıtmak ve yönetmek için ortam oluşturma
3. Machine Learning modeli

### <a name="azure-ad-token"></a>Azure AD belirteç
Azure CLI'yı kullanırken kullanarak oturum açın `az login`. CLI, Azure Active Directory (Azure AD) belirtecinizi .azure dosyasından kullanır. API'leri kullanmak istiyorsanız, aşağıdaki seçenekleri vardır.

##### <a name="acquire-the-azure-ad-token-manually"></a>Azure AD belirteçlerini el ile Al
Kullanabileceğiniz `az login` ve en son, giriş dizininize .azure dosyasından belirteci alma.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Program aracılığıyla Azure AD belirtecini Al
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Hizmet sorumlusu oluşturduktan sonra çıkış kaydedin. Şimdi, Azure AD'den bir belirteç almak için kullanabilirsiniz:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
Belirteç bir yetkilendirme üst bilgisi API çağrıları için konur.


## <a name="register-a-model"></a>Bir modeli kaydedin

Model kayıt adımı, oluşturduğunuz Azure Model Yönetimi hesabı ile makine öğrenimi modelinizi kaydeder. Bu kayıt, modelleri ve kayıt zamanında atanan sürümlerine izleme sağlar. Kullanıcı, model adını sağlar. Sonraki kayıt modelleri aynı adla yeni bir sürüm ve kimliği oluşturur.

### <a name="request"></a>İstek

| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Açıklama
Bir model kaydeder.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| model | body | Bir model kaydetmek için kullanılan yükü. | Evet | [Model](#model) |


### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Tamam. Model kaydı başarılı oldu. | [Model](#model) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Sorgu bir hesapta modellerin listesi
### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Açıklama
Bir hesapta modellerin listesi sorgular. Etiket ve ada göre sonuç listesini filtreleyebilirsiniz. Filtre iletilmezse, sorgu hesaptaki tüm modelleri listeler. Döndürülen listeden sayfalandırılmış ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| ad | sorgu | Nesne adı. | Hayır | dize |
| etiket | sorgu | Model etiketi. | Hayır | dize |
| count | sorgu | Bir sayfa alınacak öğelerin sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfayı almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedModelList](#paginatedmodellist) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Model ayrıntılarını Al
### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{id}  
 |

### <a name="description"></a>Açıklama
Kimliğe göre bir modeli alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Model](#model) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Bir bildirim kayıtlı modeli ve tüm bağımlılıkları ile kaydetme

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Açıklama
Bir bildirim kayıtlı modeli ve tüm bağımlılıklarını kaydeder.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| manifestRequest | body | Bir bildirim kaydetmek için kullanılan yükü. | Evet | [Bildirimi](#manifest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Bildirim kaydı başarılı oldu. | [Bildirimi](#manifest) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Sorgu bildirimleri bir hesapta listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Açıklama
Bir hesapta bildirim listesini sorgular. Sonuç listesi modeli Kimliğine göre filtreleyebilir ve bildirim adı. Filtre iletilmezse, sorgu hesaptaki tüm bildirimleri listeler. Döndürülen listeden sayfalandırılmış ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| ModelID | sorgu | Model kimliği | Hayır | dize |
| manifestName | sorgu | Bildirim adı. | Hayır | dize |
| count | sorgu | Bir sayfa alınacak öğelerin sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfayı almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedManifestList](#paginatedmanifestlist) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Bildirim ayrıntılarını Al

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{id} | 

### <a name="description"></a>Açıklama
Bildirim kimliğe göre alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Bildirimi](#manifest) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Görüntü oluştur

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Açıklama
Bir Docker görüntüsü Azure Container Registry'de bir görüntü oluşturur.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| imageRequest | body | Bir görüntü oluşturmak için kullanılan yükü. | Evet | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. Bir GET çağrısı görüntü oluşturma görevinin durumunu gösterir. | İşlem konumu |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Sorgu bir hesapta görüntüleri listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Açıklama
Bir hesapta görüntülerin listesini sorgular. Bildirim kimliği ve adına göre sonuç listesini filtreleyebilirsiniz. Filtre iletilmezse, sorgu hesaptaki tüm görüntüleri listeler. Döndürülen listeden sayfalandırılmış ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| Bildirim kimliği | sorgu | Bildirim kimliği | Hayır | dize |
| manifestName | sorgu | Bildirim adı. | Hayır | dize |
| count | sorgu | Bir sayfa alınacak öğelerin sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfayı almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedImageList](#paginatedimagelist) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Görüntü ayrıntılarını Al

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{id} | 

### <a name="description"></a>Açıklama
Kimliğe göre bir görüntü alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Görüntü Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Görüntü](#image) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Hizmet oluşturma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Açıklama
Bir görüntüden bir hizmet oluşturur.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| serviceRequest | body | Bir hizmet oluşturmak için kullanılan yükü. | Evet | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. Bir GET çağrısı ve hizmet oluşturma görevi durumunu gösterir. | İşlem konumu |
| 409 | Belirtilen ada sahip bir hizmet zaten var. |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Sorgu bir hesapta hizmetlerin listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Açıklama
Bir hesapta hizmetlerin listesi sorgular. Model adı/kimliği, bildirim ad/kimliği, görüntü kimliği, hizmet adı veya Machine Learning işlem kaynak kimliği. sonuç listesini filtreleyebilirsiniz. Filtre iletilmezse, sorgu hesabındaki tüm hizmetler listelenir. Döndürülen listeden sayfalandırılmış ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| serviceName | sorgu | Hizmet adı. | Hayır | dize |
| ModelID | sorgu | Model adı. | Hayır | dize |
| modelName | sorgu | Model kimliği | Hayır | dize |
| Bildirim kimliği | sorgu | Bildirim kimliği | Hayır | dize |
| manifestName | sorgu | Bildirim adı. | Hayır | dize |
| imageId | sorgu | Görüntü Kimliği | Hayır | dize |
| computeResourceId | sorgu | Machine Learning işlem kaynak kimliği | Hayır | dize |
| count | sorgu | Bir sayfa alınacak öğelerin sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfayı almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedServiceList](#paginatedservicelist) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Hizmet ayrıntılarını Al

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Açıklama
Bir hizmet kimliğine göre alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [ServiceResponse](#serviceresponse) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Bir hizmeti güncelleştirin

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| PUT |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Açıklama
Mevcut bir hizmet güncelleştirir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| serviceUpdateRequest | body | Varolan bir hizmeti güncelleştirmek için kullanılan yükü. | Evet |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. Bir GET çağrısı güncelleştirme hizmeti görevin durumunu gösterir. | İşlem konumu |
| 404 | Belirtilen Kimliğe sahip bir hizmet yok. |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Bir hizmeti Sil

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| DELETE |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Açıklama
Bir hizmet siler.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. |  |
| 204 | Belirtilen Kimliğe sahip bir hizmet yok. |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Hizmet anahtarları alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>Açıklama
Hizmet anahtarları alır.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Hizmet kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [AuthKeys](#authkeys)
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Hizmet anahtarları yeniden oluştur

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  / API'si/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / anahtarlarını yeniden oluştur | 

### <a name="description"></a>Açıklama
Hizmet anahtarlarını yeniden oluşturur ve döndürür.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Hizmet kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| regenerateKeyRequest | body | Varolan bir hizmeti güncelleştirmek için kullanılan yükü. | Evet | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [AuthKeys](#authkeys)
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Bir hesapta dağıtımını sorgulama

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments | 

### <a name="description"></a>Açıklama
Bir hesapta dağıtımını sorgular. Sonuç listesi, belirli bir hizmet için oluşturulan dağıtımlarda döndüreceği hizmet Kimliğine göre filtreleyebilirsiniz. Filtre iletilmezse, sorgu hesabı içindeki tüm dağıtımlarda listeler.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |
| serviceId | sorgu | Hizmet kimliği | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [DeploymentList](#deploymentlist) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Dağıtım ayrıntılarını Al

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments/{id} | 

### <a name="description"></a>Açıklama
Dağıtım kimliğine göre alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | Dağıtım kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Dağıtım](#deployment) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>İşlem ayrıntıları alın

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/operations/{id} | 

### <a name="description"></a>Açıklama
İşlem kimliğine göre zaman uyumsuz işlem durumunu alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model Yönetimi hesabını bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model Yönetimi hesabının adı. | Evet | dize |
| id | yol | İşlem kimliği. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." gibi bir şey olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [OperationStatus](#asyncoperationstatus) |
| default | İşlemin neden başarısız olduğunu belirten bir hata yanıtı. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Tanımlar

<a name="asset"></a>
### <a name="asset"></a>Varlık
Docker görüntüsü oluşturma sırasında gerekli varlık nesnesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kimliği**  <br>*İsteğe bağlı*|Varlık Kimliği|dize|
|**mime türü**  <br>*İsteğe bağlı*|Model içeriğin MIME türü. MIME türü hakkında daha fazla bilgi için bkz: [IANA medya türlerinin listesini](https://www.iana.org/assignments/media-types/media-types.xhtml).|dize|
|**paketten**  <br>*İsteğe bağlı*|Burada Docker görüntüsü oluşturma sırasında içerik paketinden ihtiyacımız gösterir.|boole|
|**URL**  <br>*İsteğe bağlı*|Varlık konum URL'si.|dize|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Zaman uyumsuz işlemin durumu.

*Tür*: sabit listesi (NotStarted, çalışan, iptal edildi, başarılı, başarısız)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
İşlem durumu.


|Ad|Açıklama|Şema|
|---|---|---|
|**oluşturulma zamanı**  <br>*İsteğe bağlı*  <br>*salt okunur*|Zaman uyumsuz işlemi oluşturma saati (UTC).|dize (tarih)|
|**endTime**  <br>*İsteğe bağlı*  <br>*salt okunur*|Zaman uyumsuz işlem bitiş saati (UTC).|dize (tarih)|
|**Hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|
|**Kimliği**  <br>*İsteğe bağlı*|Zaman uyumsuz işlem kimliği.|dize|
|**OperationType**  <br>*İsteğe bağlı*|Zaman uyumsuz işlem türü.|Enum (görüntü, hizmet)|
|**resourceLocation**  <br>*İsteğe bağlı*|Kaynak oluşturulamaz veya zaman uyumsuz işlemi tarafından güncelleştirildi.|dize|
|**durumu**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Bir hizmet için kimlik doğrulaması anahtarları.


|Ad|Açıklama|Şema|
|---|---|---|
|**primaryKey**  <br>*İsteğe bağlı*|Birincil anahtar.|dize|
|**ikincil anahtarı oluşturma**  <br>*İsteğe bağlı*|İkincil anahtarı.|dize|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>Otomatik Ölçeklendiricinin
Otomatik ölçeklendiricinin ayarları.


|Ad|Açıklama|Şema|
|---|---|---|
|**autoscaleEnabled**  <br>*İsteğe bağlı*|Etkinleştirmek veya otomatik ölçeklendiricinin devre dışı bırakın.|boole|
|**maxReplicas**  <br>*İsteğe bağlı*|Kadar ölçeklendirme yapmanızı, pod çoğaltmalarının sayısı üst sınırı.  <br>**En düşük değer**: `1`|integer|
|**minReplicas**  <br>*İsteğe bağlı*|Pod çoğaltmalarının aşağı ölçeklendirme için en küçük sayısı.  <br>**En düşük değer**: `0`|integer|
|**refreshPeriodInSeconds**  <br>*İsteğe bağlı*|Otomatik ölçeklendirme tetikleyicisinin zaman yenileyin.  <br>**En düşük değer**: `1`|integer|
|**targetUtilization**  <br>*İsteğe bağlı*|Otomatik ölçeklendirme tetikler kullanım yüzdesi.  <br>**En düşük değer**: `0`  <br>**Maksimum değer**: `100`|integer|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Machine Learning işlem kaynağı.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kimliği**  <br>*İsteğe bağlı*|Kaynak kimliği.|dize|
|**type**  <br>*İsteğe bağlı*|Kaynak türü.|Enum (küme)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Küme içindeki bir kapsayıcı için kaynakları ayırmak için yapılandırma.


|Ad|Açıklama|Şema|
|---|---|---|
|**CPU**  <br>*İsteğe bağlı*|CPU rezervasyon belirtir. Kubernetes için biçim: bkz [anlamı, CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|dize|
|**Bellek**  <br>*İsteğe bağlı*|Bellek ayırma belirtir. Kubernetes için biçim: bkz [bellek anlamını](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|dize|


<a name="deployment"></a>
### <a name="deployment"></a>Dağıtım
Azure Machine Learning dağıtımının bir örneği.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdAt**  <br>*İsteğe bağlı*  <br>*salt okunur*|Dağıtım oluşturma zamanı (UTC).|dize (tarih)|
|**expiredAt**  <br>*İsteğe bağlı*  <br>*salt okunur*|Dağıtım süresi saati (UTC).|dize (tarih)|
|**Kimliği**  <br>*İsteğe bağlı*|Dağıtım kimliği|dize|
|**imageId**  <br>*İsteğe bağlı*|Bu dağıtımla ilişkili resim kimliği.|dize|
|**serviceName**  <br>*İsteğe bağlı*|Hizmet adı.|dize|
|**Durumu**  <br>*İsteğe bağlı*|Geçerli dağıtım durumu.|dize|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Dağıtım nesneleri dizisi.

*Tür*: <[dağıtım](#deployment)> dizi


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Model Yönetimi Hizmeti hata ayrıntısı.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kod**  <br>*Gerekli*|Hata kodu.|dize|
|**İleti**  <br>*Gerekli*|Hata iletisi.|dize|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Model Yönetimi Hizmeti hata nesnesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kod**  <br>*Gerekli*|Hata kodu.|dize|
|**Ayrıntıları**  <br>*İsteğe bağlı*|Hata ayrıntı nesneleri dizisi.|<[ErrorDetail](#errordetail)> dizi|
|**İleti**  <br>*Gerekli*|Hata iletisi.|dize|
|**statusCode**  <br>*İsteğe bağlı*|HTTP durum kodu.|integer|


<a name="image"></a>
### <a name="image"></a>Görüntü
Azure Machine Learning resim.


|Ad|Açıklama|Şema|
|---|---|---|
|**computeResourceId**  <br>*İsteğe bağlı*|Machine Learning işlem kaynağında oluşturulan ortam kimliği.|dize|
|**oluşturulma zamanı**  <br>*İsteğe bağlı*|Görüntü oluşturma zamanı (UTC).|dize (tarih)|
|**creationState**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|
|**Açıklaması**  <br>*İsteğe bağlı*|Resim açıklama metni.|dize|
|**Hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|
|**Kimliği**  <br>*İsteğe bağlı*|Görüntü Kimliği|dize|
|**imageBuildLogUri**  <br>*İsteğe bağlı*|Karşıya yüklediğiniz günlükler görüntü yapıdan URI'si.|dize|
|**imageLocation**  <br>*İsteğe bağlı*|Oluşturulan görüntüyü Azure Container Registry konumu dizesi.|dize|
|**ImageType**  <br>*İsteğe bağlı*||[ImageType](#imagetype)|
|**Bildirimi**  <br>*İsteğe bağlı*||[Bildirimi](#manifest)|
|**Adı**  <br>*İsteğe bağlı*|Görüntü adı.|dize|
|**Sürüm**  <br>*İsteğe bağlı*|Görüntü sürümü Model Yönetimi hizmeti tarafından ayarlayın.|integer|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Azure Machine Learning görüntü oluşturma isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**computeResourceId**  <br>*Gerekli*|Machine Learning işlem kaynağında oluşturulan ortam kimliği.|dize|
|**Açıklaması**  <br>*İsteğe bağlı*|Resim açıklama metni.|dize|
|**ImageType**  <br>*Gerekli*||[ImageType](#imagetype)|
|**manifestId**  <br>*Gerekli*|Görüntü oluşturulacak bildirim kimliği.|dize|
|**Adı**  <br>*Gerekli*|Görüntü adı.|dize|


<a name="imagetype"></a>
### <a name="imagetype"></a>Resim Türü
Resim türünü belirtir.

*Tür*: sabit listesi (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Bildirim
Azure Machine Learning bildirimi.


|Ad|Açıklama|Şema|
|---|---|---|
|**Varlıklar**  <br>*Gerekli*|Varlıklar listesi.|<[Varlık](#asset)> dizi|
|**oluşturulma zamanı**  <br>*İsteğe bağlı*  <br>*salt okunur*|Bildirim oluşturma zamanı (UTC).|dize (tarih)|
|**Açıklaması**  <br>*İsteğe bağlı*|Açıklama metin bildirimi.|dize|
|**driverProgram**  <br>*Gerekli*|Bildirimin sürücü program.|dize|
|**Kimliği**  <br>*İsteğe bağlı*|Bildirim kimliği|dize|
|**modelIds**  <br>*İsteğe bağlı*|Model kimliklerini kayıtlı modellerin listesi. Dahil edilen modelleri hiçbirini kayıtlı olmayan isteği başarısız olur.|<string> Dizi|
|**modelType**  <br>*İsteğe bağlı*|Modelleri, Model Yönetimi Hizmeti ile zaten kayıtlı belirtir.|Enum (kayıtlı)|
|**Adı**  <br>*Gerekli*|Bildirim adı.|dize|
|**targetRuntime**  <br>*Gerekli*||[TargetRuntime](#targetruntime)|
|**Sürüm**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model Yönetimi hizmeti tarafından atanan bildirim sürümü.|integer|
|**webserviceType**  <br>*İsteğe bağlı*|Bildirimden oluşturulacak web hizmeti istenen türünü belirtir.|Enum (gerçek zamanlı)|


<a name="model"></a>
### <a name="model"></a>Model
Bir Azure Machine Learning model örneği.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdAt**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model oluşturma zamanı (UTC).|dize (tarih)|
|**Açıklaması**  <br>*İsteğe bağlı*|Model açıklama metni.|dize|
|**Kimliği**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model kimliği|dize|
|**mime türü**  <br>*Gerekli*|Model içeriğin MIME türü. MIME türü hakkında daha fazla bilgi için bkz: [IANA medya türlerinin listesini](https://www.iana.org/assignments/media-types/media-types.xhtml).|dize|
|**Adı**  <br>*Gerekli*|Model adı.|dize|
|**etiketler**  <br>*İsteğe bağlı*|Model etiket listesi.|<string> Dizi|
|**paketten**  <br>*İsteğe bağlı*|Docker görüntüsü oluşturma sırasında model Cihazınızı kutusundan çıkarma için ihtiyacımız olmadığını gösterir.|boole|
|**URL**  <br>*Gerekli*|Model URL'si. Genellikle biz bir paylaşılan erişim imzası URL'SİNİN buraya koyun.|dize|
|**Sürüm**  <br>*İsteğe bağlı*  <br>*salt okunur*|Model Yönetimi hizmeti tarafından atanan model sürümü.|integer|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Model veri koleksiyonu bilgileri.


|Ad|Açıklama|Şema|
|---|---|---|
|**eventHubEnabled**  <br>*İsteğe bağlı*|Bir hizmet için bir olay hub'ı etkinleştirin.|boole|
|**storageEnabled**  <br>*İsteğe bağlı*|Depolama hizmeti için etkinleştirin.|boole|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Sayfalandırılmış listesini görüntüler.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesinde sonraki sayfasına için devamlılık bağlantı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Model nesneleri dizisi.|<[Görüntü](#image)> dizi|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Bildirimleri sayfalandırılmış bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesinde sonraki sayfasına için devamlılık bağlantı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Bildirim nesneler dizisi.|<[Bildirim](#manifest)> dizi|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Modelleri sayfalandırılmış bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesinde sonraki sayfasına için devamlılık bağlantı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Model nesneleri dizisi.|<[Model](#model)> dizi|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Hizmet sayfalandırılmış bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesinde sonraki sayfasına için devamlılık bağlantı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Hizmet nesneleri dizisi.|<[ServiceResponse](#serviceresponse)> dizi|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Bir hizmet oluşturma isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights'ı etkinleştirin.|boole|
|**Otomatik Ölçeklendiricinin**  <br>*İsteğe bağlı*||[Otomatik Ölçeklendiricinin](#autoscaler)|
|**ComputeResource**  <br>*Gerekli*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Gerekli*|Bir hizmet oluşturmak için görüntü.|dize|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**: `1`|integer|
|**Adı**  <br>*Gerekli*|Hizmet adı.|dize|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalışan pod çoğaltmalarının sayısı. Otomatik Ölçeklendiricinin etkinleştirilip etkinleştirilmeyeceğini belirtilemez.  <br>**En düşük değer**: `0`|integer|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Bir hizmet için bir anahtar isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**KeyType**  <br>*İsteğe bağlı*|Hangi anahtarını yeniden oluşturmak için belirtir.|Enum (birincil, ikincil)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Hizmet ayrıntılı durumu.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdAt**  <br>*İsteğe bağlı*|Hizmet oluşturma zamanı (UTC).|dize (tarih)|
|**ID**  <br>*İsteğe bağlı*|Hizmet kimliği|dize|
|**Görüntü**  <br>*İsteğe bağlı*||[Görüntü](#image)|
|**Bildirimi**  <br>*İsteğe bağlı*||[Bildirimi](#manifest)|
|**Modelleri**  <br>*İsteğe bağlı*|Modellerin listesi.|<[Model](#model)> dizi|
|**Adı**  <br>*İsteğe bağlı*|Hizmet adı.|dize|
|**scoringUri**  <br>*İsteğe bağlı*|Puanlama hizmeti için URI.|dize|
|**durumu**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*İsteğe bağlı*|Son saat (UTC) güncelleştirin.|dize (tarih)|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights'ı etkinleştirin.|boole|
|**Otomatik Ölçeklendiricinin**  <br>*İsteğe bağlı*||[Otomatik Ölçeklendiricinin](#autoscaler)|
|**ComputeResource**  <br>*Gerekli*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**: `1`|integer|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalışan pod çoğaltmalarının sayısı. Otomatik Ölçeklendiricinin etkinleştirilip etkinleştirilmeyeceğini belirtilemez.  <br>**En düşük değer**: `0`|integer|
|**Hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>ServiceUpdateRequest
Bir hizmet güncelleştirme isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights'ı etkinleştirin.|boole|
|**Otomatik Ölçeklendiricinin**  <br>*İsteğe bağlı*||[Otomatik Ölçeklendiricinin](#autoscaler)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*İsteğe bağlı*|Bir hizmet oluşturmak için görüntü.|dize|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**: `1`|integer|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalışan pod çoğaltmalarının sayısı. Otomatik Ölçeklendiricinin etkinleştirilip etkinleştirilmeyeceğini belirtilemez.  <br>**En düşük değer**: `0`|integer|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Hedef çalışma zamanı türü.


|Ad|Açıklama|Şema|
|---|---|---|
|**Özellikleri**  <br>*Gerekli*||< string, string > eşleme|
|**runtimeType**  <br>*Gerekli*|Çalışma zamanı belirtir.|Enum (SparkPython, Python)|

