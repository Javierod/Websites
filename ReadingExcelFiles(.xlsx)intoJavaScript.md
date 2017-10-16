```javascript
let url = ".xlsx";					//Path to file.
let req = new XMLHttpRequest();				//Calling http protocol
req.open("GET", url, true);				//Calling file.
req.responseType = "arraybuffer";			//Setting value.

///////////////////////////////////////////////////
//FUNCTION NAME: None.				 //
//FUNCTION PURPOSE: Read data from excel fields  //
//						 //
//PARAMETERS: 1. e - onload object.		 //
//RETURN: Nothing.				 //
///////////////////////////////////////////////////
req.onload = function(e) {
	let data = new Uint8Array(req.response);			      //Setting language (EN).
	let arr = new Array();						      //Setting array.
	for(let i=0; i!=data.length; ++i)	   	                      //Transforming req characters into ASCII values.
  		arr[i] = String.fromCharCode(data[i]);
  let bstr = arr.join('');												                      //Setting array.
	let workbook = XLSX.read(bstr, {type:"binary"});		      //Reading binary file.
	let first_sheet_name = workbook.SheetNames[0];			      //Defining fields.
	let worksheet = workbook.Sheets[first_sheet_name];		      //Filling array.
	dataArray = XLSX.utils.sheet_to_json(worksheet,{header:1,raw:true});  //Passing values to a local array of objects. as numbers.
                                                                              /*XLSX.utils.sheet_to_json(worksheet,{header:1})
                                                                                XLSX.utils.sheet_to_json(worksheet,{raw:true})
                                                                              /*
	console.log(dataArray);                                               //Sending array to console.
}

req.sedn()
  ```
