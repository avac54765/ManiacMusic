<!DOCTYPE html>
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
            width: 10px;
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
        .remove-button {
            color: white;
            border: none;
            cursor: pointer;
            font-size:15px;
            width: 25px; /* Specify the width of the button */
            height: 20px;
        }
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
</head>
<body>
    <h1>Community Favorite Songs!</h1>
    <h2>Add your favorite song below to share your opinion!</h2>
    <div class="center-container">
        <table id="flaskTable" class="table table-striped nowrap" style="width:100%">
            <thead id="flaskHead">
                <tr>
                    <th></th>
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
        function create_FAV() {
            const table = $('#flaskTable').DataTable();
            const songname = $('#songname').val();
            const artist = $('#artist').val();
            const album = $('#album').val();
            const deleteButton = `<button class="remove-button" onclick="removeRow(this)">X</button>`;
            table.row.add([deleteButton, songname, artist, album]).draw();
            const body = {
                songname: songname,
                artist: artist,
                album: album
            };
            const requestOptions = {
                method: 'POST',
                body: JSON.stringify(body),
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer my-token'
                }
            };
            fetch('http://192.168.112.141:8086/api/FAV/create', requestOptions)
                .then(response => {
                    if (response.status === 200) {
                        console.log('Song created successfully.');
                    } else {
                        throw new Error('Failed to create song: ' + response.status);
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                });
        }
        function removeRow(button) {
            const table = $('#flaskTable').DataTable();
            const selectedRow = table.row($(button).parents('tr'));
            if (selectedRow.any()) {
                const songName = selectedRow.data()[1];
                const artist = selectedRow.data()[2];
                const album = selectedRow.data()[3];
                const body = {
                    songname: songName,
                    artist: artist,
                    album: album
                };
                const requestOptions = {
                    method: 'DELETE',
                    body: JSON.stringify(body),
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': 'Bearer my-token'
                    }
                };
                fetch('http://192.168.112.141:8086/api/FAV/delete', requestOptions)
                    .then(response => {
                        if (response.status === 200) {
                            console.log('Song deleted successfully.');
                            selectedRow.remove().draw();
                        } else {
                            throw new Error('Failed to delete song: ' + response.status);
                        }
                    })
                    .catch(error => {
                        console.error('Error:', error);
                    });
            } else {
                alert('Please select a row to delete.');
            }
        }
        $(document).ready(function() {
            const table = $('#flaskTable').DataTable({
                order: [[0, 'asc']]
            });
            fetch('http://192.168.112.141:8086/api/FAV/', { mode: 'cors' })
                .then(response => {
                    if (!response.ok) {
                        throw new Error('API response failed');
                    }
                    return response.json();
                })
                .then(data => {
                    for (const row of data) {
                        const deleteButton = `<button class="remove-button" onclick="removeRow(this)">X</button>`;
                        table.row.add([deleteButton, row.songname, row.artist, row.album]);
                    }
                    table.draw();
                })
                .catch(error => {
                    console.error('Error:', error);
                });
        });
    </script>
</body>
</html>