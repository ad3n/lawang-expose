# Lawang Expose

## JSON to JSON Transformation

JSON to JSON Transformation berfungsi untuk memanipulasi response pada endpoint HTTP

- Response asli

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

- Tranformer

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

- Response akhir

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

## Lawang Internal Javascript Object and Function

Berikut adalah internal object dan function yang di-provide Lawang. Semua yang datang dari lawang baik variable maupun fungsi akan dikirim dalam format `string`, sebagian berisi JSON string sehingga perlu fungsi `JSON.parse()` untuk mengubah menjadi JSON object

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
$GT.request(url, method): string
$GT.request(url, method, body): string
$GT.request(url, method, timeout): string
$GT.request(url, method, body, headers): string
$GT.request(url, method, body, timeout): string
$GT.request(url, method, body, headers, timeout): string

$GT.get(url): string
$GT.get(url, headers): string
$GT.get(url, timeout): string
$GT.get(url, headers, timeout): string

$GT.post(url): string
$GT.post(url, body): string
$GT.post(url, timeout): string
$GT.post(url, body, headers): string
$GT.post(url, body, timeout): string
$GT.post(url, body, headers, timeout): string

$GT.put(url): string
$GT.put(url, body): string
$GT.put(url, timeout): string
$GT.put(url, body, headers): string
$GT.put(url, body, timeout): string
$GT.put(url, body, headers, timeout): string

$GT.patch(url): string
$GT.patch(url, body): string
$GT.patch(url, timeout): string
$GT.patch(url, body, headers): string
$GT.patch(url, body, timeout): string
$GT.patch(url, body, headers, timeout): string

$GT.delete(url): string 
$GT.delete(url, headers): string 
$GT.delete(url, timeout): string 
$GT.delete(url, headers, timeout): string 

/**
 * source: string
 * sql: string
 * table: string
 * identities: object
 * payload: object
 **/
$GT.query(source, sql): string 
$GT.save(source, table, payload): void
$GT.upsert(source, table, identities, payload): void
$GT.update(source, table, identities, payload): void

/**
 * payload: string
 * encoded: string
 * plainText: string
 * secret: string
 * privateKey: string
 * publicKey: string
 **/
$GT.hmac512(payload, secret): string
$GT.sha256_signing(payload, privateKey): string
$GT.sha256_verify(encoded, plainText, publicKey): bool

/**
 * xml: string
 **/
$GT.xml2json(xml): string

/**
 * body: object
 * code: int
 **/
$GT.response(body, code): void

```

Jika ada fungsi di atas yang tidak dapat digunakan, Anda menghubungi teknikal support kami untuk mendaftarkan fungsi tersebut

## Javascript Library

- [MomentJS](https://momentjs.com)
