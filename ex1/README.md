# Goal

To understand how to read your data &amp; how to navigate within your Webcom database.

For this purpose, we will use a specific namespace populated with an Open Data Set. The data set is about the available cycles & stations for services such as Paris's Vélib. This Open Data Set is provided by [JCDecaux](https://developer.jcdecaux.com/#/opendata/vls?page=dynamic).

# Data Structure

The data is represented as a JSONTree following this structure:

```javascript
{
...
"<ContractName>": {
  "cities": [
    "<City_1>", "<City_2>"
  ],
  "commercial_name": "<Name>",
  "country_code": "<Country>",
  "stations": {
    ...
    "<StationNumber>": {
      "address": "<StationAddress>",
      "available_bike_stands": <StationEmptyStands>,
      "available_bikes": <StationAvailableBikes>,
      "banking": <StationCB>,
      "bike_stands": <StationNbStands>,
      "bonus": <StationIsBonus>,
      "contract_name": "<ContractName>",
      "last_update": "<LastUpdateTimestamp>",
      "name": "<StationName>",
      "status": "<StationStatus>",
      "position": {
        "lat": "<StationLatitude>",
        "lng": "<StationLongitude>"
      }
    },
    ...
  }
},
...
}
```

# Create a reference to the namespace

To start, go to this [JS Bin](https://jsbin.com/legica/3/edit?js,console). [JS Bin](https://jsbin.com) is a web online editor, useful to play with code and libraries.

Create an account, it's more convenient. Otherwise, it will reloads the editor each time you save.

For this exercise, We will focus only on Javascript.

First, you need to [create a reference](https://webcom.orange.com/doc/Webcom.html) to the namespace :

```javascript
var ref = new Webcom('https://webcom.orange.com/base/opendata-velib');
```

# Read the data

To read the data, we will use the [once('value', callback)](https://webcom.orange.com/doc/Webcom.html#on) method.

```javascript
ref.once('value', function(snap){
    // Display the database value
    console.info(snap.val());
})
```

This will display the whole database, which is not very optimal.

# Data navigation &amp; URIs

The amount of data can be limited by focusing on a specific part of the JSONTree. For this purpose, you can use the navigation methods like [child](https://webcom.orange.com/doc/Webcom.html#child) and [parent](https://webcom.orange.com/doc/Webcom.html#parent) or create a new reference directly.

Let's try to display the informations of Station "5" of the "Toulouse" contract.

```javascript
// Using navigation
ref.child('Toulouse').child('stations').child('5').once('value', function(snap){
    // Display the station's properties
    console.info(snap.val());
});

// Using URIs
var stationRef = new Webcom('https://webcom.orange.com/base/opendata-velib/Toulouse/stations/5');
stationRef.once('value', function(snap){
    // Display the station's properties
    console.info(snap.val());
});
```

# Watch data modifications

Since this data sets is synchronized in real time. We can try to watch the stations data update.

Listen to the values changes on different parts of the tree and find the most efficient way to watch the number of available bikes for one station and for all stations of a contract using the [on('child_changed', callback)](https://webcom.orange.com/doc/Webcom.html#on) method.

```javascript
// For a station
stationRef.once('child_changed', function(snap){
    // Display the number of available bikes
    console.info(snap.val().available_bikes);
});
```

Check this [JSBin](https://jsbin.com/qebuqe/edit?js,console) for the solution.


