Streamio REST API wrapper for C#
================================

Official C# wrapper for the http://streamio.com API. Integrating video in your application has never been more awesome.

Installation
------------

TODO!
    
Usage
-----

Include it.

``` c#
using Streamio;
```

Initialize it.

``` c#
StreamioAPI api = new StreamioAPI("api-username", "api-password");
```

Use it.

``` c#
// Fetch an array of Videos
List<Video> videos = api.Videos.List();

// Pass in parameters as specified in the API docs
// This fetches the 5 most played videos tagged with Nature or Sports
List<Video> videos = api.Videos.List(new Dictionary<string, object>() {
	{"tags", "Nature, Sports"},
	{"limit", 5},
	{"order", "plays.desc"}
});

// Find a Video by id
Video video = api.Videos.Find("4c57f3975412901427000005");

// Create a Video
Video video = api.Videos.Create(new Dictionary<string, object>() {
	{"title", "Super Awesome"},
	{"description", "What an awesome video."},
	{"skip_default_encoding_profiles", true},
	{"tags", "Awesome, Tags"},
	{"file", "/path/to/awesome.mov"}
});

// Count videos with or without parameters
api.Videos.Count() // 23
api.Videos.Count(new Dictionary<string, object>() {
	{"tags", "Awesome"}
}) // 6

```

Handle errors.

``` c#
// Creating or updating with invalid parameters throws StreamioValidationException
try
{
	api.Playlists.Create(new Dictionary<string, object>() {});
}
catch(StreamioValidationException e)
{
	// Do something with the e.Errors dictionary
}

// All other errors such as 401 unauthenticated, 404 not found, 400 bad request etc
// throws StreamioException which includes server response in message output
StreamioAPI api = new StreamioAPI("faulty-username", "faulty-password");
try
{
	api.Videos.List();
}
catch(StreamioException e)
{
	// e.Message should explain what went wrong
	// conditional error handling could be done on e.Response.StatusCode
}
```

Same principles work for the other available models (Image, Playlist, Player, Audio, EncodingProfile and PublicUpload).

More Documentation
------------------

Please refer to the official Streamio API Documentation for details on parameters etc:
http://streamio.com/api/docs
