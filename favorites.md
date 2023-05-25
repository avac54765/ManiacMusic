<html>
<head>
    <!--<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Courgette">-->
    <!-- JQuery -->
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.5.1.js"></script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
    <!-- Bootstrap -->
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/dataTables.bootstrap5.min.js"></script>
    <link rel="stylesheet" href="https://cdn.datatables.net/1.11.4/css/jquery.dataTables.min.css">
    <style>
         #flaskTable th:first-child {
            width: 150px;
        }
        #flaskTable td:not(:first-child) {
            width: 200px;
        }
        table.dataTable td {
            color: black;
        }
        input[type="text"] {
            background-color: black;
            color: white;
        }
        div.dataTables_wrapper div.dataTables_filter label {
            color: white;
            margin-right: 5px;
        }
        .center-container {
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        #flaskTable {
            margin-bottom: 5px;
            color: white;
            width: 800px; /* Adjust the width as needed */
        }
        #flaskTable th, #flaskTable td {
            border-bottom: 1px solid white;
            padding: 15px; /* Adjust the padding as needed */
        }
        #flaskTable th {
            background-color: #724283;
        }
        #flaskTable tbody tr:hover {
            background-color: #724283;
        }
        button {
            background-color: #724283;
            color: white;
            text-align: center;
            font-size: 15px;
            height: 50px;
            width: 200px;
            margin: 10px;
        }
    </style>
</head>
<body>
<h1>Community Favorite Songs!</h1>
<h2>Add your favorite song below to share your opinion!</h2>
<style>
  h1 {
    text-align: center;
    margin-bottom: 10px;
    font-size: 45px;
    font-family: 'FontName', Courgette;
    }
  h2 {
    text-align: center;
    margin-bottom: 100px;
    font-size: 20px;
    color: #724283;
  }
  /* Center the table and input forms */
  .center-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  #flaskTable {
    margin-bottom: 5px;
  }
</style>
<div class="center-container">
  <table id="flaskTable" class="table table-striped nowrap" style="width:100%">
    <thead id="flaskHead">
        <tr>
            <th>Song Name</th>
            <th>Artist</th>
            <th>Album</th>
        </tr>
    </thead>
    <tbody id="flaskBody"></tbody>
  </table>
</div>

<form class="center-container">
    <p><label>
        Song Name:
        <input type="text" name="songname" id="songname" required>
    </label></p>
    <p><label>
        Artist:
        <input type="text" name="artist" id="artist" required>
    </label></p>
    <p><label>
        Album:
        <input type="text" name="album" id="album" required>
    </label></p>
    <p>
        <button type="button" onclick="create_FAV()">Submit New Song</button>
    </p>
</form>

<script>
$(document).ready(function() {
    const table = $('#flaskTable').DataTable({
        order: [[0, 'asc']] // Specify sorting column and direction
    });

    // Fetch data from the API
    fetch('http://172.26.151.226:8086/api/FAV/', { mode: 'cors' })
        .then(response => {
            if (!response.ok) {
                throw new Error('API response failed');
            }
            return response.json();
        })
        .then(data => {
            for (const row of data) {
                table.row.add([row.songname, row.artist, row.album]);
            }
            table.draw();
        })
        .catch(error => {
            console.error('Error:', error);
        });
});

function create_FAV() {
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

    const create_fetch = 'http://172.26.151.226:8086/api/FAV/create';

    fetch(create_fetch, requestOptions)
        .then(response => {
            if (response.status == 211) {
                alert('Song name is missing, or is less than 2 characters, please refresh and enter a valid song name');
            }
            if (response.status == 212) {
                alert('Artist is missing, or is less than 2 characters, please refresh and enter a valid artist');
            }
            if (response.status == 213) {
                alert('Album is missing, or is less than 2 characters, please refresh and enter a valid album');
            }
            if (response.status !== 200) {
                throw new Error('Database create error: ' + response.status);
            }
            return response.json();
        })
        .then(data => {
            const table = $('#flaskTable').DataTable();
            table.row.add([data.songname, data.artist, data.album]).draw();
        })
        .catch(error => {
            console.error('Error:', error);
        });
}
</script>

</body>
</html>






