
<table>
  <thead>
  <tr>
    <th>Song Name</th>
    <th>Artist</th>
    <th>Album</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>


<form action="javascript:create_FAV()">
    <p><label>
        Song Name:
        <input type="text" name="songname" id="songname" required>
    </label></p>
    <p><label>
        Artist:
        <input type="text" name="srtist" id="artist" required>
    </label></p>
    <p><label>
        Album:
        <input type="text" name="album" id="album" required>
    </label></p>
    <p>
        <button>Submit New Song</button>
    </p>
</form>

<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");
  // prepare URL's to allow easy switch from deployment and localhost
  //const url = "https://teambaddieflask.duckdns.org/api/ISPE"
  const url = "http://127.0.0.1:8086/api/FAV"
  const create_fetch = url + '/create';
  const read_fetch = url + '/';

  // Load users on page entry
  read_FAV();


  // Display User Table, data is fetched from Backend Database
  function read_FAV() {
    // prepare fetch options
    const read_options = {
      method: 'GET', // *GET, POST, PUT, DELETE, etc.
      mode: 'cors', // no-cors, *cors, same-origin
      cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
      credentials: 'omit', // include, *same-origin, omit
      headers: {
        'Content-Type': 'application/json'
      },
    };

    // fetch the data from API
    fetch(read_fetch, read_options)
      // response is a RESTful "promise" on any successful fetch
      .then(response => {
        // check for response errors
        if (response.status !== 200) {
          const errorMsg = 'Database read error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
        }
        
        // valid response will have json data
        response.json().then(data => {
            console.log(data);
            for (let row in data) {
              console.log(data[row]);
              add_row(data[row]);
            }
        })
    })
    // catch fetch errors (ie ACCESS to server blocked)
    .catch(err => {
      console.error(err);
      const tr = document.createElement("tr");
      const td = document.createElement("td");
      td.innerHTML = err;
      tr.appendChild(td);
      resultContainer.appendChild(tr);
    });
  }

  function create_FAV(){
    const body = {
        songname: document.getElementById("songname").value,
        artist: document.getElementById("artist").value,
        album: document.getElementById("album").value,
    };
    const requestOptions = {
        method: 'POST',
        body: JSON.stringify(body),
        headers: {
            "content-type": "application/json",
            'Authorization': 'Bearer my-token',
        },
    };

    // URL for Create API
    // Fetch API call to the database to create a new user
    fetch(create_fetch, requestOptions)
      .then(response => {
        // trap error response from Web API
        if (response.status == 211) {
          alert('Song name is missing, or is less than 2 characters, please refresh and enter a valid song name')
        }
        if (response.status == 212) {
          alert('Artist is missing, or is less than 2 characters, please refresh and enter a valid artist')
        }
        if (response.status == 213) {
          alert('Album is missing, or is less than 2 characters, please refresh and enter a valid album')
        }

        if (response.status !== 200) {
          const errorMsg = 'Database create error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
        }
        // response contains valid result
        response.json().then(data => {
            console.log(data);
            //add a table row for the new/created userid
            add_row(data);
        })
    })
  }

  function add_row(data) {
    const tr = document.createElement("tr");
    const songname = document.createElement("td");
    const artist = document.createElement("td");
    const album = document.createElement("td");

    // obtain data that is specific to the API
    songname.innerHTML = data.songname; 
    artist.innerHTML = data.artist; 
    album.innerHTML = data.album; 
   

    // add HTML to container
    tr.appendChild(songname);
    tr.appendChild(artist);
    tr.appendChild(album);

    resultContainer.appendChild(tr);
  }

</script>

<!-- END OF NEW CODE-->

<style>
 button {
            background-color: #128ca7;
            color: black;
            text-align: center;
            font-size: 15px;
            height: 75;
            width: 500;
            margin-left: auto;
            margin-right: auto;
            padding: 15px 32px;
            display: flex;
            justify-content: center;
         }

</style>


