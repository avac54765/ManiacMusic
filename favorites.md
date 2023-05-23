<html>
<head>
    <!-- JQuery -->
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.5.1.js"></script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
    <!-- Bootstrap -->
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/dataTables.bootstrap5.min.js"></script>
    <link rel="stylesheet" href="https://cdn.datatables.net/1.11.4/css/jquery.dataTables.min.css">
    <style>
        #flaskTable th:first-child {
            width: 75px;
        }
        #flaskTable td:not(:first-child) {
          width: 150px;
        }
        table.dataTable td {
        color: black;
        }
        input[type="text"] {
            background-color: black;
            color: white;
        }
    </style>
</head>
<body>

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

<form>
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
        order: [[0, 'asc']] // Specify the initial sorting column and direction
    });

    // Fetch data from the API and populate the table
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




