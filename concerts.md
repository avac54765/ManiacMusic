# Concert API, table, and search here

use api from here: https://rapidapi.com/seatgeek/api/seatgeek/


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
        Song Name
        <input type="text" name="songname" id="songname" required>
    </label></p>
    <p><label>
        Date of Completion:
        <input type="date" name="date2" id="date2" required>
    </label></p>
    <p><label>
        Number of Hours Completed:
        <input type="number" name="duration2" id="duration2" required>
    </label></p>
    <p><label>
        Grade:
        <!--<input type="text" name="grade" id="grade" required>-->
         <select name="Grade" id="grade">
      <option value="A">A</option>
      <option value="B">B</option>
      <option value="C">C</option>
      <option value="D">D</option>
      <option value="F">F</option>
  </select>
    </label></p>
    <p>
        <button>Submit</button>
    </p>
</form>

<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");
  // prepare URL's to allow easy switch from deployment and localhost
  const url = "https://teambaddieflask.duckdns.org/api/ISPE"
  //const url = "https://flask.nighthawkcodingsociety.com/api/users"
  const create_fetch = url + '/create';
  const read_fetch = url + '/';

  // Load users on page entry
  read_ISPE();


  // Display User Table, data is fetched from Backend Database
  function read_ISPE() {
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
        name2: document.getElementById("name2").value,
        date2: document.getElementById("date2").value,
        duration2: document.getElementById("duration2").value,
        grade: document.getElementById("grade").value
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
          alert('Name is missing, or is less than 2 characters, please refresh and enter a valid name')
        }
        if (response.status == 212) {
          alert('Duration is missing, or is not an integer, please enter a valid duration')
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
    const name2 = document.createElement("td");
    const date2 = document.createElement("td");
    const duration2 = document.createElement("td");
    const grade = document.createElement("td");

    // obtain data that is specific to the API
    name2.innerHTML = data.name2; 
    date2.innerHTML = data.date2; 
    duration2.innerHTML = data.duration2; 
    grade.innerHTML = data.grade; 

    // add HTML to container
    tr.appendChild(name2);
    tr.appendChild(date2);
    tr.appendChild(duration2);
    tr.appendChild(grade);

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

<br>
<br>
<button type="button" onclick="window.print();" class>Click here to print your report</button>
</body>

</html>
