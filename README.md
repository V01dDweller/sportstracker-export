# Sports Tracker Export

Thanks to [Chelsie Town](https://morioh.com/p/ba8808b91fd5).

Almost all sport tracking sites do not have “Export all” functionality. They want you to stay with them as long as possible. The same with Sports Tracker. There is no such functionality on their website, you can export workouts only one by one (which is annoying).

But with a bit of JS magic, we can download all workouts in GPX format.

 1. Login at http://www.sports-tracker.com/
 1. Go to http://www.sports-tracker.com/diary/workout-list
 1. Click “Show more” button at the end of the page until you have all workouts loaded.
 1. Go to developer console (press Ctrl + Shift + I in Google Chrome)
 1. Insert following JS and press Enter:

 ```javascript
var key = "sessionkey=";
var valueStartIndex = document.cookie.indexOf(key) + key.length;
var token = document.cookie.substring(valueStartIndex, document.cookie.indexOf(';', valueStartIndex));
var delay = 2000;

var loopThroughItems = function (items) {
  var i = 0;
  function printEntry() {
    var href = items[i].getAttribute("href");
    var id = href.substr(href.lastIndexOf('/') + 1, 24);
    var url = 'https://api.sports-tracker.com/apiserver/v1/workout/exportGpx/' + id + '?token=' + token;
    console.log('Remaining: ' + (items.length - i) + ' url: ' + url);
    document.location = url;
    i++; // Increment the position
    if (i < items.length) { // If there are more chars, schedule another
      setTimeout(printEntry, delay);
    }
  }
  printEntry(); // Print the first entry/char
}

var items = document.querySelectorAll("ul.diary-list__workouts li a");
loopThroughItems(items);
```
