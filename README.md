let express = require("express");
let app = express();
app.use(express.json());
app.use(function(req,res,next){
    res.header("Access-Control-Allow-Origin","*");
    res.header("Access-Control-Allow-Methods", "GET,POST,PUT,DELTE,PATCH,OPTIONS,HEAD");
    res.header("Access-Control-Allow-headers", "Origin, X-Requsted-With, Content-Type, Accept");
    next();
});

const port = 2410;
app.listen(port,()=> console.log(`Node app listening on port ${port}!`));

let fs = require("fs");
let fname1 = "output1.txt";
let fname2 = "output2.txt";
let arr1 = [];
let arr2 = [];

app.get("/input1/:value",function(req,res){
    InsertVal(req,res);
});

app.get("/input2/:value",function(req,res){
    InsertVal(req,res);
});

async function InsertVal(req,res){
    let value = req.params.value;
    arr1.push(value);
    arr1.reduce((acc,cur)=>{
        acc = cur[0];
        let Find = arr2.find((ar) => ar[0] === acc);
        if(Find){
            //nothing
        }else{
            arr2.push(cur);
        }
    },"");
    let data1 = arr2.toString();
    let arr3 = arr1.sort();
    let data2 = arr3.toString();
    try{
        await fs.promises.writeFile(fname1,data1);
        let data3 = await fs.promises.readFile(fname1,"utf8");
        await fs.promises.writeFile(fname2,data2);
        let data4 = await fs.promises.readFile(fname2,"utf8");
        let data5 = data3.split(",");
        let data6 = data4.split(",");
        res.send("output1 : "+data5+"  output1 : "+data6);
    }
    catch(err){
        res.send(err);
    }
}


