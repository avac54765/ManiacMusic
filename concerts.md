
<h1>Today's Concerts!</h1>
<h2>Search for a specific concert below!<h2>

<html>
<head>
  <title>Event List</title>
  <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.25/css/jquery.dataTables.min.css">
  <style>
    #eventTable td {
      color: black;
    }
    div.dataTables_wrapper div.dataTables_filter label {
        color: white;
    }
      body {
      font-family: Arial, sans-serif;
    }
    #eventTable {
      border-collapse: collapse;
      width: 100%;
    }
    #eventTable th,
    #eventTable td {
      padding: 10px;
      text-align: left;
    }
    #eventTable th {
      background-color: #724283;
      color: white;
    }
    #eventTable tbody tr:nth-child(even) {
      background-color: #f2f2f2;
    }
    #eventTable tbody tr:hover {
      background-color: #724283;
    }
    div.dataTables_wrapper div.dataTables_filter label {
      color: white;
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
  </style>
</head>
<body>
  <table id="eventTable">
    <thead>
      <tr>
        <th class="sortable" data-sort="title">Event Title</th>
        <th class="sortable" data-sort="datetime_local">Date and Time</th>
      </tr>
    </thead>
    <tbody>
      <!-- Event data will be inserted dynamically here -->
    </tbody>
  </table>

  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script src="https://cdn.datatables.net/1.10.25/js/jquery.dataTables.min.js"></script>
  <script>
    $(document).ready(function() {
    var apiUrl = 'https://api.seatgeek.com/2/events?q=concert';
    var clientId = 'MzM3NjkyNzh8MTY4NDgxODg3Mi45OTMyOTk1';
    var clientSecret = 'bb0a4e78293d02ac50573254f61e3fd487680ca5678a8c58d1d7656ed5bff1f8';

    $.ajax({
      url: apiUrl,
      data: {
        client_id: clientId,
        client_secret: clientSecret
      },
      success: function(response) {
        var events = response.events;
        var tableBody = $('#eventTable tbody');

        $.each(events, function(index, event) {
          // Format the date in "month/day/year" format
          var date = new Date(event.datetime_local).toLocaleDateString(undefined, {
            month: 'numeric',
            day: 'numeric',
            year: 'numeric'
          });

          // Format the time in "2:00" format
          var time = new Date(event.datetime_local).toLocaleTimeString(undefined, {
            hour: 'numeric',
            minute: '2-digit'
          });

          var newRow = '<tr>' +
            '<td>' + event.title + '</td>' +
            '<td>' + date + ' ' + time + '</td>' +
            '</tr>';
          tableBody.append(newRow);
        });

        $('#eventTable').DataTable();
      },
      error: function(xhr, status, error) {
        console.log('Error:', error);
      }
    });
  });
</script>
</body>
</html>