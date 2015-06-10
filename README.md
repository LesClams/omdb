# omdb
A simple Node.JS module to access and normalize data from the
[OMDb API](http://www.omdbapi.com/) by Bryan Fritz.

## Installation
```bash
npm install --save omdb
```
## Examples

```javascript
var omdb = require('omdb');

omdb.search('saw', function(err, movies) {
    if(err || movies.length < 1) {
        return new Error('No movies found');
    }

    movies.forEach(function(movie) {
        console.log(movie);
    });
});

omdb.get({ title: 'Saw', year: 2004 }, true, function(err, movie) {
    if(err || !movie) {
        return new Error('Movie not found');
    }

    console.log(movie);
});

omdb.get({ title: 'Game of Thrones', season: 5, episode: 7 }, function (err, episode) {
    if(err || !episode) {
        return new Error('Episode not found');
    }

    console.log(episode);
});
```

## API
### omdb.search(terms, callback)
Run a search request on the API.

`terms` can either be a string of search terms, or the following object:
```javascript
{
    terms: String, // `s` can also be used
    year: Number, // optional (`y` can also be used)
    type: 'series' || 'movie' || 'episode' // optional
}
```

`callback` returns an array of movies. If no movies are found, the array
is empty. The array will contain objects of the following:
```javascript
{
    title: String, // the title of the movie
    type: 'series' || 'movie' || 'episode',

    // If `type` is "series":
    year: {
        from: Number,
        to: Number || undefined // (if the series is still airing)
    },

    // Otherwise,
    year: Number,

    imdb: String
}
```

### omdb.get(terms, callback)
Run a single media request on the API.

`terms` is assumed to be one of the following, respectively:

```javascript
{ imdb: 'tt0387564' }
{ title: 'Saw' }
{ title: 'Saw', year: 2004 }
{ title: 'Game of Thrones', season: 1 }
{ title: 'Game of Thrones', season: 1, episode: 2 }
```

`callback` returns an object of the movie's information. If no movies are
found, it will return `null`.

### omdb.poster(show)
Return a readable stream of the poster JPEG.

`show` is the same as the `show` argument used in `.get()`.

## License
MIT
