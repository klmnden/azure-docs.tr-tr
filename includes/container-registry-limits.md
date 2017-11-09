| Kaynak | Temel | Standart | Premium |
|---|---|---|---|---|
| Depolama | 10 Gib'den | 100 Gib'den| 500 Gib'den |
| Okuma işlemleri: dakika başına<sup>1, 2</sup> | 1k | 300k | 10.000 k |
| Yazma işlemleri: dakika başına<sup>1, 3</sup> | 100 | 500 | 2k |
| MB/sn bant genişliği karşıdan<sup>1</sup> | 30 | 60 | 100 |
| MB/sn bant genişliği karşıya<sup>1</sup> | 10 | 20 | 50 |
| Web kancaları | 2 | 10 | 100 |
| Coğrafi çoğaltma | Yok | Yok | [Desteklenen *(Önizleme)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *okuma işlemleri:*, *yazma işlemleri:*, ve *bant genişliği* minimum tahminleri. Kullanım gerektirdiğinden performansı artırmak için ACR çalışır.

<sup>2</sup> [docker çekme](https://docs.docker.com/registry/spec/api/#pulling-an-image) katmanları görüntü yanı sıra bildirim alma sayısına dayalı olarak birden çok okuma işlemleri için çevirir.

<sup>3</sup> [docker itme](https://docs.docker.com/registry/spec/api/#pushing-an-image) gönderilen gerekir Katmanlar sayısına göre birden çok yazma işlemleri için çevirir. A `docker push` içeren *okuma işlemleri:* varolan bir görüntü yönelik bir bildirim almak için.