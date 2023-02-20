# Node API Challenge

## Konular

- RESTful API oluşturma.
- CRUD işlemleri oluşturma.
- API uç noktaları yazma.

## Bilgi

Kullanıcılar üzerinde CRUD işlemleri gerçekleştiren bir API oluşturmak için Node.js ve Express'i kullanın.

### Kurulum

- **Fork** ve **Klon** layın.
-  `npm install`.

### Veritabanı erişimi

Veritabanı erişimi, `data` klasörü içerisinde yer alan `db.js` dosyası kullanılarak yapılacaktır. Bu dosya aşağıdaki metodları içerir:

- `find()`: find çağrılması, veritabanında bulunan tüm kullanıcıların bir dizisine çözümlenen bir Promise döndürür.
- `findById()`: bu metot, tek parametre olarak bir `id` bekler ve sağlanan `id`e karşılık gelen kullanıcıyı veya o `id`ye sahip kullanıcı bulunamazsa boş bir dizi döndürür.
- `insert()`: insert'i çağırmak onu bir kullanıcı nesnesinden geçirmek, onu veritabanına ekleyecek ve eklenen kullanıcının 'kimliği' ile bir nesne döndürecektir. Nesne şöyle görünür: `{ id: 123 }`.
- `update()`: iki argüman kabul eder, ilki güncellenecek kullanıcının "kimliği" ve ikincisi uygulanacak "değişikliklere" sahip bir nesnedir. Güncellenen kayıtların sayısını döndürür. Sayı 1 ise, kaydın doğru bir şekilde güncellendiği anlamına gelir.
- `remove()`: remove yöntemi, ilk parametre olarak bir 'id' kabul eder ve kullanıcıyı veritabanından başarıyla sildikten sonra, silinen kayıtların sayısını döndürür.

Sağlanan veritabanına veri eklemek, güncellemek, kaldırmak ve veri almak için bir yolumuz olduğuna göre artık API üzerinde çalışma zamanı.

### API'yi Başlatın ve Gereksinimleri Uygulayın

- Sunucuyu başlatmak için kök klasörden (_package.json_ dosyasının bulunduğu yer) `npm run server` yazın. Sunucu, siz değişiklik yaptıkça otomatik olarak yeniden başlayacak şekilde yapılandırılmıştır.
- API gereksinimlerini uygulamak için gerekli kodu ekleyin.
- **Alıştırmalar üzerinde çalışırken _Postman_ kullanarak API'yi test edin.**

### Kullanıcı Şeması

Veritabanındaki kullanıcılar aşağıdaki nesne yapısına uygundur:

```js
{
  name: "Jane Doe", // String, required
  bio: "Not Tarzan's Wife, another Jane",  // String
  created_at: Mon Aug 14 2017 12:50:16 GMT-0700 (PDT) // Date, defaults to current date
  updated_at: Mon Aug 14 2017 12:50:16 GMT-0700 (PDT) // Date, defaults to current date
}
```

### Aşağıdaki sorguları gerçekleştirmek için uç noktaları yazın

`index.js` içine aşağıdaki _uç noktaları_'ı uygulamak için gerekli kodu ekleyin:


| Metod  | URL            | Açıklama                                                                                                                               |
| ------ | -------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| POST   | /api/users     | `İstek gövdesi` içinde gönderilen bilgileri kullanarak bir kullanıcı oluşturur.                                                        |
| GET    | /api/users     | Veritabanında bulunan tüm kullanıcı nesnelerinin bir dizisini döndürür.                                                                |
| GET    | /api/users/:id | Belirtilen `id` ile kullanıcı nesnesini döndürür.                                                                                      |
| DELETE | /api/users/:id | Belirtilen `id` ile kullanıcıyı kaldırır ve silinen kullanıcıyı döndürür                                                               |
| PUT    | /api/users/:id | `İstek gövdesinden` alınan verileri kullanarak kullanıcıyı belirtilen `id` ile günceller. Değiştirileni döndürür, **orijinali DEĞİL**. |

#### Uç Nokta Özellikeri

İstemci `/api/users` e `POST` isteği atarsa:

- Request bodyde `name` ya da  `bio` eksikse:

  - HTTP `400` durum kodu (Bad Request).
  - şu JSON'u cevapta döndürür: `{ message: "Lütfen kullanıcı için bir name ve bio sağlayın" }`.

- Eğer _user_ bilgileri geçerliyse:

  - Yeni _user_ i veritabanına kaydedin.
  - HTTP `201` (Created) durum kodu.
  - yeni oluşturulan _kullanıcı nesnesi_ id yi içerecek şekilde döndürün.

- _user_ kaydedilirken hata oluştuysa:
  - HTTP `500` (Server Error) durum kodu cevaplar.
  - Şu JSON'u döndürün: `{ message: "Veritabanına kaydedilirken bir hata oluştu" }`.

İstemci `/api/users` e `GET` isteği atarsa :

- Veritabanından kullanıcılar alınırken hata oluşursa:
  - HTTP `500` kodu yanıtlar.
  - Şu JSON'u döndürür: `{ message: "Kullanıcı bilgileri alınamadı" }`.

İstemci `/api/users/:id` 'e  `GET` isteği yaparsa :

- Eğer _user_ belirtilen `id` mevcut değilse:

  - HTTP `404` (Not Found) yanıtlar.
  - şu JSON'u döndürür: `{ message: "Belirtilen ID'li kullanıcı bulunamadı" }`.

- _user_ veritabanından alınırken bir hata oluşursa:
  - HTTP `500` yanıtlar.
  - şu JSON'u döndürür: `{ message: "Kullanıcı bilgisi alınamadı" }`.

İstemci `/api/users/:id` 'e `DELETE` isteği yaparsa :

- Eğer belirtilen `id` li kullanıcı mevcut değilse:

  - HTTP `404` (Not Found) yanıtlar.
  - şu JSON'u döndürür: `{ message: "Belirtilen ID li kullanıcı bulunamadı" }`.

- Eğer kullanıcı silinirken hata oluşursa:
  - HTTP `500` yanıtlar.
  - şu JSON'u döndürür: `{ message: "Kullanıcı silinemedi" }`.

İstemci `/api/users/:id` 'e  `PUT` isteği atarsa :

- Belirtilen `id` li _user_ yoksa:

  - HTTP `404` (Not Found).
  - şu JSON'u döndürür: `{ message: "Belirtilen ID'li kullanıcı bulunamadı" }`.

- Request bodysinde  `name` ya da `bio` yoksa:

  - HTTP `400` (Bad Request).
  - şu JSON'u döndürür: `{ message: "Lütfen kullanıcı için name ve bio sağlayın" }`.

- _user_ i güncellerken bir hata oluşursa:

  - HTTP `500`.
  - şu JSON'u döndürür: `{ message: "Kullanıcı bilgileri güncellenemedi" }`.

- Eğer kullanıcı varsa ve girilen bilgiler geçerliyse:

  - "istek gövdesi"nde gönderilen yeni bilgileri kullanarak veritabanındaki kullanıcı nesnesini güncelleyin.
  - HTTP `200` (OK).
  - güncellenen _user nesnesini_ döndürür.


## Esnek Görevler

Esnek görevler için `cors` ara yazılımını aktif etmeniz gerekecek. Şu adımları takip edin:

- `cors` npm modulünü ekleyin: `npm i cors`.
- `server.use(express.json())` arkasına `server.use(cors())` ekleyin.

Bir React uygulaması oluşturun ve bunu sunucunuza bağlayın:

- React uygulaması herhangi bir yerde olabilir, ancak bu proje için onu çözüm klasörü içinde oluşturun.
- API'deki `/api/users` uç noktasına bağlanın ve kullanıcıların listesini gösterin.
- görüntülenen her kullanıcıya onu sunucudan kaldıracak bir silme butonu ekleyin.
- veri eklemek ve güncellemek için formlar ekleyin.
- Kullanıcı listesini uygun gördüğünüz şekilde stilleyin.