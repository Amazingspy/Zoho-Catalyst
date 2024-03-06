const express = require('express')
const catalyst = require('zcatalyst-sdk-node')

const app = express()

app.use(express.json());

app.get("/tabledata", async (req, res) => {
  let catalystApp = catalyst.initialize(req);
  let datastore = catalystApp.datastore();
  console.log("Datastore initialized");

  //let table = datastore.table(5295000000061016);

  async function getMyPagedRows(nextToken = undefined) {
    datastore
      .table(5295000000179049)
      .getPagedRows({ nextToken, maxRows: 100 }) //Define the maximum rows to be fetched in a single page and pass it along with nextToken
      .then(({ data, next_token, more_records }) => {
        console.log("rows : ", data);
        res.status(200).send(data);
        if (more_records) {
          getMyPagedRows(next_token);
        } //Fetch the next set of records and the token string for the next iteration
      })
      .catch((err) => {
        console.log(err.toString());
      });
  }

  getMyPagedRows();
});

app.post("/data", (req, res)=>{
       var catalystApp = catalyst.initialize(req);
       var table = catalystApp.datastore().table(5295000000179049)

       var details = {
            Name: req.body.Name,
            Rollno: req.body.Rollno
       };

       var insertPromise = table.insertRow(details);
       insertPromise.then((row) => {
      console.log("Inserted Row: " + JSON.stringify(row));
      res.status(200).json(row);
    })
    .catch((err) => {
      console.log(err);
      res.send(err);
    });
})

app.post('/update', (req, res)=>{
     var catalystApp = catalyst.initialize(req);
     var table = catalystApp.datastore().table(5295000000179049);

    data = 
      {
      Name: req.body.Name,
      Rollno: req.body.Rollno, 
      ROWID: req.body.ROWID
      }
     
     var updaterow = table.updateRow(data);
     updaterow.then((response) =>{
       console.log("Updated rows: " + JSON.stringify(response));
       res.status(200).json(response);
     })
     .catch((err)=>{
       console.log(err.toString());
       res.send(err);
     })

     
})

app.delete('/delete', (req, res)=>{
  var catalystApp = catalyst.initialize(req);
  var table = catalystApp.datastore().table(5295000000179049)

  var row = table.deleteRow(req.body.ROWID);
  // var rowPromise = row.delete();
  row.then((response)=>{
    console.log("Deleted row: " + JSON.stringify(response));
    res.status(200).json(response);
  })
  .catch((err)=>{
    console.log(err);
    res.send(err);
  })
})

app.all('/', (req, res) => {
	res.status(200).send("The server is online")
})

module.exports = app;
