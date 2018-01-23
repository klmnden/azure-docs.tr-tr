Azure kaynaklarınızı mantıksal olarak kategorilere ayırmak için etiketler uygulayabilirsiniz. Her etiket bir ad ve değerden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz. Bu etiket olmadan, bir kaynağın geliştirme, test veya üretime yönelik olduğunu belirlemekte zorluk yaşayabilirsiniz. Ancak, "Ortam" ve "Üretim" yalnızca örnektir. Aboneliğinizi düzenlemek için en mantıklı ad ve değerleri siz tanımlarsınız.

Etiketleri uyguladıktan sonra aboneliğinizde bu etiket adını ve değerini taşıyan tüm kaynakları alabilirsiniz. Etiketler farklı kaynak gruplarında bulunan ilgili kaynakları almanızı sağlar. Bu yaklaşım, faturalama veya yönetim için kaynakları düzenlemeniz gerektiğinde yararlıdır.

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Her kaynak veya kaynak grubu en fazla 15 etiket adı/değer çifti içerebilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Kaynak grupları, her biri 15 etiket adı/değer çiftine sahip çok sayıda kaynak içerebilir. Bir kaynak ile ilişkilendirmeniz gereken 15'ten fazla değer varsa, etiket değeri için JSON dizesi kullanın. JSON dizesi, tek etiket adına uygulanan birden fazla değer içerebilir. Bu makalede, etikete bir JSON dizesi atama örneği gösterilmektedir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.
* Bu karakterler desteklenmez:
  * `<`
  * `>`
  * `%`
  * `&`
  * `\\`
  * `?`
  * `/`
