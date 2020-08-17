## APIController

The `APIController`  module is responsible for making calls to the Spotify API. To have our app get data from the Spotify API, we have to go through an Authorization process. Read the **Client Credentials Flow** section of the Spotify Authorization Guide [here](https://developer.spotify.com/documentation/general/guides/authorization-guide/#client-credentials-flow). This type of flow does not allow access to any user-specific data, so we can only access public data.



Inside the APIController module, we're going to write a private method called `_getToken`  that will make the http request for a token. Note: it's common syntax for private methods to have an underscore in the beginning.

Use the information in the Authorization Guide to fill in the necessary fields. Notice also that we've declared variables for the client ID and secret that you received when you registered the app. 

Hint: use these variables in the `Authorization`  section of your http request.

<details>
  <summary>Bigger Hint:</summary>
  ![modules](images/clientflow.png)

</details>


```js
const APIController = (function() {
    
    const clientId = config.CLIENT_ID;
    const clientSecret = config.CLIENT_SECRET;

    // private methods
    const _getToken = async () => {

        const result = await fetch('<INSERT CORRECT ENDPOINT>', {
            method: '<INSERT CORRECT METHOD>',
            headers: {
              	//content-type: how should the parameters be encoded?
                'Content-Type' : '<INSERT CONTENT TYPE>', 
              	
              	//btoa() encodes a string in base64
              	//what is the string that needs to be encoded?
                'Authorization' : 'Basic ' + btoa('<INSERT AUTH STRING>')
            },
						
          	//what request body parameters are needed?
            //the format should be a string, no spaces. ex/  'parameter_name=value'
            body: '<INSERT REQUEST PARAMETERS>'
        });

        const data = await result.json();
        return data.access_token;
    }

})();
```



The second private method we need is `_getPlaylistItems`.  This is responsible for getting all the tracks from any playlist. Within the `APIController`,  add in `_getPlaylistItems`. 

`_getPlaylistItems`  has three parameters

- access token
- playlistId- this is a unique base-62 identifier that you can find at the end of every Spotify URI for an artist, track, album, playlist, etc. 
  - ex: the link for the Today's Top Hits playlist is https://open.spotify.com/playlist/37i9dQZF1DXcBWIGoYBM5M
  - the id is the string of characters after [playlist/](), so 37i9dQZF1DXcBWIGoYBM5M
- limit- the number of tracks you want from the playlist



Read the documentation for the getting playlist tracks endpoint [here](https://developer.spotify.com/documentation/web-api/reference/playlists/get-playlists-tracks/). Notice that in the documentation, there are certain **Query Parameters**  that you can add. To add a query parameter, it needs to be in the form of a string like so:

```js
//key-value pairs connected by &
const queryString = '{param_1_Name}={param_1_Value}&{param_2_Name}={param_2_Value}'

//then, add your queryString onto the api endpoint with a question mark before
const result = await fetch('{api_endpoint}' + '?' + queryString, {
  //blah blah stuff here
})
```



Using the given code, add these things:

1. find the correct api endpoint
2. append a queryString that sets the query parameter `limit`  to the `limit`  parameter that the function receives
3. set the correct headers

!! For the headers, there should be one field called `Authorization`  set to the value `'Bearer' + token`

```js
const _getPlaylistItems = async (token, playlistId, limit) => {
  			//set the query parameter limit to this limit ^ param
  			const queryString = '<QUERY STRING>'
        
        //hint: the api endpoint should include the playlistId parameter somewhere
        const result = await fetch('<API ENDPOINT>' + '?' + queryString, {
            method: 'GET',
            headers: { '<HEADER_FIELD>' : '<HEADER VALUE>'}
        });

        const data = await result.json();
        return data.items;
    }
```


