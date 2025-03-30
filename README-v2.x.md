# LAWANG API MANAGEMENT PLATFORM

# Key Features

- Secure (mengimplementasikan [Zero Trust](https://en.wikipedia.org/wiki/Zero_trust_security_model))
- Fast and Stable
- Zero Downtime
- Multiple Backend (HTTP, Database (SQL), Javascript and File System)
- Easy to Customize (menggunakan Javascript)
- Lighweight (Low Memory Footprint)

# Lawang Arsitektur

Secara sederhana, arsitektur Lawang memisahkan antara administratif (Console) dan engine (Core). Ini dilakukan agar proses administratif tidak berpengaruh terhadap performance dari engine (Core) dan agar kesalahan konfigurasi yang terjadi secara tidak sengaja tidak langsung berimbas ke engine (Core).

Berikut adalah arsitektur Lawang secara umum

## 1. Dengan Load Balancer

![Lawang Architecture](https://github.com/user-attachments/assets/ef1a596c-efb8-442c-b5e0-cf10a246d71e)

## 2. Tanpa Load Balancer

![Lawang Architecture Without LB](https://github.com/user-attachments/assets/f3c1b3c2-ba7b-42fb-a784-ea6d73c4d7a0)

# Deployment Process (Backend/Endpoint/Policy/Interceptor)

Seperti terlihat pada gambar arsitektur di atas, terdapat proses **Get Config** yang menghubungkan antara Console dan Core. Config di sini meliputi:
> Setting
>
> File Storage Config
>
> Database Config
>
> Backend
>
> Endpoint
>
> Policy
>
> Interceptor

Process **Get Config** (untuk memudahkan kita sebut saja deployment), terjadi dalam beberapa proses yaitu:
> 1. Bootstrapping (Start Core - ini akan mengambil semua config yang disebutkan di bawah) 
> 2. Manual Restart (triggered by Console)
> 3. File Storage Config Deployment (triggered by Console)
> 4. Database Config Deployment (triggered by Console)
> 5. Backend Deployment (triggered by Console)
> 6. Endpoint Deployment (triggered by Console)
> 7. Policy Deployment (triggered by Console)
> 8. Interceptor Deployment (triggered by Console)

Proses deployment ini menggunakan [gRPC](https://grpc.io/docs/what-is-grpc/) untuk memastikan efektifitas dan efisiensi serta keamanan dalam pertukaran data karena pada gRPC menggunakan contract yang disebut dengan [Protocol Buffer](https://protobuf.dev/overview/).

# Authentication and Authorization



# Lawang Javascript Engine

Lawang API Management menggunakan v8 Javascript Engine dengan sangat efektif dan efisien memisahkan antara proses kompilasi dan proses eksekusi. Berikut adalah diagram flow eksekusi Javascript pada Lawang  

![Javascript](https://github.com/user-attachments/assets/2827d556-f499-4b1e-a8e1-bf87e54da5fd)

# Policy

_Policy_ adalah mekanisme untuk memodifikasi _body request_ maupun _response_ ketika Api dipanggil. _Policy_ menempel pada endpoint dan berlaku pada Api yang memenuhi kondisi pada _rule_. _Rule_ dibuat menggunakan Javascript sehingga lebih mudah dan fleksibel, menggunakan _objects_ maupun _functions_ berikut:

```js
// string
$PC.uri
// string
$PC.method
// string
$PC.queries
// string
$PC.params
// string
$PC.headers
// string
$PC.request

/**
 * request: object
 * response: object
 **/
$PC.result(request, response): void
```

## _Policy Scope_

Terdapat tiga _scope_ dari _policy_ yaitu *request*, *logging* dan *response* yang gunanya untuk memodifikasi maupun mem-_filter_ `request` dan `response` sebelum dikirim ke _caller_ (pemanggil)

## Contoh _policy_

```js
// You can add conditions and/or your logic

let request = JSON.parse($PC.request)

request.test = "injected"

let response = JSON.parse($PC.response)

response.test = "injected"

$PC.result(request, response)

```

Anggap _scope_ dari _policy_ di atas adalah *logging* maka pada data _log_ pada _fields_ `request_body` dan `response_body` akan ditambahkan _field_ `test` yang berisi `injected`.

# Interceptor

_Interceptor_ adalah mekanisme untuk me-_reject request_ atau meneruskan _request_ berdasarkan _rules_. Sama seperti _Policy_, _interceptor_ menempel pada endpoint dan berlaku pada Api yang memenehi _rule_. _Rule_ dibuat menggunakan Javascript sehingga lebih mudah dan fleksibel, menggunakan _objects_ maupun _functions_ berikut:

```js
// string
$RL.uri
// string
$RL.method
// string
$RL.queries
// string
$RL.params
// string
$RL.headers
// string
$RL.request

/**
 * body: object
 * code: int
 * contentType: string
 **/
$RL.result(body, code): object
$RL.result(body, code, contentType): object
```

# Request Validator

_Request Validator_ adalah mekanisme untuk memastikan _format request body_ tidak sembarang dikirim berdasarkan _JSON Schema_ yang telah ditentukan. _Request Validator_ menempel pada endpoint sama hal nya dengan _Interceptor_ dan _Policy_. _JSON Schema Specification_ yang digunakan adalah [2020-12](https://json-schema.org/draft/2020-12)

## Contoh Request Validator

```js
{
    "type": "object",
    "properties": {
    	"name": {"type": "string"},
    	"age": {"type": "integer", "minimum":20}
    },
	"required": ["name", "age"]
}
```

dari contoh _schema_ diatas, _request body_ harus memiliki property dengan field sebagai berikut: `name` dengan tipe data `string`  dan `age` dengan tipe data `integer` dan _value_ dari `age` _minimum_ adalah 20


# Services

_Service_ adalah kumpulan fungsionalitas yang disediakan Lawang untuk memberikan kemudahan dan fleksibilitas dalam pembuatan serta pengelompokan Api.

## HTTP _Service_

HTTP _Service_ adalah _service_ yang bertugas untuk meng-_handle_ HTTP _Endpoint_ pada internal Lawang. _By default_, HTTP _Service_ menerapkan _circuit breaker_ per _backend/connection/host_ untuk menghindari _traffic flooding_ dan mengoptimalkan kinerja dari Lawang itu sendiri. HTTP Service juga menerapkan [zero trust](https://www.krakend.io/docs/design/zero-trust) untuk memastikan keamanan transmisi antara _client_ dan _backend/connection/host_.

HTTP _Service support Basic Auth, Single Key Auth (Api Key Only)_ maupun _Double Keys Auth (Api Key and Token)_, selain itu HTTP _Service_ juga mendukung _response transformation_ untuk memudahkan dalam memanipulasi _response_ dari _endpoint_. _Response transformation_ hanya dapat digunakan untuk _json response_.  

### _JSON to JSON Transformation (Response Transformation)_

JSON to JSON _Transformation_ berfungsi untuk memanipulasi _response_ pada HTTP _Service_ menggunakan spesifikasi sesuai dengan spesifikasi [GJson](https://github.com/tidwall/gjson) dengan _syntax_ sebagai berikut:

```json
[
	{
		"dest": "GSJON spec"
	}
]
```

Dan berikut adalah contoh penggunaan dari _response transformer_:

- _Response_ asli

```json
[
  {
    "id": "1",
    "name": "Google Pixel 6 Pro",
    "data": {
      "color": "Cloudy White",
      "capacity": "128 GB"
    }
  },
  {
    "id": "2",
    "name": "Apple iPhone 12 Mini, 256GB, Blue",
    "data": null
  },
  {
    "id": "3",
    "name": "Apple iPhone 12 Pro Max",
    "data": {
      "color": "Cloudy White",
      "capacity GB": 512
    }
  },
  {
    "id": "4",
    "name": "Apple iPhone 11, 64GB",
    "data": {
      "price": 389.99,
      "color": "Purple"
    }
  },
  {
    "id": "5",
    "name": "Samsung Galaxy Z Fold2",
    "data": {
      "price": 689.99,
      "color": "Brown"
    }
  },
  {
    "id": "6",
    "name": "Apple AirPods",
    "data": {
      "generation": "3rd",
      "price": 120
    }
  },
  {
    "id": "7",
    "name": "Apple MacBook Pro 16",
    "data": {
      "year": 2019,
      "price": 1849.99,
      "CPU model": "Intel Core i9",
      "Hard disk size": "1 TB"
    }
  },
  {
    "id": "8",
    "name": "Apple Watch Series 8",
    "data": {
      "Strap Colour": "Elderberry",
      "Case Size": "41mm"
    }
  },
  {
    "id": "9",
    "name": "Beats Studio3 Wireless",
    "data": {
      "Color": "Red",
      "Description": "High-performance wireless noise cancelling headphones"
    }
  },
  {
    "id": "10",
    "name": "Apple iPad Mini 5th Gen",
    "data": {
      "Capacity": "64 GB",
      "Screen size": 7.9
    }
  },
  {
    "id": "11",
    "name": "Apple iPad Mini 5th Gen",
    "data": {
      "Capacity": "254 GB",
      "Screen size": 7.9
    }
  },
  {
    "id": "12",
    "name": "Apple iPad Air",
    "data": {
      "Generation": "4th",
      "Price": "419.99",
      "Capacity": "64 GB"
    }
  },
  {
    "id": "13",
    "name": "Apple iPad Air",
    "data": {
      "Generation": "4th",
      "Price": "519.99",
      "Capacity": "256 GB"
    }
  }
]
```

- _Tranformer_

```json
[
    {
        "Id": "id",
        "Name": "name",
        "Type": "!Gadget",
        "Available": "!true",
        "Stock": "!100",
        "Price": "data.*ice",
        "Color": "data.*lor",
        "Capacity": "data.*city"
    }
]
```

- _Response_ akhir

```json
[
	{
		"Available": true,
		"Capacity": "128 GB",
		"Color": "Cloudy White",
		"Id": 1,
		"Name": "Google Pixel 6 Pro",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 2,
		"Name": "Apple iPhone 12 Mini, 256GB, Blue",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Cloudy White",
		"Id": 3,
		"Name": "Apple iPhone 12 Pro Max",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Purple",
		"Id": 4,
		"Name": "Apple iPhone 11, 64GB",
		"Price": 389.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Brown",
		"Id": 5,
		"Name": "Samsung Galaxy Z Fold2",
		"Price": 689.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 6,
		"Name": "Apple AirPods",
		"Price": 120,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 7,
		"Name": "Apple MacBook Pro 16",
		"Price": 1849.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 8,
		"Name": "Apple Watch Series 8",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Red",
		"Id": 9,
		"Name": "Beats Studio3 Wireless",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "64 GB",
		"Color": "data.*lor",
		"Id": 10,
		"Name": "Apple iPad Mini 5th Gen",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "254 GB",
		"Color": "data.*lor",
		"Id": 11,
		"Name": "Apple iPad Mini 5th Gen",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "64 GB",
		"Color": "data.*lor",
		"Id": 12,
		"Name": "Apple iPad Air",
		"Price": 419.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "256 GB",
		"Color": "data.*lor",
		"Id": 13,
		"Name": "Apple iPad Air",
		"Price": 519.99,
		"Stock": 100,
		"Type": "Gadget"
	}
]
```

*NB: Jika Anda perlu lebih dari sekedar _response manipulations_, saya sarankan Anda menggunakan Javascript _Service_.*

## _Database Service_

_Database Service_ adalah _service_ yang bertugas untuk meng-_handle endpoint database_. _Database Service_ mem-_provide_ beberapa _driver database_ antara lain MySQL, PostgSQL, Oracle, dll yang dapat mengimplementasikan [`database/sql`](https://go.dev/wiki/SQLDrivers). Pada _Database Service_, hanya diperbolehkan melakukan _query_ `SELECT`.

## _File Service_

_File Service_ adalah _service_ yang bertugas untuk mengakses _file_ dalam _server_ secara aman. _File Service_ hanya dapat diakses menggunakan _credentials_ (_private endpoint_)

## Javascript _Service_

Javascript _Service_ adalah _service_ yang bertugas untuk meng-_handle_ _endpoint_ Javascript. Anda dapat melakukan apapun pada _service_ ini termasuk _HTTP Call_, _Database Query, Database Manipulation, Load File, Store File,_ dan pastinya memanipulasi _request_ maupun _response_. Javascript _Service_ diperuntukan untuk mengakomodir _custom logic_ serta untuk memudahkan dalam integrasi dengan sistem lain tanpa perlu membuat _middleware system_ antara Lawang dan _client_.

Javascript _Service_ menggunakan EcmaScript6 dengan beberapa penambahan _objects_ maupun _functions_ bawaan namun tidak mendukung untuk menggunakan EcmaScript6 _module_, sehingga `export` dan `import` _keywords_ **tidak dapat digunakan** pada Lawang.

### _Internal Objects_ dan _Functions_

Berikut adalah _internal objects_ dan _functions_ yang tersedia pada Lawang. Semua yang datang dari lawang baik *_objects_* maupun *_response functions_* akan dikirim dalam format `string` kecuali untuk tipe data primitif, sebagian berisi JSON _string_ sehingga perlu fungsi `JSON.parse()` untuk mengubah menjadi JSON _object_


```js
// string
$GT.uri
// string
$GT.method
// string
$GT.queries
// string
$GT.params
// string
$GT.headers
// string
$GT.payload
// string
$GT.files
// string
$GT.client_ip

/**
 * payload: string
 **/
$GT.uuid(): string
$GT.md5(payload): string

/**
 * url: string
 * method: string
 * body: object
 * headers: object
 * timeout: int (in second)
 **/
$GT.request(method, url): object
$GT.request(method, url, body): object
$GT.request(method, url, timeout): object
$GT.request(method, url, body, headers): object
$GT.request(method, url, body, timeout): object
$GT.request(method, url, body, headers, timeout): object

$GT.get(url): object
$GT.get(url, headers): object
$GT.get(url, timeout): object
$GT.get(url, headers, timeout): object

$GT.post(url): object
$GT.post(url, body): object
$GT.post(url, timeout): object
$GT.post(url, body, headers): object
$GT.post(url, body, timeout): object
$GT.post(url, body, headers, timeout): object

$GT.put(url): object
$GT.put(url, body): object
$GT.put(url, timeout): object
$GT.put(url, body, headers): object
$GT.put(url, body, timeout): object
$GT.put(url, body, headers, timeout): object

$GT.patch(url): object
$GT.patch(url, body): object
$GT.patch(url, timeout): object
$GT.patch(url, body, headers): object
$GT.patch(url, body, timeout): object
$GT.patch(url, body, headers, timeout): object

$GT.delete(url): object 
$GT.delete(url, headers): object 
$GT.delete(url, timeout): object 
$GT.delete(url, headers, timeout): object 

/**
 * source: string
 * sql: string
 * params: []any
 * timeout: int
 * table: string
 * identities: object
 * payload: object
 **/
$GT.query(source, sql): []object
$GT.query(source, sql, params): []object
$GT.query(source, sql, params, timeout): []object 
$GT.save(source, table, payload): void
$GT.upsert(source, table, identities, payload): void
$GT.update(source, table, identities, payload): void

/**
 * payload: string
 * claim: object
 * encoded: string
 * useBase64Encoded: boolean
 * plainText: string
 * secret: string
 * privateKey: string
 * publicKey: string
 **/
$GT.hmac512(payload, secret): string
$GT.hmac256(payload, secret): string
$GT.hmac256(payload, secret, useBase64Encoded): string
$GT.sha256_signing(payload, privateKey): string
$GT.sha256_verify(encoded, plainText, publicKey): bool
$GT.jwtrs256_signing(claim, privateKey): string
$GT.jwtrs256_verify(token, publicKey): string

/**
 * xml: string
 * json: string
 * spec: string
 **/
$GT.xml2json(xml): object
$GT.transform(json, spec): object

/**
 * source: string
 * path: string
 * body: string
 **/
$GT.load_file(source, path): []byte
$GT.check_file(source, path): int
$GT.store_file(source, body, path): void
$GT.mkdir(source, path): void

/**
 * body: object
 * code: int
 * contentType: string
 **/
$GT.response(body): object
$GT.response(body, code): object
$GT.response(body, code, contentType): object

/**
 * payload: string
 **/
$GT.log(payload): void

/**
 * file: string
 **/
$GT.import(file): void

```

*NB: Untuk `$GT.query()`, untuk penggunaan _query binding_, maka pada `sql` untuk _parameter placeholder_ disesuaikan dengan dialek atau SQL masing-masing platform*
