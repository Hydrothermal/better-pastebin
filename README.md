# better-pastebin

[![NPM](https://nodei.co/npm/better-pastebin.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/better-pastebin/)

The Pastebin API wrapper Node deserves. `better-pastebin` is the most fully-featured module available for connecting to [pastebin.com](http://pastebin.com), both to its API and directly to the site itself for accomplishing tasks that the API doesn't support. It uses [`cheerio`](https://github.com/cheeriojs/cheerio), [`request`](https://github.com/request/request), and [`xml2js`](https://github.com/Leonidas-from-XIV/node-xml2js).

## Installation
`npm install better-pastebin`

## Example
```javascript
var paste = require("better-pastebin");

paste.setDevKey("xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");
paste.login("username", "password", function(success, data) {
    if(!success) {
        console.log("Failed (" + data + ")");
        return false;
    }

    paste.create({
        contents: "test contents",
        name: "test paste",
        privacy: "1"
    }, function(success, data) {
        if(success) {
            //data contains the URL of the created paste
        } else {
            //data contains an Error object indicating why the creation failed
        }
    });
});
```

## Usage
### Setting the devkey
```javascript
paste.setDevKey("xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");
```

This is necessary before performing any other API call. It's synchronous, so it doesn't require a callback. You can find your devkey [on pastebin.com](http://pastebin.com/api) after signing in.

### Logging in
```javascript
paste.login(username, password, function(success, data) {
    //data contains this session's userkey
});
```

Logs in to Pastebin. Any actions requiring a login should be placed within the callback.

### Getting a paste
```javascript
paste.get(paste_id, function(success, data) {
    //data contains the contents of the paste 
});
```

Gets the contents of a paste. Either the ID or the URL of a paste can be supplied.

### Creating a paste
```javascript
paste.create(options, function(success, data) {
    //data contains the URL of the created paste
});
```

Creates a new paste. You do not need to be logged in to do this, but you can only create global or unlisted pastes as a guest. `options` can either be the contents of the paste as a string, or an object containing one or more of the following keys:

* `options.contents`: The contents of the paste; this is the only mandatory property.
* `options.anonymous` (default: `false`): If true, the paste will be created as a guest even if logged in.
* `options.expires` (default: `"N"`): When the paste should expire.
* `options.format` (default: `"text"`): The syntax highlighting of the paste.
* `options.privacy` (default: `"0"`): The privacy of the paste.
* `options.name` (default: `""`): The name of the paste.

### Creating a paste from a file
```javascript
paste.createFromFile(options, function(success, data) {
    //data contains the URL of the created paste
});
```

Creates a new paste from the contents of a file. This works the same way that `paste.create` does, except that `options.path` should be specified instead of `options.contents`, or `options` should be the path to the file you want to upload as a string. It also accepts an additional key, `options.encoding`:

* `options.encoding` (default: `"utf8"`): The encoding of the file.

### Editing a paste
```javascript
paste.edit(paste_id, options, function(success, data) {
    //data contains the new contents of the paste
});
```

Updates a paste's contents. You must be logged in to do this, and you can only edit a paste that you created. Either the ID or the URL of a paste can be supplied. `options` can either be the new contents of the paste as a string, or an object containing one or more of the following keys:

* `options.contents`: The new contents of the paste.
* `options.expires`: When the paste should expire.
* `options.format`: The syntax highlighting of the paste.
* `options.privacy`: The privacy of the paste.
* `options.name`: The name of the paste.

If left unspecified, these default to the paste's existing options.

### Listing the logged-in user's pastes
```javascript
paste.list(limit, function(success, data) {
    //data contains an array of objects of information about each paste
});
```

Lists up to 1,000 of the logged-in user's created pastes. `limit` specifies how many pastes to return; it can be a number between 1 and 1,000, and defaults to 50. You must be logged in to do this.

### Listing trending pastes
```javascript
paste.listTrending(function(success, data) {
    //data contains an array of objects of information about each paste
});
```

Lists currently trending pastes. The array of pastes is identical in structure to the one returned by `paste.list`.

### Deleting a paste
```javascript
paste.delete(paste_id, function(success, data) {
    //data contains the ID of the deleted paste
});
```

Deletes a paste. You must be logged in to do this, and you can only delete a paste that you created. Either the ID or the URL of a paste can be supplied.

### Getting information about the logged-in user
```javascript
paste.user(function(success, data) {
    //data contains an object of information about the logged-in user
});
```

Returns information about the logged-in user. You must be logged in to do this.

### Valid options
#### `expires`
```
N = Never
10M = 10 Minutes
1H = 1 Hour
1D = 1 Day
1W = 1 Week
2W = 2 Weeks
1M = 1 Month
```

#### `privacy`
```
0 = Public
1 = Unlisted
2 = Private
```

Note that private pastes can only be created if you are signed in. These values should be passed as a string, because `0` is a falsy value and can cause some issues.

#### `format`
Refer to http://pastebin.com/api#5 for the full list of syntax highlighting options.

### License
All code in this repository is licensed under the MIT license. See `LICENSE` for more information.