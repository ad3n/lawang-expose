# Lawang Expose

## JSON to JSON Transformation

JSON to JSON Transformation berfungsi untuk memanipulasi response dari endpoint

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

## Javascript API

```js
$GT.uuid()
$GT.request(url, method, )

```
